
## @DATA 
- @Getter/@Setter
- @ToString
- @EqualsAndHashCode
- @RequiredArgConstructor
를 합쳐 놓은 선물세트

## @RequiredArgsConstructor
- lombok으로 스프링에서 `DI`의 방법 중 생성자 주입을 임의의 코드 없이 자동으로 설정해주는 어노테이션
- 초기화 되지 않은 **final필드나, @NonNull이 붙은 필드에 대해 생성자를 생성**해줌 
- 새로운 필드를 추가할 때 **다시 생성자를 만들어서 관리해야하는 번거로움을 없애줌**
- 클래스가 의존하는 필드를 간단하게 초기화할 수 있음 
```java
@RequiredArgsConstructor
public class Person {
    private final String name;
    private final int age;
    private String address;
    
} // 아래 코드와 같음
public class Person {
    private final String name;
    private final int age;
    private String address;

	public Person(final String name, final int age) {
    	this.name = name;
        this.age = age;
    }
```
각 어노테이션에 세부 옵션을 지정해줄 수 있음 
- staticName : 정적 팩토리 메소드 생성
- access : 접근 제한자를 지정 
- onConstructor : 생성자 접근 제어자를 설정, 기본값 public
- force : final 필드가 선언된 경우 컴파일 타임에 기본값을 `0` / `null` / `false`로 설정 **(필드 초기화)**
### staticName
```java
@AllArgsConstructor(staticName = "of")
public class Person {
    private String name;
    private int age;
    private String address;
}//아래와 같음
@AllArgsConstructor(staticName = "of")
public class Person {
    private String name;
    private int age;
    private String address;
	
    public static Person of(String name, int age, String address) {
    	new Person(name, age, address);
    }
}
```
### force
```java
@NoArgsConstructor(force = true)
public class Person {
    private final String name;
    private final int age;
    private String address;
}//아래와 같음
public class Person {
    private final String name = null;
    private final int age = 0;
    private String address;
```
## @NOArgsConstructor
: 파라미터가 없는 디폴트 생성자를 생성 
```java
@NoArgsConstructor
public class Person {
    private String name;
    private int age;
    // getters and setters
}// 아래의 코드와 같음

public class Person {
    private String name;
    private int age;
    
	public Person(){}
}
```

## @AllArgsConstructor
: 모든 필드 값을 파라미터로 받는 생성자를 생성 
```java
@AllArgsConstructor
public class Person {
    private String name;
    private int age;
    // getters and setters
}//아래의 코드와 같음

public class Person {
    private String name;
    private int age;
	
    public Person(String name, int age) {
    	this.name = name;
        this.age = age;
    }
}
```

## 빌더 패턴
객체를 정의하고 그 객체를 **생성**할 때 보통 생성자를 통해 생성한다 하지만 생성자는 **단점**이 존재하는데 
<U>객체를 별도 builder를 두는 방법이 있고 이를 빌더 패턴이라고 함</U>
쉽게 말해서 객체를 생성하는 패턴이라고 생각함 
```java
Orders order = Orders.Builder()
	.ordersId(UUID.randoms());
	.email("bbgiloo@gmail.com");
	.address("안산시");
	.postCode("1234-12");
	.builder();
```

### 빌더를 써야하는 이유
1. 생성자의 단점인 생성자의 파라미터가 많아지만 가독성이 안좋아짐 
```java
Orders order = new Orders(UUID.randoms(), "bbgiloo98@gmail.com", "안산시", "1234-12");
```

2. 어떤 값을 먼저 설정해도 상관없다
	- 생성자의 경우 정해진 파라미터 순서대로 꼭 값을 넣어줘야 함 -> 에러 발생 확률 증가
```java
public Order(UUID.randoms(), String email, String address, String postCode){
	this.oriderId = UUID.randoms();
	this.emial = email;
	this.address = address;
	this.postCode = postCode;
}
```

### @Builder
위의 빌더 생김새를 만들 수 있게 해줌 
원래 레거시한 코드는 길어짐 
```java
class Example<T> {
   	private T foo;
   	private final String bar;
   	
   	private Example(T foo, String bar) {
   		this.foo = foo;
   		this.bar = bar;
   	}
   	
   	public static <T> ExampleBuilder<T> builder() {
   		return new ExampleBuilder<T>();
   	}
   	
   	public static class ExampleBuilder<T> {
   		private T foo;
   		private String bar;
   		
   		private ExampleBuilder() {}
   		
   		public ExampleBuilder foo(T foo) {
   			this.foo = foo;
   			return this;
   		}
   		
   		public ExampleBuilder bar(String bar) {
   			this.bar = bar;
   			return this;
   		}
   		
   		@java.lang.Override public String toString() {
   			return "ExampleBuilder(foo = " + foo + ", bar = " + bar + ")";
   		}
   		
   		public Example build() {
   			return new Example(foo, bar);
   		}
   	}
   }
```

### @Builder 옵션
- builderMethodName
	- @Builder를 사용하면 생성하는 메소드의 이름은 기본 값인 builder()이다 
	- 이를 새롭게 네이밍 할 수 있는 어노테이션 <br>
	
- buildMethodName
	- builder()로 얻은 빌더에 필드 값들을 입력하고 마지막에 객체를 생성하는 동작인 빌드 메소드의 이름을 네이밍할 수 있는 어노테이션 값
	- 기본 값은 build()로 create()등 필요한대로 네이밍하면 됨 이에 맞춰서 메소드도 같이 리네이밍해주기도 함
```java
Orders order = Orders.builder()
	.builder(); //create()
```

- builderClassName
	- @Builder를 사용하면 각 필드의 값을 세팅하는 메소드의 반환값의 빌더 클래스의 이름이 000Builder로 자동 설정됨 
	```java
	Orders order = Orders.builder()
		.orderId(UUID.randoms()) // return OrdersBuilder
		.email("ban@qweq.qwe") //return OrdersBuilder
		.build();
	
```