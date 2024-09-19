
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
