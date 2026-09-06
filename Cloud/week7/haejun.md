# Week 07 - Cloud / DevOps Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] 클라우드 아키텍처에서 고가용성을 설계할 때 단일 장애 지점을 어떻게 제거할 수 있는지 설명해 주세요.

답변: 장애 도메인을 나누고 여러 AZ에 인스턴스와 데이터를 분산.
LB, 자동 복구, 복제와 백업으로 한 구성 요소의 장애가 전체 장애로 이어지지 않게 해야함.

참고 자료: https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/

---

## [ROLE-042] Multi-AZ 구성에서 Load Balancer, Subnet, Instance 또는 Pod를 어떻게 배치해야 하는지 설명해 주세요.

답변: Load Balancer는 여러 AZ에 걸쳐 배치하고, 각 AZ의 독립 Subnet에 인스턴스 또는 Pod를 분산. 
특정 AZ에 의존하지 않도록 구성 필요.

참고 자료: https://kubernetes.io/docs/setup/best-practices/multiple-zones/

---

## [ROLE-043] Auto Scaling 정책을 CPU, Memory, Request Count, Custom Metric 기준으로 설계할 때 각각의 장단점을 설명해 주세요.

답변: CPU와 Memory는 설정이 쉽지만 실제 요청량을 직접 반영하지 못할 수 있음.
Request Count는 사용자 부하에 적합하고, Custom Metric은 업무 특성을 잘 반영하지만 수집과 기준 설계가 어렵다.

참고 자료: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

---

## [ROLE-044] 클라우드 환경에서 Scale Out과 Scale Up을 선택하는 기준을 설명해 주세요.

답변: Scale Out은 인스턴스 수를 늘려 가용성과 확장성을 높이고, (수평적 확장)
Scale Up은 한 인스턴스의 사양을 높여 구조를 단순하게 함. (수직적 확장)
무중단 확장과 장애 격리가 중요하면 Scale Out을 우선해야함.

참고 자료: https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html

---

## [ROLE-045] Kubernetes에서 Node, Pod, ReplicaSet, Deployment의 관계를 설명해 주세요.

답변: Node는 Pod를 실행하는 서버이고, Pod는 하나 이상의 컨테이너 단위. ReplicaSet은 원하는 Pod 수를 유지하며, Deployment는 ReplicaSet과 배포 버전을 관리.

참고 자료: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

---

## [ROLE-046] Kubernetes Service의 ClusterIP, NodePort, LoadBalancer 타입 차이를 설명해 주세요.

답변: ClusterIP는 클러스터 내부 전용, NodePort는 각 Node의 포트를 통한 외부 접근용. 
LoadBalancer는 클라우드 외부 로드 밸런서 혹은 MetalLB와 같은 LB 구현과 연결.

참고 자료: https://kubernetes.io/docs/concepts/services-networking/service/

---

## [ROLE-047] Kubernetes Ingress Controller가 필요한 이유와 LoadBalancer Service와의 차이를 설명해 주세요.

답변: Ingress Controller는 여러 HTTP 서비스의 라우팅, TLS 종료, 도메인 기반 규칙을 중앙에서 처리.
LoadBalancer Service가 보통 서비스마다 외부 로드 밸런서를 만드는 것과 다름.

참고 자료: https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/

---

## [ROLE-048] Kubernetes에서 ConfigMap과 Secret의 차이를 설명하고, Secret 사용 시 주의할 점을 설명해 주세요.

답변: ConfigMap은 일반 설정값, Secret은 비밀번호나 인증서 같은 민감 정보용. 
Secret도 기본적으로 암호화가 보장되지 않으므로 RBAC, 저장 시 암호화, 접근 최소화가 필요.

참고 자료: https://kubernetes.io/docs/concepts/configuration/secret/

---

## [ROLE-049] Kubernetes에서 Resource Request와 Limit이 무엇이고, 설정하지 않으면 어떤 문제가 발생할 수 있는지 설명해 주세요.

답변: Request는 스케줄링과 최소 보장 자원량에 사용되고, Limit은 컨테이너의 최대 사용량.
설정하지 않으면 스케줄링이 불안정하고 한 Pod의 과도한 사용이 다른 Pod에 영향을 줄 수 있습니다. (best-effort로 설정)
* Request != Limit으로 설정하면 Bustable / Request == Limit으로 설정시 Guaranteed.

참고 자료: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

---

## [ROLE-050] Kubernetes에서 OOMKilled가 발생하는 원인과 대응 방법을 설명해 주세요.

답변: 컨테이너가 Limit보다 많은 메모리를 사용하거나 Node 메모리가 부족하면 발생.
사용량과 Limit을 확인하고 누수나 과도한 캐시를 수정한 뒤 적절한 Request/Limit 설정 필요

참고 자료: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

---

## [ROLE-051] Kubernetes에서 Readiness Probe, Liveness Probe, Startup Probe의 차이를 설명해 주세요.

답변: Readiness는 트래픽을 받을 준비가 되었는지, Liveness는 계속 실행 가능한지 확인.
Startup은 느리게 시작하는 앱의 초기화가 끝날 때까지 다른 Probe를 보호. (시작시에 너무 오래걸리면 바로 Liveness 혹은 Readiness에 걸려서 재시작 될 가능성이 있기에)

참고 자료: https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/

---

## [ROLE-052] Rolling Update 과정에서 무중단 배포를 위해 고려해야 할 Kubernetes 설정을 설명해 주세요.

답변: `maxUnavailable`과 `maxSurge`를 조정하고 Readiness Probe를 설정해야 힘.
충분한 Replica 수와 PDB를 유지하며 graceful shutdown도 구성.

참고 자료: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

---

## [ROLE-053] CI/CD 파이프라인에서 Build, Test, Image Build, Push, Deploy 단계의 흐름을 설명해 주세요.

답변: 코드를 Build하고 Test로 검증한 뒤 컨테이너 Image를 빌드.
이미지를 Registry에 Push하고 승인 또는 자동화된 Deploy 단계에서 배포합니다.

참고 자료: https://docs.github.com/en/actions/use-cases-and-examples/deploying/deploying-docker-images

---

## [ROLE-054] 컨테이너 이미지를 빌드할 때 Multi-stage Build를 사용하는 이유를 설명해 주세요.

답변: Build 단계와 실행 단계를 분리해 최종 이미지에서 컴파일 도구와 소스 코드를 제거. 
이미지 크기, 공격 표면과 배포 시간을 줄일 수 있다.

참고 자료: https://docs.docker.com/build/building/multi-stage/

---

## [ROLE-055] 컨테이너 이미지 보안을 위해 점검해야 할 요소를 설명해 주세요.

답변: 취약점이 적은 최신 Base Image와 보안 패키지를 사용.
비밀정보를 넣지 않고 Non-root 실행, 이미지 서명, 취약점 스캔과 최소 권한을 적용.

참고 자료: https://docs.docker.com/build/building/best-practices/

---

## [ROLE-056] Infrastructure as Code에서 상태 파일 State가 중요한 이유와 관리 시 주의할 점을 설명해 주세요.

답변: State는 실제 인프라와 코드의 대응 관계를 저장해 변경 사항을 계산하는 기준이 됨.
원격 저장소, 잠금, 접근 제어, 암호화와 백업을 사용하고 동시 수정을 막아야 함.

참고 자료: https://developer.hashicorp.com/terraform/language/state

---

## [ROLE-057] 클라우드 비용이 갑자기 증가했을 때 어떤 순서로 원인을 분석할 수 있는지 설명해 주세요.

답변: 비용 대시보드에서 증가한 계정, 서비스, 리전, 태그를 확인해야 함. 
사용량과 배포·스케일링·데이터 전송 변화를 비교하고 원인을 수정한 뒤 예산 알림을 설정.

참고 자료: https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html

---

## [ROLE-058] 모니터링 지표 중 CPU 사용률, Memory 사용률, Error Rate, Latency, Throughput이 각각 무엇을 의미하는지 설명해 주세요.

답변: CPU와 Memory는 자원 사용량, Error Rate는 실패 요청 비율.
Latency는 응답 지연 시간, Throughput은 단위 시간당 처리량이며 함께 봐야 병목 판단 가능.

참고 자료: https://sre.google/sre-book/monitoring-distributed-systems/

---

## [ROLE-059] 장애 대응에서 Alert의 임계값을 설정할 때 고려해야 할 점을 설명해 주세요.

답변: 정상적인 변동과 실제 장애를 구분하도록 과거 추세와 사용자 영향을 기준으로 정해야 함.
임계값뿐 아니라 지속 시간, 여러 지표 조합, 심각도별 통지 대상을 함께 설계.

참고 자료: https://sre.google/sre-book/monitoring-distributed-systems/

---

## [ROLE-060] 장애 발생 후 Postmortem을 작성하는 이유와 포함해야 할 내용을 설명해 주세요.

답변: 문제 원인 파악 / 지식 공유 / 책임 전가 방지 / 미래 실패 방지
문제가 발생한 시점부터 종료까지의 과정을 상세히 기록하고
문제가 발생한 근본 원인을 찾고, 분석해야 함.
문제 해결을 위해 취했던 대응 조치를 분석하고, 개선점을 찾은 다음 교훈을 도출하고 미래 실패 방지를 위한 개선 작업을 정의해야 함.

참고 자료: https://newdevsimple.tistory.com/120

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