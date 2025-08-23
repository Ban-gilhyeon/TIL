
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

