어떠한 Object에 있는 필드 값들을 원하는 Object에 자동으로 Mapping 시켜주는 라이브러리 
즉, UserDto -> UserEntity로 자동 매핑해준다고 생각하면 됨

### ModelMapper 의존성 추가 
- Maven
```xml
<dependency>
  <groupId>org.modelmapper</groupId>
  <artifactId>modelmapper</artifactId>
  <version>3.0.0</version>
</dependency>
```
- gradle
```gradle
dependencies{
	...
	implementation 'org.modelmapper:modelmapper:3.0.0'
    ...
}
```

### 사용법
```java
ModelMapper.map(Object source, Class<D> destinationType)
```
이를 이용해 아래와 같이 간단하게 사용하여 DTO를 순식간에 Entity로 바꿔 줄 수 있다...! 
```java
public UserDto createUSer(UserDto userDto) {  
    userDto.setUserId(UUID.randomUUID().toString());  
    ModelMapper mapper = new ModelMapper();  
	//STRICT는 필드 값이 정확히 일치해야만 매핑을 해주는 방식
	mapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);
      
    UserEntity userEntity = mapper.map(userDto, UserEntity.class);  
    userEntity.setEncryptedPwd("encrypted_Pwd");  
    return null;  
}
```
