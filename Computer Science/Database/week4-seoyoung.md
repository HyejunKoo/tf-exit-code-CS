# Week 04 - Database / Storage / Cache

---

## 제출 기준

* 필수 답변: COMMON-121 ~ COMMON-140
* 선택 답변: COMMON-141 ~ COMMON-160

---

## 필수 질문

## [COMMON-121] RDB와 NoSQL의 차이에 대해 설명해 주세요.

답변:

RDB는 데이터를 행과 열로 구성된 테이블에 저장하고, 테이블 간 관계와 정해진 스키마를 기반으로 데이터를 관리한다. SQL, JOIN, 제약조건, 트랜잭션을 활용해 데이터 정합성이 중요한 서비스에 적합하다.

NoSQL은 목적에 따라 Key-Value, Document, Wide-column, Graph 등의 형태로 데이터를 저장한다. 스키마가 유연하고 수평 확장이 쉬워 대규모 트래픽이나 비정형 데이터를 처리하는 데 유리하지만, 트랜잭션과 일관성 보장 수준은 제품마다 다르다.

* RDB: 정형 데이터, 복잡한 관계, 데이터 정합성이 중요한 경우
* NoSQL: 유연한 데이터 구조, 대규모 분산 처리, 빠른 확장이 필요한 경우

참고 자료:

* [AWS - 관계형 데이터베이스와 비관계형 데이터베이스 차이](https://aws.amazon.com/ko/compare/the-difference-between-relational-and-non-relational-databases/)

---

## [COMMON-122] Primary Key, Candidate Key, Foreign Key, Unique Key의 차이에 대해 설명해 주세요.

답변:

* Candidate Key: 각 행을 유일하게 식별할 수 있는 최소 컬럼 집합이다. 하나의 테이블에 여러 개 존재할 수 있다.
* Primary Key: Candidate Key 중 대표로 선택된 키다. 중복과 NULL을 허용하지 않으며 테이블당 하나만 지정할 수 있다.
* Foreign Key: 다른 테이블 또는 같은 테이블의 Primary Key나 Unique Key를 참조하여 참조 무결성을 보장한다.
* Unique Key: 컬럼 값의 중복을 제한한다. 테이블에 여러 개 지정할 수 있으며 NULL 허용 여부와 처리 방식은 DBMS마다 다를 수 있다.

참고 자료:

* [PostgreSQL 공식 문서 - Constraints](https://www.postgresql.org/docs/17/ddl-constraints.html)

---

## [COMMON-123] 정규화가 무엇이고, 정규화를 하지 않으면 어떤 이상 현상이 발생하는지 설명해 주세요.

답변:

정규화는 데이터 중복과 불필요한 종속성을 줄이기 위해 테이블을 적절하게 분리하는 과정이다. 데이터의 일관성을 유지하고 삽입, 수정, 삭제 과정에서 발생하는 이상 현상을 방지하는 것이 목적이다.

* 삽입 이상: 필요한 데이터를 추가하기 위해 관계없는 데이터까지 입력해야 하는 현상
* 갱신 이상: 중복된 데이터 중 일부만 수정되어 값이 서로 달라지는 현상
* 삭제 이상: 특정 데이터를 삭제하면서 필요한 다른 데이터까지 함께 삭제되는 현상

참고 자료:

* [Microsoft Learn - 데이터베이스 정규화 설명](https://learn.microsoft.com/ko-kr/office/troubleshoot/access/database-normalization-description)
* [Velog - 데이터베이스 키, 조인, 이상 현상, 인덱스](https://velog.io/%40hyehyes/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%82%A4Key-%EC%A1%B0%EC%9D%B8Join-SQL-Injection-SQL-vs-NoSQL-%EC%9D%B4%EC%83%81Anomaly-%EC%9D%B8%EB%8D%B1%EC%8A%A4Index)

---

## [COMMON-124] 반정규화가 필요한 상황은 언제인지 설명해 주세요.

답변:

반정규화는 조회 성능을 높이기 위해 정규화된 테이블을 합치거나 중복 데이터와 집계 데이터를 의도적으로 저장하는 방식이다.

* 여러 테이블의 JOIN이 반복되어 조회 성능이 실제 병목으로 확인된 경우
* 읽기 요청이 쓰기 요청보다 훨씬 많은 경우
* 통계나 집계 결과를 반복해서 계산해야 하는 경우
* 데이터 웨어하우스나 조회 중심 시스템을 구성하는 경우

데이터 중복으로 인해 수정 비용과 일관성 관리가 어려워지므로, 실행 계획과 성능 측정을 통해 필요성이 확인된 경우에만 적용해야 한다.

참고 자료:

* [Velog - DB 정리](https://velog.io/%40jun7867/DB-%EC%A0%95%EB%A6%AC)

---

## [COMMON-125] Join이 무엇이고, Inner Join과 Outer Join의 차이를 설명해 주세요.

답변:

Join은 두 개 이상의 테이블을 관련 컬럼을 기준으로 결합하여 하나의 결과로 조회하는 연산이다.

* Inner Join: 조인 조건을 만족하는 행만 반환한다.
* Left Outer Join: 왼쪽 테이블의 모든 행과 조건을 만족하는 오른쪽 행을 반환한다. 대응되는 행이 없으면 오른쪽 컬럼은 NULL이 된다.
* Right Outer Join: 오른쪽 테이블의 모든 행을 유지한다.
* Full Outer Join: 양쪽 테이블의 모든 행을 유지한다.

참고 자료:

* [PostgreSQL 공식 문서 - Table Expressions](https://www.postgresql.org/docs/current/queries-table-expressions.html)

---

## [COMMON-126] 인덱스가 무엇이고, 왜 조회 성능을 향상시키는지 설명해 주세요.

답변:

인덱스는 특정 컬럼의 값과 해당 데이터의 위치를 별도의 자료구조로 저장하는 객체다. 전체 테이블을 순차적으로 확인하지 않고 인덱스 트리를 따라 필요한 데이터 위치를 찾을 수 있어 디스크 I/O와 검색 범위를 줄인다.

다만 인덱스도 저장 공간을 사용하며, INSERT, UPDATE, DELETE 시 인덱스까지 수정해야 하므로 쓰기 성능이 저하될 수 있다.

참고 자료:

* [MySQL 공식 문서 - Optimization and Indexes](https://dev.mysql.com/doc/refman/8.4/en/optimization-indexes.html)

---

## [COMMON-127] B-Tree와 B+Tree에 대해 설명하고, DB 인덱스에서 B+Tree가 많이 사용되는 이유를 설명해 주세요.

답변:

* B-Tree: 내부 노드와 리프 노드 모두 키와 실제 데이터 또는 데이터 위치를 저장할 수 있다.
* B+Tree: 내부 노드는 탐색을 위한 키만 저장하고, 실제 데이터 위치는 리프 노드에만 저장한다. 리프 노드끼리는 연결 리스트 형태로 연결된다.

B+Tree는 내부 노드에 더 많은 키를 저장할 수 있어 트리 높이를 낮출 수 있다. 또한 리프 노드가 순서대로 연결되어 있어 범위 검색과 정렬된 순차 조회에 유리하므로 DB 인덱스에서 많이 사용된다.

참고 자료:

* [MySQL 공식 문서 - MySQL Glossary](https://docs.oracle.com/cd/E17952_01/mysql-5.7-en/glossary.html)
* [Velog - 데이터베이스와 B+Tree](https://velog.io/%40taehyeon96/Part-1.-%EC%A0%84%EC%82%B0-%EA%B8%B0%EC%B4%88-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)

---

## [COMMON-128] 복합 인덱스에서 컬럼 순서가 중요한 이유를 설명해 주세요.

답변:

복합 인덱스는 여러 컬럼을 지정된 순서대로 정렬하여 저장한다. 대부분의 B-Tree 계열 복합 인덱스는 인덱스의 앞쪽 컬럼부터 사용하는 Leftmost Prefix 원칙을 따른다.

예를 들어 `(user_id, created_at)` 인덱스는 `user_id` 또는 `user_id와 created_at` 조건에는 사용할 수 있지만, 일반적으로 `created_at`만 사용하는 조건에는 효율적으로 사용하기 어렵다.

컬럼 순서는 실제 쿼리를 기준으로 결정해야 한다.

* 동등 조건으로 자주 조회하는 컬럼
* 범위 조건으로 조회하는 컬럼
* ORDER BY나 GROUP BY에 사용하는 컬럼
* 데이터 중복도와 조회 범위

참고 자료:

* [MySQL 공식 문서 - Multiple-Column Indexes](https://dev.mysql.com/doc/refman/8.4/en/multiple-column-indexes.html)

---

## [COMMON-129] 인덱스를 사용했는데도 Table Full Scan이 발생할 수 있는 이유를 설명해 주세요.

답변:

인덱스가 존재하더라도 옵티마이저가 전체 테이블을 읽는 것이 더 저렴하다고 판단하면 Table Full Scan이 발생할 수 있다.

* 조건에 해당하는 데이터가 많아 인덱스 선택도가 낮은 경우
* 테이블의 크기가 매우 작은 경우
* 복합 인덱스의 선행 컬럼을 조건에 사용하지 않은 경우
* 인덱스 컬럼에 함수나 연산, 암시적 타입 변환을 적용한 경우
* 일반 B-Tree 인덱스에서 `LIKE '%keyword'`와 같이 앞에 와일드카드를 사용한 경우
* 통계 정보가 오래되어 옵티마이저가 잘못된 비용을 계산한 경우

`EXPLAIN`이나 `EXPLAIN ANALYZE`를 통해 실행 계획과 실제 조회 행 수를 확인해야 한다.

참고 자료:

* [MySQL 공식 문서 - EXPLAIN Statement](https://dev.mysql.com/doc/refman/8.4/en/explain.html)
* [MySQL 공식 문서 - Optimizer Statistics](https://dev.mysql.com/doc/refman/8.4/en/optimizer-statistics.html)

---

## [COMMON-130] 트랜잭션이 무엇이고, ACID에 대해 설명해 주세요.

답변:

트랜잭션은 하나의 논리적인 작업 단위로 처리되어야 하는 연산들의 집합이다. 트랜잭션 내부 작업은 모두 성공하여 Commit되거나, 문제가 발생하면 모두 취소되어 Rollback되어야 한다.

* Atomicity: 모든 작업이 전부 수행되거나 전부 수행되지 않아야 한다.
* Consistency: 트랜잭션 전후에 데이터베이스의 제약조건과 일관성이 유지되어야 한다.
* Isolation: 동시에 실행되는 트랜잭션이 서로의 중간 상태에 영향을 주지 않아야 한다.
* Durability: Commit된 결과는 장애가 발생해도 영구적으로 보존되어야 한다.

참고 자료:

* [Oracle 공식 문서 - Transactions](https://docs.oracle.com/database/121/CNCPT/transact.htm)

---

## [COMMON-131] 트랜잭션 격리 수준에 대해 설명해 주세요.

답변:

트랜잭션 격리 수준은 동시에 실행되는 트랜잭션이 서로의 변경 내용을 어느 정도까지 볼 수 있는지를 정의한다.

* Read Uncommitted: Commit되지 않은 데이터도 읽을 수 있어 Dirty Read가 발생할 수 있다.
* Read Committed: Commit된 데이터만 읽지만, 같은 데이터를 다시 조회했을 때 결과가 달라질 수 있다.
* Repeatable Read: 트랜잭션 동안 동일한 행을 반복 조회하면 같은 결과를 보장한다.
* Serializable: 트랜잭션을 순차적으로 실행한 것과 같은 결과를 보장하는 가장 높은 격리 수준이다.

격리 수준이 높을수록 데이터 일관성은 강화되지만, 잠금 대기나 트랜잭션 재시도로 인해 동시 처리 성능이 낮아질 수 있다. 세부 동작은 DBMS의 MVCC와 잠금 구현에 따라 차이가 있다.

참고 자료:

* [MySQL 공식 문서 - Transaction Isolation Levels](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html)

---

## [COMMON-132] Dirty Read, Non-repeatable Read, Phantom Read에 대해 설명해 주세요.

답변:

* Dirty Read: 다른 트랜잭션이 아직 Commit하지 않은 데이터를 읽는 현상이다. 해당 트랜잭션이 Rollback되면 존재하지 않는 값을 읽은 것이 된다.
* Non-repeatable Read: 한 트랜잭션에서 같은 행을 두 번 조회했는데, 다른 트랜잭션의 수정이나 삭제로 결과가 달라지는 현상이다.
* Phantom Read: 같은 조건으로 범위 조회를 반복했는데, 다른 트랜잭션의 삽입이나 삭제로 결과 행의 개수가 달라지는 현상이다.

Non-repeatable Read는 기존 행의 값이 달라지는 문제이고, Phantom Read는 조건에 해당하는 행 집합이 달라지는 문제다.

참고 자료:

* [PostgreSQL 공식 문서 - Transaction Isolation](https://www.postgresql.org/docs/16/transaction-iso.html)

---

## [COMMON-133] DB Lock이 무엇이고, Shared Lock과 Exclusive Lock의 차이에 대해 설명해 주세요.

답변:

DB Lock은 여러 트랜잭션이 같은 데이터에 동시에 접근할 때 데이터의 일관성을 보호하기 위한 동시성 제어 방식이다.

* Shared Lock: 데이터를 읽기 위한 잠금이다. 여러 트랜잭션이 동시에 Shared Lock을 획득할 수 있지만, 해당 데이터에 대한 Exclusive Lock은 제한된다.
* Exclusive Lock: 데이터를 수정하거나 삭제하기 위한 잠금이다. 다른 트랜잭션의 Shared Lock과 Exclusive Lock을 제한한다.

잠금을 오래 유지하거나 너무 넓은 범위에 적용하면 대기 시간과 Deadlock 가능성이 증가한다.

참고 자료:

* [MySQL 공식 문서 - InnoDB Locking](https://dev.mysql.com/doc/refman/8.4/en/innodb-locking.html)

---

## [COMMON-134] Optimistic Lock과 Pessimistic Lock의 차이에 대해 설명해 주세요.

답변:

* Optimistic Lock: 충돌이 자주 발생하지 않을 것이라고 가정한다. 조회 시 잠금을 선점하지 않고 버전 번호나 수정 시간을 비교하여, 수정 시점에 데이터가 변경되었는지 검사한다. 충돌하면 작업을 실패시키고 재시도해야 한다.
* Pessimistic Lock: 충돌이 발생할 가능성이 높다고 가정한다. 데이터를 조회하거나 수정하기 전에 `SELECT FOR UPDATE`와 같은 방식으로 잠금을 획득하여 다른 트랜잭션의 접근을 제한한다.

Optimistic Lock은 동시성이 높지만 충돌 시 재시도 로직이 필요하다. Pessimistic Lock은 충돌을 직접 방지하지만 대기 시간과 Deadlock 가능성이 증가한다.

참고 자료:

* [Spring Data 공식 문서 - Optimistic Locking](https://docs.spring.io/spring-data/relational/reference/jdbc/entity-persistence.html)
* [Spring Data 공식 문서 - Pessimistic Locking](https://docs.spring.io/spring-data/relational/reference/jdbc/transactions.html)

---

## [COMMON-135] DB Deadlock이 발생하는 상황과 해결 방법에 대해 설명해 주세요.

답변:

Deadlock은 두 개 이상의 트랜잭션이 서로 보유한 잠금이 해제되기를 순환 형태로 기다리는 상황이다.

예를 들어 트랜잭션 A가 데이터 1을 잠근 후 데이터 2를 기다리고, 트랜잭션 B가 데이터 2를 잠근 후 데이터 1을 기다리면 Deadlock이 발생한다.

DBMS는 일반적으로 Deadlock을 감지하면 트랜잭션 하나를 희생 대상으로 선택하여 Rollback한다.

* 모든 트랜잭션이 같은 순서로 데이터에 접근하게 한다.
* 트랜잭션 범위와 실행 시간을 줄인다.
* 적절한 인덱스를 사용해 잠금 범위를 줄인다.
* 한 번에 수정하는 데이터의 수를 줄인다.
* Deadlock으로 Rollback된 트랜잭션을 재시도하도록 구현한다.

참고 자료:

* [MySQL 공식 문서 - Deadlocks in InnoDB](https://dev.mysql.com/doc/refman/8.4/en/innodb-deadlocks.html)

---

## [COMMON-136] Connection Pool이 무엇이고, 왜 필요한지 설명해 주세요.

답변:

Connection Pool은 데이터베이스 연결을 미리 생성하거나 재사용 가능한 상태로 관리하는 공간이다.

요청마다 DB 연결을 새로 생성하면 네트워크 연결, 인증, 세션 생성 과정이 반복되어 비용이 발생한다. Connection Pool을 사용하면 기존 연결을 빌려 사용한 뒤 반환하여 응답 시간을 줄이고, 동시에 생성되는 DB 연결 수를 제한할 수 있다.

Pool 크기가 지나치게 크면 DB의 메모리와 스레드가 소모되고 쿼리 경합이 증가하며, 너무 작으면 애플리케이션 요청이 연결을 기다리게 된다. 따라서 DB 처리 능력과 애플리케이션 트래픽에 맞게 크기와 Timeout을 설정해야 한다.

참고 자료:

* [HikariCP와 Connection Pool](https://byeongbumseo.github.io/posts/HikariCP/)

---

## [COMMON-137] Cache를 사용하는 이유와 주의해야 할 점을 설명해 주세요.

답변:

Cache는 자주 조회되거나 계산 비용이 높은 데이터를 빠른 저장소에 보관하여 응답 시간을 줄이고 DB와 외부 시스템의 부하를 낮추기 위해 사용한다.

주의해야 할 점은 다음과 같다.

* 원본 데이터와 캐시 데이터가 달라지는 정합성 문제
* 캐시 만료 순간 요청이 DB로 몰리는 Cache Stampede
* 존재하지 않는 데이터를 반복 조회하는 Cache Penetration
* 특정 키에 요청이 집중되는 Hot Key 문제
* 메모리 부족과 부적절한 데이터 Eviction
* 캐시 장애가 전체 서비스 장애로 확산되는 문제

TTL, 명시적인 캐시 무효화, 만료 시간 분산, 요청 병합, 모니터링 등을 함께 적용해야 한다.

참고 자료:

* [Redis 공식 문서 - Cache Aside](https://redis.io/docs/latest/develop/use-cases/cache-aside/)
* [Redis 공식 문서 - Key Eviction](https://redis.io/docs/latest/develop/reference/eviction/)

---

## [COMMON-138] Cache Aside 패턴에 대해 설명해 주세요.

답변:

Cache Aside는 애플리케이션이 캐시와 DB를 직접 관리하는 방식으로, Lazy Loading이라고도 한다.

조회 과정은 다음과 같다.

* 먼저 캐시에서 데이터를 조회한다.
* 캐시에 데이터가 있으면 바로 반환한다.
* 캐시에 데이터가 없으면 DB에서 조회한다.
* DB에서 조회한 데이터를 캐시에 저장한 뒤 반환한다.

데이터를 수정할 때는 일반적으로 DB를 먼저 수정한 뒤 관련 캐시를 삭제한다. 이후 다음 조회 요청이 최신 데이터를 DB에서 읽어 캐시를 다시 채운다.

필요한 데이터만 캐싱할 수 있다는 장점이 있지만, 최초 요청은 느리고 캐시 만료 시 Stampede가 발생하거나 캐시와 DB 사이에 일시적인 불일치가 생길 수 있다.

참고 자료:

* [Redis 공식 문서 - Cache Aside](https://redis.io/docs/latest/develop/use-cases/cache-aside/)

---

## [COMMON-139] Redis와 같은 In-memory DB를 사용하는 이유를 설명해 주세요.

답변:

In-memory DB는 데이터를 주로 메모리에 저장하기 때문에 디스크 기반 저장소보다 빠른 읽기와 쓰기 성능을 제공한다.

Redis는 단순한 Key-Value뿐만 아니라 String, Hash, List, Set, Sorted Set 등의 자료구조와 TTL, 원자적 연산을 지원한다.

* 자주 조회되는 데이터 캐싱
* 로그인 세션 저장
* 인증 코드와 임시 데이터 저장
* 분산 락
* 요청 횟수 제한
* 실시간 순위표
* 메시지 전달과 이벤트 스트림

메모리는 디스크보다 비싸고 용량이 제한적이므로 TTL과 Eviction 정책을 설정해야 한다. 중요한 데이터를 저장할 경우에는 복제와 RDB Snapshot, AOF 등의 영속성 설정도 고려해야 한다.

참고 자료:

* [Redis 공식 문서 - What is Redis?](https://redis.io/tutorials/what-is-redis/)
* [Redis 공식 문서 - Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)

---

## [COMMON-140] SQL Injection이 무엇이고, 어떻게 방어할 수 있는지 설명해 주세요.

답변:

SQL Injection은 사용자 입력값을 SQL 문자열에 직접 연결했을 때, 공격자가 SQL 구문을 삽입하여 쿼리의 의도를 변경하는 공격이다. 이를 통해 인증을 우회하거나 데이터를 조회, 수정, 삭제할 수 있다.

주요 방어 방법은 다음과 같다.

* Prepared Statement와 Parameterized Query를 사용한다.
* 사용자 입력값을 SQL 문자열에 직접 연결하지 않는다.
* 동적으로 지정해야 하는 테이블명이나 컬럼명은 Allow List 방식으로 검증한다.
* ORM을 사용하더라도 안전한 파라미터 바인딩 방식을 사용한다.
* 애플리케이션 DB 계정에 최소 권한만 부여한다.
* 입력값 검증과 길이 및 형식 제한을 적용한다.

문자열 Escape만으로 방어하는 방식은 우회 가능성이 있으므로 주요 방어 수단으로 사용해서는 안 된다.

참고 자료:

* [OWASP - SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

---

## 선택 질문

## [COMMON-141] 기본키는 인덱스라고 할 수 있을까요? 기본키와 인덱스의 관계를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-142] 외래키는 자동으로 인덱스가 생성될까요? DBMS별로 차이가 있을 수 있는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-143] ORDER BY와 GROUP BY가 인덱스와 어떤 관계를 가지는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-144] COUNT(*), COUNT(1), COUNT(column)의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-145] 실행 계획이 무엇이고, 쿼리 최적화에 어떻게 활용할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-146] View가 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-147] Materialized View가 무엇인지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-148] WAL, Redo Log, Undo Log의 역할을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-149] MySQL InnoDB의 MVCC가 무엇인지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-150] DB Replication이 무엇이고, Master-Slave 구조의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-151] Replication Lag이 발생했을 때 어떤 문제가 생길 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-152] Sharding이 무엇이고, 어떤 기준으로 샤딩할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-153] Partitioning과 Sharding의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-154] NoSQL에서 Eventual Consistency가 무엇인지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-155] CAP 이론에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-156] 캐시 무효화가 어려운 이유와 해결 방법을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-157] Write-through, Write-back, Write-around 캐시 전략을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-158] DB 서버 부하가 높아질 때 애플리케이션과 DB 관점에서 어떤 대응을 할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-159] N+1 문제가 무엇이고, 어떻게 해결할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-160] 대용량 데이터 조회 API를 설계할 때 Pagination, Cursor, Index 관점에서 고려할 점을 설명해 주세요.

답변:

참고 자료:
