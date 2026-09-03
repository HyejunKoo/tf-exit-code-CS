# Week 07 - Data Engineer Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] 대용량 데이터 파이프라인에서 확장성과 안정성을 설계할 때 고려해야 할 요소를 설명해 주세요.

답변:
확장성 측면에서는 파이프라인의 각 컴포넌트를 수평 확장 가능하도록 설계하고, 데이터 볼륨 증가에 따라 Kafka Partition, Spark Executor, Consumer 수를 독립적으로 늘릴 수 있어야 합니다. 안정성 측면에서는 멱등성 있는 재처리 설계, 장애 격리를 위한 Dead Letter Queue, 서킷 브레이커 패턴을 적용하여 단일 장애가 전체 파이프라인으로 전파되지 않도록 합니다. 모니터링과 SLA 기반 알림, Backfill 전략도 함께 설계해야 운영 시 빠른 장애 대응이 가능합니다.

추가 설명)
| 관점 | 고려 요소 | 적용 방법 |
|------|----------|----------|
| 확장성 | 수평 확장 | Kafka Partition 증설, Spark Executor 조정 |
| 확장성 | 병목 식별 | Consumer Lag, 처리 지연 모니터링 |
| 안정성 | 멱등성 | Idempotency Key, UPSERT 패턴 |
| 안정성 | 장애 격리 | Dead Letter Queue, 재시도 제한 |
| 안정성 | 복구 가능성 | Checkpoint, Backfill 설계 |
| 운영성 | 관측 가능성 | 메트릭, 로그, 알림 통합 |

참고 자료:
- [Apache Kafka 공식 문서 - Operations](https://kafka.apache.org/documentation/#operations)

---

## [ROLE-042] 데이터 파이프라인에서 Exactly-once 처리가 어려운 이유와 현실적인 대응 방법을 설명해 주세요.

답변:
분산 시스템에서는 네트워크 장애나 프로세스 재시작 시 메시지가 중복 전달될 수 있고, Producer와 Consumer 양쪽에서 모두 Exactly-once를 보장해야 하므로 구현 복잡도가 높습니다. Kafka는 Idempotent Producer와 Transactional API를 통해 Kafka 내부에서의 Exactly-once를 지원하지만, 외부 저장소까지 포함한 End-to-end Exactly-once는 저장소가 트랜잭션을 지원해야 가능합니다. 현실적으로는 At-least-once 전달을 기본으로 하고, 소비 측에서 멱등성 처리로 중복을 제거하는 방식이 많이 사용됩니다.

추가 설명)
```
[Exactly-once 구현 조건]
1. Idempotent Producer: 동일 메시지 재전송 시 Broker에서 중복 제거
2. Transactional API: Producer → Kafka → Consumer 원자적 처리
3. 외부 저장소 멱등성: DB UPSERT 또는 고유 ID 기반 중복 체크
```

| 보장 수준 | Kafka 설정 | 외부 저장소 필요 조건 | 성능 영향 |
|-----------|-----------|---------------------|----------|
| At-most-once | acks=0 | 없음 | 낮음 |
| At-least-once | acks=all + 재시도 | 없음 | 중간 |
| Exactly-once | enable.idempotence=true + transactional.id | 트랜잭션 지원 | 높음 |

참고 자료:
- [Apache Kafka 공식 문서 - Semantics](https://kafka.apache.org/documentation/#semantics)

---

## [ROLE-043] Kafka Producer의 Acknowledgement 설정과 데이터 유실 가능성의 관계를 설명해 주세요.

답변:
acks=0은 Producer가 Broker의 응답을 기다리지 않아 가장 빠르지만 네트워크 장애 시 메시지 유실 가능성이 가장 높습니다. acks=1은 Leader Broker에만 쓰기 성공 응답을 받으므로, Leader 장애 시 Follower에 복제되지 않은 메시지가 유실될 수 있습니다. acks=all은 모든 ISR Broker에 복제가 완료된 후 응답을 받으므로 유실 가능성이 가장 낮지만 지연시간이 증가합니다.

추가 설명)
```
[acks=0]  Producer → Broker  (응답 없이 바로 다음 전송)
[acks=1]  Producer → Leader  (Leader 응답 후 다음 전송, Follower 복제 미확인)
[acks=all] Producer → Leader → Follower1, Follower2 → 전체 ISR 응답 후 다음 전송
```

