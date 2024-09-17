# WEB

 - Web의 개념 :
 월드 와이드 웹 (World Wide Web)이란 인터넷에 연결된 사용자들이 서로의 **정보**를 공유할 수 있는 공간 
 - Web의 특징 : 
   - 인터넷 상에서 텍스트, 그림, 소리, 영상 등과 같은 멀티미디어 정보를 `하이퍼 텍스트` 방식으로 연결하여 제공 
   - `하이퍼텍스트(hyper text)`란 문서 내부에 또는 다른 문서로 연결되는 참조를 집어 넣음으로써 웹 상에 존재하는 여러 문서끼리 참조할 수 있는 기술을 의미함 
   - 문서 내부에서 또 다른 문서로 연결되는 참조를 `하이퍼링크(hyper link)`라고 함 

# HTTP

 : 하이퍼텍스트 전송 프로토콜(HyperText Transfer Protocol)
 - Web의 토대
 - 하이퍼텍스트 링크를 사용하여 웹의 페이지를 로드하는데 사용
 - 네트워크 장치 간에 정보를 전송하도록 설계된 **애플리케이션 계층** 프로토콜
 - 네트워크 프로토콜 스택의 다른 계층 위에서 실행됨
 - HTTP를 통한 일반적인 흐름 :
   -  클라이언트 시스템에서 서버에 요청 -> 서버에서 응답 메세지를 보냄
  
## HTTP 특징
1. 웹에서 이루어지는 모든 데이터 교환의 기초이다
2. 클라이언트-서버 프로토콜
   - 클라이언트 요청을 생성하기 위해 연결을 연 다음 **응답을 받을 때까지 기다리는 모델**
   - 각 요청은 클라이언트에 의해 **초기화**
   - 이 둘(클라이언트-서버)은 데이터 스트림이 아닌 개별적인 **메세지 교환**을 통해 통신
   - `클라이언트`에 의해 전송되는 메세지를 `요청(Reqeusts)`이라 함
   - 요청에 대해 `서버`에서 응답으로 전송하는 메세지를 `응답(Reponses)`이라고 함
3. 상태가 없고 세션이 있다
   - **상태를 저장하지 않는다 (Statelss)**
   - 예) `www.sample.com/page1`요청 후 `www.sample.com/page2` 를 요청하는 경우 **이 둘의 요청은 서로 연관성을 가지지 않고 독립적**이다. 즉, `page1`에서 만들어진 데이터는 `page2`를 요청할 때 유지되지 않는다.
   - HTTP `쿠키`를 사용하면 상태를 저장하는 `세션`을 사용할 수 있다. 
4. 확장 가능하다
   - `HTTP 헤더`를 사용하면 HTTP를 확장하기 쉽다. 클라이언트와 서버가 새로운 헤더에 대해 합의한다면, 새로운 기능을 언제든지 추가할 수 있다.

## HTTP Method
  - GET : 리소스를 조회
  - POST : 데이터 추가, 등록
  - PUT : 리소스 대체, 수정/해당 리소스가 없으면 새롭게 생성
  - DELETE : 리소스 삭제
  - PATCH : 리소스 부분 변경(수정)
  - HEAD : `GET`과 비슷, HTTP 메세지의 body부분을 제외하고 조회
  - OPTIONS : 
    - 서버와 브라우저가 통신하기 위한 **통신 옵션**을 확인하기 위함
    - 서버가 어떤 Method, Header, content-type을 제공하는지 알 수 있음
  - CONNECT : 대상 자원으로 식별되는 서버에 대한 연결 요청
  ### GET Method
  - 리소스를 조회하는 메소드 
    - 예) 서버에게 클라이언트가 "이 페이지를 보고싶어" 
    - URL 입력, 링크를 클릭하는 경우도  `GET`요청에 해당
  - `GET`요청은 **멱등성**이라는 개념을 지니고 있음
    - **멱등성** 
    - ![alt text](image-13.png)
  - `GET`요청에서 서버에 데이터를 전달하는 경우, **쿼리스트링**을 통해서 전달 됨
    - **쿼리스트링** : 
      - 네이버에 "프로그래머스" 검색 시 URL에 query=프로그래머스 부분
      - ![alt text](image-14.png)
      - *해당 쿼리스트링은 클라이언트에게 전달하는 데이터의 정보가 무방비 상태로 노출되므로 유의해야 함
  - `GET` 메서드는 **캐싱**을 이용하므로 **조회 속도가 빠름**
  ### Post Method
  - 주로 새로운 리소스를 생성(create)하는데 사용 
  - 성공적으로 create를 완료하면 **201(Created) HTTP 응답** 반환
  - 데이터를 **메세지 바디에 쿼리 피라미터 형식으로 전달**
    - 쿼리 피라미터는 `Key-Value` 형식으로 되어있음
    - `GET`방식과 비교하면, 데이터가 외부로 노출되지 않으므로 보완상 이점을 가짐
    - ![alt text](image-15.png)
  - `POST`로 조회가 가능하긴 하나, **멱등성을 지니지 않음** -> `POST` 메소드를 여러번 수행시 같은 결과값이 안나올 수도 있음
  - **캐싱을 사용하지 않으므로** `GET`방식보다 조회에 있어서 느림
  - 데이터를 전송할 때 `Body`에 담아 전송하므로, **메세지 길이에 제한 없음**
  - 문자열 데이터 뿐만아니라, `RadioButton`같은 객체들의 값도 전송 가능 
  ### PUT Method
  - 리소스를 완전히 **대체하는 개념(덮어쓰기)**
  - 클라이언트가 리소스를 식별할 수 없음
    - 클라이언트가 구체적인 리소스 위치를 아는 상태에서, `URL` 지정
    - 예 ) PUT/post/1 : 1번 게시글 수정 요청 등
    - **부분 수정 불가능** 만약 기존에 **A, B 라는 데이터가 존재**했는데 C라는 데이터를 담아 `PUT` 요청을 보낸다면 **A, B는 모두 삭제되고 C로 대체**
    - **멱등성**을 지님
  ### PATCH Method
  - PUT과 같이 리소스를 수정하는 역할, 하지만 **부분 수정 가능**
  - 기존 데이터 A, B가 존재, B = C로 대체후 `PATCH 요청`하면 **데이터가 A,C로 변경**
  - **멱등성 X**
  ### DELETE Method
  - 리소스를 **제거**하는 역할
  - **멱등성 O**
-------------------------블로그 Web과 HTTP이야기 작성완료---------------------
# spring 기본 개념 
 - spring은 여러 프로젝트로 구성되어 있는 자바 기반의 프로그래밍에 있어서 방대한 기능을 제공하는 Framework임
- spring  FrameWork
- sprint boot 
  - boot는 시스템을 사용 가능한 상태로 만드는 것을 의미 
  - SpringApplication을 통한 손쉬운 실행
  - Auto Configuration (bean 설정 자동화)
  - 쉬운 외부 화경 설정 - Properties, YAML, Commandline 설정

# MVC 구조
 - `Model`-`View`-`Controller`의 약자로 디자인 패턴의 일종
 - 비지니스 처리 로직과 사용자 인터페이스를 구분시켜 `서로 영향 없이 개발 가능`
 - `Model` : 무엇을 할지에 대해 정의 
   - 프로그램이 작업하는 세계관의 요소들을 `개념적`으로 정의한 것
   - 처리되는 데이터, 데이터 베이스, 내부 알고리즘 등 내부 비지니스에 관한 로직의 처리를 수행 --> 사용자에게 보이지 않는 로직 
   - 해당 도메인 세계를 얼마만큼 이해하고 있는지와도 밀접한 연관이 있음
   - 물리적인 요소뿐 아니라 추상적인 요소 또한 해당 작업을 수행하는데 특정 책임과 역할로서 구분 될 수 있다면 최대한 구체적으로 작은 `entitiy`를 유지하면서 `Model`설계하는 것이 중요하다.
 - `View` : 사용자에게 보여지는 영역, jsp등 사용자 인터페이스를 담당 
 - `Controller` : 모델에게 어떻게 할 것인지를 알려주며, `Model`과 `View` 사이를 연결하는 역할 사용자의 입출력을 받아서 데이터를 처리함
  ![alt text](image-10.png)
 - MVC 1 : `JSP`가 등장하며 생긴 **페이지 중심적인 아키텍처**로 `Controller`와 `View`를 모두 `JSP`로 처리 하였다. 
 - 비즈니스 로직이 복잡해지면서 유지보수가 어려움 유지보수성을 높이기 위해 아래 MVC 2 패턴이 고안됨 
  ![alt text](image-11.png)
 - MVC 2 : `Model`, `View`, `Controller`를 **모두 분리하여**  개발 생산성을 높였다 현재 MVC 패턴은 MVC2 패턴을 의미함 
 ### 전반적인 아키텍처 
 ![alt text](image-12.png)
 | Bean Type          | Explamation                                                           |
  |-------------------|----------------------------------------------------------------------|
  | DispatcherServlet | Sptring의 프론트 컨트롤러로 HTTP 요청처리의 전반적인 로직을 처리함   |
  | HandlerMapping    | 요청에 대한 Interceptor 리스트와 Handler를 찾아 반환 |
  | HandlerAdapter    | Handler를 실행 |
  | ViewResolver      | view name을 기반으로 요청에 알맞은 view를 가져온다 |
  | View              | model을 기반으로 화면을 렌더링 함|  

# REST 서버 
 : **Representational State Transfer** `자원`을 이름으로 구분하여 해당 자원의 `상태(정보)`를 주고 받는 모든 것을 의미함  
 **HTTP URL**을 통해 `자원`을 명시하고 **HTTP Method(POST, GET, DELETE, PUT)**를 통해 해당 자원에 대한 `CRUD오퍼레이션`을 적용하는 것
 - `자원`은 해당 소프트웨어가 관리하는 모든 것
   - 예) DB에 있는 데이터, 이미지 등 
   - `상태의 전달`은 데이터가 요청되어지는 시점에서 요청받은 자원의 상태(정보)를 전달하는 것 
     - 예 ) JSON, xml 

## REST의 구성 요소
1. 자원(Resource) : `URL`
   - 모든 자원에는 고유한 ID가 존재하고, 이 자원은 Server에 존재한다
   - 자원을 구별하는 ID는 HTTP URL로 구분하게 된다 
     - ex) /auth/signUp 등
   - Client는 URL을 이용하여 자원을 지정하고 해당 자원에 대한 조작을 Server에 요청 
2. 행위(Verb) : `HTTP Method`
   - Client는 HTTP Method(Post, GET, DELETE, PUT)를 이용하여 지정한 자원에 대한 조작을 **요청한다**
3. 표현(Representation Of Resourse)
   - Client가 Server에 자원에 대한 조작을 요청하면 Server는 이에 대한 적절한 응답(Representation)을 보낸다.
  ![alt text](image-9.png)


 ## REST의 특징
   1. Uniform(유니폼 인터페이스)
      - **Unifrom Interface**는 Client가 지정한 리소스에 대한 조작을 통일되고 `한정적인 인터페이스`로 수행하는 아키텍처 스타일을 의미함
      - `한정적인 인터페이스` : URL은 params를 이용하여 어떤 리소스에 접근할 수 있었다. 
        - 예) https://openapi.naver.com/v1/search/shop.xml?query=프로그래머스 하지만 한정적인 인터페이스를 사용하면 https://openapi.naver.com/v1/search/shop/프로그래머스 이런식으로 간편하게 나타낼 수 있음 
      - 이런 `한정적인 인터페이스`를 사용하면 **URL 길이가 짧아질 뿐만아니라 하나의 URL을 이용하여 많은 Representation을 할 수 있음**
   2. Stateless(무상태성)
      - REST는 작업을 위한 상태정보를 **별도로 저장하고 관리하는 일을 하지 않는다는 것을 의미함**
      - 세션 정보나 쿠키 정보를 별도로 저장하고 있지 않기 때문에 서버는 들어오는 요청을 **단순히 처리하기만 하면 됨** 개발자들의 편의성 개선 ~~백엔드 개발자들이 프론트들한테 살짝 삐짐..ㅋ~~ 근데 그래서 로그인 구현할 때 토큰을 사용하는 것 같음
      - **서비스의 자유도 상승, 서버에 불필요한 정보를 관리하지 않아도 됨 => 구현이 단순해짐**
   3. Cacheable(캐시 가능)
      - REST는 웹 표준인 `HTTP`를 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 사용할 수 있음 
      - `HTTP`가 가진 `캐싱기능`을 적용할 수 있음
      - HTTP 프로토콜 표준에서 사용하는 `Last-Modified` 태그나 `E-Tag`등을 이용하면 쉽게 구현이 가능하다고 함
      - **HTTP 캐시** 
        - 웹 사이트나 앱 Client가 이용하는 서비스 과정에서 이미지, HTML, 파일과 같이 **재사용** 할 수 있는 HTTP 자원들을 **임시로 보관하는 저장공간** 재사용을 위해 
        - 예시 
        - 캐시 X 
          - Client가 웹 브라우저에서 매번 똑같은 100MB 영상를 웹 서버로 받아온다 
          - 매번 동영상이 다운로드 완료까지 긴 대기시간이 생기며 서버 응답의 네트워크 **트래픽 및 비용 발생**
        - 캐시 O 
          - 첫번째 요청에 100MB 영상을 캐시에 **임시 저장** 한다 Client가 두번째 요청을 한다면 캐시에서 영상을 반환
          - 두번째 요청부터 영상 다운도르 없이 바로 시청 가능, 서버에 요청 X 네트워크 **트래픽 및 비용 감소**
          - 약간 리소스계의 세션
   4. Self-Descriptivenss(자체 표현구조)
      - **REST API** 만 보고도 이를 쉽게 이해할 수 있음 ~~유니폼 인터페이스가 더 맞는 말이 아닌가..~~
      - https://naver.com/web/backend/devcourse 이것만 보고 devcourse라는 것이 어떤 과정인지 알 수 있는 것 처럼.. 
   5. Client - Server 구조
      - API 제공, Client는 사용자 인증이나 컨텍스트(세션, 로그인정보)등을 직접 관리하는 구조로 되어 있어 **Server - Client 간 역할이 분명하게 되기 때문에 각 필드에서 개발해야할 점이 명확해지고 `의존성`이 줄어들게 됨**
   6. 계층형 구조
      - REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층 등을 추가해 **구조상의 유연성**을 둘 수 있고 Proxy, 게이트웨이와 같은 **네트워크 기반의 중간 매체를 사용할 수 있음**

# Domain Driven Design
- Entity
  : 엔터티는 다른 엔터티와 구별할 수 있는 식별자를 가지고 있고 시간에 흐름에 따라 지속적으로 변경이 되는 객체
  ![alt text](image.png)
- value Object
  : 값 객체는 각 속성이 개별적으로 변하지 않고 값 그 자체로 고유한 불변 객체 
  ![alt text](image-1.png)  
  => 대체로 엔터티들이 벨류 오브젝트들을 가지고 있음.
- Domain
: 사용자가 어플리케이션을 사용하는 대상 영역 -> 비지니스 그 자체 ex ) 주문관리 어플이라면 주문이 도메인 

- dependency 
  : 어떤 객체가 협력하기 위해 다른 객체를 필요로 할 때 두 객체 사이의 의존성이 존재하게 된다. 의존성은 `실행 시점`과 `구현 시점`에 서로 다른 의미를 가진다.
  - 컴파일타임 의존성 (코드를 작성하는 시점 = `코드로 조지냐`): 
    - 코드를 작성하는 시점에서 발생하는 의존성. `클래스 사이의 의존성` 
  - 런타임 의존성 (코드를 실행 run 하는 시점 = `run할 때 객체 생성해서 조지냐`):
    - 애플리케이션이 실행되는 시점의 의존성. `객체 사이의 의존성` 
- 결합도 :
  하나의 객체가 변경이 일어날 때 `관계`를 맺고 있는 다른 객체에게 `변화를 요구하는 정도` 어떤 두 요소 사이에 존재하는 의존성이 바람직할 때 두 요소가 `느슨한 결합`도 또는 `약한 결합도`를 가진다고 말한다. 반대로 두 요소의 의존성이 바람직하지 못할 때 `단단한 결합도` 또는 `강한 결합도`를 가진다고 한다.  
  의존성이 바람직할 때 => `느슨한`, `약한` 결합도  
  의존성이 바람직 하지 않을 때 => `단단한`, `강한` 결합도
- Aggregate 
  - entity들의 집합 
  - Aggregate root : 하나의 entity
![alt text](image-6.png)

# IoC (Inversion of Controll : 제어의 역전)
- 처음에는 엔터티가 사용할 클래스를 결정하고 해당 클래스의 객체를 생성함 즉, `모든 종류의 작업을 사용하는 쪽에서 제어`  
- `제어의 역전이란` 객체가 자신이 사용할 개체를 스스로 선택하지않고 스스로 생성도하지 않는다.  
- `라이브러리`를 사용하는 애플리케이션 코드는 `애플리케이션 흐름`을 직접 제어
- `프레임워크`는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용됨 -> 프레임워크가 흐름을 주도하면서 개발자가 만든 애플리케이션 코드를 사용함
- 정리 : 애플리케이션 코드가 프레임워크가 짜놓은 `틀`에서 수동적으로 동작함 이를 `Hollywood Principle`이라고 함
- ![alt text](image.png)

# Application Context
 - 일종의 IoC Container
- `IoC 컨테이너`는 객체에 대한 생성과 조합이 가능하게 하는 프레임워크이다. 
- ![alt text](image-1.png)
- Spring에서는 IoC 컨테이너를 ApplicationContext 인터페이스로 제공함 
- ![alt text](image-2.png)
- `Bean` 스프링(IoC)이 관리하는 객체 `@Bean`으로 관리
- 스프링의 ApplicationContext는 실제 만들어야할 빈 정보를 `Configuration Metadata(설정 메타데이터)`를 이용해서 IoC 컨테이너에 의해 관리되는 객체들을 생성하고 구성한다. -> 에플리케이션에서 객체들의 도면이라고 볼 수 있음
- ![alt text](image-3.png)

# DI(Dependency Injection)
- IoC는 다양한 방법으로 만들 수 있다.
  - 전략 패턴
  - 서비스 로케이터 패턴
  - 팩토리 패턴
  - 의존관계 주입 패턴
- 지금까지 Order가 어떤 Voucher객체를 생성할지 OrderService가 어떤 OrderRepository객체를 생성할지 `스스로 결정하지 않고 생성자`를 통해서 객체를 주입 받는 패턴을 `생성자 주입 패턴(DI)`라고 함
```java
  public class OrderService {
    private final VoucherService voucherService;
    private final OrderRepository orderRepository;

    public OrderService(VoucherService voucherService, OrderRepository orderRepository) { // 생성자 주입
        this.voucherService = voucherService;
        this.orderRepository = orderRepository;
    }
``` 
- 그 외에 스프링은 세터 기반의 의존관계 주입도 지원함 
## 순환 참조 문제
  : `A 클래스`가 `B 클래스`의 Bean을 주입 받고 `B 클래스`가 `A 클래스`의 Bean을 주입 받는 상황처럼 서로 순환되어 참조할 경우 발생하는 문제를 의미함 
  -> `BeanCurrentlyCreationException` 예외 발생 
  ```java
class A{
    private final B b;
    public A(B b){
        this.b=b;
    }
}

class B{
    private final A a;

    B(A a){
        this.a=a;
    }
}
@Configuration
class CircularConfig {
    @Bean
    public A a (B b){
        return new A(b);
    }

    @Beam
    B b (A a){
        return new B(a);
    }
}

  ```
  ## 컴포넌트 스캔으로 빈 등록하기 
