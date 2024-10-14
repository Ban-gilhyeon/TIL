
# 엔티티와 테이블 간의 매핑 
- Entity 예시
```java
@Entity  
@Getter  
@NoArgsConstructor(access = AccessLevel.PROTECTED)  
@Table(name = "notices")  
public class Notice extends BaseTimeEntity {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long noticeId;  
  
    // private EmergencyRoom emergencyRoom;  
    private String title;  
  
    @ManyToOne(fetch = FetchType.LAZY)  
    @JoinColumn(name = "emergency_room_id")  
    private EmergencyRoom emergencyRoom;  
  
    @ManyToOne(fetch = FetchType.LAZY)  
    @JoinColumn(name = "user_id")  
    private User user;  
  
    private Double speedScore;  
  
    private Double kindScore;  
  
    private Double facilityScore;  
  
    private String content;

//등록할 때 필요한 생성자 만들기 해야 함 
//builder사용 
}
```

## Entity, Id 관련 Annotation
- @Entity : (Necessary) JPA 관리 대상 Entity가 됨 (DB의 Table과 mapping이 된다)
	- attribue / name : Entity의 이름을 설정함 (default : 클래스 이름)
- @Table : (Optional) DB 테이블의 이름 설정 가능
	- attribue / name : DB Table의 이름 설정 (default : 클래스 이름)
- @Id : (Necessary) 해당 field 값을 DB의 PK 값으로 지정
- @GeneratedValue : 기본키 자동 생성 지원 (없을 시 수동으로 지정해주어야 함 SQL의 Auto Incread 그거 )
	- **strategy = GenerationType.IDENTITY** : 기본키 생성을 DB에 위임(commit) 여부와 상관없이 persist 때, INSERT 쿼리를 실행하고, PK 값을 가져옴 
	- **strategy = GenerationType.SEQUENCE **: DB에서 제공하는 시퀀스를 사용해서 기본키를 생성
	- **strategy = GenerationType.AUTO** : JPA가 데이터베이스의 Dialect에 따라서 적절한 전략을 자동으로 선택
	- **strategy = GenerationType.TABLE** : 별도의 키 생성 테이블을 사용하는 전략 (TABLE을 별도로 만들게되어 추가 쿼리가 발생 -> 성능적으로 좋지 않음)

Entity의 필드와 Table의 컬럼과의 매핑 Annotation
- @Column
	- (Optional) 필드와 컬럼을 매핑
	- 사용하지 않을 경우 매핑은 되지만, attribute 값이 전부 기본값이 됨
- @Column attribute 설정사항
	- nullable : null 값을 허용할 지 여부 (default : true)
		- 원시 타입 적용 시, nullable = false 로 해놔야 에러를 사전에 방지할 수 있음 
	- updateable : 컬럼 데이터를 수정할 수 있는지 여부 (default : false)
	- unique : 중복 제한 조건 추가 (default : false)
	- length : 컬럼에 저장할 수 있는 문자 길이 제한(default : false)
	- name : Table Column에 별도의 이름을 지정하여 컬럼을 생성 
- @Transient : 테이블 컬럼과 매핑하지 않음
	- 주로 임시 데이터를 메로리에서 사용하기 위한 용도 
- @Enumerated : enum 타입과 매핑할 때 사용하는 Annotation
	- EnumType.STRING : enum의 이름을 DB에 저장
	- EnumType.ORDINAL : enum의 순서를 나타내는 숫자를 테이블에 저장

# Entity 간의 연관 관계 매핑
## 연관 관계 구현 방법 
- JPA에서는 한 `Entity`가 다른 `Entity`를 참조하는 것으로 연관 관계를 구현함
- DB에서는 Spring Data JDBC에서와 마찬가지로 다른 Table의 PK값을 FK로 가져와서 Column에 저장