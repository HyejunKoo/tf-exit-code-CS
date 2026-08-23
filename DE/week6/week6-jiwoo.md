# Week 06 - Data Engineer Role-Based Interview 1

---

## 제출 기준

- 필수 답변: ROLE-001 ~ ROLE-020
- 선택 답변: ROLE-021 ~ ROLE-040

---

## 필수 질문

## [ROLE-001] 데이터 엔지니어의 역할이 무엇이고, 백엔드 개발자나 데이터 분석가와 어떤 차이가 있는지 설명해 주세요.

답변:
데이터 엔지니어는 데이터가 안정적으로 수집, 처리, 저장, 제공될 수 있도록 파이프라인과 인프라를 설계하고 운영합니다. 백엔드 개발자는 서비스 비즈니스 로직과 API를 구현하는 데 집중하고, 데이터 분석가는 이미 정제된 데이터를 바탕으로 인사이트를 도출하는 역할을 합니다. 데이터 엔지니어는 분석가가 사용할 데이터를 공급하는 파이프라인을 만들고, 백엔드 시스템에서 발생한 이벤트를 수집하여 분석 가능한 형태로 변환하는 역할을 담당합니다.

추가 설명)
| 구분 | 데이터 엔지니어 | 백엔드 개발자 | 데이터 분석가 |
|------|----------------|--------------|--------------|
| 주요 역할 | 데이터 파이프라인 설계·운영 | 서비스 로직·API 구현 | 데이터 분석·인사이트 도출 |
| 주요 산출물 | ETL/ELT 파이프라인, 데이터 웨어하우스 | REST API, 비즈니스 로직 | 대시보드, 리포트, 분석 결과 |
| 주요 도구 | Kafka, Spark, Airflow, Hadoop | Spring, Node.js, Django | SQL, Python, Tableau, Power BI |
| 데이터 관점 | 데이터 이동·변환·저장 | 데이터 생성·조회·수정 | 데이터 탐색·통계·시각화 |

참고 자료:

---

## [ROLE-002] ETL과 ELT의 차이를 설명해 주세요.

답변:
ETL은 Extract, Transform, Load의 순서로 외부 소스에서 데이터를 추출한 뒤 별도의 변환 영역에서 정제한 후 목적지에 적재하는 방식입니다. ELT는 Extract, Load, Transform의 순서로 원시 데이터를 먼저 목적지에 적재한 후, 목적지의 연산 자원으로 변환하는 방식입니다. ETL은 온프레미스 환경이나 데이터 웨어하우스 용량이 제한적일 때 유리하고, ELT는 BigQuery·Snowflake처럼 클라우드 기반의 대용량 연산이 가능한 환경에서 적합합니다.

추가 설명)
```
[ETL]
Source → [Extract] → [Transform] → [Load] → Data Warehouse
                  (별도 변환 서버)

[ELT]
Source → [Extract] → [Load] → Data Lake/Warehouse → [Transform]
                              (목적지에서 직접 변환)
```

| 구분 | ETL | ELT |
|------|-----|-----|
| 변환 시점 | 적재 전 | 적재 후 |
| 변환 위치 | 별도 변환 서버 | 목적지 DB/플랫폼 |
| 적합 환경 | 온프레미스, 소규모 | 클라우드, 대용량 |
| 원시 데이터 보존 | 어려움 | 가능 |
| 대표 도구 | Informatica, SSIS | dbt, BigQuery, Snowflake |

참고 자료:
- [AWS - ETL과 ELT의 차이](https://aws.amazon.com/ko/compare/the-difference-between-etl-and-elt/)

---

## [ROLE-003] Batch Processing과 Stream Processing의 차이를 설명해 주세요.

답변:
Batch Processing은 일정 기간 동안 쌓인 데이터를 한꺼번에 처리하는 방식으로, 높은 처리량이 필요하고 실시간성이 중요하지 않은 작업에 적합합니다. Stream Processing은 데이터가 발생하는 즉시 연속적으로 처리하는 방식으로, 실시간 이벤트 감지나 즉각적인 응답이 필요한 경우에 사용합니다. 두 방식을 함께 사용하는 Lambda Architecture나, 스트림으로 통합하는 Kappa Architecture도 활용됩니다.

추가 설명)
| 구분 | Batch Processing | Stream Processing |
|------|-----------------|------------------|
| 처리 시점 | 주기적 (1시간, 1일 단위 등) | 이벤트 발생 즉시 |
| 지연 시간 | 높음 | 낮음 (밀리초~초) |
| 처리량 | 대용량 처리에 유리 | 상대적으로 낮음 |
| 복잡도 | 낮음 | 높음 |
| 대표 도구 | Spark Batch, Hadoop MapReduce | Kafka Streams, Flink, Spark Streaming |
| 사용 예시 | 일별 정산, 월별 리포트 | 실시간 알림, 이상 탐지, 로그 수집 |

참고 자료:
- [Apache Kafka 공식 문서 - Use Cases](https://kafka.apache.org/documentation/#uses)

---

## [ROLE-004] Data Warehouse, Data Lake, Data Mart의 차이를 설명해 주세요.

답변:
Data Warehouse는 정형 데이터를 스키마에 맞게 정제·저장하고 분석 쿼리에 최적화된 저장소로, 일관된 비즈니스 데이터를 제공합니다. Data Lake는 정형·반정형·비정형 데이터를 원시 상태 그대로 저장하는 대규모 저장소로, 다양한 분석과 ML에 활용됩니다. Data Mart는 특정 부서나 도메인에 맞게 Data Warehouse의 일부 데이터를 추출한 소규모 저장소입니다.

추가 설명)
| 구분 | Data Warehouse | Data Lake | Data Mart |
|------|---------------|-----------|-----------|
| 데이터 형태 | 정형 | 정형·반정형·비정형 | 정형 |
| 스키마 | Schema-on-write | Schema-on-read | Schema-on-write |
| 저장 데이터 | 정제된 데이터 | 원시 데이터 | 특정 도메인 데이터 |
| 주요 사용자 | 비즈니스 분석가 | 데이터 과학자, 엔지니어 | 특정 부서 사용자 |
| 대표 제품 | BigQuery, Redshift, Snowflake | S3, HDFS, Azure Data Lake | 부서별 Redshift 스키마 |

참고 자료:
- [AWS - 데이터 레이크란 무엇인가요?](https://aws.amazon.com/ko/what-is/data-lake/)

---

## [ROLE-005] 정형 데이터, 반정형 데이터, 비정형 데이터의 차이를 설명해 주세요.

답변:
정형 데이터는 RDB 테이블처럼 사전에 정의된 스키마와 형식에 맞게 저장된 데이터입니다. 반정형 데이터는 JSON, XML, CSV처럼 태그나 구분자 등 어느 정도 구조는 있지만 스키마가 고정되지 않은 데이터입니다. 비정형 데이터는 텍스트, 이미지, 음성, 동영상처럼 정해진 구조 없이 저장되는 데이터로, 처리하기 위해 별도의 전처리나 ML 기법이 필요합니다.

추가 설명)
| 구분 | 정형 데이터 | 반정형 데이터 | 비정형 데이터 |
|------|------------|--------------|--------------|
| 스키마 | 고정 (사전 정의) | 유연 (자기 기술적) | 없음 |
| 저장 형태 | RDB 테이블 | JSON, XML, CSV, Parquet | 이미지, 영상, 텍스트, 음성 |
| 처리 도구 | SQL | 파서, NoSQL | ML, NLP, CV |
| 검색·분석 용이성 | 높음 | 중간 | 낮음 |
| 비율 (전체 데이터) | 약 20% | 일부 | 약 80% |

참고 자료:

---

## [ROLE-006] OLTP와 OLAP의 차이를 설명해 주세요.

