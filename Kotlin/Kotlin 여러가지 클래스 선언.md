## 일반 클래스 (Class A)
```kotlin
class User(val name: String, var age: Int){
}
```
- Java의 일반 클래스와 거의 동일
- 생성자 파라미터는 자동으로 필드 + 생성자로 변환 
- 상속 가능(open) / 불가능(final, 기본)

## 데이터 클래스 (data class A)
```kotlin
data class User(val name: String, val age: Int){
}

// JVM 변환
public final class User{
	private final String name;
	private final int age;
	
	//equals
	//hashCode
	//toString
	//copy
	
	//component 1
	//component 2  
}
```
자동 생성
- equals()
- hashCode()
- toString()
- copy()
- componentN() (Destructuring)

값 중심 객체(Value Object)에 최적화

<U>JPA 엔티티에 data Class 쓰면 안됨</U>
eqauls / hashCode에서 문제 발생 + lazy loading 호환 문제
