클라이언트에서 요청한 데이터를 서버에 전달할 때 RSA 암호화 방식 사용
서버에서 DB에 데이터를 저장할 때는 AES (개인정보)
비밀번호를 DB에 저장할 때는 SHA 방식을 사용 

RSA, AES 방식은 양방향 (복호화가 가능)
SHA 단방향 (복호화 불가능)
![[Pasted image 20251010114231.png]]


## RSA (Ron Rivest, adi Shamir and leonard Adleman)
- 비대칭키 
- 소인수분해
- lcm(a,b) : a와 b에 대한 최소 공배수
- gcd(a,b) : a와 b에 대한 최대 공약수 
![[Pasted image 20251010113934.png]]
### Flow 
1. key Generation
2. Key Distribution
3. Encryption
4. Decryption


### AES
- 복호화 가능
- 대칭키