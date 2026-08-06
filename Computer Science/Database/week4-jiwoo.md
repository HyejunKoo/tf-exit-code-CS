# Week 04 - Database / Storage / Cache

---

## 제출 기준

- 필수 답변: COMMON-121 ~ COMMON-140
- 선택 답변: COMMON-141 ~ COMMON-160

---

## 필수 질문

## [COMMON-121] RDB와 NoSQL의 차이에 대해 설명해 주세요.

답변:
RDB는 정해진 스키마와 테이블 구조에 데이터를 저장하고, SQL, JOIN, 트랜잭션으로 데이터 정합성을 보장합니다. NoSQL은 Key-Value, Document, Wide-column, Graph 등 다양한 형태로 데이터를 저장하며, 스키마가 유연하고 수평 확장이 쉬워 대규모 분산 처리에 적합합니다. RDB는 정형 데이터와 복잡한 관계가 있는 서비스에, NoSQL은 비정형 데이터와 빠른 확장이 필요한 경우에 선택합니다.

추가 설명)
| 구분 | RDB | NoSQL |
|------|-----|-------|
| 스키마 | 고정 | 유연 |
| 확장 방식 | 수직 확장 | 수평 확장 |
| 트랜잭션 | ACID 완전 지원 | 제품마다 다름 |
| 쿼리 언어 | SQL | 제품별 API |
| 대표 예시 | MySQL, PostgreSQL | MongoDB, Redis, Cassandra |
| 적합한 경우 | 정합성 중요, 복잡한 관계 | 대규모 트래픽, 유연한 구조 |

참고 자료:
- [AWS - 관계형 데이터베이스와 비관계형 데이터베이스 차이](https://aws.amazon.com/ko/compare/the-difference-between-relational-and-non-relational-databases/)

---

## [COMMON-122] Primary Key, Candidate Key, Foreign Key, Unique Key의 차이에 대해 설명해 주세요.

답변:
Candidate Key는 각 행을 유일하게 식별할 수 있는 최소 컬럼 집합으로, 테이블에 여러 개 존재할 수 있습니다. Primary Key는 Candidate Key 중 대표로 선택된 키로 중복과 NULL을 허용하지 않으며 테이블당 하나만 지정합니다. Foreign Key는 다른 테이블의 Primary Key를 참조하여 참조 무결성을 보장하고, Unique Key는 중복을 제한하되 NULL 허용 여부는 DBMS마다 다릅니다.

추가 설명)
| 키 종류 | 유일성 | NULL 허용 | 개수 제한 | 역할 |
|---------|--------|-----------|-----------|------|
| Candidate Key | O | X | 여러 개 가능 | 유일 식별 후보 |
| Primary Key | O | X | 테이블당 1개 | 대표 식별자 |
| Foreign Key | X | O | 여러 개 가능 | 참조 무결성 |
| Unique Key | O | O (DBMS별 상이) | 여러 개 가능 | 중복 제한 |

```
[회원 테이블]
id(PK) | email(UK) | name
  1    | a@a.com   | 지우

[주문 테이블]
order_id(PK) | user_id(FK → 회원.id) | amount
     1       |           1           |  5000
```

참고 자료:
- [PostgreSQL 공식 문서 - Constraints](https://www.postgresql.org/docs/17/ddl-constraints.html)

---

## [COMMON-123] 정규화가 무엇이고, 정규화를 하지 않으면 어떤 이상 현상이 발생하는지 설명해 주세요.

답변:
정규화는 데이터 중복과 불필요한 종속성을 줄이기 위해 테이블을 적절히 분리하는 과정입니다. 정규화를 하지 않으면 삽입, 갱신, 삭제 과정에서 이상 현상이 발생하여 데이터 일관성이 깨질 수 있습니다.

추가 설명)
| 이상 현상 | 설명 | 예시 |
|-----------|------|------|
| 삽입 이상 | 필요한 데이터를 추가하려면 불필요한 데이터도 함께 입력해야 하는 현상 | 학과 정보를 추가하려면 수강 학생이 있어야 함 |
| 갱신 이상 | 중복 데이터 중 일부만 수정되어 불일치가 발생하는 현상 | 교수명이 여러 행에 있을 때 일부만 변경 |
| 삭제 이상 | 특정 데이터 삭제 시 필요한 다른 데이터도 함께 삭제되는 현상 | 마지막 수강생 삭제 시 강의 정보도 사라짐 |

