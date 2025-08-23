
## 동기(Synchronous) & 비동기(Asynchronous)

동기(Synchronus)
- 작업이 끝날 때까지 기다렸다가 다음 작업을 수행하는 방식
- A 작업이 끝나야 B 작업 시작 가능
- 흐름이 직선적이라 이해하기 쉽지만, 오래걸리는 작업이 있으면 전체가 지연
- **작업의 순서가 보장됨**

비동기(Asynchronus)
- 작업을 시켜놓고 기다리지 않고 다음 작업을 바로 진행
- 작업이 끝나면 알림 (콜백, 이벤트, Future/Promise 등)으로 결과를 
- **순서 보장 X**

![[Pasted image 20250822192342.png]]



## 블로킹(Blocking) & 논블로킹(Non-Blocking)

**호출한 쪽이 "바로 제어권을 돌려받는가?"** 에 대한 개념

블로킹 (Blocking)
- 함수를 호출하면 결과가 나올 때까지 멈춰서 기다림
- 호출한 스레드는 일을 못하고 그냥 **대기 상태**

논블로킹(Non-Blocking)
- 함수를 호출하면 바로 리턴되고, 호출한 스레드는 다른 일을 계속할 수 있음
- 결과는 나중에 콜백/이벤트/폴링 방식으로 확인

![[Pasted image 20250822192437.png]]

A가 B호출(작업중) 이후 C를 호출 할 때 ) 
- B의 작업이 끝날 때까지 제어권이 A에게 없음 -> B 작업 완료 이후 C 호출, `블로킹`
- B의 결과와 상관없이 C를 호출할 수 있다면 `논-블로킹`

## 논-블로킹 VS 비동기 

- 블로킹 VS 논블로킹 -> **"호출 즉시 제어권을 돌려주냐?"
- 동기 VS 비동기 -> **"결과를 기다리냐, 알림으로 받냐"**
즉, 
- 블로킹 / 논블로킹은 `함수 호출 시점`의 동작 방식 (호출 시점의 제어권 문제)
- 동기 / 비동기는 `작업 완료 결과`를 처리하는 방식  

## Spring MVC에서의 동작 

동기 + 블로킹( 요청 1건 당 Tomcat 워커 스레드 1개가 끝까지 처리)

![[Pasted image 20250822224411.png]]
![[Pasted image 20250822190707.png]]


1. Tomcat Connector
	1. 요청 수신
	2. HTTP 요청 -> Tomcat Connector가 받음
	3. 워커 스레드 할당
2. Filter Chain
	1. 서블릿 필터가 순서대로 실행
	2. 요청/응답 공통 전처리, 후처리에 적합
3. DispatcherServlet
4. HandlerMapping, HandlerAdaper, Controller
	1. HandlerMapping으로 "누가 처리할지" 찾기
	2. HandlerApater로 "어떻게 호출할지" 결정
	3. Interceptor(PreHandle) : 인증/권한 체크, 공통 로깅 등
	4. Controller 메서드 호출
5. ViewResolver/HttpMessageConverters
6. 결과 렌더링

기본은 동기/블로킹 이지만, `Servlet3.0+` 기반으로 "요청 스레드를 빨리 놓아주는" 비동기도 가능
- Callabel<T> : 컨트롤러가 Callable을 반환하면, 현재 스레드를 반환하고 별도 풀에서 작업 후 다시 디스패치
- DeferredResult<T> / WebAsyncTask : 외부 이벤트가 끝나면 결과를 설정 -> 스프링이 다시 디스패치해서 응답 전송
- SseEmitter / StreamingResponseBody : 서버-발신 이벤트/스트리밍 응답

	비동기를 쓰면 Tomcat 워커 스레드 점유 시간을 줄여 고지연 I/O(외부 API 등)에서 동시성을 더 확보할 수 있음
	단, 실제 작업은 별도 Executor(스레드 풀) 로 오프로딩해야 함 (DB/JPA는 여전히 블로킹).
