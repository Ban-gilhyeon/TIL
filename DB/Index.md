index란 
추가적인 작업들을 통해서 테이블에서 데이터의 조회 속도를 향상 **시켜줄 수** 있는 자료구조이다.
- index를 잘못 사용했을 때 생기는 문제로 인해 향상이 안될 수 도 있음
index는 **색인**이다 책이나 잡지를 볼 때 원하는 내용을 찾을 때 까지 모든 페이지를 살펴 본다면 오랜시간이 소요됨 하지만 색인을 추가해서 내용을 찾을 수 있도록 도움을 줌 **데이터 베이스에 Index 또한 같은 역할** 
예시 ) 
Member_Id는 정렬 돼 있고, 자신만의 포인트를 가지고 있다. 
따라서 테이블 전체를 살펴보는 것이 아닌 Member_Id의 Pointer만 살펴봐 성능을 향상할 수 있다. 
![[Pasted image 20240926095616.png]]

## index 사용 주의 점 
조회 성능 향상을 생각하고, 무조건 Index를 사용하는 것은 옳지 못함 
1. 조회 과정을 수행하지 않는 : insert
	- 조회 과정을 수행하지 않는 Insert의 경우 새로운 데이터가 삽입 될 떄 마다 Indext를 최신화하도록 추가해야 함 추가적인 연산이 필요하므로 성능 저하를 일으킬 수도 있음
2. 조회 과정을 수행하는 : Select, Delete, Update 
	- Select, Delete, Update는 조회를 시작으로 함 조회에 대한 성능은 향상됨 
	- Select : 조회만 하믕로 추가 연산이 없음
	- Delete : 삭제하는 데이터의 Index를 사용하지 않도록 하는 작업(삭제는 아님) 후 추가적으로 진행
	- Update : 기존의 Index를 사용하지 않도록 하는 작업 (삭제는 아님) 후 갱신된 Index를 추가함
	Select의 경우 추가 연산이 없으므로 성능 향상을 도모할 수 있지만 Delete와 Update의 경우 추가 연산으로 인해 성능이 저하될 수 있음 <br>
	따라서 Delete, Insert, Update가 많이 수행된다면, Index가 오히려 데이터보다 크기가 커져 성능이 떨어지는 것을 초래할 수 있음  