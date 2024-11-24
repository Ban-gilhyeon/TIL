## Spring Cloud 

### Spring Gateway Tomcat으로 실행되는 경우
- 환경
	- JAVA 17
	- Spring boot 3.3.5
	- Spring Cloud 2023.0.3
	- maven
- 상황
	- `application.yml` 작성 중 `Spring: cloud: gateway: routes:`를 찾을 수 없고 `Spring: cloud: gateway: mvc: routes:`가 잡히는 상황
	- ![[Pasted image 20241113161128.png]]
	- 위의 상황에서 실행시 Netty 서버가 아닌 Tomcat으로 실행되는 상황
	- ![[Pasted image 20241113161158.png]]
- 해결
	- `pom.xml` or `build.gradle`에서 `spring-cloud-starter-gateway-mvc`가 implementation 되어있어 이를 `spring-cloud-starter-gateway`로 수정하고 sync
	- `pom.xml`![[Pasted image 20241113161244.png]]
		- `application.yml`![[Pasted image 20241113161333.png]]
 
	- ![[Pasted image 20241113160952.png]] 임을 확인

### ApiGateway LoadBalancing 500 에러 발생
- 문제 : 
	-  `ApiGateway.project`의 `application.yml`에서 uri 부분을 lb://First_Service/로 수정했을 때 500 Internal server error 발생 
	- ![[Pasted image 20241117201404.png]]
	- ![[Pasted image 20241117201618.png]]
- 환경 : 
	- JAVA 17
	- Spring boot 3.2.0
	- Spring Cloud 2023.0.3
	- maven
- 해결전
	- first_service
		- FirstServiceController.class
		```java
@RestController  
@RequestMapping("/first_service/")  
@Slf4j  
public class FirstServiceController { 
    @GetMapping("/welcome")  
    public String welcome() {  
       return "Welcome to First Service";  
    }  
    @GetMapping("/message")  
    public String message(@RequestHeader("first_request") String header) {  
       log.info(header);  
       return "Hello First Service";  
    }  
    @GetMapping("/check")  
    public String check() {  
       return "Chcek HI First_Service";  
    }}
		```
		- application.yml
	```java
server:  
  port: 8081  
  
spring:  
  application:  
    name: my_first_service  
  
eureka:  
  client:  
    register-with-eureka: true  
    fetch-registry: true  
    service-url:  
      defaultZone: http://127.0.0.1:8761/eureka
	```
	- ApiGateway_service
		- application.yml
```java
server:  
  port: 8000  
  
eureka:  
  client:  
    register-with-eureka: true  
    fetch-registry: true  
    service-url:  
      defaultZone: http://localhost:8761/eureka/  
  
spring:  
  application:  
    name: apigateway_service  
  cloud:  
  
    gateway:  
      routes:  
        - id: first_service #first-service  
          uri: lb://MY_FIRST_SERVICE #MY-FIRST-SERVICE   
predicates:  
              - Path=/first_service/**  
          filters:  
  #            - AddRequestHeader=first_request, first_requests_header2  
  #            - AddResponseHeader=first_response, first_response_header2              - CustomFilter  
        - id: second-service  
          uri: http://127.0.0.1:8082/  
          predicates:  
            - Path=/second-service/**  
          filters:  
#            - AddRequestHeader=second_request, second_requests_header2  
#            - AddResponseHeader=second_response, second_response_header2  
            - name: CustomFilter  
            - name: LoggingFilter  
              args:  
                baseMessage: Gateway Logging Filter  
                PreLogger: true  
                PostLogger: true  
  
  
#logging:  
#  level:  
#    org.springframework.cloud.gateway: DEBUG  
#    org.springframework.cloud.netflix.eureka: DEBUG  
#    org.springframework.cloud.loadbalancer: DEBUG
```
- 해결 
	- Spring Cloud와 Spring boot의 버전 문제 ? 
		- 기존의 Spring Cloud 2023.0.3 버전과 Spring boot 3.3.5 사용
		- Spring boot의 버전이 너무 높아서 Cloud와 호환이 안되나 고민 -> 3.2.0으로 수정 
		- 위의 문제와 무관..
	- 사용하지 않아도 되는 Filter문제
		- ApiGateway_Service의 `application.yml` 에 사용하지 않아도 되는 `filter` 가 너무 많아서 충돌이 일어나나? 
		- 문제와 무관
	- First_Service의 `_`인식 문제 (해결)
		- application.yml
	```java
server:  
  port: 8081  
  
spring:  
  application:  
    name: my_first_service # my-first-service 
  
eureka:  
  client:  
    register-with-eureka: true  
    fetch-registry: true  
    service-url:  
      defaultZone: http://127.0.0.1:8761/eureka
	```

	- ApiGateway_Service
		- application.yml
	```java
server:  
  port: 8000  
  
eureka:  
  client:  
    register-with-eureka: true  
    fetch-registry: true  
    service-url:  
      defaultZone: http://localhost:8761/eureka/  
  
spring:  
  application:  
    name: apigateway_service  
  cloud:  
  
    gateway:  
      routes:  
        - id: first_service #first-service  
          uri: lb://MY_FIRST_SERVICE #MY-FIRST-SERVICE  
          predicates:  
              - Path=/first_service/**  
          filters:  
  #            - AddRequestHeader=first_request, first_requests_header2  
  #            - AddResponseHeader=first_response, first_response_header2              - CustomFilter  
        - id: second-service  
          uri: http://127.0.0.1:8082/  
          predicates:  
            - Path=/second-service/**  
          filters:  
#            - AddRequestHeader=second_request, second_requests_header2  
#            - AddResponseHeader=second_response, second_response_header2  
            - name: CustomFilter  
            - name: LoggingFilter  
              args:  
                baseMessage: Gateway Logging Filter  
                PreLogger: true  
                PostLogger: true  
  
  
#logging:  
#  level:  
#    org.springframework.cloud.gateway: DEBUG  
#    org.springframework.cloud.netflix.eureka: DEBUG  
#    org.springframework.cloud.loadbalancer: DEBUG
	```
- 위와 같이 `_`를 `-`으로 수정하면 해결 가능..! 
- 왜일까?
	- Eureka는 기본적으로 **서비스 ID를 대문자로 변환**하며, **RFC 1123 호환 규칙**을 따릅니다. 이 규칙에 따르면, **서비스 이름은 알파벳, 숫자, 하이픈(`-`), 마침표(`.`)만 허용**되며, 언더스코어(`_`)는 허용되지 않습니다.

## Application.yml 인식을 못하는 문제
- 문제 : 
	- server: port: 0으로 설정 
		- 8080으로 고정된 ip를 사용
	- greeting : message : welcome 
		- log를 확인한 결과 `null` 출력 
- 해결 : 
	- 여러 mirco service를 사용하다보니 `application.yml`의 이름을 `application{-micro-service}`으로 바꿈 
	-  Spring Boot가 **기본적으로 `application.yml` 또는 `application.properties`만 자동으로 로드**