- 컴포넌트 스캔은 스프링이 직접 클래스를 검색해서 빈을 등록해주는 기능 
- 설정 클래스에 빈으로 직접 등록하지 않아도 원하는 클래스를 빈으로 등록할 수 있음
- @Sterrotype 을 이용하면 스캔 대상을 지정할 수 있음 
  
  ### 스테레오 타입 
  - 스테레오타입을 번역하면 `고정관념` 임 
  - UML에서 온 용어 그림에서 보는 것 처럼 UML다이어 그램을 확장 시켜주는 도구로서 특정 요소를 상황이나 도메인에 맞게 분류해줌
![alt text](image-7.png)
![alt text](image-8.png)



# Model (객체)
 ## Model 이란 : 
  - 스프링 프레임워크에서 Model은 MVC아키텍처에서 View와 Controller 간의 `데이터 전달`을 담당하는 `객체`이다. 
  - Model 객체는 비지니스 로직의 결과를 담아서 View에 전달하거나, 사용자 입력을 받아서 Controller에 전달하는 역할을 함
  - Model은 일반적으로 key-value 쌍의 컨테이너로 사용됨 
  - Controller 에서 데이터를 Model에 저장하고, 이를 View에 전달하여 `동적`으로 생성되는 웹페이지를 생성함
  - Model을 사용하여 데이터를 전달하는 가장 일반적인 방법은 View에서 해당 데이터를 표시하거나 사용하는 것임 
 ## 사용법 :
   : 스프링에서 Model 객체를 생성하고 사용하는 방법은 여러가지가 있음  
  

- 일반적으로 SPring MVC에서는 Controller의 메소드 매개변수에 Model인스턴스를 선언하여 사용 
- 스프링은 이 인스턴스를 자동으로 생성하고 `Controller 메소드 실행전에` 전달
```java 
  @Controller
  public class MyController{
    
    @GetMapping("/hello")
    public String hello(Model model){
      String msg = "hello"
      model.addAttribute("mgs",message);
      //key-value 쌍으로 저장 key : "msg" value : "hello"
      return "helloPage";
      // 뷰 템플릿 반환
    }
  }
```
- "helloPage" 라는 뷰 템플릿을 반환하는데 이 뷰 템플렛에서는 Model에 저장된 데이터를 사용하여 동적으로 페이지를 생성하거나 표시할 수 있음 

```html
<!DOCTYPE html>
<html>
  <head>
    <title>helloPage</title>
  </head>

  <body>
    <h1>welcome</h1>
    <p>${msg}</p>
  </body>
</html>
```
- 뷰 템플릿에서는 `${msg}` 표현식을 사용하여 Model에 저장된 데이터를 사용할 수 있음
- 위의 예시에서 "hello"라는 메세지가 웹 페이지에 표시됨
   
# ModelAndView
- ModelAndView는 값을 `뷰`로 전달하는 인터페이스이다
- 스프링 MVC에 필요한 모든 정보를 한번에 반환할 수 있다
- 객체를 생성하고, 이 객체에 `데이터와 뷰 이름을 설정`한 후 컨트롤러에서 반환하면 스프링 MVC는 이 ModelAndView 객체를 사용하여 뷰를 렌더링 함
- Model과 View를 한번에 처리할 수 있음 (펀리)  
### 사용법 
```java
  @GetMapping("/list")
    public ModelAndView list(@RequestParam(name = "page", defaultValue = "1")int page) throws SQLException {
        ModelAndView mav = new ModelAndView("list"); // /WEB-INF/views/list.jsp -> view 위치
        System.out.println(boardService.getBoards(page));
        mav.addObject("pageData", boardService.getBoards(page)); // pageData라는 메세지에 boardService.getBoards(page)라는 데이터를 넣어서 보내자
        return mav;
    }
```
## Model과 ModelAndView 차이점 

# Annotation
: `Annotation(@)`은 사전적 의미로는 **주석**이라는 뜻
자바에서 Annotation은 코드 사이에 주석처럼 쓰이며 **특별한 의미, 기능을 수행하도록 하는 기술** 즉, 프로그램에게 <U>추가적인 정보를 제공해주는 `메타 데이터`</U>라고 볼 수 있음  
 `meta data` : 데이터를 위한 데이터..

 ## - Annotation의 용도
 1. 컴파일러에게 코드 작성 **문법 에러를 체크**하도록 정보를 제공 (한번도 안해봄)
 2. 빌드나 배치시 **코드를 자동으로 생성**할 수 있도록 정보를 제공 (주로 사용하는 그것들)
 3. **실행 시(Runtime) 특정 기능을 실행**하도록 정보 제공 (아마 테스트할 때)
