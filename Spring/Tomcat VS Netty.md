
## Tomcat

흔히 Spring boot 프로젝트 생성 후 아무것도 하지 않고 빌드를 할 경우 Tomcat 서버가 열린다. 
`Tomcat`이란 ?
웹 어플리케이션 서버(엄밀히 J2EE 스펙 중 EJB 기술이 적용되지 않기에 WAS는 아님) 
아파치 톰캣은 비즈니스 로직을 처리할 때 서블릿 관리를 해준다는 점에서 `서블릿 컨테이너`에 가까움

- Apache Software Foundation에서 만든 오픈 WAS
- 자바 기반, 서블릿과 JSP를 실행할 수 있도록 지원

*WAS : J2EE 스펙을 구현한 서버로서 분산 트랜잭션, 보안, 스레드 처리 등의 기능을 처리하는 분산환경에서 사용되는 미들웨어 (즉, 웹 서버 + 웹 컨테이너로 웹 상에서 사용하는 컴포넌트를 올려 사용하는 서버)*

뇌피셜 ) Tomcat (서블릿 컨테이너) 이를 이용해서 편하게 웹 어플리케이션 개발 프레임워크 Spring -> 이를 더 편하게 (서블릿, 빈 관리 등 / 제어의 역전) 개발하기 위한 프레임워크 Spring boot

[[서블릿 VS 서블릿 컨테이너]]
### 역할

HTTP 서버 역할 : 클라이언트 요청을 받고 응답을 보냄
Servlet 컨테이너 역할 : 자바 서블릿 / JSP를 실행해서 동적인 웹 페이지를 만들어줌

### 핵심 특징
- Servlet 스펙 준수(필터, 리스너, 세션, 보안 등 표준 제공)
- 스레드 풀 기반 요청 처리(요청 마다 워커 스레드가 배정되는 구조 / 동기)
- 배포 단위 : WAR(외부 Tomcat) 또는 내장형(Spring boot가 기본 제공)
- HTTP/1.1, HTTP/2 지원, AJP 커넥터 제공
- 세션 관리/ JNDI dataSource/AccessLog/압축/SSL 설정 등 웹 운영 기능 기본 탑재

### Tomcat의 아키텍처 
![[Pasted image 20250822190707.png]]
출처 : https://kbss27.github.io/2017/11/16/tomcatarchitecture/

- 하나의 JVM에는 Tomcat 인스턴스가 하나만 존재할 수 있다. 
- Server안에는 여러개의 Service가 있을 수 있는데 Server의 주요 역할은 Connector를 정의하여 Engine에 연결 시켜주는 것이다.
- Service는 하나의 엔진만을 가지고 있으므로 Service와 Engine은 같은 의미로 봐도 무방하다
- Connector에 정의된 곳으로 Client의 Reqeust가 오면 host를 conf폴더 안에 server.xml을 보면 위의 아키텍처가 그대로 정의되어 있다

## Netty

- 비동기(Asynchronus), 이벤트 기반 네트워크 애플리케이션 프레임워크
- Java NIO (New I/O)위에서 동작 -> 논-블로킹 I/O를 효율적으로 다룰 수 있게 도와줌
- 저수준 소켓 프로그래밍의 복잡함(버퍼 관리, 멀티 플렉싱, 스레드 관리 등)을 추상화
- TCP, UDP, HTTP, WebSocket 같은 네트워크 프로토콜을 직접 구현 가능

=> 수만 개의 연결을 적은 스레드로 처리할 수 있는 고성능 네트워크 엔진 

### 핵심 특징 
- 이벤트 루프 
	- 몇개 안되는 스레드(Event Loop)가 수많은 연결을 동시에 관리 
	- 각 연결은 비동기 이벤트(읽기, 쓰기, 연결 종료 등)로 처리됨
	- 각 이벤트 루프는 항상 단일 스레드에 바인딩되어 효율적인 스레드 작업 실행 보장
	- 이벤트 큐를 사용하여 도착 순서대로 대기, 이벤트 루프에 의해 비동기적으로 처리됨
- 이벤트 루프 그룹
	- 이벤트 루프 인스턴스 그룹
	- `BossGroup`(일반적으로 스레드 1개) 수신 연결을 처리하고 이를 WorkerGroup의 이벤트 루프 할당
	- `WorkGroup`(스레드 N개) 설정된 연결을 처리하며, 각 연결은 자체 EventLoop로 관리
- 논-블로킹 I/O
	- 소켓 읽기/쓰기가 블로킹 되지 않음 -> 스레드가 대기하지 않고 다른 작업 처리 가능
