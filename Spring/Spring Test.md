
## CT(UT, 단위 테스트)
- FAST : 빠르게 동작하여 자주 돌릴 수 있어야 함
- Independent : 테스트는 독립적이며 서로 의존해서는 안된다
- Repeatable : 어느 환경에서도 반복이 가능해야 한다 
- Self-Validating : 테스트는 성공 또는 실패로 결과를 내어 자체 검증되어야 한다
- Timely : 테스트는 적시에 테스트 하려는 실제 코드를 구현하기 직전에 구현해야 한다

**Test Suite** : 
- 테스트 묶음은 여러 `테스트 케이스`, `테스트 묶음`, 또는 둘 다의 모임 
- 서로 같이 실행되어야 할 테스트를 종합하는데 사용 됨

**SUT** (System Uner Test)
- CT에서 각각의 `Test Suite` 은 무엇을 테스트할 것인지 그 대상을 명확히 해야 함
- 하나의 테스트에서 <U>테스트 하고자 하는 주요 대상이 되는</U> `Unit`, `Component` 을 `SUT`라고 부름
- 시스템의 일부분이기 때문에 `Unit` 과 상호 작용
- 상호작용 중 `SUT` **자신이 의존하고 있는 객체**가 있을 수 있는데 이를 `DOC`(Depended On Component)라고 부름 
![[Pasted image 20240909102349.png]]

**Test Double**
: 주연배우가 위험한 촬영을 할 때 `스턴트맨(스턴트 더블)`을 고용하는 것에 유래
- `SUT`를 테스트할 때 `DOC`가 문제가 되는 경우가 종종 있음 
- `DOC`가 외부 리소스에 의존적인 경우, `SUT`테스트는 더 어려워짐 
- 위의 경우 테스트를 위해 실제 (`SUT`가 아니라) `DOC`를 스턴트맨,  ` TEST DOUBLE `로 대체할 수 있음 
- `SUT`가 실제 API라고 생각할 정도로 비슷한 API, 필요한 API만 구현하면 됨
- `TEST DOUBLE` 은 테스트 목적으로 만드는 **a Production Object**을 지칭함
- `MOCK`과 유사한 의미를 가지며 `Test double`이 좀 더 상위 의미
- `Mock` 과 `stub` 모두 Test Double의 종류

**Test Isolation**
: 위 처럼 `SUT`를 테스트할 떼 `DOC` 때문에 테스트가 어려운 경우 이 `DOC`를 `Test Double`로 교체하여 `SUT`가 외부 환경 변화에 영향을 받지 않도록 만드는 과정(~~환경이 더 맞지 않을까 싶음.. 진짜 뇌피셜로~~)을 `Test Isolation`이라고 부름 
- 이런 환경에서 테스트 하는 것을 `Solitary Test`라고 함
- 반면 외부 리소스를 사용하더라도 무방할 때 
	- 의존성을 제거하지 않고 테스트 수행 -> `Sociable Test`라고 함

**UseCase**
: 요구사항 달성을 위해서 소프트웨어 내에서 일어나느 여러가지 시나리오 
	1. 사용자가 제공해야 하는 입력
	2. 사용자에게 보여줄 출력
	3. 해당 출력을 생성하기 위한 처리 단계 기술
모든 `CT`적어도 하나의 UseCase를 달성하기 위해 사용 됨 
(UseCase 충족을 위해 적절히 동작하는지 테스트 하는 것이 관건)

**Unit Test** 과정 
`setup`, `exercise`, `verify`, `teardown` 4단계로 진행
1. Setup Phase
	- `SUT`를 테스트하기 위한 환경 구성
	- 적절한 인스턴스 생성, 테스트를 위한 Input Data (TC)를 구성
2. Exercise Phase
	- 테스트하고자 하는 동작을 수행
	- `SUT`의 상태를 변경하고 이를 명시적으로 확인할 수 있어야 함
3. Verify Phase
	- 수행한 테스트가 예상한 결과를 도출하였는지를 체크
4. Teardwon Phase
	- 해당 테스트가 수행되지 이전 상태로 환경을 다시 돌려 놓는 작업

## Junit  
: junit은 단위 테스트를 지원하는 오픈소스 프레임워크로 다음과 같은 특징을 가진다
- 문자 혹은 GUI 기반으로 실행
- @Test 메소드를 호출할 때 마다 새 인스턴스 생성
- 예상결과를 검증하는 assertion 제공
- 자동 샐행, 자체 결과 확인 및 즉각적인 피드백 제공
- 테스트 방식을 구분할 수 있는 어노테이션을 제공하며, 어노테이션만으로 간결하게 실행이 가능
### 어노테이션의 종류
> @DisplayName 
> 	: 테스트 이름 명시
> @Test
> 	 : 테스트를 수행할 메소드, Junit은 각 테스트끼리 영향을 주지 않도록 테스트 실행, 객체를 매 테
> 	 스트마다 만들고 종료 시 삭제 
>  @BeforeAll
> 	 : 전체 테스트를 시작하기 전에 1회 실행 (ex DB연결, 테스트 환경 초기화, 전체 테스트 실행 주기에 한 번만 호출)
>  @BeforeEach
> 	  : 테스트 케이스를 시작하기 전마다 실행 (테스트 메서드에 사용하는 객체 초기화, 테스트에 필요한 데이터 삽입 등)
>  @AfterAll
> 	 : 전체 테스트를 마치고 종료하기 전에 1회 실행 (ex DB연결 종료, 공통으로 사용하는 자원 해제 등)
>  @AfterEach
> 	 : 테스트 케이스를 종료하기 전 마다 실행 (테스트 이후 특정 데이터를 삭제 등)

### AssertJ
-  Junit과 사용해 가독성을 높여주는 `라이브러리`로 다양한 문법을 지원함
-  기존 Junit은 기댓값과 실제 비교 대상이 확실히 보이지 않아 잘 구분이 안됨 (isEqualTo 등 명확한 의미의 메서드로 대체 가능)
- 

### Give - when - Then 패턴(모델)
: Give-When-Then 패턴은 `TestCode` 스타일을 표현하는 방식 
- Given
	- 테스트에서 `구체화` 하고자 하는 행동을 시작하기 전에 `테스트 상태`를 설명하는 부분 
- When
	- 구제화하고자 하는 그 행동, 테스트를 진행하는 단계 (어떤 메소드를 실행시키면)
- Then
	- 어떤 `특정한 행동` 때문에 발생할 것이라고 예상되는 변화에 대한 설명 (어떤 결과가 예상,기대된다다)
![[Pasted image 20240909094415.png]]
- 보편적으로 하나의 `Class`에 하나의 단위 테스트를 작성함 -> 위의 그림에서 SUT가 `class`에 해당함
- SUT(`class`)가 제공하는 Method(기능)들을 테스트 한다고 생각하면 됨 
- 다른 SUT(class)에 도움을 받는 것을 차단 -> Test double : stub 처리한다~ 
- **CT가 어떻게 보면 기능 명세서가 될 수 있음** 
 ```java
 예시 
 기능 : 사용자들의 평균 점수 조회
 시나리오 : 0점 이상의 점수들에 대한 평균을 구함
public class AverageScoreCalculator {
	private static Integer sum = 0;
	private static Integer count = 0;
	public void addScore(Integer score) {
		if (!validateScores(score))	{
			throw new IllegalStateException("Invalid score");
		}
		sum += score;
		count++;
	}
	private boolean validateScores(Integer score) {
		return score > 0;
	}
	public Double getAverageScore() {
		return (double) (sum / count);
	}
}

```
위 클래스를 `JUnit` 을 이용하여 테스트 클래스를 작성함 
- 점수의 평균이 일치하는지 테스트 (도출한 답이 맞는지 확인)
```java
// JUnit을 이용한 테스트 클래스 
// 점수의 평균이 일치하는지 테스트 

@DisplayName("점수의 평균 테스트")
	@Test
	void averageScoreTest() {
		//given 양수의 점수가 주어짐
		AverageScoreCalculator averageScoreCalculator = new AverageScoreCalculator();
		int[] scores = {10,20,30,40,50};
		//when 평균을 구하는 메소드에 점수를 추가하고 계산
		for (int i = 0; i<scores.length; i++)	{
			averageScoreCalculator.addScore(scores[i]);
		}
		Double averageScore = averageScoreCalculator.getAverageScore();
		//then 메소드의 결과가 어떤 것과 같다.. (일반적인 계산이랑 같은지 확인)
		assertThat(averageScore).isEqualTo(Arrays.stream(scores).average().getAsDouble());
	}```


 - 평균 점수의 범위 테스트 (AverageScoreCalculator에서 반환되는 점수가 양수 && 100점 이하인지 인지 확인)
