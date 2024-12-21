
![[Pasted image 20241217075926.png]]

## 동작 순서
1. 클라이언트가 `Request`요청을 하면 (api 호출?) `DispatcherServlet`이 우선 요청을 받음 (web.xml url-pattern에 등록된 애들만)
2. `DispatcherServlet`에서 `HandlerMapping`에게 보내 요청을 처리할 수 있는 `Controller`를 찾는다.
	1. `@RequestMapping`,  `@GetMapping`, `@PostMapping` 등을 통해서 찾음
3. 로직 처리 Controller -> Service -> DAO -> DB 그리고 회수
4. `DispatcherServlet`에 `View` 이름을 반환
5. `ViewResolver`를 통해 출력할 View화면을 검색
6. 처리 결과를 `View`에 송신하고, View하면을 클라이언트에 전송

여기서 `DispatcherServlet`은 왜 거치게 될까? 바로 `HandlerMapping`을 하면 되는 것이 아닌가? 
GPT에게 물어보자..
## DispatcherServlet의 존재 이유.. GPT 답변
1. 단일 진입점 (Front Controller 패턴)
	- 직접 `HandlerMapping`을 호출하게 되면 요청 분기 처리(라우팅)를 중구난방으로 작성해야 됨
	- 유지보수 어려움
2. 확장성과 유연성
	- 요청 처리 과정에서 여러가지 부가 작업 수행 가능
	- `HandlerMapping`을 찾는 것도 일부분일 뿐 다양한 컴포넌트와의 협력 관리
3. 중앙 집중식 예외처리
4. 다양한 요청 방식 처리
	- `HandlerAdapter`와 `MessageConverters`를 활용해 요청 데이터 분석 및 적절한 핸들러 실행
5. 프레임워크와의 일관된 통합
### DispatcherServlet 없이 직접 HandlerMapping을 호출할 경우의 문제점

1. **반복 코드**: 요청마다 라우팅, 예외 처리, 데이터 변환 등을 수작업으로 작성해야 함.
2. **유지보수 어려움**: 요청을 처리하는 공통 로직이 중복되면 유지보수가 복잡해집니다.
3. **확장성 부족**: 요청 흐름을 유연하게 제어하기 어렵습니다. 추가 기능(예: 인터셉터, 예외 처리)을 도입하려면 많은 변경이 필요합니다.
라고 한다..

그럼  `DispatcherServlet`은 뭘까?
## DispatcherServlet
HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 Controller에 위임해주는 `Front Controller`라고 정의할 수 있다..
- Tomcat과 같은 Servlet Container가 요청을 받음
- 공통적인 작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 `Controller`를 찾아서 위임
### 장점
과거 모든 Servlet을 URL 매핑을 위해 `web.xml`에 모두 등록해야 했지만, DispatcherServle이 해당 애플리케이션으로 들어오는 모든 요청을 핸들링 해주고 공통 작업을 처리하면서 Controller를 구현해 놓으면 **서비스 로직에 좀 더 집중할 수 있게 됨** 
### 한계?
#### 정적 자원(Static Resources) 처리 
- Dispatcher Servlet이 이미지나 HTML/CSS/JS 등과 같은 정적 파일에 대한 요청을 모두 받기 때문에 **정적자원**을 불러오지 못하는 상황도 발생하기 도함 이를 위해 **2가지 방법을 고안함**
	1. 정적 자원 요청과 애플리케이션 요청을 분리 
	2. 애플리케이션 요청을 탐색하고 없으면 정적 자원 요청으로 처리
##### 1. 정적 자원 요청과 애플리케이션 요청 분리 
- /apps 의 URL로 접근하면 Dispathcer Servlet이 담당
- /resources의 URL로 접근하면 Dispatcher Servlet이 컨트롤 X 
	하지만 상당히 코드가 지저분해질 수 있다고 함
	모든 요청에 대해서 저런 URL을 붙여줘야 함 -> 직관적인 설계 X 
##### 2. 애플리케이션 요청을 탐색하고 없으면 정적 자원 요청으로 처리 
Dispatcher Servlet이 요청을 처리할 컨트롤러를 먼저 찾고 요청에 대한 컨트롤러를 찾을 수 없을 경우
2차원적으로 설정된 자원 경로를 탐색하여 자원을 탐색
- 영역을 분리하여 효율적인 리소스 관리 지원
- 확장을 용이하게 해줌 

