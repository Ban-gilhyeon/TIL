기존의 방식의 개발은 개발자가 클래스 객체를 생성하고 그 객체들을 호출하며 개발을 진행해 왔음.
그러다 보니 반복되는 코드가 생기고 그로 인해 코드가 불필요하게 길어지고 개발자의 피로도 또한 높아 지고 있었음.  (HttpServlet 클래스를 활용하여 개발자가 직접 해당하는 화면이나 파라미터들을 매핑) 
이를 `Spring framework` 가 도와주는 역할을 하게 됨
## Spring framework 
: 모든 종류의 배포 플렛폼에서 최신 Java 기반 `엔터프라이즈` 에플리케이션을 위한 포괄적인 프로그래밍 및 구성 모델을 제공 함 
	- `엔터프라이즈` : 기업을 대상으로 개발하는, 대규모 데이터 처리와 동시에 여러 사용자로 부터 행해지는 매우 큰 규모의 환경을 `엔터프라이즈`환경이라고 함 

### Spring boot 
- boots는 시스템을 사용 가능한 상태로 만드는 것을 의미 ( 컴퓨터 부팅할 때 그 boot)
- Spring Application을 통한 손 쉬운 실행 
- Auto Configuration (Bean 설정 자동화)
- 쉬운 외부 환경 설정
	- Properties, YAML, Commandline 설정 

## Spring Framework의 핵심 개념
- Spring framework는 IoC기반임
	- IoC의 구성요소는 `DI`, `DL` 
	- `DL(DependencyLookup)` 의존성 검색 
	- `DI (Dependecy Injection)`의존성 주입

## Spring Framwork의 특징 
- POJO(Plain Old Java Object)
	: 이전 **EJB(Enterprise JavaBeans)**는 확장 가능한 재사용이 가능한 로직을 개발하기 위해 사용되어왔음, `EJB`는 한가지 기능을 위해 불필요한 복잡한 로직이 과도하게 들어가는 단점 존재<br>
	POJO는 `getter`/`setter`를 가진 단순 자바 오브젝트로 정의하고 있음 이러한 **단순한 오브젝트**는 <U>의존성이 없고 추후 테스트 및 유지보수가 편리한 유연성의 장점을 가짐</U><br>
	이러한 장점으로 인해 객체지향적인 다양한 설계와 구현이 가능해지고 POJO의 기반의 Framework가 조명 받고 있음
- AOP(Aspect Oriented Programming) 
	: 대부분 소프트 웨어 개발 프로세스에서 사용하는 방법은 OOP(Object Oriented Programming) 임 <br>
	OOP는 객체지향 원칙에 따라 관심사가 같은 데이터를 한곳에 모아 분리하고 낮은 결합도를 갖게하여 독립적이고 유연한 모듈로 캡슐화를 하는 것을 말함 <br>
	하지만 이러한 과정 중에서 **중복된 코드**들이 많아지고 `가독성`,`확장성`,`유지보수성`을 떨어뜨림 **이러한 문제를 보완하기 위해 AOP**등장<br>
	`AOP`에서는 **핵심기능과 공통 기능을 분리**시켜 로직에 영향을 끼치지 않게 공통기능을 끼워 넣는 개발 형태이며 <br>
	- 무분별하게 중복되는 코드를 한 곳에 모아 중복되는 코드를 제거
	- `공통 기능`을 **한 곳에 보관**함으로써 <U>공통 기능 하나의 수정</U>으로 모든 `핵심 기능`들의 공통기능을 수정할 수 있음
	- 효율 적인 유지보수가 가능 
- MVC 구조
	- Model
	- View
	- Controller 
	의 구조로 개발을 나눠서 하게 됨
	
[[IoC, DI, DL]]

