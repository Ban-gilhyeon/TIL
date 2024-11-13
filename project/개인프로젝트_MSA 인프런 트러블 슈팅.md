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