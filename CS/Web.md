# WEB  
 - Web의 개념 :
 월드 와이드 웹 (World Wide Web)이란 인터넷에 연결된 사용자들이 서로의 **정보**를 공유할 수 있는 공간 
 - Web의 특징 : 
   - 인터넷 상에서 텍스트, 그림, 소리, 영상 등과 같은 멀티미디어 정보를 `하이퍼 텍스트` 방식으로 연결하여 제공 
   - `하이퍼텍스트(hyper text)`란 문서 내부에 또는 다른 문서로 연결되는 참조를 집어 넣음으로써 웹 상에 존재하는 여러 문서끼리 참조할 수 있는 기술을 의미함 
   - 문서 내부에서 또 다른 문서로 연결되는 참조를 `하이퍼링크(hyper link)`라고 함 
 ## HTTP
 : 하이퍼텍스트 전송 프로토콜(HyperText Transfer Protocol)
 - Web의 토대
 - 하이퍼텍스트 링크를 사용하여 웹의 페이지를 로드하는데 사용
 - 네트워크 장치 간에 정보를 전송하도록 설계된 **애플리케이션 계층** 프로토콜
 - 네트워크 프로토콜 스택의 다른 계층 위에서 실행됨
 - HTTP를 통한 일반적인 흐름 :
   -  클라이언트 시스템에서 서버에 요청 -> 서버에서 응답 메세지를 보냄
 ### HTTP 요청
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


