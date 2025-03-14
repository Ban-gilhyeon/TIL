[[JPA- Entity Mapping]]
[[JPA-JpaRepository, CrudRepositroy]]
[[JpaHeadbutt 중 궁금한 내용 공부]]
# JPA(Java Persistence API) 
- Java 진영에서 `ORM` 기술 표준으로 사용되는 인터페이스의 모음 
- 관계형 데이터베이스를 사용하는 방식을 효율적(**매핑**)으로 관리할 수 있는 프레임 워크
- 실제적으로 구현된 것이 아니라 구현된 클래스와 **매핑** 해주기 위해 사용되는 프레임워크
- 객체 지향 프로그래임과 데이터 베이스를 <U>자동으로 연결해주는 기술</U>
- JPA를 구현한 대표적인 오픈소스로는 Hibernate가 있음 
- ![[Pasted image 20240913234132.png]]


-> JPA를 사용하면 개발자는 객체 중심의 개발을 할 수 있으며, 데이터베이스의 테이블 생성, 쿼리 작성과 같은 복잡한 작업을 JPA가 대신 처리해주기 때문에 **개발자는 비즈니스 로직에 집중** `생산성`, `유지 보수성`이 크게 **향상**
-> JPA는 Java 어플리케이션과 데이터베이스 사이의 **패러다임 불일치 문제를 해결** 
	`복잡한 객체 관계` -> `관계형 데이터 베이스` 
	JPQL(Java Persistence Query Language)을 제공 -> 데이터베이스의 종류에 관계 없이 쿼리 작성 가능
	![[Pasted image 20240913234219.png]]

## ORM (Object - Relational Mapping)
: 애플리케이션 `class` 와 `RDB(Relational DataBase)`의 테이블을 매핑한다는 뜻
기술적으로는 에플리케이션의 객체를 RDB 테이블에 자동으로 **영속화** 해주는 것이라고 보면 됨

## 장점 
- SQL 문이 아닌 Method를 통해 DB를 조작할 수 있음 -> 로직에만 집중 
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어듬, 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높임 
- 객체지향적인 코드 작성이 가능, 오직 객체 지향적 접근만 고려하면 되기 때문에 생산성 증가
- 매핑하는 정보가 `Class`로 명시 되었기 때문에 `ERD`를 보는 의존도를 낮출 수 있고 **유지보수 및 리팩토링에 유리**
	- `MySQL` 사용하다가 `Oracle`사용해야 한다면... 에휴... 였지만 ORM이 쿼리를 날려주기 때문에 쿼리를 수정할 필요 없음
## 단점
- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있음
- 복잡하고 무거운 쿼리는 속도를 위해 별도의 튜닝이 필요함 -> 결국 SQL문을 써야 할 지도..
- 학습 비용이 비쌈 -> 어렵다고..
## JPA 사용 시 고려사항
: JPA는 데이터베이스와 상호작용을 추상화하고 자동화하지만, 이로 인해 성능 저하를 초래할 수 있다. 
JPA의 자동화된 처리 과정 중 **불필요한 쿼리가 실행되거나, 상호작용이 비효율적일 수 있음**
<U>따라서 JPA를 사용할 때는 성능 최적화를 위한 전략을 함께 고려해야 함</U>

## JPA 원리

![[Pasted image 20240913234701.png]]
1. **JPA는 애플리케이션과 JDBC사이에서 동작함**
2. 에플리케이션(repository가 아닐까..? service인가..?)에서 JPA로 명령을 하면 JPA를 구현한 **Hibernate**가 해석해서 JDBC API를 사용하여 SQL을 실행하고 그 결과를 반환받는다.  
3. `findAll()`과 같은 메소드를 사용해도 SELECT * ~~ 와 같은 쿼리 발생

### JPA CRUD 메소드
- 저장 : jpa.persist(member)
- 조회 : Member member = jpa.find(memberId)
- 수정 : member.setName("변경할 이름")
- 삭제 : jpa.remove(member)

### JPA 구동방식
![[Pasted image 20240917114538.png]]
JPA의 `Persistence`클래스가 `META-INF/persistence.xml`설정 파일을 읽어서 `EntityManagerFactroy`라는 클래스를 생성하고 여기서 필요할 때마다 `EnetityManager`를 만들어서 사용<br>
~~근데 JPA 좀 해줌..~~ 
```java
	
```

### JPQL


