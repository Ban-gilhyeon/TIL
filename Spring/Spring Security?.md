스프링 기반의 애플리케이션의 보안(인증, 권한)을 담당하는 프레임워크
필터 기반 동작이기 때문에 MVC와 분리되어 관리 및 동작

![[Pasted image 20251010140429.png]]

[용어]
- 접근 주체(Principal) : 보호된 대상에 접근하는 유저
- 인증(Authentication) : `증명하다`라는 의미 
- 인가(Authorization) :  `권한부여, 허가`의 의미 

## Security Filter Chain
Sping Security는 다양한 기능을 가진 필터들을 10개 이상 기본적으로 제공 -> Filter Chain이라고 함
![[Pasted image 20251010141045.png]]
