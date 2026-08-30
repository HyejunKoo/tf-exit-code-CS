# Week 07 - Cloud / DevOps Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] 클라우드 아키텍처에서 고가용성을 설계할 때 단일 장애 지점을 어떻게 제거할 수 있는지 설명해 주세요.

답변:

단일 장애 지점은 그 하나가 멈추면 서비스 전체가 멈추는 지점입니다. 제거하려면 DNS, 로드 밸런서, 애플리케이션 서버, DB 같은 요청 경로의 각 계층마다 같은 역할의 대체 자원을 두어야 합니다. 이때 서버가 세션이나 파일을 자기 디스크에 들고 있으면 대체 자원으로 넘어갈 수 없으므로 상태를 외부로 빼고, 헬스 체크와 자동 전환을 붙여 감지부터 우회까지 자동으로 이뤄지게 해야 합니다.

참고 자료:
- https://velog.io/@sweet_sumin/단일-장애-지점이란-무엇인가요-SPOF

---

## [ROLE-042] Multi-AZ 구성에서 Load Balancer, Subnet, Instance 또는 Pod를 어떻게 배치해야 하는지 설명해 주세요.

답변:

가용 영역마다 퍼블릭과 프라이빗 서브넷을 한 쌍씩 만들어 최소 두 개 이상의 AZ에 대칭 구조를 잡습니다. 로드 밸런서는 퍼블릭 서브넷에 여러 AZ를 지정해 만들고, 실제 인스턴스는 프라이빗 서브넷에 두되 Auto Scaling 그룹에 여러 AZ의 서브넷을 등록해 AZ 간 고르게 분산합니다. 데이터베이스는 다른 AZ에 대기 인스턴스를 두는 Multi-AZ로 구성해 장애 시 자동 승격되게 합니다.

참고 자료:
- https://junhyunny.github.io/aws/aws-alb-and-target-group-setup/

---

## [ROLE-043] Auto Scaling 정책을 CPU, Memory, Request Count, Custom Metric 기준으로 설계할 때 각각의 장단점을 설명해 주세요.

답변:

CPU는 별도 설치 없이 수집되고 부하와 상관관계가 커 무난하지만, 외부 API 대기처럼 CPU를 안 쓰며 느려지는 워크로드는 확장되지 않습니다. Memory는 메모리 집약 서비스에 유효하나 정상 캐시와 누수를 구분하지 못하고, Request Count는 사용자 부하와 직결되지만 요청당 처리 비용이 다르면 실제 부하만큼 늘릴 수 없습니다. Custom Metric은 큐에 쌓인 메시지 수 같은 실제 병목을 쓸 수 있지만 수집 파이프라인을 따로 운영해야 하고 그게 죽으면 스케일링 판단을 할 수 없습니다.

참고 자료:
- https://velog.io/@mirrorkyh/Kubernetes-오토스케일링-HPAHorizontalPodAutoscaler

---

## [ROLE-044] 클라우드 환경에서 Scale Out과 Scale Up을 선택하는 기준을 설명해 주세요.

답변:

Scale Up은 장비 한 대의 사양을 올리는 방식이라 애플리케이션을 고치지 않아도 되지만 하드웨어 상한이 있고 교체 시 다운타임이 따릅니다. Scale Out은 장비를 늘려 부하를 나눠 확장 한계가 없지만 로드 밸런싱과 데이터 정합성을 직접 다뤄야 합니다. 상태를 공유하지 않는 구조인지와 병목이 단일 노드 성능인지 전체 처리량인지로 판단하며, 보통 대량 분석은 Scale Out, 빠른 단순 트랜잭션은 Scale Up이 유리합니다.

참고 자료:
- https://tech.gluesys.com/blog/2020/02/17/storage_3_intro.html

---

## [ROLE-045] Kubernetes에서 Node, Pod, ReplicaSet, Deployment의 관계를 설명해 주세요.

답변:

Node는 컨테이너가 실제로 도는 워커 머신이고, Pod는 컨테이너를 묶은 배포 최소 단위로 스케줄러가 Node에 배치합니다. ReplicaSet은 라벨 셀렉터로 대상 Pod를 골라 선언한 개수를 유지하고, Deployment는 그 상위에서 버전 업데이트와 롤백을 담당합니다. 즉 Deployment가 ReplicaSet을, ReplicaSet이 Pod를 만들고 Pod가 Node에서 실행되며, 운영 시엔 Deployment만 수정합니다.

참고 자료:
- https://velog.io/@squarebird/Kubernetes-Replica-Set과-Deployment

---

## [ROLE-046] Kubernetes Service의 ClusterIP, NodePort, LoadBalancer 타입 차이를 설명해 주세요.

답변:

ClusterIP는 기본 타입으로 내부 전용 가상 IP와 DNS 이름을 부여해 파드끼리만 통신하게 합니다. NodePort는 ClusterIP를 감싸 모든 노드에 같은 포트를 열어 외부 트래픽을 받고, LoadBalancer는 다시 감싸 클라우드 로드 밸런서를 자동 생성해 외부 IP를 부여합니다. 실제 노출엔 LoadBalancer를 쓰지만 서비스마다 만들면 비용이 늘어 보통 ClusterIP와 Ingress를 조합합니다.

참고 자료:
- https://velog.io/@pinion7/Kubernetes-리소스-Service에-대해-이해하고-실습해보기

---

## [ROLE-047] Kubernetes Ingress Controller가 필요한 이유와 LoadBalancer Service와의 차이를 설명해 주세요.

답변:

Ingress는 호스트나 경로에 따라 어느 서비스로 보낼지 적어 둔 규칙일 뿐, 이를 실행하는 컨트롤러를 쿠버네티스가 기본 제공하지 않아 NGINX Ingress Controller 같은 걸 따로 설치해야 동작합니다. LoadBalancer Service는 4계층에서 서비스마다 로드 밸런서를 만들어 외부 IP가 늘어나는 반면, Ingress는 7계층에서 로드 밸런서 하나를 공용 진입점으로 두고 호스트와 경로로 분기해 비용과 관리 포인트를 줄입니다. 대신 단일 진입점이라 컨트롤러를 이중화해야 합니다.

참고 자료:
- https://velog.io/@mrcocoball2/Kubernetes-Nginx-Ingress-Controller-활용해보기

---

## [ROLE-048] Kubernetes에서 ConfigMap과 Secret의 차이를 설명하고, Secret 사용 시 주의할 점을 설명해 주세요.

답변:

ConfigMap은 민감하지 않은 설정값을, Secret은 비밀번호나 토큰처럼 노출되면 안 되는 값을 주입하는 리소스로 주입 방식은 거의 같습니다. 주의할 점은 Secret의 base64가 암호화가 아니라 인코딩이라 조회 권한만 있으면 원문을 볼 수 있고 etcd에도 그대로 남는다는 것입니다. 그래서 etcd 암호화를 켜고 RBAC로 읽기 권한을 최소화하며, 매니페스트를 Git에 그대로 올리지 않고 외부 비밀 저장소와 연동합니다.

참고 자료:
- https://velog.io/@pinion7/Kubernetes-리소스-Secret에-대해-이해하고-실습해보기

---

## [ROLE-049] Kubernetes에서 Resource Request와 Limit이 무엇이고, 설정하지 않으면 어떤 문제가 발생할 수 있는지 설명해 주세요.

답변:

Request는 스케줄러가 파드를 어느 노드에 올릴지 정하는 최소 확보량이고, Limit은 컨테이너가 넘어설 수 없는 상한입니다. 둘 다 없으면 스케줄러가 실사용량을 몰라 노드에 파드를 너무 많이 배치하고, 한 파드가 자원을 독점하면 같은 노드의 다른 파드까지 느려집니다. 또 제한이 없는 파드는 QoS가 BestEffort라 자원 부족 시 가장 먼저 제거되고, CPU 기반 HPA는 Request를 분모로 계산해 Request가 없으면 오토스케일링이 동작하지 않습니다.

참고 자료:
- https://velog.io/@whdgnszz1/K8s-Request-Limit

---

## [ROLE-050] Kubernetes에서 OOMKilled가 발생하는 원인과 대응 방법을 설명해 주세요.

답변:

OOMKilled는 컨테이너가 메모리 Limit을 넘어 커널의 OOM Killer가 프로세스를 강제 종료한 상태입니다. CPU는 부족하면 스로틀링으로 늦추지만 메모리는 이미 쓴 만큼을 바로 반납할 수 없어 종료뿐이며, 원인은 Limit이 낮거나 메모리 누수, 트래픽 급증으로 나뉩니다. 노드 부족으로 밀려난 Evicted와 구분하고, 사용량이 계속 늘어나면 누수를 의심해 프로파일링하며, Limit만 올리면 반복되므로 실제 사용량을 보고 조정합니다.

참고 자료:
- https://velog.io/@tedigom/Production-환경에서-고려해야-할Kubernetes-이슈-트러블슈팅

---

## [ROLE-051] Kubernetes에서 Readiness Probe, Liveness Probe, Startup Probe의 차이를 설명해 주세요.

답변:

셋 다 kubelet이 컨테이너 상태를 확인하지만 실패 시 조치가 다릅니다. Liveness는 정상 동작 중인지를 보고 실패하면 재시작해 데드락처럼 응답 없는 상황을 복구하고, Readiness는 트래픽 받을 준비가 됐는지를 보고 실패해도 재시작하지 않고 Service 엔드포인트에서만 제외합니다. Startup은 초기화 완료를 확인해 성공 전까지 나머지 둘을 보류시켜, 기동이 오래 걸리는 앱이 준비 전에 Liveness로 계속 재시작되는 것을 막습니다.

참고 자료:
- https://velog.io/@rockwellvinca/kubernetes-상태를-확인하는-방법-Probe-startupProbe-livenessProbe-readinessProbe

---

## [ROLE-052] Rolling Update 과정에서 무중단 배포를 위해 고려해야 할 Kubernetes 설정을 설명해 주세요.

답변:

maxUnavailable을 0, maxSurge를 1 이상으로 두어 새 파드가 준비된 뒤 기존 파드를 내리게 하고 replicas는 2개 이상이어야 합니다. Readiness Probe로 초기화 전 요청 유입을 막고, 종료 시엔 preStop 훅으로 잠시 대기시키고 terminationGracePeriodSeconds를 그보다 길게 잡아 처리 중인 요청을 마무리할 시간을 줍니다. 실제로 preStop까지 넣어야 무중단이 되는 경우가 많고, 외부 로드 밸런서는 타깃 그룹 등록을 반영하는 readiness gate까지 확인합니다.

참고 자료:
- https://velog.io/@jjmoon4682/Kubernetes-무중단-배포

---

## [ROLE-053] CI/CD 파이프라인에서 Build, Test, Image Build, Push, Deploy 단계의 흐름을 설명해 주세요.

답변:

푸시가 발생하면 Build 단계에서 소스를 체크아웃해 의존성을 받아 컴파일해 결과물을 만듭니다. Test 단계에서 단위와 통합 테스트, 정적 분석을 돌려 실패하면 진행을 막고, Image Build에서 결과물을 이미지화하며 커밋 해시로 태그를 붙여 롤백 대상을 특정합니다. Push에서 레지스트리에 올리며 취약점을 스캔하고, Deploy에서 대상이 이미지를 받아 컨테이너를 교체하는데 GitOps면 파이프라인은 태그만 갱신하고 반영은 클러스터 배포 도구가 처리합니다.

참고 자료:
- https://velog.io/@soheelog/CICD-파이프라인-구축과-MSA-아키텍처

---

## [ROLE-054] 컨테이너 이미지를 빌드할 때 Multi-stage Build를 사용하는 이유를 설명해 주세요.

답변:

빌드에 필요한 JDK나 컴파일러는 실행 시엔 필요 없는데, 한 스테이지로 빌드하면 이것들이 레이어로 남아 최종 이미지에 포함됩니다. 도커 이미지는 레이어가 쌓이는 구조라 나중에 삭제해도 이전 레이어 용량은 남아 사후 정리로는 줄일 수 없습니다. Multi-stage Build는 빌드 전용 스테이지에서 만든 결과물만 COPY로 실행용 이미지에 복사해 빌드 레이어를 빼므로, 크기가 크게 줄고 공격 표면과 자격 증명 유출 위험도 함께 줄어듭니다.

참고 자료:
- https://velog.io/@jism0211/멀티스테이징-빌드로-도커-딥다이브

---

## [ROLE-055] 컨테이너 이미지 보안을 위해 점검해야 할 요소를 설명해 주세요.

답변:

먼저 최소 베이스 이미지를 쓰고 멀티스테이지 빌드로 런타임에 불필요한 빌드 도구를 제외해 공격 표면을 줄입니다. 포함된 패키지의 취약점을 Trivy 같은 스캐너로 검사해 CI에서 심각도가 높으면 빌드를 실패시키고, 실행은 root가 아닌 별도 사용자로 하며 민감 정보는 이미지에 하드코딩하지 않고 런타임에 주입합니다. 마지막으로 베이스 태그를 latest 대신 특정 버전으로 고정하고 서명으로 출처와 무결성을 검증합니다.

참고 자료:
- https://velog.io/@luckyprice1103/도커Docker-컨테이너-보안

---

## [ROLE-056] Infrastructure as Code에서 상태 파일 State가 중요한 이유와 관리 시 주의할 점을 설명해 주세요.

답변:

상태 파일은 코드에 선언한 리소스와 실제로 만들어진 리소스를 연결한 기록이라, 실행 시 이 기록과 코드를 비교해 무엇을 추가하고 삭제할지 판단합니다. 그래서 사라지거나 실제 인프라와 어긋나면 이미 있는 리소스를 다시 만들거나 운영 자원을 관리 대상에서 놓칩니다. 로컬이나 Git에 두면 잠금이 없어 충돌이 나고 민감한 값이 노출되므로, S3 같은 원격 백엔드에 버전 관리와 암호화, 잠금을 켜서 저장하고 환경 단위로 분리합니다.

참고 자료:
- https://velog.io/@kubernetes/Terraform-상태관리

---

## [ROLE-057] 클라우드 비용이 갑자기 증가했을 때 어떤 순서로 원인을 분석할 수 있는지 설명해 주세요.

답변:

먼저 조회 기간을 일 단위로 바꿔 언제부터 늘었는지 짚고, 서비스별로 묶어 어느 서비스가 늘었는지 좁힙니다. 다음으로 사용 유형과 리전 단위로 나눠 보는데, 컴퓨팅보다 데이터 전송이나 NAT 게이트웨이 요금이 원인인 경우가 많기 때문입니다. 여기서 비용 할당 태그로 어느 팀이나 리소스인지 확인하고, 그 시점의 배포 이력이나 트래픽과 대조해 원인을 확정한 뒤 예산 알림이나 VPC 엔드포인트로 재발을 막습니다.

참고 자료:
- https://velog.io/@jiyeon_hong/AWS-비용-절감을-위한-참고사항

---

## [ROLE-058] 모니터링 지표 중 CPU 사용률, Memory 사용률, Error Rate, Latency, Throughput이 각각 무엇을 의미하는지 설명해 주세요.

답변:

CPU 사용률은 확보한 연산 자원 중 실제로 쓰는 비율로 부족하면 스로틀링이 걸리고, Memory 사용률은 할당 대비 사용량이라 이미 쓴 만큼을 바로 반납할 수 없어 한계를 넘으면 프로세스가 종료됩니다. Error Rate는 실패한 요청 비율로 5xx뿐 아니라 200이지만 내용상 실패인 것도 봐야 하고, Latency는 응답까지 걸린 시간으로 평균보다 p95나 p99로 보며, Throughput은 단위 시간당 처리량으로 받는 부하의 크기를 나타냅니다. 앞 둘은 원인을 찾는 자원 지표, 뒤 셋은 사용자 체감 지표라 알람은 뒤쪽에 걸고 앞쪽은 원인 분석용으로 둡니다.

참고 자료:
- https://velog.io/@sororiri/서비스-모니터링

---

## [ROLE-059] 장애 대응에서 Alert의 임계값을 설정할 때 고려해야 할 점을 설명해 주세요.

답변:

CPU 같은 원인 지표보다 에러율이나 지연처럼 사용자 영향과 직결되는 증상 지표에 거는 편이 오탐이 적습니다. 값은 감이 아니라 평소 트래픽 패턴과 배치 시간대를 포함한 과거 데이터의 변동 폭을 보고 잡고, 순간 스파이크로 울리지 않게 조건이 일정 시간 유지될 때만 발생하도록 지속 시간을 둡니다. 또 즉시 호출과 티켓용 심각도를 나누고, 같은 원인으로 쏟아지지 않게 그룹화하며 조치할 게 없는 알람은 대시보드로 내립니다.

참고 자료:
- https://velog.io/@langoustine/prometheus-with-alertmanager-for-problem-detection

---

## [ROLE-060] 장애 발생 후 Postmortem을 작성하는 이유와 포함해야 할 내용을 설명해 주세요.

답변:

장애를 급하게 막고 끝내면 근본 원인이 남아 반복되므로, 왜 일어났는지를 기록으로 정리해 재발을 막으려고 작성합니다. 특정인을 탓하는 문서가 되면 구성원이 정보를 숨겨 정확한 분석이 불가능하므로, 비난하지 않는 것을 원칙으로 삼고 조직에 공개해 다른 팀도 같은 위험을 점검하게 합니다. 포함할 내용은 장애 요약과 영향 범위, 발생부터 탐지와 복구까지의 타임라인, 근본 원인과 트리거, 복구 방법, 잘된 점과 부족한 점, 담당자와 기한이 명시된 액션 아이템입니다.

참고 자료:
- https://brunch.co.kr/@svillustrated/13

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
