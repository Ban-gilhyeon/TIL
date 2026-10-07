
Java의 모든 예외 객체는 `Throwable`을 상속한다.

`Throwable`은 크게 `Error`와 `Exception`으로 나뉜다.

`Exception` 중 `RuntimeException` 계열은 Unchecked Exception이다.

그 외 `Exception` 계열은 Checked Exception이다.

## 상속 구조

```text
Object
└── Throwable
    ├── Error
    │   ├── OutOfMemoryError
    │   └── StackOverflowError
    └── Exception
        ├── RuntimeException       // Unchecked Exception
        │   ├── NullPointerException
        │   ├── IllegalArgumentException
        │   └── IndexOutOfBoundsException
        └── IOException             // Checked Exception
            └── SQLException 등
```

## Error

JVM이나 실행 환경에서 발생하는 심각한 문제다.

애플리케이션이 일반적으로 복구하기 어렵기 때문에 무리하게 `catch`하지 않는다.

- `OutOfMemoryError`: JVM 메모리 부족
- `StackOverflowError`: 호출 스택이 가득 참

## Exception

애플리케이션이 처리할 수 있는 예외다.

외부 자원 문제나 잘못된 입력처럼 복구 가능성이 있는 상황을 표현한다.

### Checked Exception

`RuntimeException`을 상속하지 않는 `Exception` 계열이다.

컴파일러가 `try-catch` 또는 `throws` 선언을 강제한다.

```java
public void readFile() throws IOException {
    Files.readString(Path.of("file.txt"));
}
```

대표적으로 `IOException`, `SQLException`, `ClassNotFoundException`이 있다.

### Unchecked Exception

`RuntimeException`과 그 하위 타입이다.

컴파일러가 처리를 강제하지 않는다.

잘못된 인자, 객체 상태, 프로그래밍 오류를 표현하는 경우가 많다.

- `NullPointerException`
- `IllegalArgumentException`
- `IllegalStateException`
- `IndexOutOfBoundsException`

## Checked와 Unchecked 비교

| 구분 | Checked | Unchecked |
|---|---|---|
| 기준 | `Exception`이지만 `RuntimeException` 아님 | `RuntimeException` 계열 |
| 처리 강제 | 컴파일러가 강제 | 강제하지 않음 |
| 주요 원인 | 파일·DB·외부 시스템 | 잘못된 입력·상태·프로그래밍 오류 |
| 예시 | `IOException`, `SQLException` | `NPE`, `IllegalArgumentException` |

## Kotlin과의 차이

Kotlin은 Java와 달리 Checked Exception 처리를 컴파일러가 강제하지 않는다.

따라서 `try-catch`나 `throws`를 작성할지는 개발자가 결정한다.

실패 가능성을 `T?`, `Result<T>`, 도메인 예외 등으로 표현할 수도 있다.

## 면접 답변

> Java 예외 구조의 최상위에는 `Throwable`이 있습니다.
>
> `Throwable`은 JVM 수준의 심각한 문제인 `Error`와 애플리케이션에서 처리할 수 있는 `Exception`으로 나뉩니다.
>
> `Exception` 중 `RuntimeException` 계열은 Unchecked Exception으로 컴파일러가 처리를 강제하지 않습니다.
>
> 그 외 예외는 Checked Exception으로 `try-catch`나 `throws` 선언이 필요합니다.
>
> Kotlin은 Checked Exception 처리를 강제하지 않는다는 차이가 있습니다.

## 주의점

Checked Exception이라고 무조건 좋은 것은 아니다.

복구 가능한 예외만 적절히 처리해야 한다.

계층마다 의미 없는 `catch`를 반복하지 않아야 한다.

`Error`까지 일반 예외처럼 처리하려고 하면 근본 원인을 숨길 수 있다.
