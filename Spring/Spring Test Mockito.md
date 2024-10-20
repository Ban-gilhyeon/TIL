
## Mockito? Mock?
mockito : 
- 테스트를 편하게 하도록 모의 객체(Mock)를 만드는 Mocking 프레임워크
- Mock 객체를 쉽게 만들고 관리하고 검증할 수 있는 방법을 제공
mock : 
-  진짜 객체와 비슷하지만 물리적으로 같지 않고 프로그래머가 직접 행동을 관리하는 객체
- Mock은 모든 상호작용을 기억한다. 사용자는 Mock의 어떤 메서드가 실행되었는지 선택적으로 검증할 수 있다.
stubbing : 
- 테스트 코드에서 Mock 객체를 사용할 때, Mock의 특정 메서드 호출과 응답을 정의하는 것


## Mockito Annotation

### @Mock
- Mock 객체의 인스턴스 내부는 비어있음 (null)
```java
@Mock // 어노테이션 사용
List<String> mockedList;

//직접선언
List<String> mockedList = mock(List.class);

@Test // 어노테이션을 파라미터로 직접 넣어줄 수도 있음
public void createStudyService(@Mock MemberService memberservice){
...
}
```
- Mock 객체 Stubbing(행동 정의)
	- null을 리턴, `Optional`타입은 `optional.empty` 리턴
	- primirive 타입은 기본 primitive값 (int : 0, boolean : false) 리턴
	- 컬렉션은 비어있는 컬렉션
	- void 메소드는 예외를 던지지 않고 아무런 일도 발생 X 
- Stubbing 하기
	- 특정 파라미터를 입력 받았을 때 지정한 값을 리턴할 수 있다
	- 어떠한 파라미터를 입력 받더라도 지정한 값을 리턴할 수 있다
### @InjectMocks
: 해당 클래스가 필요한 의존성과 맞는 mock 객체들을 감지하여 해당 클래스의 객체가 만들어질 때 사용하여 객체를 만들고 해당 변수에 객체를 주입하게 됨

### @Spy
: 예를 들어 `OrderRepository`의 메소드 중 `createOrder()`는 stub하고 `findOrderlist()`는 실제 기능을 그대로 사용하고 싶은 경우가 생각보다 빈번히 발생함
즉, 하나의 객체를 선택적으로 stub할 수 있도록 하는 기능
- @Spy로 만든 Mock 객체는 **진짜 객체**이며 메소드 실행시 <U>stubbing을 하지 않으면 기존의 객체의 로직을 실행한 값을</U>,  `stubbing을 한 경우 스터빙 값을 리턴함`

### @InjectMcok
: DI를 @Mock이나 @Spy로 생성된 mock 객체를 자동으로 주입해주는 어노테이션

### when( )

```java
Member member = new member();
member.setId(1L);
member.setEamil("test@email.com")

//1L로 검색 시에만 리턴하는 stubbing
when(memberService.findById(1L)).thenReturn(Optional.of(member));

//인자를 any로 입력 시 항상 같은 객체를 리턴 stubbing
when(memberService.findById(any())).thenReturn(Optional.of(member));

Study study = new Study(10,"java");

//stubbing 된 member가 리턴됨
studyService.createNewStudy(1L, study);
```
- Mockito `when()`
	- Mockito의 `when()`은 TestDouble 중 Stub를 만들 수 있는 강력한 무기 
	- `when`을 통해 Mock 객체의 메소드를 호출 했을 때 특정한 응답을 주도록 만들 수 있다. 
	- **`when()`은 특정 조건이 만족 될 때 모의 객체 (Mock Object)의 동작을 정의하는데 사용**
	- Stub : Mock 객체의 특정한 입력 시 특정한 출력을 주도록 만드는 것
- Mockito `thenReturn()`
	- 