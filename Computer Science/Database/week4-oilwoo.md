# Week 04 - Database / Storage / Cache

---

## 제출 기준

- 필수 답변: COMMON-121 ~ COMMON-140
- 선택 답변: COMMON-141 ~ COMMON-160

---

## 필수 질문

## [COMMON-121] RDB와 NoSQL의 차이에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **RDB는 관계가 있는 데이터를 정해진 테이블 형태로 저장하고, NoSQL은 용도에 따라 문서·Key-Value·그래프 등 다양한 형태로 저장하는 데이터베이스**입니다.

| 구분 | RDB | NoSQL |
| --- | --- | --- |
| 데이터 모델 | 행과 열로 구성된 테이블 | Document, Key-Value, Wide-Column, Graph 등 |
| 스키마 | 미리 정의된 스키마 중심 | 비교적 유연한 스키마 |
| 관계 표현 | Foreign Key와 Join 사용 | 중첩 데이터나 애플리케이션 로직 등 제품별 방식 사용 |
| 강점 | 복잡한 관계, 데이터 무결성, 트랜잭션 처리 | 대규모 분산 처리, 유연한 데이터 구조, 특정 조회 패턴 |

- RDB는 주문과 결제처럼 여러 데이터의 관계와 정합성이 중요하고 복잡한 조회가 필요한 경우에 적합합니다.
- NoSQL은 형식이 자주 바뀌는 상품 정보, 대용량 로그, 세션처럼 특정 형태의 데이터를 여러 서버에서 빠르게 처리해야 할 때 고려할 수 있습니다.
- 다만 모든 NoSQL이 트랜잭션이나 강한 일관성을 지원하지 않는 것은 아니며, 현대 RDB도 수평 확장이 가능합니다. 따라서 이름만 보고 선택하기보다 데이터 구조, 조회 방식, 일관성 요구사항을 기준으로 판단해야 합니다.

참고 자료:
- https://aws.amazon.com/ko/compare/the-difference-between-relational-and-non-relational-databases/
- https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/bp-general-nosql-design.html

---

## [COMMON-122] Primary Key, Candidate Key, Foreign Key, Unique Key의 차이에 대해 설명해 주세요.

답변:
- Key는 테이블의 행을 식별하거나 테이블 사이의 관계와 데이터 무결성을 보장하기 위해 사용합니다.

| 종류 | 설명 |
| --- | --- |
| Candidate Key | 행을 유일하게 식별하면서 불필요한 컬럼이 없는 최소한의 Key이며, 한 테이블에 여러 개 존재할 수 있음 |
| Primary Key | Candidate Key 중 대표로 선택한 Key이며, 중복과 `NULL`을 허용하지 않고 테이블당 하나만 지정 가능 |
| Foreign Key | 다른 테이블의 Primary Key나 Unique Key 등을 참조하여 참조 무결성을 보장하는 Key |
| Unique Key | 지정한 컬럼 또는 컬럼 조합의 값이 중복되지 않도록 하는 제약조건이며, 한 테이블에 여러 개 지정 가능 |

- Primary Key는 제약조건 하나가 여러 컬럼으로 구성된 복합키일 수 있습니다.
- Foreign Key 자체에는 중복 값이 들어갈 수 있으며, 별도로 `NOT NULL`을 지정하지 않았다면 일반적으로 `NULL`도 사용할 수 있습니다.
- Unique Key의 `NULL` 허용 개수와 비교 방식은 DBMS마다 다르므로 실제 사용하는 DBMS의 동작을 확인해야 합니다.
- 예를 들어 회원 테이블에서 `id`와 `email`이 모두 회원을 식별할 수 있다면 둘 다 Candidate Key가 될 수 있습니다. `id`를 Primary Key로 선택하고 `email`에는 `UNIQUE NOT NULL`을 적용할 수 있으며, 주문 테이블의 `member_id`는 회원의 `id`를 참조하는 Foreign Key가 됩니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/relational-databases/tables/unique-constraints-and-check-constraints
- https://learn.microsoft.com/ko-kr/sql/relational-databases/tables/create-foreign-key-relationships
- https://www.postgresql.org/docs/current/ddl-constraints.html

---

## [COMMON-123] 정규화가 무엇이고, 정규화를 하지 않으면 어떤 이상 현상이 발생하는지 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **정규화는 데이터의 종속 관계를 기준으로 테이블을 나누어 중복을 줄이고 일관성을 높이는 과정**입니다.
- 대표적인 정규형은 다음과 같습니다.
  - **제1정규형(1NF)**: 각 컬럼이 하나의 원자값을 가지도록 합니다.
  - **제2정규형(2NF)**: 복합 Candidate Key의 일부에만 의존하는 부분 함수 종속을 제거합니다.
  - **제3정규형(3NF)**: Key가 아닌 컬럼 사이의 이행 함수 종속을 제거합니다.
- 정규화하지 않아 같은 정보가 여러 행에 반복되면 다음과 같은 이상 현상이 발생할 수 있습니다.
  - **삽입 이상**: 필요한 다른 정보가 없어서 새로운 정보를 저장하지 못하는 현상
  - **갱신 이상**: 중복된 값 중 일부만 수정되어 서로 다른 값이 남는 현상
  - **삭제 이상**: 한 행을 삭제하면서 유지해야 할 다른 정보까지 사라지는 현상
- 예를 들어 `사원번호, 사원이름, 부서번호, 부서이름`을 한 테이블에 저장하면 부서 이름이 여러 번 중복됩니다. 일부 행의 부서 이름만 바꾸면 갱신 이상이 발생하고, 해당 부서의 마지막 사원을 삭제하면 부서 정보까지 사라질 수 있습니다.
- 이를 사원 테이블과 부서 테이블로 분리하고 부서번호로 연결하면 부서 정보를 한 곳에서 관리할 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/office/troubleshoot/access/database-normalization-description
- https://mangkyu.tistory.com/110

---

## [COMMON-124] 반정규화가 필요한 상황은 언제인지 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **반정규화는 조회 성능을 높이기 위해 중복 데이터나 미리 계산한 값을 의도적으로 저장하는 방법**입니다.
- 다음과 같은 상황에서 고려할 수 있습니다.
  - 읽기 요청이 쓰기 요청보다 훨씬 많고 동일한 Join이나 집계가 반복되는 경우
  - 여러 대용량 테이블의 Join 때문에 실제 성능 저하가 확인된 경우
  - 보고서나 통계처럼 계산 비용이 큰 결과를 자주 조회하는 경우
  - Join을 지원하지 않거나 최소화해야 하는 분산 NoSQL 환경인 경우
- 예를 들어 요청할 때마다 수백만 건의 주문을 합산하는 대신 `일자, 총주문수, 총매출액`을 저장한 일별 매출 요약 테이블을 만들 수 있습니다.
- 조회는 빨라질 수 있지만 데이터 중복, 저장 공간과 쓰기 비용 증가, 원본과 사본의 불일치가 발생할 수 있습니다. 따라서 기준 데이터인 Source of Truth와 동기화 방법을 명확히 정해야 합니다.
- 먼저 인덱스, 실행 계획, 쿼리 개선 등을 검토하고 실제 측정으로 병목이 확인됐을 때 적용하는 것이 좋으며, Cache나 Materialized View도 대안이 될 수 있습니다.

참고 자료:
- https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/bp-relational-modeling.html
- https://learn.microsoft.com/ko-kr/azure/architecture/data-guide/relational-data/online-analytical-processing

---

## [COMMON-125] Join이 무엇이고, Inner Join과 Outer Join의 차이를 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Join은 두 개 이상의 테이블을 공통된 컬럼이나 조건으로 연결해 하나의 결과로 조회하는 연산**입니다.

| 종류 | 반환하는 결과 |
| --- | --- |
| Inner Join | 양쪽 테이블에서 조인 조건이 일치하는 행만 반환 |
| Left Outer Join | 왼쪽의 모든 행과 오른쪽에서 일치하는 행을 반환하며, 일치하지 않는 오른쪽 값은 `NULL` |
| Right Outer Join | 오른쪽의 모든 행과 왼쪽에서 일치하는 행을 반환하며, 일치하지 않는 왼쪽 값은 `NULL` |
| Full Outer Join | 양쪽의 모든 행을 반환하며, 상대 테이블과 일치하지 않는 값은 `NULL` |

- 예를 들어 회원과 주문을 `member_id`로 Inner Join하면 주문한 회원만 조회됩니다. Left Outer Join하면 주문하지 않은 회원도 결과에 포함되고 주문 정보는 `NULL`로 표시됩니다.
- `LEFT JOIN`을 사용해도 `WHERE orders.status = 'PAID'`처럼 오른쪽 테이블의 조건을 `WHERE`에 지정하면 `NULL`인 행이 제거되어 사실상 Inner Join처럼 동작할 수 있습니다. 주문하지 않은 회원도 유지하려면 목적에 따라 해당 조건을 `ON` 절에 작성해야 합니다.
- `FULL OUTER JOIN`처럼 지원 여부와 문법이 DBMS마다 다른 기능도 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/relational-databases/performance/joins
- https://learn.microsoft.com/ko-kr/training/modules/query-multiple-tables-with-joins/

---

## [COMMON-126] 인덱스가 무엇이고, 왜 조회 성능을 향상시키는지 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **인덱스는 특정 컬럼의 값과 데이터 위치를 별도로 정렬해 저장하여 원하는 행을 빠르게 찾게 해 주는 자료구조**입니다. 책의 색인과 비슷합니다.
- 인덱스가 없으면 조건에 맞는 데이터를 찾기 위해 테이블의 많은 행이나 페이지를 확인하는 Full Scan이 필요할 수 있습니다.
- B-Tree 계열 인덱스는 정렬된 Key를 따라 루트에서 리프 노드까지 탐색하므로 전체 데이터를 읽지 않고 비교적 적은 페이지 접근으로 원하는 위치를 찾을 수 있습니다.
- 정렬된 구조이므로 `=`, `<`, `>`, `BETWEEN` 등의 조건뿐 아니라 범위 조회, 정렬, Join에도 활용될 수 있습니다.
- 다만 인덱스는 저장 공간을 사용하고 `INSERT`, `UPDATE`, `DELETE` 시 함께 갱신해야 하므로 쓰기 비용이 증가합니다. 데이터가 적거나 조회 결과가 테이블의 대부분이면 Full Scan이 더 효율적일 수도 있습니다.
- 예를 들어 회원 백만 명 중 특정 이메일을 찾을 때 `email` 인덱스가 있다면 모든 회원을 확인하지 않고 정렬된 경로를 따라 해당 이메일의 위치를 찾을 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/relational-databases/indexes/indexes
- https://dev.mysql.com/doc/refman/8.4/en/optimization-indexes.html
- https://d2.naver.com/helloworld/1155

---

## [COMMON-127] B-Tree와 B+Tree에 대해 설명하고, DB 인덱스에서 B+Tree가 많이 사용되는 이유를 설명해 주세요.

답변:
- B-Tree와 B+Tree는 하나의 노드가 여러 Key와 자식을 가지며, 루트에서 각 리프 노드까지의 높이가 같은 **균형 다진 탐색 트리**입니다.

| 구분 | B-Tree | B+Tree |
| --- | --- | --- |
| 데이터 위치 | 내부 노드와 리프 노드 모두에 데이터나 데이터 위치를 저장할 수 있음 | 실제 데이터나 데이터 위치는 리프 노드에 모으고 내부 노드는 경로 탐색용 Key 중심으로 구성 |
| 검색 종료 | 내부 노드에서 끝날 수 있음 | 일반적으로 리프 노드까지 이동 |
| 리프 연결 | 필수적인 특징이 아님 | 리프 노드가 순서대로 연결됨 |
| 범위 조회 | 추가적인 트리 탐색이 필요할 수 있음 | 시작 위치를 찾은 뒤 연결된 리프를 순서대로 읽기 유리 |

- B+Tree의 내부 노드는 경로 정보에 집중하므로 한 페이지에 더 많은 Key와 자식 포인터를 저장할 수 있습니다. 따라서 Fan-out이 커지고 트리 높이가 낮아져 디스크 페이지 접근 횟수를 줄일 수 있습니다.
- 리프 노드가 정렬된 순서로 연결되어 있어 단건 조회뿐 아니라 `BETWEEN`, 부등호, 정렬 같은 범위 조회에도 유리합니다.
- 예를 들어 가격이 1만 원 이상 2만 원 이하인 상품을 찾을 때 시작 가격의 리프 노드를 찾은 뒤 연결된 리프를 순서대로 읽을 수 있습니다.
- 실제 DBMS의 인덱스는 교과서적인 B+Tree와 완전히 같지 않을 수 있고, 제품 문서에서 B-Tree라고 부르기도 하므로 세부 구조는 DBMS와 저장 엔진 문서를 확인해야 합니다.

참고 자료:
- https://www.ibm.com/docs/ko/db2/11.5.x?topic=indexes-index-structure
- https://learn.microsoft.com/ko-kr/sql/relational-databases/indexes/indexes
- https://haon.blog/database/index-basic/

---

