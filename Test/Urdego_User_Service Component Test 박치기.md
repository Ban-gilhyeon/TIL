
# UserCommander
## `signUp()` 신규 회원가입 

- 해당 메서드가 어떠한 역할인지 확인 --> 신규 User 객체 저장 메서드 
- 신규 User 객체 저장 메서드 이기 떄문에 잘 저장하는지 테스트를 진행 하면 됨 
```java
<원형 메서드>
// 신규 회원가입  
public User signUp(UserSignUpRequest userSignUpRequest) {  
    int nicknameNumber = countDuplicatedNickname(userSignUpRequest.nickname());  
    User newUser = User.create(userSignUpRequest,nicknameNumber);  
  
    UserCharacter userCharacter = userCharacterCommander.initActiveCharacter(newUser);  
  
    userRepository.save(newUser);  
    userCharacterRepository.save(userCharacter);  
  
    return newUser;  
}
```

서비스 로직은 다음과 같음 
```
* 1. 중복된 닉네임이 몇개 있는지 체크  
* 2. 유저 생성  
* 3. 캐릭터 초기화 (기본 캐릭터 지급)  
* 4. 상태 저장  
* 5. 저장된 캐릭터 정보 리턴
```
고로 
### given에 해당하는 테스트 준비는 아래와 같음
```java
//given  
UserSignUpRequest request = new UserSignUpRequest("nickanme","email@email.com","KAKAO","1");  
List<User> userList = List.of(mock(User.class), mock(User.class));  
  
//1. 중복된 닉네임이 몇개 있는지 체크  
when(userReader.findByName(request.nickname())).thenReturn(userList);  
int nicknameNumber = userList.size() + 1;  
  
//2. 유저 생성  
User mockUser = mock(User.class);  
  
//3. 캐릭터 초기화 (기본 캐릭터 지급)  
//여기서 분기가 등장 최초 가입 시 or 탈퇴 후 재 로그인 시  
//현재는 최초 가입 시로 가정  
UserCharacter mockUserCharacter = mock(UserCharacter.class);  
when(userCharacterCommander.initActiveCharacter(mockUser)).thenReturn(mockUserCharacter);
```

위에서 기술한 바와 같이 `signUp()`는 신규 `User`객체를 저장하는 메서드 고로 
### when 에 해당하는 것은 
```java
//when  
//4. 상태 저장  
when(userRepository.save(mockUser)).thenReturn(mockUser);  
when(userCharacterRepository.save(mockUserCharacter)).thenReturn(mockUserCharacter);
```

### 오답
**하지만 then에 대한 이해가 떨어졌고 어려워 했다.**
검증과 확인을 따로 생각함 
```
//5. 저장된 캐릭터 정보 리턴  
/* 제대로 저장이 됐는지 확인해야 함  
   5.1 테스트에서 의도한대로 유저 객체가 같은지?   
   5.2 테스트에서 의도한대로 닉네임 넘버링이 들어갔는지?   
   5.3 테스트에서 의도한대로 유저에 기본 캐릭터 정보가 들어갔는지?* */
```
이와 같이 확인 만을 생각했기 때문에 이상한 테스트 결과를 확인 할 수 밖에.. 

### 깨달음
then에서는 위의 플로우대로 호출이 되었는가 `verify` **검증**과 그 결과에 대한 `assert`**확인**이 되어야 함 
1. private 메서드는 어떻게 `verify` 검증하는가? 
위의 원본 서비스 로직을 보면 
`int nicknameNumber = countDuplicatedNickname(userSignUpRequest.nickname());` 에서 `countDuplicatedNickname`은 UserCommander의 private 메서드임
```java
<원형 메서드>
// 회원가입 시 닉네임 넘버링  
private int countDuplicatedNickname(String nickname){  
    List<User> userList = userReader.findByName(nickname);  
    int nicknameNumber = userList.size() + 1;  
    return nicknameNumber;  
}
```
이를 검증하려면 `verify(userCommander).countDuplicatedNickname(request.nickname());` 이런식으로 할 수 없음 private 한 메서드이니까

고로 간접적으로 검증해야 함 
`verify(userReader).findByName(request.nickname());`이와 같이 그러면 주요한 메서드에 대한 검증은 됐지만 
`countDuplicatedNickname()`가 제대로 작동을 했는지 확인하는 과정이 자연스럽게 따라오게 됨 
이는 
`assertEquals(nickname#3, result.getNickname());`로 확인할 수 있음 

![[Pasted image 20250409032744.png]]


