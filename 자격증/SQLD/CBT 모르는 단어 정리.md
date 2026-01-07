## GRANT 명령

권한 부여 명령
```sql
GRANT <권한 리스트> ON <객체명> to <사용자 리스트>
<권한 리스트> : select, insert, delete, update, references 중 한 개 이상
<사용자 리스트> : 권한을 부여받는 사용자들의 리스트
-> <사용자 리스트>에게 <객체명>에 대한 <권한 리스트>를 실행할 권리를 부여한다는 의미
```
예시 ) 
- 읽기 권한 부여
- 쓰기 권한 부여 
- 삭제 권한 부여
```SQL
grant select on student to kim
grant select, insert on student to kim -- 두개의 권한을 주는 것도 가능
```

references 권한
테이블을 외래키로 참조할 수 있는 권한
```SQL
grant references (dept_id) on department to kim
```

all privileges 권한 
해당 테이블의 모든 권한을 부여

WITH GRANT OPTION 옵션
부여받은 권한을 다른 사용자에게 전파할 수 있는 옵션 
```SQL
grant select on student to kim with grant option 
student 테이블의 select 권한을 kim이 받고 다른 사용자에게 권한을 부여할 수 있는 권한 부여 (개초딩 ㅋ)
```

revoke 
다른 사용자에게 부여 권한 회수하기 위한 명령
```sql
revoke select on student from kim
```

![[Pasted image 20251104185933.png]]

## NVL 함수 
오라클 명령인듯? 
널처리 함수 data 값이 null일 때, 대신 뭐 넣어주는거 
NVL(data, 0) --> data가  null일 때 0 넣어라

## 윈도우 함수?
partition by 
windowing

## NTILE(4) OVER (ORDER BY SAL DESC) as DATA

## 슈퍼 타입 / 서브 타입?

Subquery의 종류 중에서 Subquery가 Mainquery의 제공자 역할을 하고 Mainquery의 값이 Subquery에 주입되지 않는 유형은 무엇인가?

## INTERSECT

## EXCEPT

## Sort MERGE

## Nested Loop

## 카티션 곱? 

## 비식별 관계?

## SQL 처리 흐름도?

## CUBE 함수

## MINUS

## UNPIVOT, value for metric
```sql
SELECT * FROM (

SELECT '2024' AS YEAR, 'Q1' AS

QUARTER, 100 AS SALES, 30 AS PROFIT

FROM DUAL

UNION ALL

SELECT '2024', 'Q2', 150, 50 FROM

DUAL

)

UNPIVOT (

VALUE FOR METRIC IN (SALES, PROFIT)

);​ 

```

## 인덱스