```java
@DisplayName("평균 점수 범위 테스트")
	@Test
	void averageScoreRangeTest()	{
		//given
		AverageScoreCalculator averageScoreCalculator = new AverageScoreCalculator();
		int[] scores = {10,20,30,40,50};
		//when 
		for (int i = 0; i<scores.length; i++)	{
			averageScoreCalculator.addScore(scores[i]);
		}
		Double averageScore = averageScoreCalculator.getAverageScore();
		//then
		assertThat(averageScore >= 0 && averageScore <= 100).isTrue();
	}

```

- 개별 점수 유효성 테스트 
	- validateScores 메소드에 대한 부분인듯?
	- 근데 왜 작은 메소드인 validateScores에 안하고 averageScoreCalculator에 할까??
```java
@DisplayName("개별 잘못된 점수 테스트")
	@Test
	public void averageScoreInvalidScoreTest(){
		//given
		AverageScoreCalculator averageScoreCalculator = new AverageScoreCalculator();
		int[] scores = {10,20,30,40,-1};
		//when
		final IllegalStateException exception = assertThrows(IllegalStateException.class, () -> {
			for (int i = 0; i<scores.length; i++)	{
				averageScoreCalculator.addScore(scores[i]);
			}
		});
		//then
		assertThat(exception.getMessage()).isEqualTo("Invalid score");
	}
```

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


- **HTTP 주요 응답 코드 테스트 메서드**

| Code                      | MappingMethod           | Explain                              |
| ------------------------- | ----------------------- | ------------------------------------ |
| 200 OK                    | isOk()                  | HTTP 응답코드가 200 OK인지 검증               |
| 201 Created               | isCreated()             | HTTP 응답코드가 201 Created 검증            |
| 401 Bad Request           | isBadRequest()          | HTTP 응답코드가 400 BadRequest 검증         |
| 403 Forbidden             | isForbidden()           | HTTP 응답코드가 403 Forbidden 검증          |
| 404 Not Found             | isNotFound()            | HTTP 응답코드가 404 Not Found 검증          |
| 4**                       | is4xxClientError()      | HTTP 응답코드가 4** 검증                    |
| 500 Internal Server Error | isInternalServerError() | HTTP 응답코드가 500 InternalServerError검증 |
| 5**                       | is5xxClientError()      | HTTP 응답코드가 5** 검증                    |
- HTTP 응답 코드 
	- **1xx : Information(정보제공) : 클라이언트 요청을 받았으며 작업을 계속 진행중
	- **2xx : Success(성공) : 클라이언트가 요청한  동작 수신 이해했고 승낙하여 성공적으로 처리함
		- 200 Ok : 성공 : 서버가 요청을 성공적으로 처리하였음
		- 201 Created : 생성됨 : 요청이 처리되어 새로운 리소스가 생성됨
		- 202 Accepted : 허용됨 : 요청은 접수하였지만 처리가 완료되지 않음
	- **3xx : Redirection(리다이렉션) : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야함 
		- 301 Moved Permanentyl : 영구 이동 : 지정한 리소스가 새로운 URL로 이동하였음
		- 303 See Other : 다른 위치 보기 : 다른 위치로 요청하라 
		- 307 Temporary Redirect : 임시 리다이렉션 : 임시로 리다이렉션 요청이 필요하다
	- **4xx : Client Error(클라이언트 에러) : 클라이언트 요청에 오류가 있음
		- 400 Bad Request : 잘못된 요청 : 요청의 구문이 잘못되었다
		- 401 Unauthorized : 권한 없음 : 지정한  리소스에 대한 엑세스 권한이 없음
		- 403 Forbidden : 금지됨 : 지정한 리소스에 대한 엑세스가 금지되었음
		- 404 Not Found : 찾을 수 없음 : 지정한 리소스를 찾을 수 없음
	- **5xx : Server Error(서버 에러) : 클라이언트의 요청은 유효한데 서버가 처리에 실패함
	- 500 : Internal Server Error : 내부 서버 에러 : 서버에 에러가 발생함 
	- 501 : Not Implemented : 구현되지 않음 : 요청한 URI의 메소드에 대해 서버가 구현하고 있지 않음
	- 502 : Bad Gateway : 불량 게이트 웨이 : 게이트웨이 또는 프록시 역할을 하는 서버가 그 뒷단의 서버로부터 잘못된 응답을 받았음




post :  db에 저장해줘 (front 입장에서 back에 정보를 보낸다)
Get : front 에서 정보를 받는 놈임