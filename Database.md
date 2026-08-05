## 정규화/비정규화

## 트랜잭션(ACID)

## 인덱스, JOIN, 서브쿼리



## SQL vs NoSQL(Not only SQL)
SQL과 NoSQL의 가장 큰 차이는 데이터를 저장하는 방식과 확장 방식이다.
| 항목       | SQL (RDB)         | NoSQL                        |
| -------- | ----------------- | ---------------------------- |
| 데이터 구조   | 테이블(Row, Column)  | Key-Value, Document, Graph 등 |
| 스키마      | 고정(Schema)        | 유연(Schema-less)              |
| 관계(Join) | 강력                | 제한적                          |
| 트랜잭션     | ACID 지원           | 상대적으로 약함                     |
| 확장       | Scale-Up(수직 확장)   | Scale-Out(수평 확장)             |
| 데이터 중복   | 최소화(정규화)          | 허용(비정규화)                     |
| 대표 DB    | MySQL, PostgreSQL | MongoDB, Redis, Cassandra    |
| 용도       | 금융, 주문, 회원        | 로그, 캐시, 채팅, 대용량 데이터          |

SQL은 관계와 데이터 정합성을 중시하는 테이블 기반 데이터베이스이고, NoSQL은 유연한 스키마와 높은 확장성을 제공하는 비관계형 데이터베이스이다.
SQL is a table-based database that focuses on maintaining relationships and ensuring data consistency through structured schemas and ACID transactions. In contrast, NoSQL is a non-relational database that offers flexible schemas and high scalability, making it suitable for handling large volumes of data and distributed systems.


## 관계형 모델 



## Valkey vs Redis
Valkey는 Redis 7.2를 포크(Fork)했으나, 운영의 지속성과 성능 최적화 방향에서 차이가 벌어지고 있다.

비교 항목	Valkey (Linux Foundation)	Redis (Redis Ltd.)
라이선스	BSD 3-Clause (완전 오픈소스)	RSALv2 / SSPLv1 (제약 있음)
거버넌스	커뮤니티 및 빅테크(AWS, Google) 연합	단일 기업 독점
I/O 모델	Aggressive Multi-threading	보수적 Multi-threading (6.0+)
확장성	모든 기능을 코어(Core)에 통합 지향	고급 기능은 유료(Enterprise) 모듈화 경향
어떤 것이 다를까?
1. 멀티 스레딩의 적극적 활용 (Aggressive Multi-threading)
Redis: 6.0부터 I/O(네트워크 읽기/쓰기) 처리에만 제한적으로 멀티 스레드를 도입
Valkey: I/O뿐만 아니라 데이터 마이그레이션(Slot Migration) 백그라운드 작업 등 부하가 큰 작업에 멀티 스레드를 더 적극적으로 사용하여 메인 스레드의 Blocking을 최소화
2. 메모리 할당 최적화 (Memory Efficiency)
Valkey: jemalloc 등 고성능 메모리 할당자의 최신 기능을 빠르게 적용하여 메모리 파편화(Fragmentation)를 줄이고 동일 스펙 대비 더 높은 처리량(QPS)을 확보
