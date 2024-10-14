엔티티를 작성할 때 무언가 묶어서 필요한 필드가 있을 때 사용하면 좋다 

객체로 묶어 표현하면 보다 객체지향적인 코드가 될 것이다. 

2차 프로젝트 `EmergencyRoom` 엔티티를 작성할 때 필드 값으로 
```java
@Entity  
@Getter  
@NoArgsConstructor(access = AccessLevel.PROTECTED)  
@Table(name = "emergency_rooms")  
public class EmergencyRoom extends BaseTimeEntity {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    private String hospitalName;  
  
	// 중략 ... 
  
    private String medicalDepartments;  
  
    @Embedded private Location location;  
  
    @Embedded private BedCount bedCount;

	@Embeddable  
	@NoArgsConstructor(access = AccessLevel.PROTECTED)  
	@AllArgsConstructor  
	@Builder  
	public static class Location {  
	    private String longitude;  
	  
	    private String latitude;  
	}  
	  
	@Embeddable  
	@NoArgsConstructor(access = AccessLevel.PROTECTED)  
	@AllArgsConstructor  
	@Builder  
	public static class BedCount {  
	    private int totalBedCount;  
	  
	    private int thoracicIcuBedCount;  
	  
	    private int neurologicalIcuBedCount;  
	    //.. 중략 
	  }
```
가 있을 때 `location` 과 `bedCount` 필드를 @Embeddable로 묶어서 표현할 수 있다.