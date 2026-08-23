# Week 07 - Cloud / DevOps Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] 클라우드 아키텍처에서 고가용성을 설계할 때 단일 장애 지점을 어떻게 제거할 수 있는지 설명해 주세요.

답변: 단일 서버나 단일 AZ에 의존하지 않도록 로드밸런서 뒤에 인스턴스나 파드를 여러 AZ으로 분산. 데이터베이스도 Multi-AZ와 자동 장애 조치를 구성. 상태는 외부 저장소에 관리해 애플리케이션을 무상태로 설계. 또 장애 조치가 실제로 동작하는지 정기적으로 테스트   

참고 자료:

---

## [ROLE-042] Multi-AZ 구성에서 Load Balancer, Subnet, Instance 또는 Pod를 어떻게 배치해야 하는지 설명해 주세요.

답변: AZ별로 public subnet, private subnet을 구성하고, 외부 로드밸런서는 여러 AZ의 public subnet에 연결. 인스턴스와 kubernetes노드는 private subnet에 분산, pod도 pod Anti-Affinity나 Topology spread constraints를 이용해 여러 노드와 AZ에 배치. 한 AZ가 중단돼도 다른 AZ가 전체 요청을 처리할 수 있도록 용량도 확보해야 한다.

참고 자료:

---

## [ROLE-043] Auto Scaling 정책을 CPU, Memory, Request Count, Custom Metric 기준으로 설계할 때 각각의 장단점을 설명해 주세요.

답변: CPU는 수집이 많고 일반적이지만 I/O 중심 서비스의 실제 부하를 반영하지 못할 수 있음. Memory는 메모리 기반 서비스에 적합하지만, 사용량이 즉시 감소하지 않아 스케일인 기준으로 쓰기 어려움
request count는 트래픽을 직접 반영하지만 요청별 처리 비용이 다르면 정확도가 떨어짐
custom metric은 큐 길이, 처리 지연 등 서비스 특성을 가장 잘 반영하지만 지표 설계와 수집 시스템 필요

참고 자료:

---

## [ROLE-044] 클라우드 환경에서 Scale Out과 Scale Up을 선택하는 기준을 설명해 주세요.

답변: scale out은 인스턴스나 pod 수를 늘리는 방식으로, 무상태 서비스처럼 수평 분산이 가능한 환경에 적합, 가용성도 높일 수 있음. Scale up은 한 서버의 CPU와 메모리를 늘리는 방식으로, 수평 분산이 어려운 레거시 시스템이나 데이터베이스에 적합. 일반적으로 클라우드에서는 확장 한계와 장애 영향을 줄이기 위해 Scale out을 우선 고려

참고 자료:

---

## [ROLE-045] Kubernetes에서 Node, Pod, ReplicaSet, Deployment의 관계를 설명해 주세요.

답변: Node는 컨테이너가 실행되는 워커서버, pod는 하나 이상의 컨테이너를 포함하는 kubernetes의 최소 배포 단위. replicaset은 지정한 수의 pod가 유지되도록 관리. deployment는 replicaset을 관리하면서 선언적 배포, rolling update, rollback기능 제공

참고 자료:

---

## [ROLE-046] Kubernetes Service의 ClusterIP, NodePort, LoadBalancer 타입 차이를 설명해 주세요.

답변: clusterip: 클러스터 내부에서만 접근할 수 있는 기본 타입. NodePort: 모든 Node의 특정 포트를 열어 외부에서 접근하게 함. LoadBalancer: 클라우드의 외부 로드밸런서를 생성해 서비스를 외부에 노출
일반적으로 내부서비스에는 clusterIP,외부 서비스에는 LoadBalancer 또는 Ingress 사용

참고 자료:

---

## [ROLE-047] Kubernetes Ingress Controller가 필요한 이유와 LoadBalancer Service와의 차이를 설명해 주세요.

답변: Ingress는 host나 URL path기준으로 여러 서비스에 HTTP,HTTPS 요청을 라우팅하는 규칙. 실제로 이 규칙을 처리하려면 Ingress Controller가 필요. 로드밸런서 서비스는 보통 하나의 서비스를 외부 로드밸런서에 직접 연결. Ingress를 사용하면 하나의 load balancer로 여러 서비스를 연결, TLS 종료도 중앙 관리 가능

참고 자료:

---

## [ROLE-048] Kubernetes에서 ConfigMap과 Secret의 차이를 설명하고, Secret 사용 시 주의할 점을 설명해 주세요.

답변: ConfigMap은 일반 설정값을, Secret은 비밀번호, 토큰, 인증서 같은 민감 정보 저장. 하지만 Secret의 기본 Base64인코딩은 암호화가 아니므로, etcd 암호화와 최소권한 RBAC를 적용해야 함. Secret을 Git이나 컨테이너 이미지에 포함하지 않고, 필요하면 외부 Secret Managerㅇ르 사용하며 주기적으로 교체해야 함.

참고 자료:

---

## [ROLE-049] Kubernetes에서 Resource Request와 Limit이 무엇이고, 설정하지 않으면 어떤 문제가 발생할 수 있는지 설명해 주세요.

답변: Request는 스케줄링 시 컨테이너에 필요하다고 보장하는 자원, Limit은 컨테이너가 사용할 수 있는 최대 자원. CPU가 Limit을 넘으면 Throttling되고(느려짐), memory limit을 넘으면 OOMKilled될 수 있음. 설정하지 않으면 잘못된 노드에 파드가 배치되거나 특정 파드가 자원을 과점유해 다른 서비스까지 영향 받음

참고 자료:

---

## [ROLE-050] Kubernetes에서 OOMKilled가 발생하는 원인과 대응 방법을 설명해 주세요.

답변: 컨테이너의 메모리 사용량이 memory limit을 초과하거나 노드 전체의 메모리가 부족할 때 발생. kubectl describe pod, 이전 컨테이너 로그, 메모리 사용 추이를 확인해 메모리 사용 추이를 확인해 메모리 누수, 순간적인 사용량 증가, 부적절한 limit 설정 구분. 이후 애플리케이션을 최적화하거나 request와 limit을 조정하고 필요하면 pod또는 node 확장

참고 자료:

---

## [ROLE-051] Kubernetes에서 Readiness Probe, Liveness Probe, Startup Probe의 차이를 설명해 주세요.

답변: readiness probe는 요청을 받을 준비가 됐는지 확인. 실패하면 service의 트래픽 대상에서 제외. liveness probe는 컨테이너가 정상 동작 중인지 확인. 실패하면 컨테이너 재시작. startup probe는 초기 구동 여부 확인, 성공하기 전까지 다른 probe의 동작 지연시킴.

참고 자료:

---

## [ROLE-052] Rolling Update 과정에서 무중단 배포를 위해 고려해야 할 Kubernetes 설정을 설명해 주세요.

답변: 복제본을 2개 이상 두고 maxunavailable과 maxsurge를 적절히 설정. readiness probe를 통해 준비된 신규 pod에만 트래픽 전달, prestop과 terminationgraceperiodseconds로 기존 요청이 처리된 후 pod가 종료되도록 해야 한다. pod를 여러 노드, AZ에 분산하고 배포 실패 시 롤백할 수 있어야 한다.

참고 자료:

---

## [ROLE-053] CI/CD 파이프라인에서 Build, Test, Image Build, Push, Deploy 단계의 흐름을 설명해 주세요.

답변: 소스가 변경되면 먼저 코드를 컴파일하거나 패키징하는 빌드 수행, 단위, 통합 테스트로 정상 동작 검증. 이후 도커 이미지를 만들고 보안검사를 거쳐 Registry에 push. 마지막으로 배포 도구가 새 이미지 태그를 적용하고, 배포 후 health check와 모니터링을 통해 정상 여부 확인

