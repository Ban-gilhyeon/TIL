![[Pasted image 20250912001251.png]]

# IP (Internet Protocol)
- 지정한 IP 주소에 데이터의 조각들을 패킷(Packet)이라는 통신 단위로 최대한 빨리 목적지로 보내는 역할
- 패킷들의 순서 보장 X, 누락도 신경 안쓰고 보내는 것에만 집중
![[Pasted image 20250912001955.png]]

# TCP (Transmission Control Protocol)

## 배경
IP 규칙으로만 통신하게 부족하거나 불안전하던 여러 단점(패킷 순서 보장 X , 패킷 유실)을 보완하기 위해 `패킷 전송을 제어하여 신뢰성 보증`하는 프로토콜

## 3way-handshake
- SYN : 접속 요청 : 연결을 생성할 때 `클라이언트`가 `서버`에 시퀀스 번호를 보내는 패킷
- SYN-ACK : 시퀀스 번호를 받은 서버가 ACK 값을 생성하여 클라이언트에게 응답
- ACK : 요청 수락 : 송신측에서 수신측으로 보내는 확인 응답
![[Pasted image 20250912002653.png]]
![[Pasted image 20250912002834.png]]
![[Pasted image 20250912003448.png]]
1. 클라(SYN_SENT) -> 서버(LISTEN) :
	1. SYN(접속 요청) 패킷 전송 
	2. 클라 SYN_SENT 상태로 변경
2. 클라 <- 서버(SYN_RCVD) : 
	1. SYN(양방향 통신을 위한 클라 접속 요청) + ACK(클라의 접속 허가) 패킷 전송
	2. 서버는 (SYN_RCVD : received) 상태로 변경 클라의 ACK 패킷이 올 때까지 대기
3. 클라 -> 서버 : 
	1. 클라 ACK + 데이터 패킷 전송
	2. ESTABLISHED 상태 -> 데이터 통신 가능
4. 데이터 패킷 전송
	1. 서버에게 데이터 전송
	2. 서버는 ACK 전송 
	3. 전송 실패 시 재전송
## 4way-handshake
- PSH : 데이터를 즉시 목적지로 보내라는 뜻
- FIN : 접속 정료를 위한 플래그 (이 패킷을 보내는 곳이 현재 접속하고 있는 곳과 접속 끊자)
![[Pasted image 20250912003701.png]]
1. 클라가 서버와의 연결을 끊기 위해 `close()`메소드 호출 -> FIN 플래스 전송 및 FIN_WAIT 상태 변경
2. 서버는 클라가 `close()`호출 한걸 알고 -> CLOSE_WAIT 상태로 변경, ACK 플래그 전송 (서버 -> 클라 전송 데이터가 남으면 이 때 다 전송)
3. ACK 받은 클라가 FIN_WAIT2로 상태 변경, 서버는 `close()`메서드 호출 FIN 플래그 전송
4. 서버도 연결 닫았다는 신호를 클라가 수신하면 ACK 플래그 전송 후 TIME_WAIT 상태로 변경
5. CLOSE

## 전송 제어 기법

![[Pasted image 20250912001636.png]]
![[Pasted image 20250912002205.png]]