## - Annotation 사용 순서
- Annotation 정의 
- 클래으세 Annotation 배치
- 코드가 실행되는 중에 `Reflection`을 이용하여 추가 정보 획득 -> 기능 실시 

## - Reflection이란
- 프로그램이 실행 중에 **자신의 구조와 동작을 검사**하고 `조사`, `수정`하는 것
- 데이터를 보여주고 다른 포맷의 데이터를 처리하고, 통신을 위해 `serialization(직렬화)`를 수행
- `bunding`을 하기 위해 일반 소프트웨어 라이브러리를 만들도록 도와준다.
- **Reflection을 사용하면** 컴파일 타임에 `인터페이스`, `필드`, `메소드의 이름`을 **알지 못해도 실행 중**에 `클래스`, `인터페이스`, `필드` 및 `메소드`에 <U>접근할 수 있다. 또한 새로운 객체의 인스턴스화 및 메소드 호출을 허용</U>
- 멤버 접근 가능성 규칙을 무시할 수도 있음.. 

## Annotation 종류

### @ComponentScan 
Contextdp bean등록 해주는 Annotation 
- @Component(클래스 레벨)
  - `ApplicationContext.xml`에 `<bean id=" class="클래스명"/>` 과 같이 xml에 bean을 직접 등록하는 방법도 있음  
  - 개발자가 직접 작성한 Class를 Bean으로 등록하기 위한 Annotation
  - Component에 대한 추가 정보가 없다면 Class의 이름을 camelCase로 변경한 것이 Bean Id로 사용 됨
  - @Bean과 다르게 @Component는 name이 아닌 `value`를 이용해 Bean의 이름을 지정
  
```java
    @Component // class 이름애 대한 추가 정보 X
    public class Student{
      public Student(){sout("hi");}
    }

    @Component(value = "mystudent") // 추가 정보 O
    public class Student{
      public Student(){sout("hi");}
    }
```

- @Service(클래스 레벨)
- @Repository(클래스 레벨)
- @Controller(클래스 레벨)
- @RestController(클래스 레벨)
  - Spring에서 Controller중 View로 응답하지 않는 Controller를 의미 
  - 메소드 반환 결과를 JSON 형태로 반환
  - **HttpResponse로 바로 응답이 가능**
  - **@ResponseBody 역할을 자동적으로 해주는 Annotation**
  - @Controller와 @RestController의 차이
    - @Controller 
      - API와 view를 동시에 사용하는 경우에 사용
      - 대신 API서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환
      - **view화면 return이 주 목적**
    - @RestController
      - view가 필요없는 API만 지원하는 서비스에서 사용
      - Spring 4.0.1 부터 제공
      - **@RequestMapping 메소드가 기본적으로 @ResponseBody의미를 가정한다?**
      - **data(json, xml등) return이 주 목적**
- @Configuration(클레스 레벨)
  - @Configuration을 클래스에 적용하고 @Bean을 해당 Class의 method에 적용하면 @Autowired로 Bean을 부를 수 있음 
  - 1개 이상의 bean을 등록할때 설정 
  
- @EnableAutoConfiguration 
  - `Spring Application Context`를 만들 때 `자동으로 설정`하는 기능을 켠다 
  - `classpath`의 내용에 `기반`해서 자동으로 생성 
  - tomcat-embed-core.jar가 존재하면 톰캣 서버가 setting 됨
  
- @Bean(메소드 레벨)
  - **개발자가 직접 제어가 불가능한** 외부 라이브러리등을 Bean으로 만들 때 사용하는 Annotation
  - <U>1개의 외부 라이브러리로 부터 생성한 객체를 등록 시 사용</U> (new로 객체를 생성 후 직접 bean으로 등록할 때 사용)
  - ArrayList같은 라이브러리등을 Bean으로 등록하기 위해서는 별도로 **해당 라이브러리 객체를 반환하는 Method를 만들고 @Bean Annotation을 사용**
  - @Bean에 아무런 값을 지정하지 않으면 Method이름을 camelCase로 변경한 것이 Bean Id로 등록
```java 
  @Configuration
  public clss ApplicationConfig{
    @Bean
    public ArrayList<String> arr(){
      return new ArrayList<String>();
    }
  }

  @Configuration
  public clss ApplicationConfig{
    @Bean(name = "myList")
    public ArrayList<String> arr(){
      return new ArrayList<String>();
    }
  }
```

- @Autowired
  - @Component를 통해 등록된 빈은 다른 곳에서 @Autowired를 사용해 의존성을 주입할 수 있음
  - `속성(field)`, `setter`, `constructor(생성자)`에서 사용되며 Type에 따라 알아서 Bean을 주입 **무조건적인 객체에 대한 의존성을 주입** 시킨다
    - constructor(생성자) 주입을 사용하는 이유
      1. 필수적인 의존성의 주입 시점을 보장
      2. 필드를 final로 선언이 가능하여 외부에서의 변경 가능성 낮춤
      3. 의존성 주입이 한 곳 (생성자)에서만 일어나므로 필요한 의존성을 한 곳에 모아서 확인할 수 있음
      4. 순수 자바 코드로의 테스트가 용이
      5. Bean의 순환 참조 문제를 방지할 수 있음
  - 스프링이 자동적으로 값을 할당 Controller 클래스에서 DAO나 Service에 관한 객체 들을 주입 시킬 때 많이 사용 
  - `Dependency Injection`을 위한 곳에 사용된다
  - `type`을 먼저 확인한 후 못찾으면 `name`에 따라 주입 : `name`으로 강제하는 방법 : @Qualifier을 같이 명시 
  - ```java
    @Component
    public class Sample{
      String name;
      @Autowired // 필드 주입
      private final Class2 field;

      @Autowired // 생성자 주입
      public Sample(){}

      @Autowired // 수정자(setter) 주입, 메소드 주입
      public void setName(String name){
        this.name = name;
      }
    }  
    ```
  - @Primart 또는 @Qualifier를 통해 주입될 구현체 명시하기 
    - 인터페이스를 통해 의존성 주입을 수행하는 경우 빈으로 등록된 구현체가 여러 개라면 선택이 불가능해 문제가 발생함 
    - ```java
      @Repository
      @Primary //@Primary를 주입될 클래으세 붙여 우선적으로 주입됨을 명시 
      public class PrimaryRepository implements Repository {
          . . .
      }

      @Repository
      public class SecondaryRepository implements Repository {
          . . .
      }

      @Service
      public class Service {
          
          @Autowired
          private Repository repository // 어떤 구현체를 주입할 지 알 수 없다.

          @Autowired
          @Qulifier("PrimaryRepository")
          private Repository repository // @Qualifie를 주입 받는 클래스에 붙여 구체 클래스를 명시 
      }
  [참조] https://sddev.tistory.com/225

- @Resource 
  - `@Autowired`와 마찬가지로 Bean 객체를 주입하는데 
  - <U> 차이점은 Autowired는 타입</U>으로 **Resource는 이름으로 연결해준다.**
  - 어노테이션 사용으로 인해 특정 Framework에 종속적인 어플리케이션을 구성하지 않기 위해서는 `@Resource`를 사용할 것을 권장한다
  - @Autowired + @Qulifier의 개념 
  - **@Resource를 사용하기 위해서는 class path내에 `jsr250-api.jar` 파일을 추가해야 함**
  - 필드, 입력 파라미터가 한 개인 bean property setter method에 적용 가능

- @Required 
  - setter method에 적용해주면 Bean 생성시 필수 프로퍼티 임을 알린다
  - @Required을 사용하여 optional하지 않은 꼭 필요한 속성들을 정의한다
  - 영향 받는 bean property를 구성할 시에는 xml 설정 파일에 반드시 property를 채워야 함 (엄격한 체크, BeanInitializtion Exception 발생)

- @Lazy
  - **`지연로딩`**을 지원함
  - `@Component`나 `@Bean`과 같이 사용 
  - class가 로드될 때  스프링에서 바로 bean등록을 마치는 것이 아니라 **실제로 사용될 때 로딩이 이뤄지게 하는 방법**
- @Value
  - `properties`에서 값을 가져와 적용할 때 사용
  - Spring Boot 사용시 `application.properties`에 kay-value 값으로 spring. 가 prefix로 시작하는 설정들을 사용할 수 있음
  ```java
  @Value("${value.from.file}")
  ```
  - `private Strting valueFromFile` 이라고 구성되어 있으면 <U>value.from.file의 앖을 가져와서 해당 변수에 주입</U>
- @RequestMapping
  - 요청 URL을 어떤 method가 처리할지 mapping해주는 역할
  - Controller나 Controller의 method에 적용 
  - 요청 받는 형식인 `GET`, `POST`, `PATCH`, `PUT`, `DELETE`을 정의함 
  - 요청 받는 형식을 정의하지 않는다면 **자동으로 GET으로 설정됨**
  ```java
    @RequestMapping("/list")
    @RequestMapping("list/detail")
    @RequestMapping("login", method=RequestMethod.GET)
  ```
  - <U>`@RequestMapping`에 대한 모든 매핑 정보는 Spring에서 제공하는 `HandlerMappingClass`가 가지고 있음 </U>
- @CookieValue
  - **`쿠키` 값을 parameter로 전달 받을 때 사용**
  - 해당 쿠키가 존재하지 않으면 `500에러` 발생
  - 속성으로 `required`가 있는데 **default는 true임**
  - **false 적용 시 해당 쿠키 값이 없을 때 null로 밥ㄷ고 에러는 X**
  ```Java
  //쿠키의 key가 auth에 해당하는 값을 가져옴
  public String view(@CookieValue(value="auth")String auth){...}
  ```
- @CrossOrigin
  - `CORS` 보안상의 문제로 브라우저에서 리소스를 현재 `origin`에서 다른곳으로 `AJAX` 요청을 방지
  - <U>`@RequestMapping`이 있는 곳이에 사용 시 해당 요청은 타 도메인에서 온 `AJAX`요청을 처리해 줌</U>
  ```java
  //기본 도메인이 http://bbgiloo.tistory.com인 곳에서 온 ajax요청만 받아줄거야
  @CrossOrigin(origins ="http://bbgiloo.tistory.com", maxAge=3600)
  ```

- @ModelAttribute
  - `view`에서 전달해주는 parameter를 Class(DTO)의 멤버 변수로 `binding`해주는 역할
  - `binding` 기준은 `<input name="id"/>` 처럼 어떤 **태그의 name 값이 해당 Class 멤버 변수명과 일치해야 하고 setmethod명도 일치해야 함**
  ```java
  class person{
    String id;

    public void setId(...){...}
    public String getId(){return ...}
  }

  @Controller
  @RequestMapping("/person/*")
  public class PersonController{
    @RequestMapping(value="/info",method=RequestMethod.GET)
    // view에서 MyMEM 으로 던져준 데이터에 담긴 id 변수를 Person 타입의 person이라는 객체명으로 바인딩
    public void show(@ModelAttribute("MyMEM") Person person, Model model){
      model.addAttribute(service.read(person.getId()));
    }
  }
  ```
- @Transactional
  - 스프링에서 트랜잭션 처리를 위해 **선언적 트랜잭션**을 사용
  - **선언적 트랜잭션**
    - 설정파일 or 어노테이션 방식으로 간편하게 트랜잭션에 관한 행위를 정의하는 것


# lombok
: 생성자, getter, setter 자동으로 생성해주는 녀석인듯?

- @RequiredArgsConstructor 
  - `final`이 붙은 필드만 생성자를 자동으로 생성해주는 어노테이션


# Servlet
- 자바를 이용하여 **웹 애플리케이션을 개발**하기 위한 기술로, 웹 서버와 상호작용하여 **동적인 웹 페이지를 생성하고 처리**하는 자바 `클래스` 
-  서블릿은 웹 서버에서 실행되며, 클라이언트의 요청에 따라 동적인 컨텐츠를 생성하여 응답으로 제공하는 역할 수행 
  
## HTTPServletRequest
- java Servlet에서 HTTP **요청 정보를 담고 있는 객체**
- 클라이언트가 서버로 HTTP 요청을 보낼 때, 이 `HTTPServletRequest`객체를 통해 요청의 다양한 정보를 Servlet에 전달 
### HTTPServletRequest 메소드 정리

## HTTPServletResponse
- java Servlet에서 HTTP **응답을 다루는 객체**
- 클라이언트에게 서버로부터 전달되는 **HTTP 응답을 생성하고 제공**하는데 사용
- Servlet은 클라이언트의 요청을 받아 처리한 후 `HTTPServletResponse` 객체를 사용하여 적절한 **응답을 생성하고 클라이언트에 반환**


# Session
: 사용자가 웹 브라우저를 통해 `서버에 접속한 시점`으로 부터 웹 브라우저를 `종료하여 연결을 끝내는 시점`까지 같은 사용자로부터 오는 `일련의 요청을 하나의 상태`로 보고 그 상태를 `일정하게 유지`하는 기술

### Session의 특징 
- 브라우저마다 개별 저장소(Session객체)를 `서버에서 제공(발급)`한다.
- 각 클라이언트에게 `고유한 ID`를 부여한다.
- 웹 서버에 컨테이너 상태를 `유지하기 위한 정보`를 저장
- 세션 ID로 `클라이언트를 구분`하여 클라이언트 `요구에 맞는 서비스`를 제공
- 사용했던 정보들을 `서버에 저장`하기 때문에 `쿠키보다 보안성 우수`
- 서버에 저장되기 때문에 `서버 부하 발생`
- Http 프로토콜은 비접속형 프로토콜이라서 `매 접속마다 새로운 네트워크 연결`이 이뤄지는데 `세션이 연결 유지를 가능`하게 만듬

### 세션 동작 순서
1. 클라이언트가 페이지(resource)를 요청(request)
2. 서버는 해당 클라이언트의 `RequsetHeader`에서 `Cookie`를 확인하여 클라이언트가 해당 SessionID를 보냈는지 확인
3. 만약 SessionID가 존재하지 않으면 서버는 SessionID를 생성하여 클라이언트에게 전달
4. 서버에서 클라이언트에게 전달한 SessionID를 `쿠키`를 사용하여 서버에 저장(JSessionID)
5. 클라이언트 재 접속시 쿠키세션(JSessionID)를 이용하여 sessionID값을 서버에게 전달 (3번 작업 실행 )

### Session 및 관련 메소드 
![text](<세션 관련 메소드.png>)

# 쿠키 

# 토큰 

# JPA

# 지연로딩과 즉시 로딩 