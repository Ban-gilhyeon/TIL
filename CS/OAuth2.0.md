인터넷 사용자들이 비밀번호를 제공하지 않고 `다른 웹사이트`상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는 접근 위임을 위한 개방형 표준

신뢰할 수 있는 외부 어플리케이션(Naver, Kakao, Apple 등)의 Open API의 ID, PW를 가져다 쓰는 것
*뇌피셜* : Other Authorization 2.0이 아닐까... 

## 용어 및 역할
### 용어
Authentication 
- 인증, 접근 자격이 있는지 검증하는 단계

Authorication
- 인가, 자원에 접근할 권한을 부여하는 것
- 인가가 완료되면 리소스 접근 권한이 담근 Access Token이 클라이언트에게 부여됨

Access Token
- 리소스 서버에게서 리소스 소유자의 보호된 자원을 획득할 때 사용되는 만료 기간이 있는 Token

Refresh Token 
- Access Token 만료 시 이를 갱신하기 위한 용도로 사용하는 Token
- 일반적으로 Access Token보다 만료 기간이 긺
### 역할

Client
- Resource Server의 API를 사용하여 정보를 가져오려는 애플리케이션 서버
- Third-Party Application의 자원을 사용하기 위해 접근 요청을하는 Server 또는 Application
- 보통 우리가 개발하려는 서비스가 됨 

Resource Owner
- 어플리케이션을 이용하려는 Resource Server의 계정을 소유하고 있는 사용자
- 일반적으로 권한 서버(Authorization Server)가 Resource Owner와 Client 사이에서 중개 역할을 수행함

Resource Server 
- OAuth2.0 서비스를 제공하고 Resource를 관리하는 서버 (Naver, Kakao, Apple)
- Client 서버로 인증 서버에서 발급받은 Token을 넘겨 개인정보를 받을 수 있음

Authorizaion Server
- Client가 Resource Server의 서비스를 사용할 수 있게 인증하고 토큰을 발행해주는 인증 서버
- 사용자(Resource Owner) 서버로 ID, PW를 넘겨서 Authorization Code 발급
- Resource Server와 Authorization Server는 별개로 구분되어 있지만 아키텍처에 따라 달라질 수도 있음

## 인증 방식 및 동작 과정

### Authorization Code Grant / 권한 부여 코드 승인 방식
- 권한 부여 코드 요청 시 자체 생성한 Authorization Code를 전달하는 방식
`Response_type = code`,
`grant_type = authorization_code` 형식으로 요청 
- OAuth2.0에서 가장 기본이 되는 방식, 간편 로그인 기능에서 사용되는 방식
- Refresh Token 사용 가능
![[Pasted image 20241224110033.png]]
1. 권한 부여 코드 요청 시 `response_type = code`로 요청하게 되면 Client는 Authorization Server에서 제공하는 로그인 페이지를 브라우저에 띄워 출력
2. 해당 페이지를 통해 사용자가 로그인을 하면 Authorization Server는 권한 부여 코드 요청 시 전달 받은 redirect_uri로 Authorization Code를 전달
3. Client는 전달 받은 Authorizatio Code를 Authorization Server의 API를 통해 Access Token으로 교환

*Access Token을 획등하는 과정에서 Authorization Code 발급 과정이 들어간 이유*
- Authorization Code 발급 과정 생략 시 
	- Authorization Server는 Access Token을 전달하기 위한 방법으로 권한 부여 코드 요청 시 전달 받은 Redirect_uri를 사용할 수 밖에 없음
	- uri를 통해 데이터를 전달 받는 방법은 uri자체에 데이터를 실어 전달하는 방법 밖에 없음
	- 이 때 중요한 데이터인 Access Token이 브라우저를 통해 노출
- Authorization Code 발급 과정을 통해 백엔드 사이에어 Access Token이 전달 되기 때문에 토큰 탈취 위험이 줄어듦

### Implicit Grant / 암묵적 승인 방식
- 자격 증명을 안전하게 저장하기 힘든 클라이언트 사이드에서 OAuth2.0 인증에 최적화 된 방식으로 `Response_type = token`의 형식으로 요청 됨
- 권한 부여 코드(Authorization Code) 발급 과정 X 
	- Access Token이 uri를 통해 전달, 만료 기간을 짧게 설정해야 함
- Refresh token X 
- Authorization Server는 Client_secret을 사용해 클라이언트를 인증하지 않음
- Access Token 획득 절차 간소화 
	- 응답성과 효율성 높음
	- 보안적인 측면에서 단점
![[Pasted image 20241224111943.png]]
1. 권한 부여 승신 요청 시 `response_type`을 token으로 설정하여 요청
2. Client는 Authorization Server에서 제공하는 로그인 페이지 출력
3. 해당 페이지를 통해 사용자가 로그인 Authorization Server는 권한 부여 승인 코드 요청 시 전달받은 Redirect_uri로 Access Token 전달

### Resource Owner Password Credentails Grant / 자원 소유자 자격 증명 승인 방식
- userName, password로 Access Token을 받는 방식
- 클라이언트가 타사의 외부 프로그램일 경우에는 이 방식을 적용하면 안됨
- 자신의 서비스에서 제공하는 어플리케이션일 경우 사용
- Refresh Token 사용 가능
![[Pasted image 20241224114929.png]]
### Client Credentails Grant / 클라이언트 자격 증명 방식
- 클라이언트의 자격 증명만으로 Access Token을 획득하는 방식으로 `grant_type = client_credentials`형식으로 요청
- User가 아닌 Client(서비스 또는 애플리케이션)에 대한 인가가 필요할 때 사용되는 방식
- OAuth2.0의 권한 부여 방식 중 가장 간단
- 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있을 경우 사용
- Refresh Token X 
![[Pasted image 20241224115206.png]]