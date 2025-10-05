## URI (Uniform Resource Identifier : 통합 자원 식별자)
- **Unifrom**은 리소스를 식별하는 통일된 방식을 말함
- **Resource**란, `URI`로 식별 가능한 모든 종류의 자원 (웹 브라우저 파일 및 그 외의 리소스 포함)을 지칭함
- **Identifier**는 다른 항목과 구분하기 위해 필요한 정보
- bbilgoo.tistory.com  이런거 
즉, `URI`는 인터넷 상의 리소스 <U>자원 자체</U>를 식별하는 고유한 문자열 시퀀스이다 

## URL(Uniform Resource Loactor)
- 네트워크상에서 통합 자원(리소스)의 **위치**를 나타내기 위한 규약
- 자원 식별자와 위치를 동시에 보여줌 
- 웹 사이트 주소 + 컴퓨터 네트워크 상의 자원
- 특정 웹 페이지의 주소에 접속하기 위해서는 웹 사이트의 주소 뿐만 아니라 프로토콜(https, http, sftp, smp 등)을 함게 알아야 접속 가능, 이를 모두 나타내는 것이 `URL` 
- https://bbgiloo.tistory.com 이런거

## URI와 URL의 차이점 
`URI` = 식별자, `URL` = 식별자 + 위치
1. `URL`은 일종의 `URI`이다
 ![[Pasted image 20240921084727.png]]

2. `URL`은 프로토콜과 결합한 형태
	- 어떻게 위치를 찾고 도달할 수 있는지까지 포함되어야 하기 떄문에 **URL은 포로토콜 + 이름 or 번호**의 형태여야만함 
3. `URI`는 그 자체로 이름이 될 수 있음 

## URI, URL 구조 
![[Pasted image 20240921085428.png]]

- Scheme : 리소스에 접근하는데 사용할 프로토콜, 웹에서는 http 또는 https 사용
- Host : 접근할 대상 (서버)의 호스트 명
- Path : 접근할 대상(서버)의 경로에 대한 상세 정보 