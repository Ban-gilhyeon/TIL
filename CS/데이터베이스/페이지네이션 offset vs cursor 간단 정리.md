
## Offset
Query Offset 연산  = 건너뛰기 

```SQL
Select * 
from payment 
order by created_at desc
limit 10 offset 10 * (page - 1) ## 한페이지 당 10개 
```

<장점>
- 구현이 쉽다 
- 최적화된 Offset 기반 페이지네이션 환경에서는 테이블 내 데이터가 아무리 많아도 첫 페이지 데이터를 가져오는데 오래 걸리지 않는다.

<단점>
- 페이지 수가 늘어나면 불필요한 데이터도 조회 후, 결과에는 반영 X 
offset 연산은 limit A offset B일 경우 DB 옵티마이저는  A + B 조회 후 반영하지 않는다
고로,
Offset 기반 페이지네이션은 뒤로 갈 수록 읽어야 할 데이터의 총량 자체는 많기 때문에 성능 저하가 우려됨

## Cursor 
마지막 읽은 데이터의 다음 데이터 (이전 데이터 + 1)부터 몇개를 불러오는 방식 
```sql
# 첫 페이지 진입시 발생 쿼리
select *
from post 
order by id desc
limit 10;

# 이후 페이지 요청시 발생 쿼리
select *
from post
where id < 10 # ex) cursor값이 10인 경우
limit 10;
```

