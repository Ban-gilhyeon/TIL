## Junit 5
: junit은 단위 테스트를 지원하는 오픈소스 프레임워크로 다음과 같은 특징을 가진다
- 문자 혹은 GUI 기반으로 실행
- @Test 메소드를 호출할 때 마다 새 인스턴스 생성
- 예상결과를 검증하는 assertion 제공
- 자동 샐행, 자체 결과 확인 및 즉각적인 피드백 제공
- 테스트 방식을 구분할 수 있는 어노테이션을 제공하며, 어노테이션만으로 간결하게 실행이 가능
### 기본 어노테이션의 종류
> @Test
> 	 - 테스트를 수행할 메소드, Junit은 각 테스트끼리 영향을 주지 않도록 테스트 실행, 객체를 매 테스트마다 만들고 종료 시 삭제 
> 	 - Junit5 부터는 Test메소드에 `public`을 안써도 됨 `void create()`이런식으로 가능
>  @BeforeAll
> 	 -  전체 테스트를 시작하기 전에 1회 실행 (ex DB연결, 테스트 환경 초기화, 전체 테스트 실행 주기에 한 번만 호출)
> 	 - `static void`를 붙여줘야 함
>  @BeforeEach
> 	  - 테스트 케이스를 시작하기 전마다 실행 (테스트 메서드에 사용하는 객체 초기화, 테스트에 필요한 데이터 삽입 등)
>  @AfterAll
> 	 - 전체 테스트를 마치고 종료하기 전에 1회 실행 (ex DB연결 종료, 공통으로 사용하는 자원 해제 등)
> 	 -  `static void`를 붙여줘야 함
>  @AfterEach
> 	 - 테스트 케이스를 종료하기 전 마다 실행 (테스트 이후 특정 데이터를 삭제 등)
>  @Disabled
> 	 - `@Test` 아래에 `@Disabled` 를 달아주면 전체 테스트를 실행할 때 그 테스트만 빼고 실행함
>  @DisplayName("스터디 만들기")
> 	- 테스트 이름 명시
> 	- 일반적으로 많이 사용
> 	- 한글로 적용가능
> @DisplayNameGeneration()
> 	- 클래스와 테스트메소드 둘다 적용가능
> 	- 클래스에 작성하면 하위 메소드들 모두 적용됨 
> 	- `DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)`  
> 		- `void create_new_Study()`이라는 메소드가 있을 때 `_`를 공백으로 치환해줌 
> 		- ![[Pasted image 20241021013939.png]]

아래와 같이 실행 됨
![[Pasted image 20241021011830.png]]

![[Pasted image 20241019212211.png]]
### 대표적인 단정문
expected : 기대하는 값
actual : 실제 값
- assertArrayEquals(expected, actual) : 배열 expected와  actual이 일치함을 확인
- aseertEquals(expected, actual) : 객체 expected와 actual이 같은 값인지 확인
	- assertEquals(expected, actual, "msg") : 객체 expected와 actual이 같은 값인지 확인,
	   실패 시 메세지 출력
	- msg 자리는 원래 `Executable타입` 자리임 근데 람다식으로 만든걸 간편하게 줄여서 쓸 수 있도록 한 것
- assertSame(expected, actual)) : 객체 expected와 actual이 같은 객체인지 확인
- assertTrue(expected) : expected가 참인지 확인
- assertNotNull(expected) : 객체 expected가 null이 아님을 확인
- assertAll( () -> other assert methods ) : 
	- 여러가지 assert 메소드를 작성하면 위에서 부터 차례대로 검증함 
	- 위에 assert가 깨지면 아래 assert는 맞는지 틀린지 모름 
	- `assertAll()`로 묶으면 위에가 깨져도 아래까지 검증함 
	- ![[Pasted image 20241021021234.png]]
- assertThrows(exception 타입) : 어떤 예외가 발생하는지 확인  
- assertTimeout(Duration.ofSeconds(10), ( ) -> 어떤 행동) : 특정 시간(10초) 이내에 어떤 행동을 끝내야 한다. 

### 조건에 따라 테스트 실행하기
: 특정한 환경 (os, 환경 변수  등등)
- assumeTrue(특정한 환경) : 특정한 환경에서만 테스트를 진행해라  
```java
@Test
@DisplayName("스터디 만들기")
void create_new_study(){
	// 로컬 환경에서만 테스트를 해라 
	assumeTrue("LOCAL".equalsIgnoreCase(System.getenv("TEST_ENV")))
	// true 값이면 아래의 테스트를 진행, false면 중지

	assertEquals(...);
}
```
- @EnabledOnOs(OS.MAC) : OS가 Mac인 경우에만 실행해라
- @DisdabledOnOS({OS.MAC, OS.LINUX}) : OS가 MAC,LINUX인 경우에만 실행 X
- @EnabledOnJre(자바 버전) : 자바 버전이 몇일 떄만 실행해라 

### 테스트 태깅과 필터링 (내 레벨에선 .. 잘 안 쓸듯)
- @Tag("name")
	- 테스트 메소드 위에 붙임
	- 테스트를 묶을 수 있음

### 반복 테스트 
- @RepeatedTest(횟수)
```java
@DisplayName("반복 스터디 만들기")  
@RepeatedTest(value = 10, name = "{displayName}, {currentRepetition}/{totalRepetitions}")  
void create_new_Study2(RepetitionInfo repetitionInfoinfo) {  
    System.out.println("test" + repetitionInfoinfo.getCurrentRepetition() + "/" + repetitionInfoinfo.getTotalRepetitions());  
}
```
![[Pasted image 20241021030425.png]]
- @ParameterizeTest()
	- 파라미터에 있는 배열 만큼 테스트 돌리기 
	- @ValueSource() 
		- 배열을 만들어주는거지.. 
```java
	@DisplayName("여자친구 만들기")  
	@ParameterizedTest(name = "{index} {displayName} message = {0}")  
	@ValueSource(strings = {"여자친구", "사귀고","싶다"})  
	void parameterizedTest(String message) {  
	    System.out.println(message);  
	}
```
![[Pasted image 20241021030834.png]]
- @NullAndEmptySource 
	- @NullSource + @EmptySource
	- @ValuserSource를 추가하는 어노테이션 Null, Empty 
	- ![[Pasted image 20241021031531.png]]

### 테스트 인스턴스
```java 
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)  
class StudyTest {  
	int value = 1;
    @Test  
    void create_new_Study() {  
       IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> new Study(-10));  
       assertEquals("스터디 최대 참석자는 0보다 커야 합니다.",exception.getMessage());  
       Study study = new Study(value++); // 1   
       /*assertNotNull(study);  
       assertEquals(StudyStatus.DRAFT, study.getStatus(), "스터디를 처음 만들면 상태 값이 DRAFT여야 함");       assertTrue(study.getLimit() > 0, "스터디 최대 참석 인원은 0보다 커야 함");*/       assertAll(  
             () -> assertNotNull(study),  
             () -> assertEquals(StudyStatus.DRAFT, study.getStatus(),"스터디 처음 만들면 상태 값은 DRAFT"),  
             () -> assertTrue(study.getLimit() > 0, "스터디 참석 인원은 0보다 커야 함")  
       );    }  
    @DisplayName("반복 스터디 만들기")  
    @RepeatedTest(value = 10, name = "{displayName}, {currentRepetition}/{totalRepetitions}")  
    void create_new_Study2(RepetitionInfo repetitionInfoinfo) { 
	   System.out.println(value++) //1
       System.out.println("test" + repetitionInfoinfo.getCurrentRepetition() + "/" + repetitionInfoinfo.getTotalRepetitions());  
    }
```
위와 같이 `StudyTest.class`에 인스턴스로 `int value = 1 `이라고 했을 때
하위 Test 메소드에서 `value++`를 하더라도 항상 1이 출력이 됨 
- @TestInstance(TestInstance.Lifecycle.PER_CLASS)
	- 위와 같이 인스턴스를 공유하고 싶다면 어노테이션으로 가능
	- BeforeAll, AfterAll의 경우 `static`이 강제되지만 위의 어노테이션을 씀으로써 강제되지 않음

### 테스트 순서
: JUnit의 로직에 따라 순서가 정해져 있긴 함
그러나 들어나져 있지 않은 이유는 단위테스트가 순서에 의존하면 안됨 -> 단위테스트는 서로 의존X 
- TestMethodOrder(MethodOrderer.(class))
	- Alphanumeric
	- OrderAnnoation
		- @Order(숫자)의 순서대로 실행 됨, 낮은 값일 수록 우선순위 UP
	- Random

### 확장 모델
JUnit 4의 확장 모델은 `@RunWith(Runner)`, `TestRule`, `MethodRule`
Junit 5의 확장 모델은 단 하나, `Extension`


### AssertJ
-  Junit과 사용해 가독성을 높여주는 `라이브러리`로 다양한 문법을 지원함
-  기존 Junit은 기댓값과 실제 비교 대상이 확실히 보이지 않아 잘 구분이 안됨 (isEqualTo 등 명확한 의미의 메서드로 대체 가능)
- assertThat : Junit의 assert메소드를 다르게 적을 수 있음 (취향차이)
	- AssertJ : assertThat(actual.getLimit()).isGreaterThan(0); 
	- Junit : assertTrue(expected.getLimit() > 0); 
### 주요 Assert 메소드

- **주요 비교 검증 테스트 메소드 

| method Name       | explain         |
| ----------------- | --------------- |
| isEqualTo(A)      | A값과 같은지 검증      |
| isNotEqualTo(A)   | A값과 다른지 검증      |
| contains(A)       | A값을 포함하는지 검증    |
| doesNOtContain(A) | A값을 포함하지 않는지 검증 |
| startWith(A)      | 접두사가 A인지 검증     |
| endsWith(A)       | 접미사가 A인지 검증     |
| isEmpty()         | 비어있는 값인지 검증     |
| isNotEmpty()      | 비어있지 않은 값인지 검증  |
| isPositive()      | 양수인지 검증         |
| isNegative()      | 음수인지 검증         |
| isGreeterThen(a)  | a값보다 큰 값인지 검증   |
| isLessThan(a)     | a보다 작은 값인지 검증   |