정규화 단계: 1NF (원자값) → 2NF (부분 함수 종속 제거) → 3NF (이행 함수 종속 제거) → BCNF

참고 자료:
- [Microsoft Learn - 데이터베이스 정규화 설명](https://learn.microsoft.com/ko-kr/office/troubleshoot/access/database-normalization-description)

---

## [COMMON-124] 반정규화가 필요한 상황은 언제인지 설명해 주세요.

답변:
반정규화는 조회 성능을 높이기 위해 정규화된 테이블을 합치거나 중복 데이터를 의도적으로 저장하는 방식입니다. 여러 테이블의 JOIN이 반복되어 실제 성능 병목이 확인된 경우, 읽기 요청이 쓰기보다 훨씬 많은 경우, 집계 결과를 반복 계산해야 하는 경우에 적용을 고려합니다. 단, 데이터 중복으로 일관성 관리가 어려워지므로 성능 측정을 통해 필요성이 확인된 경우에만 적용해야 합니다.

추가 설명)
| 상황 | 반정규화 적용 방법 |
|------|------------------|
| 잦은 JOIN 병목 | 테이블 병합 또는 컬럼 복제 |
| 집계 반복 계산 | 집계 결과 컬럼 추가 (ex: order_count) |
| 읽기 중심 서비스 | 조회용 복제 테이블 구성 |
| 데이터 웨어하우스 | 스타 스키마, 팩트/디멘전 테이블 구성 |

정규화와 반정규화는 트레이드오프 관계: 반정규화 적용 시 쓰기 비용과 일관성 관리 비용이 증가합니다.

참고 자료:

---

## [COMMON-125] Join이 무엇이고, Inner Join과 Outer Join의 차이를 설명해 주세요.

답변:
Join은 두 개 이상의 테이블을 관련 컬럼을 기준으로 결합하여 하나의 결과로 조회하는 연산입니다. Inner Join은 조인 조건을 만족하는 행만 반환하고, Outer Join은 조건을 만족하지 않는 행도 NULL로 채워 반환합니다.

추가 설명)
| Join 종류 | 반환 범위 |
|-----------|----------|
| INNER JOIN | 양쪽 테이블 모두에 매칭되는 행만 |
| LEFT OUTER JOIN | 왼쪽 테이블 전체 + 오른쪽 매칭 (없으면 NULL) |
| RIGHT OUTER JOIN | 오른쪽 테이블 전체 + 왼쪽 매칭 (없으면 NULL) |
| FULL OUTER JOIN | 양쪽 테이블 전체 (없으면 NULL) |
| CROSS JOIN | 양쪽 테이블의 모든 조합 (카테시안 곱) |

```
A 테이블     B 테이블
id | name   id | score
 1 | 지우    1  |  90
 2 | 민수    3  |  85

INNER JOIN → id=1인 행만 (id=2, id=3은 제외)
LEFT JOIN  → id=1, id=2 (id=2의 score는 NULL)
```

참고 자료:
- [PostgreSQL 공식 문서 - Table Expressions](https://www.postgresql.org/docs/current/queries-table-expressions.html)

---

## [COMMON-126] 인덱스가 무엇이고, 왜 조회 성능을 향상시키는지 설명해 주세요.

답변:
인덱스는 특정 컬럼의 값과 데이터 위치를 별도의 자료구조로 저장한 객체입니다. 전체 테이블을 순차 탐색하는 Full Table Scan 대신 인덱스 트리를 따라 필요한 데이터 위치를 빠르게 찾을 수 있어 디스크 I/O와 검색 범위를 줄입니다. 단, 인덱스도 저장 공간을 차지하고 INSERT, UPDATE, DELETE 시 인덱스도 수정해야 하므로 쓰기 성능이 저하될 수 있습니다.

추가 설명)
```
[인덱스 없음]        [인덱스 있음]
테이블 전체 순차 탐색  인덱스 B+Tree 탐색
O(N)               O(log N)

인덱스 구조 (B+Tree):
      [50]
    /      \
  [20,30] [60,80]
 /  |  |    |  \
리프 노드들 ← → ← → (연결 리스트)
(실제 데이터 위치 저장)
```

| 장점 | 단점 |
|------|------|
| 조회 성능 향상 (O(log N)) | 추가 저장 공간 필요 |
| 정렬/범위 조회 빠름 | 쓰기 성능 저하 (INSERT/UPDATE/DELETE) |
| ORDER BY 최적화 | 인덱스 설계 비용 |

참고 자료:
- [MySQL 공식 문서 - Optimization and Indexes](https://dev.mysql.com/doc/refman/8.4/en/optimization-indexes.html)

---

## [COMMON-127] B-Tree와 B+Tree에 대해 설명하고, DB 인덱스에서 B+Tree가 많이 사용되는 이유를 설명해 주세요.

답변:
B-Tree는 내부 노드와 리프 노드 모두 키와 데이터를 저장하고, B+Tree는 내부 노드에는 탐색 키만 저장하고 실제 데이터는 리프 노드에만 저장합니다. B+Tree의 리프 노드는 연결 리스트로 이어져 있어 범위 검색과 순차 조회에 유리합니다. 내부 노드에 더 많은 키를 저장할 수 있어 트리 높이가 낮아지고 디스크 I/O 횟수가 줄어들기 때문에 DB 인덱스에서 B+Tree를 주로 사용합니다.

추가 설명)
```
[B-Tree]
내부 노드: [키 | 데이터 | 포인터]
리프 노드: [키 | 데이터]
→ 범위 검색 시 리프 노드 사이를 이동해야 해서 비효율적

[B+Tree]
내부 노드: [키 | 포인터]  ← 데이터 없음, 키만 저장
리프 노드: [키 | 데이터] ↔ [키 | 데이터] ↔ [키 | 데이터]
                         (연결 리스트로 연결)
→ 범위 검색: 첫 리프 찾고 다음 노드 순회만 하면 됨
```

| 구분 | B-Tree | B+Tree |
|------|--------|--------|
| 데이터 저장 위치 | 내부 + 리프 | 리프만 |
| 리프 연결 | X | 연결 리스트 O |
| 범위 검색 | 비효율적 | 효율적 |
| 내부 노드 키 수 | 상대적으로 적음 | 더 많음 (트리 높이 낮춤) |

참고 자료:
- [MySQL 공식 문서 - MySQL Glossary](https://docs.oracle.com/cd/E17952_01/mysql-5.7-en/glossary.html)

---

## [COMMON-128] 복합 인덱스에서 컬럼 순서가 중요한 이유를 설명해 주세요.

답변:
복합 인덱스는 지정된 컬럼 순서대로 정렬하여 저장하므로, Leftmost Prefix 원칙에 따라 인덱스 앞쪽 컬럼부터 순서대로 사용해야 인덱스를 효율적으로 활용할 수 있습니다. 예를 들어 `(user_id, created_at)` 인덱스는 `user_id` 단독 조건이나 `user_id + created_at` 조건에는 사용 가능하지만, `created_at` 단독 조건에는 일반적으로 효율적으로 사용할 수 없습니다. 컬럼 순서는 동등 조건, 범위 조건, ORDER BY/GROUP BY 순으로 배치하는 것이 일반적입니다.

추가 설명)
```
인덱스: (user_id, status, created_at)

활용 가능한 쿼리:
  WHERE user_id = 1
  WHERE user_id = 1 AND status = 'active'
  WHERE user_id = 1 AND status = 'active' AND created_at > '2024-01-01'

활용 불가 (선행 컬럼 누락):
  WHERE status = 'active'              ← user_id 없음
  WHERE created_at > '2024-01-01'     ← user_id, status 없음
```