참고 자료:

---

## [ROLE-054] 컨테이너 이미지를 빌드할 때 Multi-stage Build를 사용하는 이유를 설명해 주세요.

답변: 빌드 환경과 실행환경을 분리해 최종 이미지에는 실행에 필요한 파일만 포함하기 위해 사용. 컴파일러와 빌드도구가 제거되므로 이미지 크기, 전송 시간이 줄고 불필요한 패키지가 줄어 공격 표면도 작아짐

참고 자료:

---

## [ROLE-055] 컨테이너 이미지 보안을 위해 점검해야 할 요소를 설명해 주세요.

답변: 신뢰할 수 있는 최소 크기의 Base Image를 사용하고, 이미지 태그보다 버전이나 Digest를 고정하는 것이 좋다. 취약점 스캔과 SBOM 검사 수행, 불필요한 패키지와 권한 제거, non-root 사용자로 실행하게 해야함. 비밀번호나 API키를 이미지에 포함하지 않고, 서명된 이미지만 배포하도록 관리

참고 자료:

---

## [ROLE-056] Infrastructure as Code에서 상태 파일 State가 중요한 이유와 관리 시 주의할 점을 설명해 주세요.

답변: Terraform State는 코드에 정의된 리소스와 실제 클라우드 리소스의 연결 관계 저장. State가 유실되거나 동시에 수정되면 중복 생성이나 잘못된 변경 발생 가능 -> 원격 backend에 저장하고 locking과 버전관리를 적용해야 함. state에는 민감정보가 포함될 수 있어, 암호화와 접근 권한 관리도 필요

참고 자료:

---

## [ROLE-057] 클라우드 비용이 갑자기 증가했을 때 어떤 순서로 원인을 분석할 수 있는지 설명해 주세요.

답변: 먼저 cost explorer에서 증가시점, 서비스 리전 계정 유저타입 별 비용 비교. 다음으로 해당 시점의 배포, auto scaling, 트래픽 변화, 신규 리소스 생성 이력 확인. 원인이 된 리소스를 특정한 후 과도한 스케일링, 미사용 리소스, 데이터 전송량, 할인 적용 누락 등을 점검하고 조치. 이후 budget과 cost anomaly detection을 설정해 재발 감지

참고 자료: 

---

## [ROLE-058] 모니터링 지표 중 CPU 사용률, Memory 사용률, Error Rate, Latency, Throughput이 각각 무엇을 의미하는지 설명해 주세요.

답변: cpu 사용률은 컴퓨팅 자원을 얼마나 사용하고 있는지, memory 사용률은 메모리를 얼마나 점유하고 있는지를 의미. error rate는 요청 중 오류가 발생하는 비율. latency는 요청 처리에 걸리는 시간. throughput은 단위 시간당 처리할 수 있는 요청이나 작업의 양을 의미

참고 자료:

---

## [ROLE-059] 장애 대응에서 Alert의 임계값을 설정할 때 고려해야 할 점을 설명해 주세요.

답변: 임계값은 단순히 높은 수치로 설정하기보다 정상적인 트래픽과 사용 패턴을 기준으로 설정. 넘 낮으면 알람이 과도하게 발생하는 알람 피로가 생기고, 너무 높으면 실제 장애를 놓칠 수 있기 때문에 서비스 특성에 맞게 적절한 기준을 설정해야 한다.

참고 자료:

---

## [ROLE-060] 장애 발생 후 Postmortem을 작성하는 이유와 포함해야 할 내용을 설명해 주세요.

답변: 포스트모덤은 장애의 원인을 분석하고 같은 장애가 반복되지 않도록 개선하기 위해 작성. 발생시간과 영향범위 , 장애 원인, 대응 과정과 복구 과정, 그리고 재발 방지를 위한 개선 사항 포함

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