| acks 설정 | 속도 | 유실 가능성 | 적합 상황 |
|-----------|------|------------|----------|
| 0 | 가장 빠름 | 높음 | 유실 허용 가능한 로그 |
| 1 | 중간 | 중간 | 일반적인 이벤트 |
| all | 느림 | 낮음 | 결제, 주문 등 중요 데이터 |

참고 자료:
- [Apache Kafka 공식 문서 - Producer Configs](https://kafka.apache.org/documentation/#producerconfigs)

---

## [ROLE-044] Kafka Consumer Offset이 무엇이고, Offset Commit 전략에 따라 어떤 차이가 발생하는지 설명해 주세요.

답변:
Offset은 Partition 내에서 Consumer가 마지막으로 처리한 메시지의 위치를 나타내는 숫자로, Consumer Group별로 독립적으로 관리됩니다. Auto Commit은 일정 주기로 자동으로 Offset을 커밋하므로 편리하지만, 처리 완료 전에 커밋되면 장애 시 메시지 유실이 발생할 수 있습니다. Manual Commit은 메시지 처리가 완료된 후 명시적으로 커밋하므로 At-least-once를 보장하며, 더 세밀한 제어가 가능합니다.

추가 설명)
```
[Auto Commit - 유실 위험]
처리 중 → 자동 커밋 → 장애 → 오프셋이 커밋됐으므로 재처리 불가 → 유실

[Manual Commit - 중복 위험]
처리 완료 → 커밋 전 장애 → 오프셋 미커밋 → 재시작 시 동일 메시지 재처리 → 중복
```

| Commit 전략 | 동작 | 유실 위험 | 중복 위험 | 권장 상황 |
|------------|------|----------|----------|----------|
| Auto Commit | 주기적 자동 커밋 | 있음 | 낮음 | 유실 허용 가능한 경우 |
| Manual Commit (at-least-once) | 처리 후 커밋 | 없음 | 있음 | 일반 데이터 파이프라인 |
| Transactional | 처리와 커밋 원자적 | 없음 | 없음 | 금융, 결제 등 |

참고 자료:
- [Apache Kafka 공식 문서 - Consumer Configs](https://kafka.apache.org/documentation/#consumerconfigs)

---

## [ROLE-045] Kafka에서 Rebalancing이 발생하는 상황과 이로 인해 생길 수 있는 문제를 설명해 주세요.

답변:
Rebalancing은 Consumer Group 내의 Consumer가 추가되거나 제거되거나, Consumer가 Heartbeat를 보내지 못하면 Group Coordinator가 Partition을 재배분하는 과정입니다. Rebalancing이 발생하는 동안 모든 Consumer는 메시지 소비를 멈추는 Stop-the-world 현상이 발생하여 파이프라인 처리가 중단됩니다. Consumer 처리 시간이 길거나 GC 지연으로 Heartbeat가 늦어지면 불필요한 Rebalancing이 반복되므로, session.timeout.ms와 max.poll.interval.ms 설정을 처리 환경에 맞게 조정해야 합니다.

추가 설명)
| Rebalancing 발생 원인 | 대응 방법 |
|--------------------|----------|
| Consumer 추가 | 정상적인 확장, 불가피 |
| Consumer 장애/종료 | 빠른 재시작, 리소스 안정화 |
| Heartbeat 지연 (GC, 처리 지연) | max.poll.interval.ms 증가, 배치 크기 조정 |
| session.timeout 초과 | session.timeout.ms 조정 |

Kafka 2.4 이후 Cooperative Sticky Assignor를 사용하면 Incremental Rebalancing으로 Stop-the-world를 최소화할 수 있습니다.

참고 자료:
- [Apache Kafka 공식 문서 - Consumer Group Rebalance](https://kafka.apache.org/documentation/#impl_consumer)

---

## [ROLE-046] Kafka Partition 수를 설계할 때 고려해야 할 요소를 설명해 주세요.

답변:
Partition 수는 Consumer Group의 최대 병렬 소비 수를 결정하므로, 목표 처리량을 Consumer 하나의 처리 속도로 나누어 필요한 최소 Partition 수를 산정합니다. Partition 수는 늘리기는 쉽지만 줄이기는 어렵고, 너무 많으면 Broker와 Controller의 메타데이터 부하가 증가합니다. 순서 보장이 필요한 데이터의 경우 Partition 수를 줄이거나 Key-based 파티셔닝을 적용하여 같은 키가 같은 Partition으로 라우팅되도록 설계합니다.

추가 설명)
| 고려 요소 | 영향 | 가이드 |
|----------|------|-------|
| 목표 처리량 | Partition = 처리량 / Consumer 1개 처리 속도 | 여유분 포함 |
| Consumer 수 | Partition 수 >= Consumer 수 | 초과 Consumer는 유휴 상태 |
| 순서 보장 요구 | Partition 내 순서만 보장 | 전역 순서 필요 시 Partition=1 |
| Broker 부하 | Partition이 많을수록 메타데이터 부하 | 적절한 상한선 유지 |
| Replication Factor | Partition × RF만큼 스토리지 소비 | 스토리지 용량 고려 |

참고 자료:
- [Apache Kafka 공식 문서 - Design](https://kafka.apache.org/documentation/#design)

---

## [ROLE-047] Kafka에서 메시지 키를 설계할 때 순서 보장과 부하 분산을 어떻게 고려해야 하는지 설명해 주세요.

답변:
Kafka는 같은 키를 가진 메시지를 동일한 Partition에 라우팅하므로, 키를 기준으로 순서 보장과 Partition별 부하 분산이 결정됩니다. 키의 카디널리티가 낮으면 특정 Partition에 데이터가 편중되는 Hotspot이 발생할 수 있고, 키가 없으면 Round-robin으로 분산되어 부하는 균등하지만 순서는 보장되지 않습니다. 실무에서는 순서가 필요한 단위를 키로 사용하되, 편중이 발생하지 않도록 키의 카디널리티를 충분히 높여 설계합니다.

추가 설명)
```
[키 없음 (Round-robin)]  분산 균등 O / 순서 보장 X
user_id=1 → P0
user_id=2 → P1
user_id=3 → P2
user_id=4 → P0  (다음 순서)

[키 있음 (user_id 기반)]  순서 보장 O / 편중 위험
user_id=1 → 항상 P0
user_id=2 → 항상 P1
→ VIP 사용자가 특정 key에 몰리면 P0 Hotspot 발생
```

| 키 설계 | 순서 보장 | 부하 분산 | Hotspot 위험 |
|--------|----------|----------|------------|
| 키 없음 | X | 균등 | 낮음 |
| 저카디널리티 키 | O | 편중 | 높음 |
| 고카디널리티 키 | O (키 내) | 양호 | 낮음 |

참고 자료:
- [Apache Kafka 공식 문서 - Producers](https://kafka.apache.org/documentation/#theproducer)

---

## [ROLE-048] Kafka Consumer Lag이 증가했을 때 원인을 분석하는 순서를 설명해 주세요.

답변:
먼저 kafka-consumer-groups.sh 또는 모니터링 도구로 어느 Topic, Partition에서 Lag이 발생했는지 확인합니다. Lag이 특정 Partition에 집중되어 있으면 해당 Partition을 담당하는 Consumer의 처리 속도 저하나 장애를 의심하고, 전체 Partition에서 증가한다면 Producer의 전송량 급증을 확인합니다. Consumer의 CPU, 메모리, GC 상태를 확인하고, 처리 로직의 외부 의존성인 DB 쿼리 지연, API 응답 지연 여부도 점검합니다.

추가 설명)
```
[Lag 증가 원인 분석 순서]

1. Lag 발생 위치 확인
   → 특정 Partition만? → 해당 Consumer 장애 의심
   → 전체 Partition?  → Producer 급증 또는 Consumer 전체 처리 저하

2. Consumer 리소스 확인
   → CPU 과부하, GC 빈도 증가, 메모리 부족

3. Consumer 처리 로직 확인
   → 외부 API/DB 지연, 배치 크기 과다

4. 조치
   → Consumer 인스턴스 수 증가 (Partition 수 이하)
   → 처리 로직 최적화
   → max.poll.records 조정
```

참고 자료:
- [Apache Kafka 공식 문서 - Monitoring](https://kafka.apache.org/documentation/#monitoring)

---

## [ROLE-049] 데이터 파이프라인에서 Dead Letter Queue가 무엇이고, 어떤 상황에서 사용하는지 설명해 주세요.

답변:
Dead Letter Queue는 정해진 재시도 횟수 이후에도 처리에 실패한 메시지를 별도의 큐나 Topic에 격리하는 패턴입니다. 처리 불가능한 메시지가 메인 파이프라인을 반복 점유하여 전체 흐름을 막는 Poison Pill 문제를 방지할 수 있습니다. DLQ에 격리된 메시지는 나중에 원인 분석 후 수동 또는 자동으로 재처리하거나 폐기하는 방식으로 운영합니다.

추가 설명)
```
[정상 흐름]
Producer → Topic → Consumer → 처리 성공 → Offset Commit

[DLQ 적용 흐름]
Producer → Topic → Consumer → 처리 실패
                             → 재시도 (N회)
                             → 실패 → Dead Letter Topic → 격리·분석
```

| DLQ 사용 상황 | 예시 |
|-------------|------|
| 스키마 파싱 오류 | 메시지 형식이 Consumer 기대값과 다름 |
| 외부 시스템 오류 | DB Insert 반복 실패 |
| 비즈니스 로직 오류 | 유효하지 않은 데이터 |
| 리소스 과부하 | 일시적 처리 불가 상태 |

참고 자료:
- [Apache Kafka 공식 문서 - Error Handling](https://kafka.apache.org/documentation/#connect_errorreporting)

---

## [ROLE-050] 데이터 파이프라인에서 중복 데이터가 발생할 수 있는 원인과 제거 방법을 설명해 주세요.

답변:
중복 데이터는 At-least-once 전달 보장 환경에서 네트워크 장애 후 재전송, Consumer 재시작 시 Offset 미커밋 상태에서 재처리, 파이프라인 Backfill 실행 등의 상황에서 발생합니다. 제거 방법으로는 각 레코드에 고유한 Idempotency Key를 부여하고, 적재 시 UPSERT나 INSERT IGNORE를 사용하여 같은 키가 들어와도 결과가 달라지지 않도록 멱등성을 확보합니다. 스트리밍 처리에서는 일정 시간 창 내의 중복을 Window 기반으로 탐지하고 제거하는 방법도 사용합니다.

추가 설명)
| 중복 발생 원인 | 제거 방법 |
|-------------|----------|
| 네트워크 장애 후 재전송 | Kafka Idempotent Producer |
| Offset 미커밋 후 재처리 | Manual Commit + UPSERT |
| Backfill 중복 실행 | 고유 ID 기반 중복 체크 테이블 |
| 파이프라인 이중 실행 | 실행 잠금, DAG 설계 |

```
[UPSERT 예시 - 중복 방지]
INSERT INTO orders (order_id, amount, status)
VALUES (?, ?, ?)
ON CONFLICT (order_id) DO UPDATE SET status = EXCLUDED.status;
```

참고 자료:
- [Apache Kafka 공식 문서 - Exactly-once Semantics](https://kafka.apache.org/documentation/#semantics)

---

## [ROLE-051] CDC 기반 데이터 동기화에서 Snapshot과 Streaming 변경 로그의 차이를 설명해 주세요.

답변:
Snapshot은 CDC 파이프라인이 처음 시작될 때 소스 DB의 현재 전체 데이터를 한 번 읽어 목적지에 초기 적재하는 과정입니다. Streaming 변경 로그는 Snapshot 이후 DB의 binlog, WAL 같은 트랜잭션 로그를 실시간으로 읽어 INSERT, UPDATE, DELETE 이벤트를 연속적으로 전달하는 방식입니다. 실제 운영에서는 Snapshot으로 초기 데이터를 맞추고, 이후 Streaming 변경 로그로 지속적으로 동기화하는 방식을 함께 사용합니다.

추가 설명)
```
[CDC 초기 구성]
소스 DB → [Snapshot 전체 읽기] → 목적지 DB (초기 적재)
                   ↓
        이후 binlog/WAL 스트리밍 시작
소스 DB → [INSERT/UPDATE/DELETE 이벤트] → Kafka → 목적지 DB (실시간 동기화)
```

| 구분 | Snapshot | Streaming 변경 로그 |
|------|----------|-------------------|
| 시점 | 최초 1회 | 지속적 실시간 |
| 데이터 범위 | 전체 현재 데이터 | 변경된 레코드만 |
| 부하 | DB Full Scan으로 높음 | 낮음 |
| 지연 | 없음 (일괄 복사) | 밀리초~초 단위 |

참고 자료:
- [Debezium 공식 문서 - Snapshots](https://debezium.io/documentation/reference/stable/connectors/mysql.html#mysql-snapshots)

---

## [ROLE-052] CDC 파이프라인에서 스키마 변경이 발생했을 때 어떤 문제가 생길 수 있는지 설명해 주세요.

답변:
소스 DB에서 컬럼이 추가, 삭제, 타입 변경이 발생하면 CDC가 전달하는 이벤트의 형태가 달라져 Downstream Consumer가 파싱 오류를 일으킬 수 있습니다. Avro나 Protobuf 같은 직렬화 포맷을 사용하는 경우, Schema Registry에서 호환되지 않는 스키마 변경이 적용되면 Consumer가 역직렬화에 실패합니다. 이를 방지하기 위해 Backward/Forward Compatible 스키마 변경 정책을 적용하고, Schema Registry로 변경 이력을 중앙 관리하는 것이 권장됩니다.

추가 설명)
| 스키마 변경 종류 | 영향 | 호환성 |
|--------------|------|-------|
| 컬럼 추가 (nullable) | 낮음 | Backward Compatible |
| 컬럼 추가 (not null) | 높음 | Breaking Change |
| 컬럼 삭제 | 높음 | Breaking Change |
| 컬럼 타입 변경 | 높음 | Breaking Change |
| 컬럼 이름 변경 | 높음 | Breaking Change |

Schema Registry와 Confluent Avro를 사용하면 BACKWARD, FORWARD, FULL 호환성 정책을 강제할 수 있습니다.

참고 자료:
- [Debezium 공식 문서 - Schema Changes](https://debezium.io/documentation/reference/stable/connectors/mysql.html#mysql-schema-history-topic)

---

## [ROLE-053] Spark에서 Transformation과 Action의 차이를 설명해 주세요.

답변:
Transformation은 기존 RDD나 DataFrame을 변환하여 새로운 RDD나 DataFrame을 반환하는 연산으로, 실제 계산이 즉시 실행되지 않고 DAG에 변환 계획만 추가됩니다. Action은 count, collect, save 같이 실제 계산을 트리거하여 결과를 드라이버에 반환하거나 저장소에 쓰는 연산으로, 이 시점에 Lazy Evaluation이 실행됩니다. 이 구조 덕분에 Spark는 Action 실행 시 전체 Transformation 체인을 최적화하여 실행 계획을 수립합니다.

추가 설명)
| 구분 | Transformation | Action |
|------|---------------|--------|
| 실행 시점 | 즉시 실행 안 됨 (Lazy) | 즉시 실행 |
| 반환값 | 새로운 RDD/DataFrame | 결과값 또는 저장 |
| 예시 | map, filter, groupBy, join | count, collect, show, write |
| 역할 | 실행 계획 누적 | 계획 실행 트리거 |

```
[코드 예시]
df.filter(...)     # Transformation - 계획만 등록
  .groupBy(...)    # Transformation - 계획만 등록
  .count()         # Action - 이 시점에 전체 실행
```

참고 자료:
- [Apache Spark 공식 문서 - RDD Programming Guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)

---

## [ROLE-054] Spark에서 Lazy Evaluation이 무엇이고, 성능 최적화에 어떤 의미가 있는지 설명해 주세요.

답변:
Lazy Evaluation은 Transformation을 즉시 실행하지 않고, Action이 호출될 때 전체 변환 체인을 한 번에 평가하는 방식입니다. 이를 통해 Spark는 Catalyst Optimizer가 전체 DAG를 분석하여 불필요한 스캔 제거, 프레디케이트 푸시다운, 파이프라이닝 같은 최적화를 자동으로 적용합니다. 개발자가 단계별로 결과를 확인하고 싶다면 명시적으로 cache()나 persist()를 사용하거나 Action을 중간에 호출해야 합니다.

추가 설명)
```
[Lazy Evaluation 최적화 예시]

코드:
df.filter("country = 'KR'").select("user_id", "amount").count()

최적화 전: 전체 스캔 → filter → select → count
최적화 후: Predicate Pushdown으로 filter를 소스 스캔 단계로 내림
          → 'KR' 데이터만 읽음 → select → count
```

| 최적화 기법 | 설명 |
|-----------|------|
| Predicate Pushdown | 필터를 데이터 소스 수준으로 내려 스캔량 감소 |
| Column Pruning | 필요한 컬럼만 읽음 |
| Join Reorder | 작은 테이블을 먼저 JOIN하도록 순서 조정 |
| Pipeline | 여러 Transformation을 하나의 Stage로 병합 |

참고 자료:
- [Apache Spark 공식 문서 - Performance Tuning](https://spark.apache.org/docs/latest/sql-performance-tuning.html)

---

## [ROLE-055] Spark Shuffle이 발생하는 대표적인 연산과 Shuffle을 줄이는 방법을 설명해 주세요.

답변:
Shuffle은 데이터를 새로운 파티션으로 재분배하기 위해 네트워크를 통해 데이터를 이동하는 과정으로, groupByKey, reduceByKey, join, repartition 같은 연산에서 발생합니다. Shuffle은 디스크 I/O와 네트워크 전송이 발생하여 Spark 작업에서 가장 비용이 큰 연산이므로 최소화하는 것이 중요합니다. 줄이는 방법으로는 작은 테이블을 Broadcast Join으로 처리하거나, reduceByKey를 groupByKey 대신 사용하거나, 자주 사용하는 키 기준으로 미리 파티셔닝하여 재사용하는 방법이 있습니다.

추가 설명)
| Shuffle 발생 연산 | 대안 또는 최소화 방법 |
|-----------------|-------------------|
| groupByKey | reduceByKey 또는 aggregateByKey (로컬 집계 후 Shuffle) |
| join (대-대 테이블) | Broadcast Join (소테이블 전체 복사로 Shuffle 제거) |
| repartition | coalesce (Shuffle 없이 파티션 감소) |
| distinct | 키 기반 파티셔닝 유지 |

```
[Broadcast Join 예시 - Shuffle 제거]
from pyspark.sql.functions import broadcast
df_large.join(broadcast(df_small), "key")
→ df_small을 모든 Executor에 복사 → Shuffle 없이 로컬 Join
```

참고 자료:
- [Apache Spark 공식 문서 - Performance Tuning](https://spark.apache.org/docs/latest/sql-performance-tuning.html)

---

## [ROLE-056] Spark에서 Partition 수와 Executor Resource를 설정할 때 고려해야 할 점을 설명해 주세요.

답변:
Spark의 Partition 수는 병렬 처리의 단위로, 너무 적으면 Executor가 유휴 상태가 되고 너무 많으면 Task 스케줄링 오버헤드가 증가합니다. 일반적으로 Partition 크기를 128MB~256MB로 유지하는 것을 권장하며, Shuffle 후 파티션 수는 spark.sql.shuffle.partitions로 조정합니다. Executor의 코어 수와 메모리는 데이터 크기, Shuffle 양, 캐싱 필요 여부를 고려하여 설정하고, 하나의 Executor에 코어를 너무 많이 할당하면 HDFS I/O 성능이 저하될 수 있습니다.

추가 설명)
| 설정 | 권장 가이드 | 이유 |
|------|-----------|------|
| Partition 크기 | 128MB ~ 256MB | 너무 작으면 Task 오버헤드, 너무 크면 OOM |
| Executor 코어 수 | 코어당 1~5개 Task | HDFS 동시 접속 수 제한 고려 |
| Executor 메모리 | 데이터 + Shuffle + 오버헤드 | OOM 방지 |
| shuffle.partitions | 기본값 200, 데이터 크기에 맞게 조정 | Shuffle 파티션 수 조정 |

참고 자료:
- [Apache Spark 공식 문서 - Configuration](https://spark.apache.org/docs/latest/configuration.html)

---

## [ROLE-057] Data Lakehouse가 무엇이고, Data Lake와 Data Warehouse의 한계를 어떻게 보완하는지 설명해 주세요.

답변:
Data Lake는 원시 데이터를 저렴하게 저장할 수 있지만 ACID 트랜잭션이 없어 데이터 일관성 보장이 어렵고, Data Warehouse는 정합성이 높지만 비정형 데이터 처리와 ML 워크로드에 유연하지 않습니다. Data Lakehouse는 Data Lake의 저비용 스토리지 위에 트랜잭션 레이어를 추가하여 ACID 보장, 스키마 관리, Time Travel 같은 Data Warehouse 수준의 기능을 제공합니다. Delta Lake, Apache Iceberg, Apache Hudi 같은 오픈 테이블 포맷이 Data Lakehouse 구현의 핵심 기술입니다.

추가 설명)
| 구분 | Data Lake | Data Warehouse | Data Lakehouse |
|------|----------|---------------|---------------|
| 스토리지 비용 | 낮음 | 높음 | 낮음 |
| ACID 지원 | X | O | O |
| 스키마 | Schema-on-read | Schema-on-write | 혼합 |
| ML/비정형 지원 | O | 제한적 | O |
| 쿼리 성능 | 낮음 | 높음 | 중간~높음 |
| 대표 기술 | S3 + Hive | BigQuery, Redshift | Delta Lake, Iceberg |

참고 자료:
- [Delta Lake 공식 문서 - What is Delta Lake?](https://docs.delta.io/latest/delta-intro.html)

---

## [ROLE-058] Delta Lake, Apache Iceberg, Apache Hudi 같은 테이블 포맷이 필요한 이유를 설명해 주세요.

답변:
기존 Data Lake는 Parquet·ORC 같은 파일 포맷을 그대로 저장하면 ACID 트랜잭션, 동시성 제어, 스키마 진화가 지원되지 않아 데이터 정합성을 보장하기 어렵습니다. Delta Lake, Iceberg, Hudi는 오브젝트 스토리지 위에 트랜잭션 로그와 메타데이터 레이어를 추가하여 ACID, Time Travel, Schema Evolution, Partition Evolution을 지원합니다. 이로써 S3·GCS 같은 저비용 스토리지에서도 Data Warehouse 수준의 신뢰성 있는 데이터 관리가 가능해집니다.

추가 설명)
| 기능 | 기존 Data Lake (Parquet only) | Delta Lake / Iceberg / Hudi |
|------|------------------------------|----------------------------|
| ACID 트랜잭션 | X | O |
| Time Travel | X | O (버전별 조회 가능) |
| Schema Evolution | 수동 | O (자동 호환 관리) |
| Upsert/Delete | 파일 재작성 필요 | O (효율적으로 지원) |
| 동시 읽기/쓰기 | 위험 | O (낙관적 동시성 제어) |

참고 자료:
- [Apache Iceberg 공식 문서 - What is Iceberg?](https://iceberg.apache.org/docs/latest/)

---

## [ROLE-059] 데이터 품질 검증 규칙을 설계할 때 Null, 중복, 범위, 참조 무결성을 어떻게 확인할 수 있는지 설명해 주세요.

답변:
Null 검증은 필수 컬럼의 NULL 비율을 임계값과 비교하여 이상이 있으면 알림을 발생시키는 방식으로 구현합니다. 중복 검증은 Primary Key나 자연키 기준으로 COUNT와 COUNT(DISTINCT)를 비교하거나 Window Function으로 중복 레코드를 탐지합니다. 범위 검증은 숫자·날짜 컬럼이 허용된 범위 안에 있는지 확인하고, 참조 무결성 검증은 Foreign Key 값이 참조 테이블에 존재하는지 LEFT JOIN이나 NOT IN으로 확인합니다.

추가 설명)
| 품질 규칙 | 검증 방법 | 예시 |
|----------|----------|------|
| Null 검증 | NULL COUNT / 전체 COUNT 비율 | user_id NULL 비율 0% 유지 |
| 중복 검증 | COUNT vs COUNT(DISTINCT) | order_id 중복 없음 |
| 범위 검증 | MIN/MAX, BETWEEN 조건 | 나이: 0~150, 가격: 0 이상 |
| 참조 무결성 | LEFT JOIN + IS NULL | 주문의 user_id가 회원 테이블에 존재 |
| 형식 검증 | REGEX 패턴 매칭 | 이메일, 전화번호 형식 |
| 적시성 검증 | MAX(created_at) vs 현재 시각 | 최근 1시간 내 데이터 존재 |

참고 자료:
- [Apache Airflow 공식 문서 - Data Quality Checks](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html)

---

## [ROLE-060] 데이터 파이프라인 장애 발생 시 재처리와 Backfill을 안전하게 수행하는 방법을 설명해 주세요.

답변:
재처리를 안전하게 수행하려면 파이프라인이 멱등성을 갖도록 설계하여 같은 데이터를 여러 번 처리해도 결과가 달라지지 않아야 합니다. 적재 시 UPSERT나 파티션 단위 덮어쓰기를 사용하고, 실행 전 영향 범위를 확인하여 운영 환경과 격리된 상태에서 소규모 테스트를 먼저 수행하는 것이 안전합니다. Backfill 시에는 날짜 범위를 작게 나누어 단계적으로 수행하고, 각 단계의 결과를 검증한 후 다음 범위로 진행하여 오류 발생 시 영향 범위를 최소화합니다.

추가 설명)
```
[안전한 Backfill 절차]
1. 멱등성 확인: 재실행 시 결과가 동일한가?
2. 소규모 테스트: 1일치 데이터로 먼저 수행
3. 결과 검증: 레코드 수, 집계값, 이상 여부 확인
4. 단계적 확장: 주 단위 → 월 단위로 범위 확장
5. 운영 영향 최소화: 배치 작업 시간 분산, 리소스 제한
```

| 재처리 전 체크리스트 | 확인 내용 |
|------------------|----------|
| 멱등성 | UPSERT / 파티션 덮어쓰기 여부 |
| 영향 범위 | Downstream 테이블, 대시보드 영향 확인 |
| 실행 환경 격리 | 운영 파이프라인과 분리하여 실행 |
| 모니터링 | 재처리 중 이상 신호 실시간 확인 |
| Rollback 계획 | 실패 시 원복 방법 사전 준비 |

참고 자료:
- [Apache Airflow 공식 문서 - DAG Runs and Backfill](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html)

---

## 선택 질문

## [ROLE-061] Airflow에서 Scheduler, Executor, Worker, Metadata DB의 역할을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-062] Airflow DAG에서 재시도, Timeout, SLA, Sensor를 사용할 때 주의할 점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-063] 데이터 파이프라인에서 Idempotent하게 작업을 설계해야 하는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-064] 데이터 파이프라인에서 Checkpoint를 저장하는 이유와 장애 복구에 어떻게 활용되는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-065] Batch 처리와 Streaming 처리를 함께 사용하는 Lambda Architecture의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-066] Kappa Architecture가 무엇이고, Lambda Architecture와 비교했을 때 어떤 차이가 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-067] 데이터 모델링에서 Star Schema를 사용할 때 Fact Table과 Dimension Table을 어떻게 설계하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-068] Slowly Changing Dimension Type 1과 Type 2를 실제 예시로 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-069] 데이터 웨어하우스에서 Partition Pruning과 Predicate Pushdown이 성능에 어떤 영향을 주는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-070] Columnar Storage가 분석 쿼리에 유리한 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-071] Parquet 파일에서 Row Group, Column Chunk, Compression이 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-072] 데이터 카탈로그와 Data Lineage를 구축하면 어떤 문제를 해결할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-073] 개인정보가 포함된 데이터 파이프라인에서 마스킹, 익명화, 접근 제어를 어떻게 설계할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-074] 데이터 파이프라인 비용이 증가했을 때 Storage, Compute, Network 관점에서 어떤 항목을 점검할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-075] 실시간 이상 탐지 파이프라인에서 Kafka, Stream Processor, Feature Store, Alerting을 어떻게 연결할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-076] 로그 데이터와 이벤트 데이터를 분석 가능하게 만들기 위해 어떤 스키마 설계가 필요한지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-077] 데이터 파이프라인에서 데이터 지연이 발생했을 때 수집, 처리, 저장, 조회 단계별로 원인을 분석하는 방법을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-078] 데이터 파이프라인에서 품질 문제가 발견되었을 때 Downstream 영향 범위를 어떻게 파악할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-079] 데이터 엔지니어링 프로젝트에서 기술 선택의 trade-off를 설명할 때 어떤 기준을 사용할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-080] 본인의 데이터 엔지니어링 프로젝트를 면접에서 설명할 때 파이프라인 구조, 데이터 품질, 장애 대응, 성능 개선을 어떻게 구조화하면 좋을지 설명해 주세요.

답변:

참고 자료:
