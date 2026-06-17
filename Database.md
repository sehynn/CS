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
