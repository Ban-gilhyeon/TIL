엔터프라이즈 서비스 개발 프로젝트에 활발히 채택되고 있음

과거의 모놀리틱 아키텍처 대신 클라우드 상에서 하나의 작은 서비슷 단위로 개발해 
`변경`, `조합`,`재활용`이 가능 하도록 구성한 최신 아키텍처로 **유연성, 확장성, 안정성**을 확보할 수 있음
- 모놀리틱 아키텍처 (Monolithic Architecture)
	- 장점 
		- 개발 초기의 단순한 구조로 개발 용이
		- End-To-End 테스트 용이
	- 단점
		- 긴 빌드, 배포 시간, 장애 문제 처리 어려움
		- 유지보수 어려움
		- 개발 언어 변경 어려움
- MSA
	- 장점
		- 빠른 서비스 개발, 출시 시간의 단축
		- 유연한 확장, 서비스 별 독립적 배포 가능
		- 안정적인 서비스 운영
	- 단점
		- 상대적인 복잡성
		- 트랜잭션 유지의 어려움
		- API 관리의 중요성 증가
![[Pasted image 20241108103107.png]]

## 특징
1) 독립된 하나의 모듈에서 발생한 장애는 **전체 어플리케이션에 크게 영향**을 미치지 않음
2) 모듈 간의 의존도가 낮아 **서로 다른 기술 스택**을 사용할 수 있음
3) 각 마이크로 **서비스를 독립적으로 배포**하는 것이 용이
 ![[Pasted image 20241108102141.png]]

## 도입 과정
1) MSA 기술 컨설팅
	- 서비스 현황 및 고객 요구사항, 이슈, 향후 방향성을 종합 검토한 후 MSA 원칙에 부합하는 고도화 방안 및 로드맵 제시 
2) 분석 / 설계
	- MSA 원칙에 입각한 세분화된 업무 흐름도와 프로세스 정의서 작성을 위한 가이드 및 표준화된 구현방안 제시
3) 개발 / 구축
	- MSA 아키텍처에 따른 개발 과정을 지원하고 신기술 및 공통기능 지원
4) 검증 / 운영
	- 테스트 방법론에 대한 가이드를 비롯하여 성능 및 품질을 검증할 수 있는 방안 제시 

## 구성도 
![[Pasted image 20241108113352.png]]

남색 : Inner Architecture
회색 : Outer Architecture

- Inner Architecture
	- 개별 마이크로 서비스 구축을 위한 아키텍처로 내부 서비스를 어떻게 잘 나누는가에 대한 아키텍처
	- 서비스 정의
		- 비즈니스, 서비스 간의 종속성, 배포 용이성, 장애 대응, 운영 효율성 등을 고려하여 마이크로 서비스 정의
	- DB Access 구조
		- 각 마이크로 서비스는 자체의 데이터베이스를 소유
		- 여러 데이터베이스에 연결되어 있는 경우 데이터베이스 정합성을 보장할 수 있는 설계가 필요
- Outer Architecture
	- 개별 마이크로 서비스가 개발, 배포, 실행 되는 운영환경과 분산된 마이크로 서비스 관리 기능을 제공하는 아키텍처
	- External Gateway
		- 전체 서비스 외부로 부터 들어오는 접근을 내부 구조를 드러내지 않고 처리하기 위한 요소
		- 사용자 인증, 권한 정책 관리
	- Service Mesh
		- 마이크로 서비스 구성 요소간 네트워크를 제어하는 역할
		- Service Discovery, Service Routing, 트래픽 관리, 보안
	- Container Managenment
		- 컨테이너 기반 어플리케이션 운영 (k8s)
	- Backing Service
		- 어플리케이션이 실행되는 가운데 네트워크를 통해서 사용할 수 있는 모든 서비스
		- 데이터베이스, 캐쉬 시스템, SMTP 서비스
	- Telemetry
		- 실시간으로 먼 거리에서 우너격으로 측정할 수 있는 기능
		- 서비스 상태 모니터링
	- CI/CD Automation
		- 어플리케이션 개발 단계를 자동화하여, 어플리케이션보다 짧은 주기로 고객에게 제공하는 


## Cloud Native Architecture
- 확장 가능한 아키텍처
	- 시스템의 수평적 확장에 유연
	- 확장된 서버로 시스템의 부하 분산, 가용성 보장
	- 시스템 또는 서비스 애플리케이션 단위의 패키지 (컨테이너 기반 패키지)
	- 모니터링
- 탄력적 아키텍처
	- 서비스 생성 - 통합 - 배포, 비즈니스 환경 변화에 대응 시간 단축
	- 분활 된 서비스 구조
	- 무상태 통싱 프로토콜
	- 서비스의 추가와 삭제 자동으로 감지
	- 변경된 서비스 요청에 따라 사용자 요청 처리 (동적 처리)
- 장애 격리 
	- 특정 서비스에 오류가 발생해도 다른 서비스에 영향 주지 않음
## CI/CD
- 지속적인 통합, CI(Continuous integration)
	- 통합 서버, 소스 관리(SCM), 빌드 도구, 테스트 도구
	- ex ) jenkins, Team CI, Travis CI
- 지속적인 배포
	- Continuous Delivery
	- Continuous Deployment
	- Pipe Line
## DevOps
개발 조직 + 운영 + QA 
![[Pasted image 20241108171131.png]]
- 전체 개발 일정이 완료 될 때까지 지속적으로 진행
	- 개발
	- 피드백
	- CI
	- CD
## Container 가상화
![[Pasted image 20241108172245.png]]
- Virtualized Deployment
	- Hypervisor 기술을 통해 가상머신을 가질 수 있음 
	- 가상 머신은 각각의 어플리케이션을 이용할 수 있음 -> host 운영체제에 많은 부화를 주게 됨
	- 시스템 확장에 한계
- Container Deployment
	- 컨테이너의 라이브러리나 리소스를 공유하여 사용 
	- 적은 리소스 사용 가능 
	- 컨테이너 가상화 위에 돌아가는 서비스는 가볍고 빠르게 사용할 수 있음 

## 개발 방식의 차이

![[Pasted image 20241111104116.png]]
- MSA 방식은 서비스별로 각각 DB가 있을 수도 있음 
- 마이크로 서비스 별로 배포가 가능

## MicroService의 특징
1) Multiple Rates of Change
2) Independent Life Cycles
3) Independent Scalability
4) Isolated Failure
5) Simplify Interactions with External Dependencies
6) Polygolt Technology

SOA(서비스 지향 아키텍처)와 MSA의 차이점
- SOA 
	-  재사용을 통한 비용 절감
	- 공통의 서비스를 ESB(서비스 버스)모아 사업 측면에서 공통 서비스 형식으로 서비스 제공
	- 서비스 공유 최대화
- MSA 
	- 서비스 간의 결합도를 낮추어 변화에 능동적으로 대응
	- 각 독립된 서비스가 노출된 REST API를 사용
	- 서비스 공유 최소화
![[Pasted image 20241111113136.png]]

## Restful API
0) 기존의 리소스를 단순하게 웹 서비스 상태로 제공하기 위해 URL만 맵핑
1) 적절한 패턴으로 작성, 적절한 HTTP 메소드 X 
	1) 단순하게 `get` `post` 메소드만을 사용
	2) 성공 시 단순히 200 ok만을 출력
2) 레벨 1 + HTTP 메소드 (리소스의 상태를 구분)
	1) 제공하려는 리소스를 메소드에 맞게 제공
	2) 단순 읽기는 `get`
	3) 새로운 리소스를 추가할 때는 `post`
	4) 기존의 리소스를 수정할 때는 `put`
	5) 기존의 리소스를 삭제할 떄는 `delete`
3) 레벨 2 + HATEOAS
	1) DATA와 다음 단계에서 어떤 작업을 수행할 수 있는지까지 제공

추가적으로 고려해야 될 사항
- Consumer first
	- 개발자 입장에서보다 사용자 입장에서 명료하고 직관적인 API
	- 사용자 뿐만 아니라 다른 개발자(API 사용하고 있는)들 포함
- Make best use of HTTP
	- 타입 헤더값과 같은 HTTP의 장점을 최대한 살리기
- Request methods
	- GET : Read 
	- POST : Create
	- PUT : Update
	- DELETE : delete
- Response Status
	- 적절한 상태코드가 전달되어야 함 성공했다면 왜 성공했는지도 포함되어야 함
	- 200
	- 404
	- 400
	- 201
	- 401
- No secure info in URL
- Use plurals
	- 복수형태의 URI 값을 사용할 것
	- prefer / users to / user
	- 가능한 명사 형태로 사용할 것 
	- prefer / users / 1 to /user 1
- User nouns for resources
- For exceptopns
	- define a consistent approach
	- 일관적인 엔드 포인트를 사용할 것 
	- /search 
	- / PUT /gists/{id}/star
	- DELETE / gists / {id} / star


## API Gateway Service
![[Pasted image 20241113122327.png]]
- 인증 및 권한 부여
- 서비스 검색 통합
- 응답 캐싱
- 정책, 회로 차단기 및 `QoS` 다시 시도
- 속도 제한, 분하 분산
- 로깅 , 추적, 상관 관계
- 헤더, 쿼리 문자열 및 청구 변환
- IP호용 목록에 추가
### Spring Cloud에서의 MSA간 통신
-  RestTemplate
	```java
	RestTemplate restTemplate = new RestTemplate();
		restTemplate.getForObject("http://localhost:8080/",User.class,200);
	```
-  Feign Client
```java
@FeignClient("stores")
public interface StroreClient{
	@RequestMapping(method = RequestMethod.GET, value ="/stores")
	List<Store> getStores();
}
```

### Spring Cloud gateway
(Reactive Gateway : 그냥 gateway 추가하면 mvc-gateway라 강의와 달랐음)
- gateway의 역할
	- 요청이 들어오면 해당하는 micro-service에 Mapping 시켜줌
		- yml 파일에서 아래와 같이 설정 가능
		```yml
spring:  
  application:  
    name: apigateway-service  
  cloud:  
    gateway:  # routing 설정 부문
      routes:  
        - id: first_service  
          uri: http://localhost:8081/  
          predicates:  
            - Path=/first_service/**  
          filters:  
            - AddRequestHeader=first_request, first_requests_header2  
            - AddResponseHeader=first_response, first_response_header2

			```

		- filterConfig.java 파일을 통해서 설정가능
		```java
//@Configuration  
public class FilterConfig {  
    //@Bean  
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {  
       return builder.routes() // yml에 있는 cloud: routes: 내용과 동일한 내용임  
             .route(r -> r.path("/first_service/**")  
                   .filters(f -> f.addRequestHeader("first_request","first_request_header")  
                         .addResponseHeader("first_response","first_response_header"))  
                   .uri("http://localhost:8081"))
		```
		- CustomFilter.java 파일을 통해서 설정 가능