## [COMMON-128] 복합 인덱스에서 컬럼 순서가 중요한 이유를 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **복합 인덱스는 첫 번째 컬럼부터 차례대로 정렬되므로 실제 조회 조건에 맞는 컬럼 순서로 만들어야 검색 범위를 효율적으로 줄일 수 있습니다.**
- B-Tree 복합 인덱스 `(A, B, C)`는 먼저 `A`로 정렬하고, `A`가 같은 데이터 안에서 `B`, `B`까지 같으면 `C`로 정렬합니다.
- 따라서 일반적으로 `(A)`, `(A, B)`, `(A, B, C)`처럼 선두 컬럼부터 조건이 주어질 때 효율적으로 사용할 수 있습니다. 이를 왼쪽 접두사 규칙이라고 합니다.
- 앞쪽 컬럼에 범위 조건이 사용되면 그 오른쪽 컬럼은 탐색 범위를 줄이는 데 제한적으로 사용되고 필터링이나 Covering 용도로만 활용될 수 있습니다.
- 컬럼 순서는 단순히 카디널리티가 높은 컬럼부터 정하기보다 실제 `WHERE`, `JOIN`, `ORDER BY`, 동등·범위 조건의 조합을 기준으로 결정하고 실행 계획으로 확인해야 합니다.
- 예를 들어 `(tenant_id, created_at)` 인덱스는 특정 사용자의 최근 게시물을 조회하는 `WHERE tenant_id = ? AND created_at >= ?` 조건에 유리합니다. 반면 `created_at`만 조건으로 사용하면 선두 컬럼이 빠져 비효율적일 수 있습니다.
- 왼쪽 접두사 규칙은 일반적인 B-Tree 기준이며, Skip Scan처럼 DBMS와 버전에 따라 예외적인 최적화도 존재합니다.

참고 자료:
- https://postgresql.kr/docs/current/indexes-multicolumn.html
- https://dev.mysql.com/doc/refman/8.4/en/multiple-column-indexes.html
- https://d2.naver.com/helloworld/1155

---

## [COMMON-129] 인덱스를 사용했는데도 Table Full Scan이 발생할 수 있는 이유를 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **인덱스가 있어도 옵티마이저가 전체 테이블을 읽는 비용이 더 작다고 판단하면 Table Full Scan을 선택할 수 있습니다.**
- 인덱스는 항상 강제로 사용되는 것이 아니라 통계 정보를 바탕으로 각 실행 방법의 예상 비용을 비교해 선택됩니다.
- 다음과 같은 경우 Full Scan이 선택되거나 인덱스를 효율적으로 사용하지 못할 수 있습니다.
  - 테이블이 작거나 조건에 해당하는 데이터가 테이블의 대부분인 경우
  - 복합 인덱스의 선두 컬럼을 조건에서 사용하지 않은 경우
  - 인덱스 컬럼에 함수, 연산, 형 변환을 적용한 경우
  - `LIKE '%keyword'`처럼 앞에 와일드카드를 사용한 경우
  - 통계 정보가 오래됐거나 실제 데이터가 한쪽으로 치우친 경우
- 예를 들어 전체 회원 중 95%가 `active = true`라면 인덱스를 읽고 다시 테이블의 행을 찾는 랜덤 접근보다 테이블 전체를 순차적으로 읽는 편이 저렴할 수 있습니다.
- 원인을 확인할 때는 `EXPLAIN` 또는 `EXPLAIN ANALYZE`로 실행 계획과 예상·실제 행 수를 비교하고, 통계 갱신과 쿼리·인덱스 구조를 함께 점검해야 합니다.

참고 자료:
- https://postgresql.kr/docs/current/using-explain.html
- https://dev.mysql.com/doc/refman/8.4/en/explain.html
- https://d2.naver.com/helloworld/1155

---

## [COMMON-130] 트랜잭션이 무엇이고, ACID에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **트랜잭션은 여러 데이터베이스 작업을 하나의 논리적 단위로 묶어 모두 성공시키거나 모두 취소하는 기능**입니다.
- 정상적으로 완료되면 `COMMIT`하여 변경을 확정하고, 일부 작업이 실패하면 `ROLLBACK`하여 이전 상태로 되돌립니다.
- 트랜잭션이 신뢰성 있게 처리되기 위해 만족해야 하는 네 가지 특성을 ACID라고 합니다.
  - **Atomicity(원자성)**: 작업은 모두 성공하거나 모두 실패해야 합니다.
  - **Consistency(일관성)**: 트랜잭션 전후에 제약조건과 비즈니스 규칙을 만족하는 유효한 상태가 유지되어야 합니다.
  - **Isolation(격리성)**: 동시에 실행되는 트랜잭션들이 서로의 중간 상태에 부적절하게 영향을 주지 않아야 합니다.
  - **Durability(지속성)**: 커밋된 결과는 장애가 발생하더라도 보존되어야 합니다.
- 일관성은 DB가 잘못된 비즈니스 로직까지 자동으로 고쳐 준다는 뜻은 아닙니다. 필요한 제약조건과 로직을 올바르게 구현해야 합니다.
- 예를 들어 A 계좌에서 1만 원을 차감하고 B 계좌에 1만 원을 더하는 계좌 이체는 하나의 트랜잭션으로 처리해야 합니다. 입금에 실패하면 출금도 취소되어야 하며, 커밋했다면 서버가 재시작되어도 결과가 남아야 합니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/windows/win32/cossdk/acid-properties
- https://aws.amazon.com/ko/compare/the-difference-between-acid-and-base-database/
- https://dev.mysql.com/doc/refman/8.4/en/mysql-acid.html

---

## [COMMON-131] 트랜잭션 격리 수준에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **트랜잭션 격리 수준은 동시에 실행되는 트랜잭션이 서로의 변경을 어느 정도까지 볼 수 있는지 정하는 기준**입니다.
- SQL 표준에서 설명하는 네 단계는 다음과 같습니다.

| 격리 수준 | Dirty Read | Non-repeatable Read | Phantom Read |
| --- | --- | --- | --- |
| Read Uncommitted | 가능 | 가능 | 가능 |
| Read Committed | 방지 | 가능 | 가능 |
| Repeatable Read | 방지 | 방지 | 가능 |
| Serializable | 방지 | 방지 | 방지 |

- **Read Uncommitted**는 커밋되지 않은 변경도 읽을 수 있어 정합성 문제가 가장 많이 발생할 수 있습니다.
- **Read Committed**는 커밋된 데이터만 읽지만, 같은 트랜잭션 안에서도 다시 조회하면 다른 트랜잭션이 커밋한 값이 보일 수 있습니다.
- **Repeatable Read**는 같은 행을 반복해서 조회할 때 동일한 결과를 보장하고, **Serializable**은 트랜잭션들을 순차적으로 실행한 것과 같은 결과를 보장합니다.
- 격리 수준이 높아질수록 정합성은 강화되지만 잠금 대기나 트랜잭션 실패·재시도 등이 늘어 처리량이 낮아질 수 있습니다.
- 표는 SQL 표준의 일반적인 설명이며 DBMS 구현에 따라 다릅니다. PostgreSQL은 Read Uncommitted를 Read Committed처럼 처리하고 Repeatable Read에서도 Phantom Read를 방지합니다. 기본 격리 수준도 MySQL InnoDB는 Repeatable Read, PostgreSQL은 Read Committed입니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/odbc/reference/develop-app/transaction-isolation-levels
- https://postgresql.kr/docs/current/transaction-iso.html
- https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html

---

## [COMMON-132] Dirty Read, Non-repeatable Read, Phantom Read에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Dirty Read는 커밋되지 않은 값을 읽는 문제, Non-repeatable Read는 같은 행의 값이 달라지는 문제, Phantom Read는 같은 조건으로 조회한 행의 집합이 달라지는 문제**입니다.
- **Dirty Read**
  - 트랜잭션 A가 변경했지만 아직 커밋하지 않은 데이터를 트랜잭션 B가 읽는 현상입니다.
  - 이후 A가 Rollback하면 B는 확정되지 않은 값을 사용한 셈이 됩니다.
- **Non-repeatable Read**
  - A가 한 행을 읽은 뒤 B가 해당 행을 수정하거나 삭제하고 Commit하여, A가 같은 행을 다시 읽었을 때 값이 달라지는 현상입니다.
- **Phantom Read**
  - A가 특정 조건의 행들을 조회한 뒤 B가 조건에 해당하는 행을 추가·변경·삭제하고 Commit하여, A가 같은 조건으로 다시 조회했을 때 행의 집합이 달라지는 현상입니다.
- 예를 들어 A가 `status = 'READY'`인 주문 5건을 조회한 뒤 B가 해당 조건의 주문을 추가하고 Commit하여 A의 두 번째 조회에서 6건이 반환되면 Phantom Read입니다.
- Non-repeatable Read는 주로 **같은 행의 값 변화**, Phantom Read는 **조건에 해당하는 행 집합의 변화**라는 차이가 있습니다. 실제 발생 여부는 DBMS의 MVCC와 잠금 구현에 따라 달라질 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/odbc/reference/develop-app/transaction-isolation-levels
- https://postgresql.kr/docs/current/transaction-iso.html

---

## [COMMON-133] DB Lock이 무엇이고, Shared Lock과 Exclusive Lock의 차이에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **DB Lock은 여러 트랜잭션이 같은 데이터에 동시에 접근할 때 충돌을 조절하여 데이터의 일관성을 지키는 장치**입니다.
- **Shared Lock(S Lock, 공유 잠금)**은 데이터를 읽기 위한 잠금입니다. 같은 데이터에 여러 Shared Lock이 함께 존재할 수 있지만, 변경을 위한 Exclusive Lock과는 충돌합니다.
- **Exclusive Lock(X Lock, 배타 잠금)**은 데이터를 수정하거나 삭제하기 위한 잠금입니다. 다른 트랜잭션의 Shared Lock과 Exclusive Lock 요청 모두와 충돌합니다.

| 현재 잠금 \ 요청 잠금 | Shared Lock | Exclusive Lock |
| --- | --- | --- |
| Shared Lock | 허용 | 대기 |
| Exclusive Lock | 대기 | 대기 |

- 예를 들어 A와 B가 같은 상품 행에 Shared Lock을 걸고 읽는 것은 가능하지만, C가 수정하기 위해 Exclusive Lock을 요청하면 기존 잠금이 해제될 때까지 기다려야 합니다.
- Lock의 범위와 유지 기간은 행·페이지·테이블, 격리 수준, DBMS에 따라 다릅니다. 특히 MVCC를 사용하는 DBMS에서는 일반 조회가 과거 버전을 읽어 Exclusive Lock과 직접 충돌하지 않을 수도 있습니다.
- 잠금을 오래 유지하면 대기 시간과 Deadlock 가능성이 증가하므로 트랜잭션 범위를 작게 유지해야 합니다.

참고 자료:
- https://dev.mysql.com/doc/refman/8.4/en/innodb-locking.html
- https://learn.microsoft.com/ko-kr/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver17

---

## [COMMON-134] Optimistic Lock과 Pessimistic Lock의 차이에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Optimistic Lock은 충돌이 적다고 보고 작업 후 변경 여부를 검사하고, Pessimistic Lock은 충돌이 발생한다고 보고 작업 전에 데이터를 잠그는 방식**입니다.
- **Optimistic Lock(낙관적 락)**
  - 데이터를 읽을 때 DB Lock을 오래 점유하지 않고, 수정 시 Version이나 Timestamp가 처음 읽은 값과 같은지 확인합니다.
  - 값이 달라졌다면 다른 트랜잭션이 먼저 수정한 것이므로 현재 수정을 실패시키고 재조회나 재시도를 수행합니다.
  - 실제 DB 잠금이라기보다 애플리케이션의 조건부 `UPDATE`를 이용한 동시성 제어 방식인 경우가 많습니다.
- **Pessimistic Lock(비관적 락)**
  - `SELECT ... FOR UPDATE` 등으로 먼저 DB Lock을 획득하고, 트랜잭션이 끝날 때까지 충돌하는 다른 작업을 대기시킵니다.
  - 충돌이 잦은 상황에 유리하지만 잠금 대기와 Deadlock 때문에 처리량이 감소할 수 있습니다.
- 예를 들어 재고가 1개이고 Version이 3일 때 두 요청이 동시에 읽었다면, Optimistic Lock은 `WHERE id = 1 AND version = 3` 조건의 수정 중 하나만 성공시킵니다. Pessimistic Lock은 첫 요청이 재고 행을 잠근 동안 두 번째 요청을 기다리게 합니다.
- 충돌이 드물고 읽기가 많은 환경에서는 Optimistic Lock, 재고 차감처럼 충돌 가능성이 높고 순차 처리가 중요한 환경에서는 Pessimistic Lock을 고려할 수 있지만 재시도 비용과 잠금 유지 시간을 함께 비교해야 합니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/dotnet/framework/data/adonet/optimistic-concurrency
- https://techblog.woowahan.com/10795/

---

## [COMMON-135] DB Deadlock이 발생하는 상황과 해결 방법에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **DB Deadlock은 여러 트랜잭션이 서로가 가진 Lock이 해제되기만 기다려 아무도 진행하지 못하는 상태**입니다.
- 예를 들어 A는 회원 1의 행을 잠근 뒤 회원 2의 행을 기다리고, B는 회원 2를 잠근 뒤 회원 1을 기다리면 순환 대기가 만들어집니다.
- DBMS는 보통 이런 순환 관계를 감지하면 하나의 트랜잭션을 Victim으로 선택해 Rollback하고 나머지가 진행되게 합니다. 단순히 Lock을 오래 기다리는 Blocking과 순환 대기인 Deadlock은 구분해야 합니다.
- Deadlock을 줄이는 방법은 다음과 같습니다.
  - 모든 트랜잭션이 테이블과 행에 접근하는 순서를 동일하게 정합니다.
  - 트랜잭션 범위를 작게 하고 외부 API 호출처럼 오래 걸리는 작업을 포함하지 않습니다.
  - 적절한 인덱스로 불필요하게 넓은 범위가 잠기지 않도록 합니다.
  - Deadlock으로 실패한 트랜잭션은 전체를 Rollback한 뒤 짧은 지연을 두고 재시도합니다.
- 예를 들어 계좌 이체 시 항상 더 작은 계좌번호의 행부터 잠그도록 규칙을 정하면 서로 반대 순서로 잠그는 상황을 줄일 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/relational-databases/sql-server-deadlocks-guide?view=sql-server-ver17
- https://dev.mysql.com/doc/refman/8.4/en/innodb-deadlocks.html

---

## [COMMON-136] Connection Pool이 무엇이고, 왜 필요한지 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Connection Pool은 미리 생성하거나 사용이 끝난 DB Connection을 보관했다가 재사용하는 공간**입니다.
- DB Connection을 새로 만들 때마다 네트워크 연결, Handshake, 인증 등의 비용이 발생합니다. 요청마다 연결을 만들고 끊는 대신 Pool에서 빌리고 반납하면 응답 시간과 서버 자원 사용을 줄일 수 있습니다.
- 애플리케이션은 Pool에서 Connection을 빌려 쿼리를 실행하고, `close()`를 호출해 물리적으로 끊기보다 Pool에 반납합니다.
- 최대 Connection 수를 제한하여 애플리케이션의 요청이 DB가 감당할 수 있는 수를 넘지 않도록 보호하는 역할도 합니다.
- Pool이 너무 작으면 요청이 Connection을 기다리고, 너무 크면 DB의 메모리와 Thread가 과도하게 사용될 수 있습니다. DB의 최대 연결 수, 애플리케이션 인스턴스 수, 쿼리 시간, 동시 요청 수와 Timeout을 함께 고려해 조정해야 합니다.
- 예를 들어 Pool 크기가 10이면 동시에 10개의 요청이 DB 작업을 수행하고, 추가 요청은 Connection이 반납될 때까지 정해진 시간 동안 기다립니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/connect/ado-net/sql-server-connection-pooling?view=sql-server-ver17
- https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing
- https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/rds-proxy.html

---

## [COMMON-137] Cache를 사용하는 이유와 주의해야 할 점을 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Cache는 자주 사용하는 데이터를 더 빠르고 가까운 저장소에 보관해 응답 시간과 원본 DB의 부하를 줄이는 기술**입니다.
- 매번 DB나 외부 API에서 데이터를 가져오지 않아도 되므로 읽기 성능과 처리량을 높이고 비용을 줄일 수 있습니다.
- 사용할 때는 다음 사항을 주의해야 합니다.
  - 원본이 변경됐는데 Cache가 갱신되지 않으면 오래된 데이터인 Stale Data를 반환할 수 있으므로 TTL과 무효화 정책이 필요합니다.
  - 메모리는 한정되어 있으므로 어떤 데이터를 저장할지와 LRU·LFU 같은 Eviction 정책을 정해야 합니다.
  - 인기 Key가 만료되는 순간 많은 요청이 DB로 몰리는 Cache Stampede가 발생할 수 있어 Lock, Single-Flight, 사전 갱신, TTL 분산 등을 고려해야 합니다.
  - Cache 장애가 곧바로 DB 과부하로 이어질 수 있으므로 Rate Limiting, Circuit Breaker, Warm-up 같은 대응도 필요합니다.
- 예를 들어 상품 목록은 Cache로 빠르게 제공하되, 가격이 변경되면 관련 Key를 삭제하고 짧은 TTL을 함께 사용하여 오래된 가격이 장시간 노출되지 않게 할 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://aws.amazon.com/ko/blogs/tech/chaos-experiment-for-amazon-elasticache-workload/

---

## [COMMON-138] Cache Aside 패턴에 대해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Cache Aside는 애플리케이션이 Cache를 먼저 조회하고, 값이 없을 때만 DB에서 읽어 Cache에 채우는 패턴**입니다.
- 조회 과정은 다음과 같습니다.
  1. 애플리케이션이 Cache에서 Key를 조회합니다.
  2. 값이 있으면 Cache의 값을 반환합니다.
  3. 값이 없으면 DB에서 조회합니다.
  4. DB의 결과를 Cache에 저장한 뒤 반환합니다.
- 필요한 데이터만 요청될 때 Cache에 저장하므로 Lazy Loading이라고도 하며, Cache 장애나 Miss가 발생해도 DB에서 조회할 수 있습니다.
- 쓰기 작업에서는 보통 DB를 먼저 변경한 뒤 관련 Cache Key를 삭제하여 다음 조회가 최신 값을 다시 채우게 합니다.
- DB 변경과 Cache 무효화가 하나의 원자적 작업은 아니므로 두 작업 사이의 실패나 동시 요청 때문에 잠시 오래된 값이 보일 수 있습니다. TTL, 재시도, Version, 변경 이벤트 등을 함께 사용할 수 있습니다.
- 예를 들어 `product:1`이 Cache에 없으면 상품 1을 DB에서 조회해 저장하고, 상품 가격이 수정되면 DB Commit 후 `product:1`을 삭제합니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/azure/architecture/patterns/cache-aside
- https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html
- https://redis.io/docs/latest/develop/use-cases/cache-aside/

---

## [COMMON-139] Redis와 같은 In-memory DB를 사용하는 이유를 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **Redis 같은 In-memory DB는 데이터를 메모리에서 처리하여 매우 빠른 응답과 높은 처리량이 필요할 때 사용**합니다.
- Redis는 Key-Value 구조를 기반으로 String, Hash, List, Set, Sorted Set, Stream 같은 자료구조와 원자적 연산을 제공합니다.
- 여러 애플리케이션 인스턴스가 공통으로 접근할 수 있어 Cache, Session, 인증번호, 실시간 순위, 요청 횟수 제한, 메시지 처리 등에 활용할 수 있습니다.
- 예를 들어 사용자의 로그인 Session을 Redis에 저장하면 어느 애플리케이션 서버가 요청을 받더라도 같은 Session을 확인할 수 있습니다.
- 메모리는 디스크보다 비싸고 용량이 제한적이므로 최대 메모리와 Eviction 정책을 설정해야 합니다. 큰 Key나 만료되지 않는 Key가 쌓이지 않도록 모니터링도 필요합니다.
- Redis는 RDB Snapshot과 AOF 같은 영속화 기능을 제공하지만 설정과 장애 시점에 따라 최근 데이터가 유실될 수 있습니다. Cache인지 원본 저장소인지 역할을 정하고 복제, Backup, 복구 정책을 설계해야 합니다.

참고 자료:
- https://aws.amazon.com/ko/redis/
- https://redis.io/docs/latest/develop/data-types/
- https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/

---

## [COMMON-140] SQL Injection이 무엇이고, 어떻게 방어할 수 있는지 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **SQL Injection은 공격자가 입력값을 이용해 개발자가 의도한 SQL의 구조를 바꾸고 데이터를 조회·변경하게 만드는 공격**입니다.
- 예를 들어 로그인 SQL을 문자열로 이어 붙일 때 비밀번호 입력에 `' OR 1=1 --` 같은 값이 들어가면 조건을 항상 참으로 바꾸거나 뒤의 SQL을 주석 처리할 수 있습니다.
- 가장 중요한 방어 방법은 Prepared Statement와 Parameterized Query를 사용하여 SQL 문장과 사용자 입력값을 분리하는 것입니다. 그러면 입력은 SQL 명령이 아니라 하나의 값으로 처리됩니다.

```java
PreparedStatement statement = connection.prepareStatement(
    "SELECT id FROM member WHERE email = ? AND password_hash = ?"
);
statement.setString(1, email);
statement.setString(2, passwordHash);
```

- Parameter는 일반적으로 값에만 사용할 수 있으므로 테이블명, 컬럼명, 정렬 방향처럼 SQL 구조를 동적으로 정해야 한다면 허용 목록(Allowlist)에서만 선택해야 합니다.
- ORM이나 Stored Procedure를 사용해도 내부에서 문자열을 이어 붙이면 안전하지 않으며, 특수문자 제거와 Escape만으로 방어하려 해서도 안 됩니다.
- 입력값 검증, DB 계정의 최소 권한, 일반화된 오류 응답, 공격 로그와 모니터링을 함께 적용하면 피해 범위를 줄일 수 있습니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/sql/relational-databases/security/sql-injection?view=sql-server-ver17
- https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
- https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html

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