답변:
OLTP는 Online Transaction Processing으로, 주문·결제·회원가입 같은 짧고 빈번한 트랜잭션을 빠르게 처리하는 데 최적화된 시스템입니다. OLAP는 Online Analytical Processing으로, 대용량 데이터에 대한 집계·분석 쿼리를 빠르게 실행하기 위해 컬럼 기반 저장과 파티셔닝 등을 활용합니다. OLTP는 운영 DB에, OLAP는 Data Warehouse에 각각 적합합니다.

추가 설명)
| 구분 | OLTP | OLAP |
|------|------|------|
| 목적 | 트랜잭션 처리 | 데이터 분석 |
| 쿼리 특성 | 단순, 짧은 읽기·쓰기 | 복잡한 집계, 대용량 스캔 |
| 데이터 규모 | 수 GB | 수 TB~PB |
| 응답 속도 | 밀리초 단위 | 초~분 단위 |
| 최적화 방향 | 행 기반 저장, 인덱스 | 컬럼 기반 저장, 파티셔닝 |
| 대표 제품 | MySQL, PostgreSQL, Oracle | BigQuery, Redshift, Snowflake |

참고 자료:
- [AWS - OLTP와 OLAP의 차이](https://aws.amazon.com/ko/compare/the-difference-between-olap-and-oltp/)

---

## [ROLE-007] 데이터 파이프라인이 무엇이고, 일반적인 구성 요소를 설명해 주세요.

답변:
데이터 파이프라인은 데이터를 소스에서 목적지까지 자동으로 이동·변환·적재하는 일련의 처리 흐름입니다. 일반적으로 수집, 처리, 저장, 서빙의 단계로 구성되며, 각 단계를 오케스트레이션 도구로 연결하여 의존성과 실행 순서를 관리합니다. 안정적인 파이프라인은 장애 시 재처리, 모니터링, 데이터 품질 검증을 포함합니다.

추가 설명)
```
[Source]         [Ingestion]        [Processing]       [Storage]         [Serving]
DB / API   →   Kafka / Fluentd  →   Spark / Flink  →  DW / Data Lake  →  BI / API / ML
이벤트 로그        메시지 큐            변환·정제             장기 저장           분석·활용
```

| 단계 | 역할 | 대표 도구 |
|------|------|----------|
| 수집 | 소스에서 데이터 추출 | Kafka, Fluentd, Debezium |
| 처리 | 변환, 집계, 정제 | Spark, Flink, dbt |
| 저장 | 적재 및 보관 | S3, BigQuery, Redshift |
| 오케스트레이션 | 실행 순서·스케줄 관리 | Airflow, Dagster, Prefect |
| 서빙 | 분석·서비스에 데이터 제공 | Tableau, REST API, Feature Store |

참고 자료:
- [Google Cloud - 데이터 파이프라인이란?](https://cloud.google.com/learn/what-is-a-data-pipeline?hl=ko)

---

## [ROLE-008] 데이터 수집 단계에서 API, DB CDC, 로그, 메시지 큐를 각각 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
API는 외부 서비스나 파트너사의 데이터를 주기적으로 가져올 때 사용하며, HTTP 폴링 방식으로 배치 수집에 적합합니다. DB CDC는 Database Change Data Capture로, DB의 변경 이벤트를 실시간으로 캡처하여 다른 시스템에 전파할 때 사용합니다. 로그는 애플리케이션 동작 이벤트를 파일이나 스트림으로 수집할 때 사용하고, 메시지 큐는 서비스 간 비동기 이벤트를 안정적으로 전달하고 소비자가 속도를 조절할 수 있도록 버퍼 역할을 합니다.

추가 설명)
| 수집 방식 | 동작 방식 | 적합한 상황 | 대표 도구 |
|-----------|----------|------------|----------|
| API | HTTP 폴링 / Webhook | 외부 서비스 데이터, 주기적 수집 | REST API, Airbyte |
| DB CDC | DB 변경 로그 캡처 | DB 변경 실시간 동기화, 마이크로서비스 | Debezium, AWS DMS |
| 로그 수집 | 파일·스트림 수집 | 앱 이벤트, 에러 로그, 클릭 스트림 | Fluentd, Logstash, Filebeat |
| 메시지 큐 | Producer-Consumer 구조 | 고속 이벤트, 서비스 간 비동기 통신 | Kafka, RabbitMQ, AWS SQS |

참고 자료:
- [Debezium 공식 문서 - What is Debezium?](https://debezium.io/documentation/reference/stable/index.html)

---

## [ROLE-009] 데이터 품질을 관리할 때 정확성, 완전성, 일관성, 적시성을 어떻게 고려해야 하는지 설명해 주세요.

답변:
정확성은 저장된 데이터가 실제 값과 일치하는지를 검증하는 것으로, 소스 데이터와의 비교 검사나 범위 검사를 통해 확인합니다. 완전성은 필수 항목에 NULL이나 누락 없이 모든 데이터가 수집되었는지 확인하는 것입니다. 일관성은 여러 시스템이나 테이블 간에 동일한 데이터가 서로 모순되지 않는지를 점검하고, 적시성은 데이터가 정해진 SLA 안에 적시에 도착하고 처리되었는지를 모니터링합니다.

추가 설명)
| 품질 지표 | 의미 | 검증 방법 |
|-----------|------|----------|
| 정확성 | 데이터가 실제 값과 일치 | 범위 검사, 형식 검사, 소스 비교 |
| 완전성 | 필수 데이터 누락 없음 | NULL 비율, 레코드 수 비교 |
| 일관성 | 시스템 간 데이터 모순 없음 | 크로스 시스템 조회, 중복 ID 검사 |
| 적시성 | 정해진 시간 안에 처리 완료 | SLA 모니터링, 처리 지연 알림 |

참고 자료:

---

## [ROLE-010] 데이터 스키마 설계에서 정규화와 비정규화를 어떻게 판단할 수 있는지 설명해 주세요.

답변:
정규화는 데이터 중복을 제거하고 무결성을 보장하기 위해 테이블을 분리하는 것으로, 트랜잭션이 많고 데이터 정합성이 중요한 OLTP 환경에 적합합니다. 비정규화는 JOIN을 줄이고 조회 성능을 높이기 위해 중복을 허용하며 테이블을 합치는 것으로, 읽기 위주의 분석 쿼리가 많은 OLAP·Data Warehouse 환경에 적합합니다. 실제 설계에서는 쓰기 빈도, 읽기 빈도, 데이터 규모, SLA를 종합적으로 고려하여 판단합니다.

추가 설명)
| 구분 | 정규화 | 비정규화 |
|------|-------|---------|
| 목적 | 중복 제거, 무결성 보장 | 조회 성능 최적화 |
| JOIN 횟수 | 많음 | 적음 |
| 저장 공간 | 효율적 | 중복으로 증가 |
| 쓰기 성능 | 유리 | 갱신 이상 발생 가능 |
| 읽기 성능 | 복잡한 쿼리 | 단순 쿼리로 빠름 |
| 적합 환경 | OLTP, 운영 DB | OLAP, Data Warehouse |

참고 자료:
- [Microsoft Learn - 데이터베이스 정규화 설명](https://learn.microsoft.com/ko-kr/office/troubleshoot/access/database-normalization-description)

---

## [ROLE-011] Partitioning이 무엇이고, 대용량 데이터 처리에서 왜 중요한지 설명해 주세요.

답변:
Partitioning은 대용량 테이블이나 파일을 특정 기준으로 분할하여 저장하는 기법으로, 날짜·지역·카테고리 등의 컬럼을 기준으로 나눌 수 있습니다. 쿼리 실행 시 필요한 파티션만 스캔하는 Partition Pruning이 가능해지므로, 전체 데이터를 스캔하지 않아 처리 속도가 크게 향상됩니다. 또한 파티션 단위로 데이터를 삭제하거나 아카이빙할 수 있어 데이터 생명주기 관리에도 유리합니다.

추가 설명)
| Partitioning 종류 | 기준 | 예시 |
|------------------|------|------|
| Range Partitioning | 값 범위 | 날짜별 (2024-01, 2024-02 ...) |
| List Partitioning | 특정 값 목록 | 지역별 (서울, 부산, 대구) |
| Hash Partitioning | 해시 함수 결과 | user_id % 파티션 수 |

```
[파티션 없는 경우]
쿼리: WHERE date = '2024-01'  →  전체 테이블 Full Scan

[파티션 있는 경우]
쿼리: WHERE date = '2024-01'  →  2024-01 파티션만 스캔 (Partition Pruning)
```

참고 자료:
- [PostgreSQL 공식 문서 - Table Partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html)

---

## [ROLE-012] 데이터 저장소에서 Columnar Format이 무엇이고, Parquet 같은 포맷을 사용하는 이유를 설명해 주세요.

답변:
Columnar Format은 데이터를 행 단위가 아닌 컬럼 단위로 저장하는 방식으로, 분석 쿼리에서 특정 컬럼만 읽을 때 불필요한 I/O를 줄일 수 있습니다. 같은 컬럼의 데이터는 유사한 값들이 연속으로 저장되어 압축률이 높아지고, 결과적으로 스토리지 비용과 쿼리 비용이 모두 감소합니다. Parquet는 Apache에서 만든 오픈소스 컬럼형 포맷으로, Spark·BigQuery·Hive 등 대부분의 빅데이터 도구와 호환되어 사실상 표준으로 사용됩니다.

추가 설명)
```
[Row 기반 저장]
Row1: id=1, name=Alice, age=30
Row2: id=2, name=Bob,   age=25
→ SELECT age만 해도 name, id 모두 읽어야 함

[Columnar 저장]
id   컬럼: 1, 2, 3 ...
name 컬럼: Alice, Bob ...
age  컬럼: 30, 25 ...
→ SELECT age 시 age 컬럼 블록만 읽음
```

| 구분 | Row 기반 (CSV, JSON) | Columnar (Parquet, ORC) |
|------|---------------------|------------------------|
| 쓰기 성능 | 유리 | 불리 |
| 분석 쿼리 | 불필요한 컬럼도 읽음 | 필요 컬럼만 읽음 |
| 압축률 | 낮음 | 높음 |
| 적합 환경 | OLTP, 행 단위 처리 | OLAP, 분석 |

참고 자료:
- [Apache Parquet 공식 문서 - Overview](https://parquet.apache.org/docs/overview/)

---

## [ROLE-013] Apache Kafka가 무엇이고, 데이터 파이프라인에서 어떤 역할을 하는지 설명해 주세요.

답변:
Apache Kafka는 분산 이벤트 스트리밍 플랫폼으로, 높은 처리량과 내구성을 갖춘 메시지 큐 역할을 합니다. 데이터 파이프라인에서는 소스 시스템과 목적지 시스템 사이의 버퍼 역할을 하여 Producer와 Consumer를 분리하고, 소비 속도 차이를 흡수합니다. 메시지를 디스크에 보관하는 특성상 컨슈머 장애 시에도 오프셋을 조정하여 재처리할 수 있어 신뢰성 높은 파이프라인 구성에 핵심 도구입니다.

추가 설명)
```
[Producer]          [Kafka Broker]              [Consumer]
서비스 A  →  Topic(Partition 0, 1, 2)  →  데이터 엔지니어링 팀
서비스 B  →  메시지 디스크 보관          →  ML팀
서비스 C  →  오프셋 기반 소비 추적      →  실시간 대시보드
```

| 특징 | 설명 |
|------|------|
| 높은 처리량 | 초당 수백만 건 메시지 처리 가능 |
| 내구성 | 메시지를 디스크에 영속 저장 |
| 수평 확장 | Broker, Partition 추가로 확장 |
| 재처리 가능 | 오프셋 기반으로 과거 메시지 재소비 |
| 분리성 | Producer와 Consumer 독립 운영 |

참고 자료:
- [Apache Kafka 공식 문서 - Introduction](https://kafka.apache.org/documentation/#gettingStarted)

---

## [ROLE-014] Kafka의 Topic, Partition, Consumer Group의 역할을 설명해 주세요.

답변:
Topic은 메시지를 분류하는 논리적인 채널로, 데이터 파이프라인에서 이벤트 종류별로 Topic을 구분하여 관리합니다. Partition은 Topic을 병렬 처리를 위해 분할한 단위로, 각 Partition은 독립된 순서를 유지하며 메시지를 저장합니다. Consumer Group은 하나의 Topic을 구독하는 Consumer들의 집합으로, 각 Partition은 Consumer Group 내의 하나의 Consumer에만 할당되어 병렬 소비가 가능합니다.

추가 설명)
```
Topic: order-events

Partition 0: [msg1] [msg4] [msg7] ...  →  Consumer A
Partition 1: [msg2] [msg5] [msg8] ...  →  Consumer B
Partition 2: [msg3] [msg6] [msg9] ...  →  Consumer C

(Consumer A, B, C 는 같은 Consumer Group)
```

| 개념 | 역할 | 특징 |
|------|------|------|
| Topic | 메시지 분류 채널 | 이름으로 구분, 여러 Partition 포함 |
| Partition | 병렬 처리 단위 | 파티션 내 순서 보장, 번호로 식별 |
| Consumer Group | 병렬 소비 그룹 | Partition과 Consumer 1:1 할당 |
| Offset | 메시지 위치 추적 | Consumer Group별 독립 관리 |

참고 자료:
- [Apache Kafka 공식 문서 - Design](https://kafka.apache.org/documentation/#design)

---

## [ROLE-015] Kafka에서 메시지 순서 보장이 어떻게 이루어지는지 설명해 주세요.

답변:
Kafka는 같은 Partition 내에서만 메시지 순서를 보장합니다. Topic 전체 수준의 전역 순서는 보장하지 않으며, 서로 다른 Partition 간에는 순서가 뒤섞일 수 있습니다. 특정 키를 기준으로 같은 키를 가진 메시지를 항상 동일한 Partition으로 라우팅하는 Key-based 파티셔닝을 사용하면, 같은 키에 대한 메시지 순서를 보장할 수 있습니다.

추가 설명)
```
[Key = user_id 기반 파티셔닝]

user_id=1 메시지  →  항상 Partition 0  →  순서 보장
user_id=2 메시지  →  항상 Partition 1  →  순서 보장
user_id=3 메시지  →  항상 Partition 2  →  순서 보장

Partition 0 내: [msg(t=1)] → [msg(t=2)] → [msg(t=3)]  (순서 보장 O)
Partition 0 vs Partition 1: 상대 순서 보장 X
```

| 수준 | 순서 보장 여부 | 방법 |
|------|--------------|------|
| Partition 내부 | O | 기본 동작 |
| Topic 전체 (글로벌) | X | Partition 1개 사용 (처리량 희생) |
| 동일 Key | O | Key-based 파티셔닝 |

참고 자료:
- [Apache Kafka 공식 문서 - Guarantees](https://kafka.apache.org/documentation/#semantics)

---

## [ROLE-016] 데이터 파이프라인에서 중복 처리와 멱등성이 중요한 이유를 설명해 주세요.

답변:
파이프라인은 네트워크 장애, Consumer 재시작, 재처리 등으로 인해 같은 메시지를 여러 번 처리할 가능성이 있습니다. 중복이 허용되면 집계 결과가 틀어지거나, 이중 결제 같은 심각한 비즈니스 오류로 이어질 수 있습니다. 멱등성은 같은 작업을 여러 번 실행해도 결과가 동일하게 유지되는 성질로, 파이프라인의 재처리 안정성을 보장하기 위한 핵심 설계 원칙입니다.

추가 설명)
| 처리 보장 수준 | 의미 | 중복 가능성 | 손실 가능성 |
|--------------|------|------------|------------|
| At-most-once | 최대 1회 처리 | 없음 | 있음 |
| At-least-once | 최소 1회 처리 | 있음 | 없음 |
| Exactly-once | 정확히 1회 처리 | 없음 | 없음 |

멱등성 구현 방법:
- 고유 ID 기반 중복 체크 (idempotency key)
- UPSERT 사용 (INSERT OR UPDATE)
- Kafka의 Transactional Producer + Idempotent Producer 설정

참고 자료:
- [Apache Kafka 공식 문서 - Exactly-once Semantics](https://kafka.apache.org/documentation/#semantics)

---

## [ROLE-017] Airflow와 같은 Workflow Orchestration 도구가 필요한 이유를 설명해 주세요.

답변:
데이터 파이프라인은 여러 태스크 사이에 의존성이 있어, 앞 태스크가 성공한 뒤에야 다음 태스크를 실행해야 하는 경우가 많습니다. Workflow Orchestration 도구는 이러한 의존성과 실행 순서를 코드로 정의하고, 스케줄링·모니터링·실패 알림·재처리를 자동화합니다. Airflow 없이 cron만 사용하면 태스크 의존성 관리나 장애 시 재처리가 어렵고, 파이프라인 상태를 시각적으로 파악하기 어렵습니다.

추가 설명)
| 기능 | cron 단독 사용 | Airflow 사용 |
|------|--------------|-------------|
| 태스크 의존성 관리 | 수동 구현 필요 | DAG으로 선언적 정의 |
| 실패 시 재시도 | 직접 구현 | 자동 retry 설정 |
| 스케줄링 | 가능 | 가능 (더 풍부한 옵션) |
| 모니터링 | 어려움 | UI 대시보드 제공 |
| Backfill | 어려움 | 날짜 범위 지정 재처리 |
| 알림 | 직접 구현 | Slack·Email 연동 |

참고 자료:
- [Apache Airflow 공식 문서 - Overview](https://airflow.apache.org/docs/apache-airflow/stable/index.html)

---

## [ROLE-018] DAG가 무엇이고, 데이터 워크플로우를 DAG로 표현하는 이유를 설명해 주세요.

답변:
DAG는 Directed Acyclic Graph의 약자로, 방향은 있지만 사이클이 없는 그래프 구조입니다. 데이터 파이프라인의 태스크를 노드로, 의존 관계를 방향 있는 엣지로 표현하여 실행 순서와 병렬 실행 가능 여부를 명확히 정의합니다. 사이클이 없어야 무한 루프 없이 파이프라인이 종료됨을 보장할 수 있으며, Airflow는 이 DAG 구조를 기반으로 태스크를 스케줄링하고 실행합니다.

추가 설명)
```
[DAG 예시: 일별 데이터 파이프라인]

[extract_data] → [validate_data] → [transform_data] → [load_to_dw]
                                 ↘                  ↗
                                  [send_alert]
```

- 방향성: 태스크 A가 완료된 후 B가 실행됨을 명시
- 비순환성: 순환 의존이 없어야 파이프라인이 정상 종료 가능
- 병렬 실행: 의존성이 없는 태스크는 동시 실행 가능

참고 자료:
- [Apache Airflow 공식 문서 - DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html)

---

## [ROLE-019] 데이터 파이프라인 모니터링에서 어떤 지표와 로그를 확인해야 하는지 설명해 주세요.

답변:
파이프라인 모니터링에서는 처리량, 지연시간, 에러율, Consumer Lag 등의 메트릭을 주기적으로 수집하고 임계값 초과 시 알림을 설정합니다. 로그는 각 태스크의 실행 로그와 에러 로그를 중앙화하여 장애 원인을 빠르게 파악할 수 있도록 합니다. SLA 기반으로 파이프라인이 정해진 시간 안에 완료되었는지 적시성 모니터링도 함께 수행해야 합니다.

추가 설명)
| 모니터링 항목 | 지표 | 이상 신호 |
|-------------|------|----------|
| 처리량 | 초당 처리 건수 | 급격한 감소 |
| 지연시간 | 이벤트 생성~처리 완료 시간 | SLA 초과 |
| 에러율 | 실패 태스크 / 전체 태스크 | 에러율 증가 |
| Consumer Lag | Kafka 미소비 메시지 수 | Lag 지속 증가 |
| 데이터 품질 | NULL 비율, 레코드 수 | 기준치 이탈 |
| 리소스 사용률 | CPU, 메모리, 디스크 | 이상 급증 |

참고 자료:
- [Apache Airflow 공식 문서 - Logging and Monitoring](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/logging-monitoring/index.html)

---

## [ROLE-020] 데이터 엔지니어링에서 장애 재처리와 Backfill이 무엇인지 설명해 주세요.

답변:
장애 재처리는 파이프라인 실행 중 오류가 발생했을 때 실패한 태스크부터 다시 실행하는 것으로, 멱등성이 보장되어야 안전하게 수행할 수 있습니다. Backfill은 파이프라인이 신규로 배포되었거나 과거 데이터에 오류가 있었을 때, 특정 날짜 범위의 과거 데이터를 소급하여 재처리하는 작업입니다. Airflow에서는 날짜 범위를 지정하여 DAG를 실행하는 방식으로 Backfill을 지원하며, 이 경우에도 멱등성이 핵심 전제 조건입니다.

추가 설명)
```
[장애 재처리]
태스크 A(성공) → 태스크 B(실패) → 재실행 → 태스크 B부터 다시 실행

[Backfill]
신규 파이프라인 배포 후, 2024-01-01 ~ 2024-03-31 데이터 소급 처리
airflow dags backfill -s 2024-01-01 -e 2024-03-31 <dag_id>
```

| 구분 | 장애 재처리 | Backfill |
|------|-----------|---------|
| 목적 | 실패한 태스크 복구 | 과거 데이터 소급 처리 |
| 트리거 | 파이프라인 실패 | 신규 배포, 데이터 수정 |
| 범위 | 실패 지점부터 | 지정 날짜 범위 전체 |
| 전제 조건 | 멱등성 | 멱등성 |

참고 자료:
- [Apache Airflow 공식 문서 - DAG Runs and Backfill](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html)

---

## 선택 질문

## [ROLE-021] CDC가 무엇이고, 데이터 동기화에 어떤 장점이 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-022] Slowly Changing Dimension, SCD가 무엇이고 Type 1, Type 2의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-023] Star Schema와 Snowflake Schema의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-024] Fact Table과 Dimension Table의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-025] Data Lineage가 무엇이고, 데이터 신뢰성 관리에 왜 중요한지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-026] 데이터 카탈로그가 무엇이고, 조직에서 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-027] 데이터 거버넌스가 무엇이고, 권한 관리와 개인정보 보호 관점에서 왜 중요한지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-028] 데이터 파이프라인에서 Schema Evolution이 발생했을 때 어떤 문제가 생길 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-029] Kafka에서 At-most-once, At-least-once, Exactly-once 처리 의미를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-030] Kafka Consumer Lag이 무엇이고, Lag이 증가했을 때 어떤 원인을 의심할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-031] Spark가 무엇이고, 대용량 데이터 처리에서 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-032] Spark의 RDD, DataFrame, Dataset의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-033] Spark에서 Shuffle이 무엇이고, 성능에 어떤 영향을 주는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-034] 데이터 처리에서 Window Function이 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-035] 데이터 파이프라인에서 Checkpoint와 Retry를 설계할 때 고려해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-036] Lambda Architecture와 Kappa Architecture의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-037] 데이터 파이프라인에서 비용 최적화를 위해 고려할 수 있는 요소를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-038] 실시간 이상 탐지 파이프라인을 설계한다면 어떤 구조로 설계할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-039] 데이터 파이프라인에서 보안과 개인정보 비식별화를 어떻게 고려해야 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-040] 본인의 데이터 프로젝트를 설명할 때 데이터 수집, 처리, 저장, 품질 검증, 활용 결과를 어떻게 구조화하면 좋을지 설명해 주세요.

답변:

참고 자료:
