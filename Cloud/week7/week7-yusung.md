# Week 07 - Cloud / DevOps Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] 클라우드 아키텍처에서 고가용성을 설계할 때 단일 장애 지점을 어떻게 제거할 수 있는지 설명해 주세요.

답변:

- 단일 장애 지점 (Single Point of Failure, SPOF)
  - 하나의 컴포넌트가 장애 나면 전체 서비스가 중단되는 지점
  - 고가용성(HA) 설계의 핵심은 SPOF를 식별하고 이중화·분산으로 제거하는 것
- SPOF 제거 방법
  - Multi-AZ / Multi-Region: 동일 서비스를 여러 AZ·Region에 분산 배치
  - Load Balancer 이중화: ALB/NLB를 통해 여러 인스턴스·Pod에 트래픽 분산, 헬스체크로 장애 인스턴스 제외
  - DB 이중화: Primary-Standby(Active-Passive), Read Replica, Multi-AZ RDS
  - Stateless 설계: 애플리케이션 서버는 상태를 외부(Redis, DB)에 저장 → 인스턴스 교체·확장 용이
  - DNS Failover: Route 53 Health Check + Failover Routing으로 장애 Region 전환
- 계층별 점검
  - Compute: 최소 2개 이상 인스턴스/Pod, 다른 AZ에 분산
  - Network: LB, NAT Gateway, IGW 등 단일 구성 요소 이중화
  - Storage: S3(기본 11 9s), EBS Snapshot, Cross-Region Replication
  - DNS, CDN: Route 53, CloudFront 등 관리형 서비스의 HA 활용

참고 자료:

---

## [ROLE-042] Multi-AZ 구성에서 Load Balancer, Subnet, Instance 또는 Pod를 어떻게 배치해야 하는지 설명해 주세요.

답변:

- Multi-AZ (Multi-Availability Zone)
  - 하나의 Region 내 여러 AZ에 리소스를 분산하여 AZ 장애 시에도 서비스 유지
- Load Balancer 배치
  - ALB/NLB는 Region 레벨 서비스 → 별도 AZ 지정 없이 자동으로 Multi-AZ에 분산
  - Public Subnet에 배치, 각 AZ의 Target(인스턴스/Pod)에 트래픽 라우팅
  - Target Group에 여러 AZ의 인스턴스/Pod 등록 → AZ 장애 시 해당 Target만 제외
- Subnet 배치
  - Public Subnet: AZ마다 1개씩 (예: ap-northeast-2a, 2b, 2c) → LB, NAT Gateway
  - Private Subnet: AZ마다 1개씩 → 애플리케이션 서버, Pod Node
  - DB Subnet: AZ마다 1개씩 → RDS Multi-AZ, ElastiCache
- Instance / Pod 배치
  - 최소 2개 AZ에 균등 분산 (예: AZ-a 2대, AZ-b 2대)
  - Kubernetes: Node를 여러 AZ에 분산, Pod Anti-Affinity로 동일 AZ 집중 방지
  - Auto Scaling Group: 여러 AZ에 걸쳐 인스턴스 생성
- 트래픽 흐름
  - 사용자 → LB(Public) → App(Private, Multi-AZ) → DB(Private, Multi-AZ)

참고 자료:

---

## [ROLE-043] Auto Scaling 정책을 CPU, Memory, Request Count, Custom Metric 기준으로 설계할 때 각각의 장단점을 설명해 주세요.

답변:

- CPU 기준
  - CPU 사용률(예: 70% 초과) 임계값으로 Scale Out/In
  - 장점: 설정 간단, 대부분 워크로드에 적용 가능, CloudWatch 기본 제공
  - 단점: CPU-intensive가 아닌 I/O-bound, Memory-bound 워크로드에 부적합
  - 지연: CPU 포화 후 스케일링 → 반응이 늦을 수 있음
- Memory 기준
  - Memory 사용률로 스케일링 (CloudWatch Agent 또는 Container Insights 필요)
  - 장점: 메모리 누수, 캐시-heavy 애플리케이션에 적합
  - 단점: Memory 메트릭 수집 설정 추가 필요, CPU와 별도 정책 관리
- Request Count 기준
  - ALB Request Count per Target, RPS(Requests Per Second) 기준
  - 장점: 실제 트래픽 부하를 직접 반영, 사용자 경험과 밀접
  - 단점: 요청당 리소스 소비가 불균일하면 부정확 (무거운 요청 vs 가벼운 요청)
- Custom Metric 기준
  - 큐 길이, DB Connection Pool, 비즈니스 KPI 등 자체 정의 메트릭
  - 장점: 애플리케이션 특성에 맞는 정밀한 스케일링
  - 단점: 메트릭 발행·수집 파이프라인 구축 필요, 운영 복잡도 증가
- 실무 권장
  - 기본: CPU + Request Count 조합
  - Kubernetes HPA: CPU/Memory + Custom Metric(Prometheus Adapter)
  - Scale Out Cooldown, Scale In Cooldown 설정으로 thrashing 방지

참고 자료:

---

## [ROLE-044] 클라우드 환경에서 Scale Out과 Scale Up을 선택하는 기준을 설명해 주세요.

답변:

- Scale Up (Vertical Scaling, 수직 확장)
  - 기존 인스턴스의 CPU, Memory, 디스크 등 스펙을 증가
  - 예: t3.medium → t3.large, RDS 인스턴스 클래스 업그레이드
- Scale Out (Horizontal Scaling, 수평 확장)
  - 동일 스펙의 인스턴스·Pod를 추가하여 부하 분산
  - 예: EC2 2대 → 4대, Kubernetes Replica 3 → 6
- Scale Out 선택 기준
  - Stateless 애플리케이션, LB 뒤에 배치 가능
  - 트래픽 변동이 크고 Auto Scaling 필요
  - 단일 인스턴스 한계(인스턴스 타입 최대 스펙)에 도달
  - 고가용성 요구 (Multi-AZ, 장애 격리)
  - 클라우드 네이티브, MSA, Kubernetes 환경
- Scale Up 선택 기준
  - Stateful 워크로드 (DB, 단일 노드 Redis 등)
  - 애플리케이션이 분산 처리를 지원하지 않음
  - Scale Out 비용·복잡도가 Scale Up보다 클 때
  - 단기적·일시적 부하 (인스턴스 타입 변경이 더 빠름)
  - 라이선스·소프트웨어 제약 (단일 인스턴스만 지원)
- Trade-off
  - Scale Out: HA·탄력성 우수, Stateless 필수, 데이터 일관성 고려
  - Scale Up: 구현 단순, SPOF 위험, 하드웨어 한계 존재

참고 자료:

---

## [ROLE-045] Kubernetes에서 Node, Pod, ReplicaSet, Deployment의 관계를 설명해 주세요.

답변:

- Node (노드)
  - Kubernetes 클러스터를 구성하는 워커 머신 (VM 또는 물리 서버)
  - kubelet, kube-proxy, Container Runtime(docker, containerd) 실행
  - Pod가 실제로 스케줄링되어 실행되는 호스트
- Pod (파드)
  - Kubernetes에서 배포·스케줄링되는 최소 단위
  - 하나 이상의 컨테이너를 포함, 동일 Pod 내 컨테이너는 네트워크·스토리지 공유
  - Node 위에 스케줄링되어 실행
- ReplicaSet (레플리카셋)
  - 지정한 수(replicas)만큼 동일 Pod를 유지하는 컨트롤러
  - Pod 장애 시 자동으로 새 Pod 생성, desired state 유지
  - label selector로 관리할 Pod 식별
- Deployment (디플로이먼트)
  - ReplicaSet을 관리하는 상위 추상화
  - 선언적 업데이트, Rolling Update, Rollback 지원
  - Deployment → ReplicaSet → Pod 계층 구조
- 관계 요약
  - Deployment(배포 정의) → ReplicaSet(복제본 관리) → Pod(실행 단위) → Node(실행 환경)
  - Deployment spec 변경 → 새 ReplicaSet 생성 → 점진적 Pod 교체 (Rolling Update)
  - 직접 ReplicaSet 생성 가능하나, 일반적으로 Deployment 사용 권장

참고 자료:

---

## [ROLE-046] Kubernetes Service의 ClusterIP, NodePort, LoadBalancer 타입 차이를 설명해 주세요.

답변:

- Service
  - Pod는 IP가 동적으로 변경되므로, 안정적인 접근점(Cluster IP, DNS)을 제공
  - label selector로 Pod 그룹에 트래픽 라우팅
- ClusterIP (기본값)
  - 클러스터 내부에서만 접근 가능한 가상 IP
  - 클러스터 내부 서비스 간 통신 (Frontend → Backend API)
  - 외부에서 직접 접근 불가
- NodePort
  - 모든 Node의 특정 포트(30000~32767)에 Service 노출
  - `<NodeIP>:<NodePort>`로 클러스터 외부에서 접근 가능
  - Load Balancer 없이 간단한 외부 노출, 개발·테스트용
  - 단점: Node IP 변경 시 접근 불가, 포트 관리 부담
- LoadBalancer
  - 클라우드 LB(AWS ELB, GCP LB)를 자동 프로비저닝
  - 외부 IP/DNS로 Service 노출, 프로덕션 환경에 적합
  - NodePort + Cloud LB 조합으로 동작
- 선택 기준
  - 내부 통신: ClusterIP
  - 외부 노출(소규모): NodePort
  - 외부 노출(프로덕션): LoadBalancer 또는 Ingress

참고 자료:

---

## [ROLE-047] Kubernetes Ingress Controller가 필요한 이유와 LoadBalancer Service와의 차이를 설명해 주세요.

답변:

- Ingress Controller
  - HTTP/HTTPS 라우팅 규칙(host, path)을 정의하고, 외부 트래픽을 내부 Service로 라우팅
  - L7(애플리케이션 계층) 로드 밸런싱, SSL/TLS 종료, Virtual Host 지원
  - 예: nginx-ingress, AWS ALB Ingress Controller, Traefik
- 필요한 이유
  - Service LoadBalancer 타입: Service마다 LB 1개 → 비용·IP 낭비, L4 수준
  - Ingress: 하나의 LB/Ingress Controller로 여러 Service를 path/host 기반 라우팅
  - 예: example.com/api → api-service, example.com/web → web-service
  - SSL 인증서 중앙 관리, URL Rewrite, Rate Limiting 등 L7 기능
- LoadBalancer Service vs Ingress
  - LoadBalancer: L4(TCP/UDP), Service 1:1 LB, 클라우드 LB 자동 생성
  - Ingress: L7(HTTP/HTTPS), 여러 Service를 하나의 진입점으로, Ingress Controller 필요
  - Ingress Controller가 LoadBalancer 타입 Service를 생성하거나, 기존 LB와 연동
- 사용 패턴
  - 프로덕션: Ingress + Ingress Controller (ALB, nginx) → ClusterIP Service
  - gRPC, TCP 전용: LoadBalancer 또는 NLB Ingress

참고 자료:

---

## [ROLE-048] Kubernetes에서 ConfigMap과 Secret의 차이를 설명하고, Secret 사용 시 주의할 점을 설명해 주세요.

답변:

- ConfigMap
  - 비민감 설정 데이터(설정 파일, 환경 변수)를 key-value 형태로 저장
  - 예: app.properties, log level, feature flag, DB host(비밀번호 제외)
  - Pod에 Volume 마운트 또는 env로 주입
- Secret
  - 민감 정보(비밀번호, API Key, TLS 인증서) 저장
  - Base64 인코딩 (암호화 아님) → etcd에 저장 시 Encryption at Rest 설정 권장
  - Pod에 Volume/env로 주입, RBAC으로 접근 제어
- 차이
  - 용도: ConfigMap(일반 설정), Secret(민감 정보)
  - 저장: ConfigMap(평문), Secret(Base64, etcd 암호화 가능)
  - 접근: Secret은 RBAC, Audit Log 더 엄격히 관리
- Secret 사용 시 주의점
  - Base64 ≠ 암호화: etcd, YAML 파일에 평문에 가깝게 저장 → Git 커밋 금지
  - External Secret Operator, Vault, AWS Secrets Manager 등 외부 시크릿 관리 연동 권장
  - RBAC: Secret 접근 권한 최소화, ServiceAccount별 필요한 Secret만
  - env vs Volume: env는 프로세스/env 노출 위험, Volume 마운트가 상대적으로 안전
  - Rotation: Secret 변경 시 Pod 재시작 필요 (일부 Operator는 자동 reload)
  - Immutable Secret: 변경 불가로 설정하여 무단 수정 방지

참고 자료:

---

## [ROLE-049] Kubernetes에서 Resource Request와 Limit이 무엇이고, 설정하지 않으면 어떤 문제가 발생할 수 있는지 설명해 주세요.

답변:

- Resource Request (요청량)
  - Pod가 스케줄링될 때 Node에 "필요한" 최소 CPU, Memory
  - Scheduler가 Node 선택 시 Request 합계가 Node 가용량 이하인지 확인
  - 단위: CPU(millicore, 1000m=1 core), Memory(Mi, Gi)
- Resource Limit (상한)
  - Pod가 사용할 수 있는 CPU, Memory 최대치
  - Memory Limit 초과 → OOMKilled, CPU Limit 초과 → Throttling(제한)
- 설정하지 않을 때 문제
  - BestEffort QoS: Request/Limit 미설정 → Node 리소스 고갈 시 가장 먼저 Evict/OOMKilled
  - Noisy Neighbor: 한 Pod가 Node 전체 CPU/Memory 독점 → 다른 Pod 성능 저하
  - 스케줄링 비효율: Scheduler가 실제 필요량을 모름 → Node 과적/과소 적재
  - Auto Scaling 부정확: HPA, Cluster Autoscaler가 리소스 사용량 기준으로 동작 불가
  - Capacity Planning 어려움: 클러스터 규모 산정 불가
- 권장
  - Request: 실제 평균 사용량 기준, Limit: Request의 1.5~2배 (Memory는 특히 중요)
  - Burstable QoS: Request < Limit, Guaranteed QoS: Request = Limit (Critical Pod)

참고 자료:

---

## [ROLE-050] Kubernetes에서 OOMKilled가 발생하는 원인과 대응 방법을 설명해 주세요.

답변:

- OOMKilled (Out Of Memory Killed)
  - Pod의 Memory 사용량이 Limit을 초과하여 Linux OOM Killer가 컨테이너 프로세스를 강제 종료
  - `kubectl describe pod` → Last State: Terminated, Reason: OOMKilled
- 발생 원인
  - Memory Limit 설정이 실제 필요량보다 낮음
  - Memory Leak: 애플리케이션 버그로 Heap/GC 대상 메모리 증가
  - 트래픽 급증: 동시 요청·캐시 증가로 Memory 사용량 급증
  - JVM Heap: Java 등에서 -Xmx가 Limit보다 크거나, Limit 미고려
  - Sidecar 컨테이너: 메인 + Sidecar 합산 Memory가 Limit 초과
- 대응 방법
  - Limit 상향: 실제 사용량 모니터링(Prometheus, metrics-server) 후 Limit 조정
  - Memory Leak 분석: Heap Dump, 프로파일링으로 원인 코드 수정
  - Request/Limit 재설정: Request를 평균 사용량, Limit을 피크 대비 여유 있게
  - HPA: Memory 기준 Scale Out으로 Pod당 부하 분산
  - JVM: -Xmx를 Limit의 70~80% 이하로 설정 (Native Memory 여유)
  - Alert: Memory 사용률 80% 이상 시 알림, OOMKilled 이벤트 모니터링

참고 자료:

---

## [ROLE-051] Kubernetes에서 Readiness Probe, Liveness Probe, Startup Probe의 차이를 설명해 주세요.

답변:

- Liveness Probe (생존 프로브)
  - 컨테이너가 "살아 있는지" 확인
  - 실패 시 kubelet이 컨테이너 재시작
  - 용도: Deadlock, 무한 루프 등으로 응답 불가 상태 감지
  - 주의: 일시적 지연(부팅, GC)에 실패하면 불필요한 재시작 → Startup Probe 또는 initialDelaySeconds
- Readiness Probe (준비 프로브)
  - 컨테이너가 "트래픽을 받을 준비가 되었는지" 확인
  - 실패 시 Service Endpoints에서 제외 → 트래픽 차단 (재시작하지 않음)
  - 용도: DB 연결 대기, 캐시 워밍업, 의존성 준비 완료 전 트래픽 유입 방지
- Startup Probe (시작 프로브)
  - 컨테이너 "시작"이 완료되었는지 확인 (Kubernetes 1.16+)
  - Startup Probe 성공 전까지 Liveness/Readiness Probe 비활성화
  - 용도: 느린 시작 애플리케이션(Spring Boot, 대용량 데이터 로드)에서 Liveness 오판 방지
- 차이 요약
  - Liveness: 실패 → 재시작
  - Readiness: 실패 → 트래픽 제외
  - Startup: 실패 → 재시작, 성공 전 Liveness/Readiness 대기
- Probe 타입: HTTP GET, TCP Socket, Exec Command

참고 자료:

---

## [ROLE-052] Rolling Update 과정에서 무중단 배포를 위해 고려해야 할 Kubernetes 설정을 설명해 주세요.

답변:

- Rolling Update
  - Deployment strategy.type: RollingUpdate
  - 새 ReplicaSet의 Pod를 점진적으로 생성하고, 기존 Pod를 점진적으로 종료
- 무중단 배포를 위한 설정
  - maxSurge: 업데이트 중 추가로 생성 가능한 Pod 수 (예: 1 또는 25%)
  - maxUnavailable: 업데이트 중 unavailable 허용 Pod 수 (예: 0 → 무중단)
  - maxUnavailable: 0, maxSurge: 1 이상 → 새 Pod Ready 후 기존 Pod 종료
  - Readiness Probe: 새 Pod가 트래픽 받을 준비 완료 후에만 Endpoints 등록
  - preStop Hook: Pod 종료 전 graceful shutdown (연결 drain, cleanup)
  - terminationGracePeriodSeconds: SIGTERM 후 SIGKILL까지 대기 시간 (기본 30초)
- 추가 고려
  - Pod Disruption Budget (PDB): minAvailable 또는 maxUnavailable로 동시 종료 Pod 수 제한
  - minReadySeconds: Pod Ready 후 N초 대기 후 다음 Pod 교체 (안정성 검증)
  - revisionHistoryLimit: Rollback을 위한 이전 ReplicaSet 보관
  - Blue-Green/Canary: Argo Rollouts, Flagger로 더 정교한 배포 전략

참고 자료:

---

## [ROLE-053] CI/CD 파이프라인에서 Build, Test, Image Build, Push, Deploy 단계의 흐름을 설명해 주세요.

답변:

- CI/CD 파이프라인
  - 코드 커밋부터 프로덕션 배포까지 자동화된 단계별 흐름
- Build (빌드)
  - 소스 코드 컴파일, 의존성 설치 (npm install, mvn compile, go build)
  - Artifact 생성 (JAR, binary, static files)
  - Trigger: Git push, PR merge, webhook
- Test (테스트)
  - 단위 테스트, 통합 테스트, Lint, SAST(정적 분석) 실행
  - 실패 시 파이프라인 중단 → 배포 차단
  - Coverage, 품질 Gate 설정 가능
- Image Build (이미지 빌드)
  - Dockerfile 기반 컨테이너 이미지 생성 (docker build, kaniko, buildkit)
  - Multi-stage Build로 최종 이미지 크기 최소화
  - 태그: Git SHA, branch, semver
- Push (푸시)
  - 빌드된 이미지를 Container Registry(ECR, GCR, Docker Hub)에 업로드
  - 이미지 스캔(Trivy, Clair)으로 취약점 검사
- Deploy (배포)
  - Kubernetes: kubectl apply, Helm upgrade, Argo CD sync
  - ECS, Lambda 등 타겟 환경에 배포
  - Rolling Update, Blue-Green, Canary 전략 적용
- 흐름: Code Push → Build → Test → (통과) → Image Build → Push → Deploy → (Post-deploy Test)

참고 자료:

---

## [ROLE-054] 컨테이너 이미지를 빌드할 때 Multi-stage Build를 사용하는 이유를 설명해 주세요.

답변:

- Multi-stage Build
  - 하나의 Dockerfile 내에서 여러 FROM(Stage)을 정의
  - 빌드 Stage와 실행 Stage를 분리, 최종 Stage만 결과 이미지에 포함
- 사용 이유
  - 이미지 크기 감소: 빌드 도구(Go compiler, Maven, node_modules)를 최종 이미지에서 제외
  - 보안: 불필요한 패키지·도구 제거 → 공격 표면 축소
  - 레이어 캐시: 빌드 Stage와 실행 Stage 분리로 캐시 효율 향상
- 예시 (Go)
  - Stage 1 (builder): golang:1.21 → go build
  - Stage 2 (runtime): alpine:3.18 → builder에서 binary만 COPY
  - 결과: 800MB → 20MB 수준으로 축소
- 예시 (Node.js)
  - Stage 1: node:20 → npm install, npm run build
  - Stage 2: nginx:alpine → dist/만 COPY
- Best Practice
  - 최종 Stage는 distroless, alpine 등 최소 베이스 이미지
  - .dockerignore로 불필요 파일 제외

참고 자료:

---

## [ROLE-055] 컨테이너 이미지 보안을 위해 점검해야 할 요소를 설명해 주세요.

답변:

- 베이스 이미지
  - 공식·신뢰할 수 있는 이미지 사용 (Docker Official, Distroless)
  - 최소 베이스 (Alpine, Distroless) → 공격 표면 축소
  - 정기적 업데이트, EOL 이미지 사용 금지
- 취약점 스캔
  - CI/CD에 Trivy, Clair, Snyk 등 이미지 스캔 통합
  - Critical/High CVE 발견 시 빌드 실패 또는 알림
  - 베이스 이미지, OS 패키지, 애플리케이션 의존성 스캔
- Dockerfile 보안
  - root 사용자 실행 지양 → non-root USER 지정
  - 불필요 패키지 설치 금지, Multi-stage Build로 빌드 도구 제외
  - Secret, .env를 이미지에 포함하지 않음
  - COPY 대신 ADD 지양 (외부 URL fetch 위험)
- 런타임 보안
  - Read-only Root Filesystem
  - Resource Limit, Security Context (runAsNonRoot, allowPrivilegeEscalation: false)
  - Pod Security Standards (Restricted, Baseline)
- 레지스트리·배포
  - Private Registry, 이미지 서명(Cosign), SBOM(Software Bill of Materials)
  - Admission Controller로 서명·스캔 통과 이미지만 배포 허용

참고 자료:

---

## [ROLE-056] Infrastructure as Code에서 상태 파일 State가 중요한 이유와 관리 시 주의할 점을 설명해 주세요.

답변:

- State (상태 파일, Terraform State)
  - IaC 도구(Terraform)가 관리하는 실제 인프라의 현재 상태를 저장한 파일
  - 리소스 ID, 속성, 의존 관계 매핑
- 중요한 이유
  - Plan/Apply 기준: State와 코드(.tf) diff → 변경 사항 계획
  - 리소스 추적: Terraform이 생성한 리소스와 실제 클라우드 리소스 매핑
  - 의존성 관리: 리소스 간 의존 순서 결정 (Create/Destroy 순서)
  - 팀 협업: 공유 State로 동일 인프라 상태 인식
- 관리 시 주의점
  - Remote State: S3, GCS, Terraform Cloud 등에 저장, Local State 금지 (팀 환경)
  - State Locking: DynamoDB 등으로 동시 Apply 방지
  - State에 민감 정보 포함 가능 → 암호화, 접근 제어, Git 커밋 금지
  - State 분리: 환경(dev/staging/prod), 팀/프로젝트별 State 분리
  - Import/Refresh: 수동 변경된 리소스는 terraform import, refresh로 State 동기화
  - Backup: State 버전 관리(S3 versioning), 삭제 시 복구 가능하도록

참고 자료:

---

## [ROLE-057] 클라우드 비용이 갑자기 증가했을 때 어떤 순서로 원인을 분석할 수 있는지 설명해 주세요.

답변:

- 1. 비용 대시보드 확인
  - AWS Cost Explorer, GCP Billing, Azure Cost Management
  - 기간별·서비스별·리소스별 비용 추이, 전월/전주 대비 증가 구간
- 2. 서비스별 Top 비용 식별
  - EC2, RDS, S3, Data Transfer, NAT Gateway, Lambda 등
  - Cost Allocation Tag, Cost Center별 분류
- 3. 리소스별 상세 분석
  - EC2: 미사용·과다 스펙 인스턴스, Spot/Reserved 미적용
  - RDS: Multi-AZ, 스토리지, Read Replica
  - S3: Storage Class, Lifecycle 미적용, Request 비용
  - Data Transfer: Cross-Region, Cross-AZ, NAT Gateway 트래픽
- 4. Auto Scaling·이벤트 확인
  - Scale Out 과다, 트래픽 급증, DDoS, 봇 트래픽
  - CloudWatch, Auto Scaling Group 이벤트
- 5. 신규·변경 리소스
  - 최근 배포, IaC 변경, 수동 생성 리소스
  - Cost Anomaly Detection 알림 확인
- 6. 최적화·예방
  - Right Sizing, Reserved Instance, Spot Instance
  - S3 Lifecycle, Idle Resource 정리, Budget·Alert 설정

참고 자료:

---

## [ROLE-058] 모니터링 지표 중 CPU 사용률, Memory 사용률, Error Rate, Latency, Throughput이 각각 무엇을 의미하는지 설명해 주세요.

답변:

- CPU 사용률 (CPU Utilization)
  - 프로세스·인스턴스가 CPU를 사용하는 비율 (%)
  - 높으면: 연산 부하, Scale Out/Up 검토
  - 낮으면: 과다 프로비저닝, Right Sizing 가능
- Memory 사용률 (Memory Utilization)
  - RAM 사용량 / 전체 RAM (%)
  - 높으면: OOM 위험, Memory Leak, Scale Out 검토
  - Swap 사용 시 성능 급격히 저하
- Error Rate (에러율)
  - 전체 요청 중 실패(4xx, 5xx) 비율 (%)
  - 서비스 품질, 배포 문제, 의존성 장애 감지
  - SLO, Alert의 핵심 지표
- Latency (지연 시간)
  - 요청부터 응답까지 소요 시간 (ms, p50/p95/p99)
  - 사용자 경험 직결, SLA/SLO 기준
  - p99: 상위 1% 느린 요청 (꼬리 지연)
- Throughput (처리량)
  - 단위 시간당 처리 요청 수 (RPS, TPS)
  - 시스템 용량, 부하 수준 파악
  - Latency와 Trade-off (Little's Law)
- Golden Signals (Google SRE): Latency, Traffic(Throughput), Errors, Saturation(CPU/Memory)

참고 자료:

---

## [ROLE-059] 장애 대응에서 Alert의 임계값을 설정할 때 고려해야 할 점을 설명해 주세요.

답변:

- Alert Fatigue 방지
  - 너무 낮은 임계값 → Alert 폭주, 실제 장애 놓침
  - Actionable Alert만: 알림 받으면 조치할 수 있는 것만
- 임계값 설정 고려
  - Baseline: 정상 구간의 평균·표준편차 파악 (예: 평소 CPU 30%, 피크 60%)
  - 여유(Margin): 70% 임계값 → 60%에서 Alert 시 false positive 증가
  - Duration: 1분 vs 5분 지속 시 Alert (일시적 스파이크 무시)
  - Severity: Critical(즉시 대응) vs Warning(모니터링) 분리
- 지표별 특성
  - CPU/Memory: Saturation, 80% 이상 지속 시
  - Error Rate: 1% → 5% 급증, 절대값 + 변화율
  - Latency: p99 500ms → 2s, SLO 기준
  - Availability: Health Check 실패 N회 연속
- SLO 기반 Alert
  - Error Budget 소진 속도, SLO 위반 예측 Alert
  - Multi-window, Multi-burn-rate (Google SRE)
- 테스트·튜닝
  - Alert 발생 후 False Positive/Negative 검토
  - On-call 피드백 반영, 임계값 주기적 재검토

참고 자료:

---

## [ROLE-060] 장애 발생 후 Postmortem을 작성하는 이유와 포함해야 할 내용을 설명해 주세요.

답변:

- Postmortem (사후 분석, 장애 보고서)
  - 장애 발생 후 원인·영향·대응·재발 방지를 문서화하는 과정
  - Blameless: 개인 비난 없이 시스템·프로세스 개선에 초점
- 작성 이유
  - 재발 방지: 근본 원인(Root Cause) 파악, Action Item 도출
  - 지식 공유: 팀·조직 전체가 장애 맥락·교훈 학습
  - 프로세스 개선: 모니터링, Runbook, On-call 절차 보완
  - 신뢰 구축: 투명한 공유로 이해관계자 신뢰
- 포함해야 할 내용
  - Summary: 장애 한 줄 요약, 심각도, 영향 범위
  - Timeline: 발생·감지·대응·복구 시각 (UTC)
  - Impact: 영향받은 사용자 수, 서비스, 비즈니스 영향
  - Root Cause: 직접 원인 + 근본 원인 (5 Whys)
  - Detection: 어떻게 발견했는지, Alert·모니터링 개선점
  - Response: 대응 과정, 잘된 점, 개선점
  - Action Items: 재발 방지 작업 (담당자, 기한)
  - Lessons Learned: 배운 점, 프로세스 변경 사항

참고 자료:

---

## 선택 질문

## [ROLE-061] VPC Peering, PrivateLink, VPN의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-062] Public IP, Private IP, Elastic IP 또는 Static IP의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-063] 클라우드 네트워크에서 Route Table이 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-064] Bastion Host가 무엇이고, 어떤 보안상 장단점이 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-065] Blue-Green 배포, Canary 배포, Rolling 배포의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-066] Canary 배포에서 어떤 지표를 보고 배포를 계속 진행하거나 중단할지 판단할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-067] 서비스 장애 시 Rollback과 Rollforward 중 어떤 방식을 선택할지 판단하는 기준을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-068] Kubernetes HPA와 Cluster Autoscaler의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-069] Kubernetes에서 Pod Disruption Budget이 무엇이고, 왜 필요한지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-070] Kubernetes에서 StatefulSet과 Deployment의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-071] 클라우드 환경에서 로그 수집 파이프라인을 어떻게 구성할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-072] OpenTelemetry가 무엇이고, 분산 추적에서 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-073] SLO, SLI, SLA의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-074] Error Budget이 무엇이고, 운영과 배포 의사결정에 어떻게 활용할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-075] 클라우드 환경에서 Backup, Snapshot, Replication을 각각 어떤 상황에서 사용하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-076] Disaster Recovery 전략에서 Pilot Light, Warm Standby, Active-Active의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-077] 클라우드 보안에서 IAM Role, Policy, Group, User의 관계를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-078] 클라우드 환경에서 암호화 at-rest와 in-transit의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-079] 운영 중인 서비스의 Latency가 갑자기 증가했을 때 인프라 관점에서 어떤 순서로 확인할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-080] 본인의 클라우드 또는 DevOps 프로젝트를 면접에서 설명할 때 아키텍처, 배포 흐름, 모니터링, 장애 대응을 어떻게 구조화하면 좋을지 설명해 주세요.

답변:

참고 자료:
