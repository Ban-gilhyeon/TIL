
## ResponseEntity<>
Spring framework에서 제공하는 클래스중 `HttpEntity`라는 클래스가 존재함
### HttpEntity 
요청 또는 응답에 해당하는 HttpHeader와 HttpBody를 포함하는 클래스이다 
```java
pulbic class HtpEntity<T>{
	private final HttpHeaders headers;
	@Nullabe
	private final T body;
}
```
```java
public class RequestEntity<T> extends HttpEntity<T>

public class ResponseEntity<t> extends HttpEntity<T>
```

`HttpEntity`클래스를 상속받아 구현한 클래스가 `RequestEntity`, `ResponseEntity`클래스이다.<br>
- ResponseEntity
	- 사용자의 HttpRequest에 대한 응답 데이터를 포함하는 클래스임
	- `HttpStatus`, `HttpHeaders`, `HttpBody`를 포함 함



## RequestPart
-----------------------
온전한 미디어 파일을 받아오는 어노테이션 
- HTTP request body에 `multipart/form-data`가 포함되어 있는 경우 사용하는 어노테이션
- `MulitpartFile`이 포함되어 있는 경우 `MultipartResolver`가 동작하여 역직렬화를 하게 됨
- 만약 `MulitpartFile` 포함되어 있지 않다면 @RequestBody와 동일하게 동작함 

### 사용법 
```java
@PostMapping(value = "/api/v1/posts/file", consumes = {MediaType.APPLICATION_JSON_VALUE, MediaType.MULTIPART_FORM_DATA_VALUE})
    public ResponseEntity<PostResponseDTO> savePostFile(@RequestPart(value = "file", required = false) MultipartFile multipartFile) throws IOException {
        if (!multipartFile.isEmpty()) {
            log.info("파일 이름 : " + multipartFile.getOriginalFilename());
        } else {
            log.info("파일이 존재하지 않습니다.");
        }
        return ResponseEntity.ok(new PostResponseDTO());
    }
```

- consumes 
	- 하나의 API에서 Json과 MultipartFile을 한번에 전달받을 수 있다
	- 특이점으로는 API에서 consume할 MediaType을 지정해줘야 한다는 점
		- 지정하지 않으면 `415 Unsupported Media Type ERROR` 를 만날 수 있음
	- consume은 해당 엔드포인트에서 수신되는 요청의 미디어 타입을 지정하는데 사용됨 
	- **클라이언트가 요청 본문에 담아 보내는 데이터의 형식을 나타냄**
	- 일반적으로 웹에서 JSON, XML, 폼 데이터 등 다양한 형식의 데이터를 POST 요청으로 보낼 수 있음 
	- consume 옵션을 사용하여 어던 미디어 타입의 요청을 처리할지 명시할 수 있음
	정리하자면 consume 옵션(`consumes = {MediaType.APPLICATION_JSON_VALUE`)을 사용하여 엔드포인트가 특정 미디어 타입을 수용하도록 설정하면, 헤당 미디어 타입의 요청만이 해당 엔드포인트로 라우팅되어 처리 됨

- @RequestPart
	- value : 예를 들어 `file` 멀티파트 요청에서 `file`이라는 이름을 가진 파트를 추출하겠다는 의미 
	- required : false인 경우 MultipartFile이 필수가 아니며, 없어도 됨
	- getOriginalFilename() : File 원본 이름을 추출할 수 있음
	- 요청 받은 MulitpartFile이 존재하는 경우 확인하려면 `isEmpty()`를 활용해 File의 존재 여부를 확인할 수 있음

## yml 
- open-in-view
	- JPA에서 영속성 컨텍스트가 데이터베이스 커넥션을 DB에 언제 돌려줄지 설정