| 컬럼 배치 순서 | 이유 |
|--------------|------|
| 동등 조건 (=) 컬럼 먼저 | 검색 범위를 즉시 좁힐 수 있음 |
| 범위 조건 (>, <, BETWEEN) 컬럼 | 범위 조건 이후 컬럼은 인덱스 활용도 낮아짐 |
| 선택도 높은 컬럼 앞에 | 중복이 적을수록 검색 범위를 더 빨리 좁힘 |

참고 자료:
- [MySQL 공식 문서 - Multiple-Column Indexes](https://dev.mysql.com/doc/refman/8.4/en/multiple-column-indexes.html)

---

## [COMMON-129] 인덱스를 사용했는데도 Table Full Scan이 발생할 수 있는 이유를 설명해 주세요.

답변:
옵티마이저는 비용 기반으로 실행 계획을 결정하므로, 인덱스가 있더라도 Full Scan이 더 저렴하다고 판단하면 Full Scan을 선택합니다. 조건에 해당하는 데이터가 너무 많거나, 인덱스 컬럼에 함수, 연산, 암시적 타입 변환을 적용한 경우, 복합 인덱스의 선행 컬럼을 생략한 경우 등이 주요 원인입니다.

추가 설명)
| 원인 | 예시 |
|------|------|
| 낮은 선택도 (데이터 분포 불균일) | status IN ('active', 'inactive') → 거의 전체 행 |
| 인덱스 컬럼에 함수 적용 | WHERE YEAR(created_at) = 2024 |
| 암시적 타입 변환 | WHERE user_id = '1' (컬럼이 INT인데 문자열 비교) |
| 앞에 와일드카드 LIKE | WHERE name LIKE '%홍길동' |
| 복합 인덱스 선행 컬럼 누락 | (user_id, created_at) 인덱스에서 created_at만 조건 |
| 테이블 크기가 매우 작은 경우 | 전체 읽기가 인덱스 탐색보다 빠름 |
| 통계 정보 오래됨 | 옵티마이저가 잘못된 비용 계산 |

`EXPLAIN` 또는 `EXPLAIN ANALYZE`로 실행 계획을 반드시 확인해야 합니다.

참고 자료:
- [MySQL 공식 문서 - EXPLAIN Statement](https://dev.mysql.com/doc/refman/8.4/en/explain.html)

---

## [COMMON-130] 트랜잭션이 무엇이고, ACID에 대해 설명해 주세요.

답변:
트랜잭션은 하나의 논리적 작업 단위로 처리되어야 하는 연산들의 집합으로, 모두 성공하여 Commit되거나 문제 발생 시 모두 취소되어 Rollback됩니다. ACID는 트랜잭션이 보장해야 하는 네 가지 특성으로, 데이터 신뢰성과 일관성의 기반이 됩니다.

추가 설명)
| 속성 | 의미 | 예시 |
|------|------|------|
| Atomicity (원자성) | 전부 성공하거나 전부 실패 | 계좌 이체: 출금과 입금이 함께 처리됨 |
| Consistency (일관성) | 트랜잭션 전후 제약조건 유지 | 잔액이 음수가 되면 안 됨 |
| Isolation (격리성) | 동시 실행 트랜잭션이 서로 영향 안 줌 | 다른 이체 작업의 중간 결과가 보이지 않음 |
| Durability (지속성) | Commit 결과는 영구 저장 | 장애가 발생해도 데이터 유지 |

```
[계좌 이체 트랜잭션]
BEGIN;
  UPDATE accounts SET balance = balance - 10000 WHERE id = 1;
  UPDATE accounts SET balance = balance + 10000 WHERE id = 2;
COMMIT; ← 둘 다 성공해야 반영
ROLLBACK; ← 하나라도 실패하면 전체 취소
```

참고 자료:
- [Oracle 공식 문서 - Transactions](https://docs.oracle.com/database/121/CNCPT/transact.htm)

---

## [COMMON-131] 트랜잭션 격리 수준에 대해 설명해 주세요.

답변:
트랜잭션 격리 수준은 동시에 실행되는 트랜잭션이 서로의 변경 내용을 어느 수준까지 볼 수 있는지 정의합니다. 격리 수준이 높을수록 데이터 일관성은 강화되지만, 잠금 대기나 트랜잭션 재시도로 인해 동시 처리 성능이 낮아집니다.

추가 설명)
| 격리 수준 | Dirty Read | Non-repeatable Read | Phantom Read | 특징 |
|-----------|-----------|---------------------|-------------|------|
| Read Uncommitted | 발생 | 발생 | 발생 | Commit 안 된 데이터도 읽음 |
| Read Committed | 방지 | 발생 | 발생 | Oracle, PostgreSQL 기본값 |
| Repeatable Read | 방지 | 방지 | 발생 | MySQL InnoDB 기본값 |
| Serializable | 방지 | 방지 | 방지 | 순차 실행과 동일, 성능 낮음 |

MySQL InnoDB의 Repeatable Read는 MVCC를 활용해 Phantom Read도 대부분 방지합니다.

참고 자료:
- [MySQL 공식 문서 - Transaction Isolation Levels](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html)

---

## [COMMON-132] Dirty Read, Non-repeatable Read, Phantom Read에 대해 설명해 주세요.

답변:
Dirty Read는 다른 트랜잭션이 아직 Commit하지 않은 데이터를 읽는 현상으로, 해당 트랜잭션이 Rollback되면 존재하지 않는 값을 읽은 것이 됩니다. Non-repeatable Read는 같은 행을 두 번 조회했을 때 다른 트랜잭션의 수정으로 값이 달라지는 현상이고, Phantom Read는 같은 범위 조회를 반복했을 때 다른 트랜잭션의 삽입이나 삭제로 결과 행 수가 달라지는 현상입니다.

추가 설명)
```
[Dirty Read]
T1: UPDATE balance = 5000 (아직 Commit 안 함)
T2: SELECT balance → 5000 읽음
T1: ROLLBACK → T2가 읽은 5000은 유령 값

[Non-repeatable Read]
T1: SELECT balance → 1000
T2: UPDATE balance = 2000; COMMIT
T1: SELECT balance → 2000 (같은 행인데 값이 달라짐)

[Phantom Read]
T1: SELECT COUNT(*) WHERE age > 20 → 5건
T2: INSERT (age=25); COMMIT
T1: SELECT COUNT(*) WHERE age > 20 → 6건 (행 수가 달라짐)
```

| 문제 유형 | 대상 | 발생 원인 |
|-----------|------|---------|
| Dirty Read | 특정 행의 값 | 미Commit 데이터 읽기 |
| Non-repeatable Read | 특정 행의 값 | 다른 트랜잭션의 UPDATE/DELETE |
| Phantom Read | 범위 결과의 행 수 | 다른 트랜잭션의 INSERT/DELETE |

참고 자료:
- [PostgreSQL 공식 문서 - Transaction Isolation](https://www.postgresql.org/docs/16/transaction-iso.html)

---

## [COMMON-133] DB Lock이 무엇이고, Shared Lock과 Exclusive Lock의 차이에 대해 설명해 주세요.

답변:
DB Lock은 여러 트랜잭션이 같은 데이터에 동시에 접근할 때 데이터 일관성을 보호하기 위한 동시성 제어 방식입니다. Shared Lock은 읽기 잠금으로 여러 트랜잭션이 동시에 획득 가능하지만 Exclusive Lock은 차단하고, Exclusive Lock은 쓰기 잠금으로 다른 모든 잠금을 차단합니다. 잠금 범위가 넓거나 오래 유지되면 대기 시간과 Deadlock 가능성이 증가합니다.

추가 설명)
| 구분 | Shared Lock (S) | Exclusive Lock (X) |
|------|----------------|-------------------|
| 목적 | 읽기 (SELECT) | 쓰기 (INSERT/UPDATE/DELETE) |
| 동시 획득 | 여러 트랜잭션 가능 | 단독만 가능 |
| S Lock과 공존 | 가능 | 불가 |
| X Lock과 공존 | 불가 | 불가 |

```
잠금 호환 행렬:
        S Lock  X Lock
S Lock    O       X
X Lock    X       X
```

참고 자료:
- [MySQL 공식 문서 - InnoDB Locking](https://dev.mysql.com/doc/refman/8.4/en/innodb-locking.html)

---

## [COMMON-134] Optimistic Lock과 Pessimistic Lock의 차이에 대해 설명해 주세요.

답변:
Optimistic Lock은 충돌이 드물 것이라 가정하고 조회 시 잠금 없이 버전 번호를 비교해 수정 시점에 충돌을 감지하며, 충돌 시 재시도가 필요합니다. Pessimistic Lock은 충돌이 자주 발생한다고 가정하고 SELECT FOR UPDATE 등으로 미리 잠금을 획득해 다른 트랜잭션의 접근을 차단합니다. Optimistic Lock은 동시성이 높지만 충돌 처리 로직이 복잡하고, Pessimistic Lock은 안전하지만 대기 시간과 Deadlock 위험이 있습니다.

추가 설명)
| 구분 | Optimistic Lock | Pessimistic Lock |
|------|----------------|-----------------|
| 충돌 가정 | 드물다 | 자주 발생한다 |
| 잠금 시점 | 수정 시 (버전 비교) | 조회 시 (SELECT FOR UPDATE) |
| 동시성 | 높음 | 낮음 |
| 충돌 처리 | 재시도 필요 | DB가 자동 대기 |
| Deadlock 위험 | 낮음 | 높음 |
| 적합한 상황 | 읽기 많고 충돌 적은 경우 | 충돌이 잦고 정확성이 중요한 경우 |

```
[Optimistic Lock]
SELECT id, version FROM items WHERE id = 1;
-- version = 3
UPDATE items SET stock = stock - 1, version = 4
WHERE id = 1 AND version = 3;
-- 영향받은 행이 0이면 충돌 → 재시도

[Pessimistic Lock]
BEGIN;
SELECT * FROM items WHERE id = 1 FOR UPDATE;
-- 다른 트랜잭션은 잠금 해제될 때까지 대기
UPDATE items SET stock = stock - 1 WHERE id = 1;
COMMIT;
```

참고 자료:

---

## [COMMON-135] DB Deadlock이 발생하는 상황과 해결 방법에 대해 설명해 주세요.

답변:
Deadlock은 두 개 이상의 트랜잭션이 서로 보유한 잠금을 상대방이 해제하기를 순환 형태로 기다리는 상태입니다. 예를 들어 트랜잭션 A가 데이터 1을 잠그고 데이터 2를 기다리는 동시에, 트랜잭션 B가 데이터 2를 잠그고 데이터 1을 기다리면 Deadlock이 발생합니다. DBMS는 감지 후 하나의 트랜잭션을 희생시켜 Rollback하고, 애플리케이션은 해당 트랜잭션을 재시도해야 합니다.

추가 설명)
```
[Deadlock 발생 시나리오]
T1: LOCK(데이터 A) → 데이터 B 기다리는 중...
T2: LOCK(데이터 B) → 데이터 A 기다리는 중...
→ 서로 무한 대기 (순환 대기)
```

| 예방/해결 방법 | 설명 |
|--------------|------|
| 동일한 접근 순서 | 모든 트랜잭션이 같은 순서로 데이터에 접근 |
| 트랜잭션 범위 최소화 | 잠금 보유 시간과 범위 줄이기 |
| 적절한 인덱스 사용 | Row Lock 대신 Range Lock 방지 |
| Deadlock 감지 및 재시도 | DBMS가 희생 트랜잭션 선택 후 재시도 |
| Lock Timeout 설정 | 일정 시간 대기 후 자동 포기 |

참고 자료:
- [MySQL 공식 문서 - Deadlocks in InnoDB](https://dev.mysql.com/doc/refman/8.4/en/innodb-deadlocks.html)

---

## [COMMON-136] Connection Pool이 무엇이고, 왜 필요한지 설명해 주세요.

답변:
Connection Pool은 DB 연결을 미리 생성하거나 재사용 가능한 상태로 관리하는 공간입니다. 요청마다 새로운 DB 연결을 생성하면 네트워크 연결, 인증, 세션 생성 비용이 반복 발생하므로, 풀에서 기존 연결을 빌려 쓰고 반환하여 응답 시간을 줄이고 동시 연결 수를 제한할 수 있습니다. 풀 크기가 너무 크면 DB 자원이 낭비되고, 너무 작으면 요청이 연결을 기다리므로 트래픽에 맞게 튜닝해야 합니다.

추가 설명)
```
[Connection Pool 없음]           [Connection Pool 있음]
앱 → 연결 생성 → DB → 연결 종료   앱 → 풀에서 연결 대여
앱 → 연결 생성 → DB → 연결 종료       ↕
(매 요청마다 생성/종료 비용)       풀 (미리 생성된 연결들)
                                  앱 → 사용 후 풀에 반환
```

| 설정 항목 | 설명 |
|-----------|------|
| maximumPoolSize | 최대 연결 수 (DB 처리 능력 기준) |
| minimumIdle | 유지할 최소 유휴 연결 수 |
| connectionTimeout | 연결 획득 대기 최대 시간 |
| idleTimeout | 유휴 연결 유지 최대 시간 |
| maxLifetime | 연결 최대 수명 |

참고 자료:

---

## [COMMON-137] Cache를 사용하는 이유와 주의해야 할 점을 설명해 주세요.

답변:
Cache는 자주 조회되거나 계산 비용이 높은 데이터를 빠른 저장소에 보관하여 응답 시간을 줄이고 DB 부하를 낮추기 위해 사용합니다. 단, 원본 데이터와 캐시 데이터가 불일치하는 정합성 문제, Cache Stampede, Cache Penetration, Hot Key 문제 등을 주의해야 합니다.

추가 설명)
| 주의 사항 | 설명 | 대응 방법 |
|----------|------|---------|
| 정합성 문제 | 원본 수정 시 캐시 불일치 | 명시적 캐시 무효화, TTL 설정 |
| Cache Stampede | 만료 순간 대량 요청이 DB로 몰림 | TTL 분산, 요청 병합, Warm-up |
| Cache Penetration | 없는 키를 반복 조회해 DB 과부하 | Null 값도 캐싱, Bloom Filter |
| Hot Key | 특정 키에 요청 집중 | 키 분산, 로컬 캐시 병행 |
| Eviction | 메모리 부족 시 필요한 데이터 삭제 | 적절한 Eviction 정책 설정 |
| 장애 전파 | 캐시 서버 장애 시 전체 DB 과부하 | 캐시 Circuit Breaker, Fallback |

참고 자료:
- [Redis 공식 문서 - Cache Aside](https://redis.io/docs/latest/develop/use-cases/cache-aside/)

---

## [COMMON-138] Cache Aside 패턴에 대해 설명해 주세요.

답변:
Cache Aside는 애플리케이션이 캐시와 DB를 직접 관리하는 방식으로 Lazy Loading이라고도 합니다. 조회 시 캐시를 먼저 확인하고, Cache Hit이면 즉시 반환하며 Cache Miss이면 DB에서 읽어 캐시에 저장 후 반환합니다. 쓰기 시에는 DB를 먼저 수정하고 관련 캐시를 삭제하여, 다음 조회 요청이 최신 데이터를 캐시에 채우게 합니다.

추가 설명)
```
[조회 흐름]
앱 → 캐시 조회
  ├─ Hit  → 캐시 데이터 반환
  └─ Miss → DB 조회 → 캐시 저장 → 반환

[쓰기 흐름]
앱 → DB 수정 → 캐시 삭제 (invalidate)
→ 다음 조회 시 Miss 발생 → DB에서 최신 데이터 캐싱
```

| 장점 | 단점 |
|------|------|
| 필요한 데이터만 선택적 캐싱 | 최초 요청 (Cold Start) 느림 |
| DB 장애 시 캐시로 일부 서비스 가능 | 캐시-DB 일시적 불일치 가능 |
| 캐시 미스 시 자동으로 데이터 채워짐 | Stampede 위험 |

참고 자료:
- [Redis 공식 문서 - Cache Aside](https://redis.io/docs/latest/develop/use-cases/cache-aside/)

---

## [COMMON-139] Redis와 같은 In-memory DB를 사용하는 이유를 설명해 주세요.

답변:
In-memory DB는 데이터를 주로 메모리에 저장하므로 디스크 기반 저장소보다 읽기, 쓰기 성능이 수십~수백 배 빠릅니다. Redis는 단순 Key-Value 외에도 String, Hash, List, Set, Sorted Set 등 다양한 자료구조와 TTL, 원자적 연산을 지원하여 캐시, 세션, 분산 락, Rate Limiting, 실시간 순위표 등 다양한 용도로 활용됩니다. 단, 메모리 비용이 높고 용량이 제한적이므로 TTL과 Eviction 정책 설정이 필요하고, 중요 데이터는 RDB Snapshot이나 AOF 등의 영속성 설정을 함께 고려해야 합니다.

추가 설명)
| Redis 자료구조 | 활용 사례 |
|--------------|----------|
| String | 캐시, 카운터, 세션 토큰 |
| Hash | 사용자 프로필, 설정 |
| List | 큐, 타임라인, 최근 방문 목록 |
| Set | 중복 없는 태그, 좋아요 집합 |
| Sorted Set | 실시간 순위표 (leaderboard) |
| TTL | 인증 코드, 임시 토큰 |

```
디스크 기반 DB: 메모리 → 디스크 I/O → 데이터 반환 (ms 단위)
In-memory DB:  메모리에서 바로 반환 (μs 단위)
```

참고 자료:
- [Redis 공식 문서 - What is Redis?](https://redis.io/tutorials/what-is-redis/)

---

## [COMMON-140] SQL Injection이 무엇이고, 어떻게 방어할 수 있는지 설명해 주세요.

답변:
SQL Injection은 사용자 입력값을 SQL 문자열에 직접 연결했을 때 공격자가 SQL 구문을 삽입하여 쿼리 의도를 변경하는 공격입니다. 인증 우회, 데이터 조회, 수정, 삭제 등이 가능하며 가장 기본적인 웹 보안 위협 중 하나입니다. Prepared Statement와 Parameterized Query를 사용하는 것이 가장 효과적인 방어 방법입니다.

추가 설명)
```
[SQL Injection 공격 예시]
입력값: ' OR '1'='1

취약한 쿼리:
SELECT * FROM users WHERE id='' OR '1'='1'
→ 항상 true이므로 모든 사용자 데이터 반환

[Prepared Statement로 방어]
SELECT * FROM users WHERE id = ?
→ 입력값을 파라미터로 바인딩, SQL 구문으로 해석되지 않음
```

| 방어 방법 | 설명 |
|-----------|------|
| Prepared Statement | 쿼리 구조와 데이터 분리 (가장 효과적) |
| Parameterized Query | 입력값을 파라미터로 바인딩 |
| ORM 사용 | 안전한 바인딩 방식 사용 |
| Allow List 검증 | 동적 테이블명, 컬럼명은 허용 목록에서만 선택 |
| 최소 권한 원칙 | DB 계정에 필요한 권한만 부여 |
| 입력값 검증 | 길이, 형식 제한 (단, 단독 방어 수단으론 부족) |

참고 자료:
- [OWASP - SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

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
