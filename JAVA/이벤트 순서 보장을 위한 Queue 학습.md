Redis 캐싱에 대한 순서를 보장하기 위해 여러가지 방법 중 메세지 큐를 사용하는 것이 제일 적합하다는 생각을 했다 
먼저 순서 보장을 위한 후보는 
- Kafka 
- RebitMQ
- Redis Streams 
- Blocking Queue
등이 있었다. 

|**구분**|**일반 Queue (**LinkedList**,** ArrayDeque **등)**|**BlockingQueue (**LinkedBlockingQueue**,** ArrayBlockingQueue **등)**|
|---|---|---|
|**단일 스레드 환경**|순서 보장 O (FIFO)|순서 보장 O (FIFO)|
|**멀티 스레드 환경**|❌ Thread-safe 아님 → **경쟁 조건 발생**|✅ Thread-safe (내부에서 락/동기화 처리)|
|**대기 처리**|❌ null 반환 또는 예외|✅ 큐가 비었거나 가득 차면 **자동으로 대기/차단**|
|**용도**|일반적인 로컬 처리에 적합|멀티스레드 환경, 생산자/소비자 구조 등에 적합|

### 일반 Queue 는…

- 멀티스레드 환경에서 동시 접근하면 **순서 꼬임, 충돌, 데이터 유실** 발생 가능

- 동기화(synchronized)를 직접 해줘야 함 → 번거롭고 실수하기 쉬움

### BlockingQueue 는…

- **Thread-safe**: 여러 스레드가 동시에 put()/take() 해도 안전하게 처리됨
- **자동 대기(Blocking)**:
	- 큐가 비었을 때 take() 하면 → 자동으로 대기
	- 큐가 가득 찼을 때 put() 하면 → 자동으로 대기
- 즉, **생산자-소비자 패턴에 최적화된 자료구조**