- 채널 파이프라인
	-  데이터 입출력 과정에서 여러 핸들러를 체인처럼 연결
	- 인코딩/디코딩, 인증, 압축, 비즈니스 로직 등을 모듈화 가능
- 프로토콜의 유연성
	-  HTTP, WebSocket 같은 표준 프로토콜 뿐 아니라 커스텀 프로토콜도 쉽게 구현 가능
- 고성능
	- 초고동시성 환경에 적합
	- Netty 기반 유명 프로젝트 : gRPC, Elasticsearch 등

### 이벤트 루프와 단일 스레드
- 하나의 스레드가 I/O 이벤트(읽기, 쓰기, 연결 등)를 반복해서 처리하는 구조
- 무한 루프 돌면서 이벤트 큐에 쌓인 I/O 작업 처리
- 각 클라이언트 연결(Channel)은 이벤트 루프에 등록 -> 모든 작업은 해당 이벤트 루프에서만 실행
- Tomcat과 달리 단일 스레드(이벤트 루프 내에서)를 사용함으로써 최소한의 리소스로 이벤트 루프를 활용하여 동시 요청을 효율적으로 처리 가능함

1. 서버 시작 :
	1. bossGroup은 연결을 허용하기 위해 1개의 스레드로 생성
	2. workGroup은 여러 스레드로 생성되며 각 스레드 자체 EvnetLoop를 관리
2. 연결 수락 : 
	1. 클라이언트가 연결되면 bossGroup의 EventLoop가 연결을 수락
	2. 클라이언트 연경을 위해 새로운 NioSocketChannel 생성
	3. BossGroup은 이 채널을 WorkGroup의 EventLoop 중 하나에 할당
3. 채널 등록 : 
	1. 채널은 할당된 EvnetLoop에 등록
	2. EventLoop는 낸부 선택기에 채널을 추가
	3. 이 지점부터 이 채널에 대한 모든 I/O 작업은 동일한 EventLoop스레드에 의해 독점적으로 처리되어 일관성과 스레드 안전성이 보장됨
4. 이벤트 처리 :
	1. 이 채널의 모든 후속 이벤트(읽기, 쓰기)는 동일한 EventLoop 스레드에서 처리됨 
	2. 요청이 도착하면 EventLoop는 선택기를 통해 이를 감지하고 ChannelPipeLine을 통해 처리
	3. 각 EventLoop는 들어오는 요청(이벤트/작업으로 처리됨)이 순차적이고 Non-Blocking 처리를 위해 대기열에 추가되는 EventQueue를 유지
	4. EventLoop는 EventQueue에서 요청을 가져와 처리, 작업이 CPU바운드이고 비차단형이면 EventLoop는 즉시 처리함 하지만 작업이 차단 작업(DB 쿼리, 외부 API 호출)을 포함하는 경우, EventLoop가 차단되지 않도록 별도의 작업자 스레드풀로 오프로드됨
	5. 병렬 스레드가 차단 작업을 완료하면 요청은 다시 Queue에 푸시
	6. Event루프가 요청을 선택하면 요청을 완료하고 ChannelOutBoundHandler를 통해 응답을 다시 보냄

![[Pasted image 20250825031610.png]]
이미지는 아래 링크에서 확인 : https://medium.com/%40gourav20056/spring-webflux-internals-how-nettys-event-loop-threads-power-reactive-apps-4698c144ef68

## 왜 서로 비교가 되는가? 

Tomcat과 Netty는 서로 완전히 다른 것을 알아볼 수 있었지만 여전히 의문점인 것은 왜 서로 비교되는 것인가? 이다 

- Tomcat : 서블릿 컨테이너 (Servlet API 기반)
- Netty : 네트워크 프레임워크 (Servlet API 기반 아님, 저수준 I/O)

이와 같이 서로 다른 성격을 지니고 있지만 자주 비교되는 이유는 Spring을 이용한 웹 애플리케이션 개발에 있다. 

#### reactive/non-blocking 
기존의 Servlet 스펙은 요청당 스레드 모델이 기본 -> 동기/블로킹에 최적화 되어 있었지만, 
현재 웹은 "실시간/스트리밍/초고동시성(웹소켓, SSE, 채팅, IoT 등)" 패턴이 많아졌다

이걸 위해 Spring 팀이 `Spring WebFlux`라는 리액티브 웹 프레임워크를 만듦
- Netty 서버 엔진을 기본으로 선택(Reactor Netty)
- 이벤트 루프 기반으로 작동

![[Pasted image 20250824014358.png]]



출처 : https://medium.com/%40gourav20056/spring-webflux-internals-how-nettys-event-loop-threads-power-reactive-apps-4698c144ef68
