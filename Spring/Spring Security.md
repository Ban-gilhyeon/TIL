- Authentication(인증)
- Authorization(권환)

### 사용법
1) 애플리케이션에 Spring Security jar을 Dependncy에 추가
2) WebSecurityConfigurerAdpater를 상속받는 Security Configuration 클래스 생성
3) Security Configuration 클래스에 @EnableWebSecurity추가
4) Authentication -> configure(AuthenticationManagerBuilder auth)메서드 재정의
5) Password encode를 위한 BCryptPasswordEncoder 빈 정의
6) Authorization -> configure(HttpSecurity http) 메서드 재정의 

## UserDetailsService
ice 
Spring Security에서 사용자의 정보를 담는 인터페이스 


