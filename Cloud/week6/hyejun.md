# Week 06 - Cloud / DevOps Role-Based Interview 1

---

## 제출 기준

- 필수 답변: ROLE-001 ~ ROLE-020
- 선택 답변: ROLE-021 ~ ROLE-040

---

## 필수 질문

## [ROLE-001] 클라우드 컴퓨팅이 무엇이고, 온프레미스 환경과 어떤 차이가 있는지 설명ㅅㅂ해 주세요.

답변: 클라우드 컴퓨팅은 서버, 스토리지, 데이터베이스 등 IT자원을 인터넷을 통해 필요한 만큼 사용하는 방식. 온프레미스는 물리장비를 직접 구매하고 관리하지만, 클라우드는 초기비용이 적고 자원을 빠르게 확장하거나 축소 가능. 

참고 자료:

---

## [ROLE-002] IaaS, PaaS, SaaS의 차이를 설명해 주세요.

답변: laaS는 서버와 네트워크 등 인프라 제공, PaaS는 애플리케이션 실행 환경까지 제공, SaaS는 완성된 소프트웨어 제공. 대표적 예는 각각 Ec2, Elastic Beanstalk, notion

참고 자료:

---

## [ROLE-003] Public Cloud, Private Cloud, Hybrid Cloud의 차이를 설명해 주세요.

답변:  Public Cloud는 클라우드 사업자의 인프라를 여러 고객이 논리적으로 분리해 사용하는 방식. private cloud는 한 조직이 전용으로 사용하는 환경, hybrid cloud는 두 환경을 함께 사용하는 방식

참고 자료:

---

## [ROLE-004] 클라우드에서 Region과 Availability Zone이 무엇인지 설명해 주세요.

답변: region은 서울, 도쿄처럼 클라우드 인프라가 위치한 물리적 지역이다. availability zone은 region 내부에서 전력과 네트워크가 독립된 데이터센터 단위이며 여러 az에 서비스를 분산하면 데이터센터 장애에 대응 가능

참고 자료:

---

## [ROLE-005] VPC가 무엇이고, 클라우드 네트워크에서 어떤 역할을 하는지 설명해 주세요.

답변: VPC는 클라우드에 구성하는 논리적으로 격리된 가상 네트워크. IP 주소 범위, Subnet, route table, gateway, security group 등을 설정해 리소스의 배치와 통신 경로를 제어한다.

참고 자료:

---

## [ROLE-006] Public Subnet과 Private Subnet의 차이를 설명해 주세요.

답변: public subnet은 internet gateway로 향하는 경로가 있어, 인터넷과 직접 통신 가능. private subnet은 인터넷과 직접 연결되지 않으며, 외부 통신이 필요하면 NAT Gateway 등을 사용한다.

참고 자료:

---

## [ROLE-007] Security Group과 Network ACL의 차이를 설명해 주세요.

답변: security group은 인스턴스 단위로 적용되는 stateful 방화벽이며 허용 규칙만 설정. network ACL은 Subnet단위로 적용되는 Stateless방화벽이며 허용과 거부 규칙 모두 설정 가능

참고 자료:

---

## [ROLE-008] NAT Gateway가 무엇이고, 어떤 상황에서 사용하는지 설명해 주세요.

답변: NAT Gateway는 Private Subnet의 리소스가 Public IP없이 인터넷으로 나갈 수 있게 한다. 패키지 다운로드, 외부 API 호출에 사용하며, 외부에서 시작한 연결이 Private Subnet으로 직접 들어오는 것은 허용하지 않음

참고 자료:

---

## [ROLE-009] Load Balancer가 무엇이고, L4 Load Balancer와 L7 Load Balancer의 차이를 설명해 주세요.

답변: 로드밸런서는 들어오는 트래픽을 여러 서버에 분산해 가용성과 확장성을 높인다. L4는 IP와 Port를 기준으로 TCP, UDP 트래픽을 처리하고, L7은 URL, Host, Header 등 HTTP 정보를 기준으로 요청 분기

참고 자료:

---

## [ROLE-010] Auto Scaling이 무엇이고, 어떤 기준으로 확장 또는 축소할 수 있는지 설명해 주세요.

답변: auto scaling은 시스템 부하에 따라 서버나 컨테이너 수를 자동으로 조정하는 기능. cpu, 메모리, 요청 수, queue 길이 등의 메트릭이나 정해진 시간표를 기준으로 확장하거나 축소 가능

참고 자료:

---

## [ROLE-011] IAM이 무엇이고, 최소 권한 원칙이 중요한 이유를 설명해 주세요.

답변: iam은 사용자와 시스템의 인증 및 클라우드 리소스 접근 권한을 관리하는 체계. 최소 권한 원칙은 필요한 권한만 부여해 계정 탈취나 실수로 인한 피해 범위를 줄이는 원칙.

참고 자료:

---

## [ROLE-012] Object Storage와 Block Storage의 차이를 설명해 주세요.

답변: object stroage는 파일을 객체단위로 저장하며 이미지, 로그, 백업에 적합. block storage는 서버에 디스크처럼 연결하며 운영체제나 데이터베이스처럼 빠른 읽기쓰기가 필요한 작업에 적합

참고 자료:

---

## [ROLE-013] 클라우드 환경에서 RDS와 같은 Managed Database를 사용하는 장단점을 설명해 주세요.

답변: managed database는 설치, 패치, 백업, 모니터링, 장애 조치 등의 운영부담을 줄일 수 있음. 반면 직접 구축한 환경보다 비용이 높을 수 있고 세부설정이 제한되며 특정 클라우드에 종속 가능

참고 자료:

---

## [ROLE-014] CDN이 무엇이고, 정적 리소스 제공에 어떤 장점이 있는지 설명해 주세요.

답변: cdn은 이미지, css, javascript같은 정적 리소스를 여러 지역의 edge서버에 캐싱해 사용자와 가까운 위치에서 제공. 이를 통해 응답 속도를 높이고 원본 서버의 트래픽과 부하를 줄일 수 있음

참고 자료:

---

## [ROLE-015] Docker가 무엇이고, VM과 비교했을 때 어떤 차이가 있는지 설명해 주세요.

답변: 도커는 애플리케이션과 실행 환경을 image로 패키징해 container로 실행하는 플랫폼. vm은 각각 guest os를 실행하지만 container은 host os의 kernal을 공유하므로 더 가볍고 빠르며 vm은 상대적으로 격리 수준이 높다.

참고 자료:

---

## [ROLE-016] Container Image와 Container의 차이를 설명해 주세요.

답변: 컨테이너 이미지는 애플리케이션 코드와 실행환경이 포함된 읽기 전용 템플릿. 컨테이너는 해당 이미지를 실제로 실행한 인스턴스이며 하나의 이미지로 여러 컨테이너를 만들 수 있음

참고 자료:

---

## [ROLE-017] Kubernetes가 무엇이고, 컨테이너 오케스트레이션이 필요한 이유를 설명해 주세요.

답변: 쿠버네티스는 컨테이너의 배포, 확장, 복구와 네트워크 연결을 자동화하는 오케스트레이션 플랫폼. 쿠버네티스 자체와 aws의 관리형 쿠버네티스 서비스인 eks는 구분해야 하며, 여러 컨테이너를 안정적으로 운영하기 위해 사용한다.

참고 자료:

---

## [ROLE-018] Kubernetes의 Pod, Deployment, Service의 역할을 설명해 주세요.

답변: 파드는 하나 이상의 컨테이너가 실행되는 쿠버네티스의 최소 배포 단위. deployment는 pod의 개수와 배포 버전을 관리하고, 서비스는 pod에 고정된 접근지점과 트래픽 분산 기능을 제공

참고 자료:

---

## [ROLE-019] CI/CD가 무엇이고, 빌드-테스트-배포 파이프라인의 흐름을 설명해 주세요.

답변: ci는 코드 변경을 자주 통합하고 자동으로 빌드, 테스트하는 과정이며, cd는 검증된 결과물을 배포 환경으로 전달하는 과정. 일반적으로 코드 push, 빌드, 테스트, 이미지 또는 artifact 생성, 저장, 배포 순서로 진행.

참고 자료:

---

## [ROLE-020] 무중단 배포가 무엇이고, Blue-Green 배포와 Rolling 배포의 차이를 설명해 주세요.

답변: 무중단 배포는 배포 중에도 사용자가 서비스를 계속 이용할 수 있게 하느 방식. blue-green은 기존 환경과 신규 환경을 별도로 구성한 뒤 트래픽을 전환하고, rolling은 기존 인스턴스를 일부씩 신규 버전으로 교체

참고 자료:

---

## 선택 질문

## [ROLE-021] 클라우드 비용 최적화를 위해 어떤 요소를 점검할 수 있는지 설명해 주세요.

답변: 사용하지 않는 리소스를 제거하고 실제 사용량에 맞게 인스턴스를 조정해야 한다. Auto Scaling, Reserved Instance, Savings Plans, Spot Instance, Storage 수명 주기를 활용하고 태그와 예산 알림으로 비용을 지속적으로 관리한다.

참고 자료:

---

## [ROLE-022] Infrastructure as Code가 무엇이고, Terraform 같은 도구를 사용하는 이유를 설명해 주세요.

답변: IaC는 콘솔에서 수행하던 인프라 구성을 코드로 정의하고 관리하는 방식이다. Terraform을 사용하면 변경 이력을 Git으로 관리하고 동일한 인프라를 반복적으로 구성해 수작업 오류와 환경 차이를 줄일 수 있다.

참고 자료:

---

## [ROLE-023] Immutable Infrastructure가 무엇인지 설명해 주세요.

답변: 모름

참고 자료:

---

## [ROLE-024] 클라우드 환경에서 Secret과 Credential을 안전하게 관리하는 방법을 설명해 주세요.

답변: Secret과 Credential은 코드나 Git에 저장하지 않고 Secrets Manager나 Vault 같은 전용 저장소에서 관리해야 한다. 접근 권한을 최소화하고 암호화, Rotation, 감사 로그를 적용하며 가능하면 장기 Access Key 대신 단기 Credential을 사용한다.

참고 자료:

---

## [ROLE-025] CloudWatch, Prometheus, Grafana 같은 모니터링 도구가 각각 어떤 역할을 할 수 있는지 설명해 주세요.

답변: CloudWatch는 AWS 리소스의 메트릭과 로그를 수집하고 알람을 제공한다. Prometheus는 시계열 메트릭을 수집·저장하고, Grafana는 Prometheus, CloudWatch, Loki 등의 데이터를 대시보드로 시각화한다.

참고 자료:

---

## [ROLE-026] 로그, 메트릭, 트레이싱의 차이를 설명해 주세요.

답변: 로그는 개별 이벤트의 상세 기록이고, 메트릭은 에러율이나 응답 시간처럼 시간에 따른 수치 데이터다. 트레이싱은 하나의 요청이 여러 서비스를 거치는 경로와 각 구간의 처리 시간을 추적한다.

참고 자료:

---

## [ROLE-027] 장애가 발생했을 때 클라우드 인프라 관점에서 어떤 순서로 원인을 분석할 수 있는지 설명해 주세요.

답변: 먼저 장애 시간, 영향 범위와 최근 배포·인프라 변경 사항을 확인한다. 이후 DNS, Load Balancer, 네트워크, 서버·Pod, 애플리케이션, Database 순서로 요청 경로를 점검하고 메트릭, 로그, 트레이싱을 이용해 원인을 좁힌다.

참고 자료:

---

## [ROLE-028] Health Check가 무엇이고, Load Balancer나 Kubernetes에서 왜 중요한지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-029] Readiness Probe와 Liveness Probe의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-030] Horizontal Scaling과 Vertical Scaling의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-031] 클라우드 환경에서 High Availability를 설계할 때 고려해야 할 요소를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-032] Backup과 Disaster Recovery의 차이를 설명하고, RTO와 RPO에 대해 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-033] Multi-AZ 구성과 Multi-Region 구성의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-034] 클라우드 네트워크에서 DNS와 TLS 인증서가 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-035] API Gateway와 Load Balancer의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-036] Message Queue를 클라우드 아키텍처에서 사용하는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-037] Kubernetes Ingress가 무엇이고, Service와 어떤 차이가 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-038] HPA가 무엇이고, 어떤 메트릭을 기준으로 동작할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-039] 클라우드 보안에서 네트워크 보안, 접근 제어, 데이터 암호화를 각각 어떻게 고려해야 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-040] 본인의 프로젝트에서 클라우드 인프라나 CI/CD를 구축했다면, 문제 상황, 선택 이유, 구성 방식, 결과를 어떻게 설명하면 좋을지 설명해 주세요.

답변:

참고 자료: