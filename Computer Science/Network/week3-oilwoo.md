# Week 03 - Network / Web / I/O

---

## 제출 기준

- 필수 답변: COMMON-081 ~ COMMON-100
- 선택 답변: COMMON-101 ~ COMMON-120

---

## 필수 질문

## [COMMON-081] OSI 7계층과 TCP/IP 4계층을 비교해 설명해 주세요.

답변:
- 쉽게 한 줄로 설명하면, **OSI 7계층은 네트워크 통신 과정을 이해하기 위해 7단계로 나눈 참조 모델**이고, **TCP/IP 4계층은 실제 인터넷에서 사용하는 프로토콜을 중심으로 4단계로 나눈 모델**입니다.
- 두 모델 모두 복잡한 네트워크 통신을 역할별 계층으로 분리합니다. 따라서 한 계층의 구현이 바뀌더라도 다른 계층에 미치는 영향을 줄일 수 있고, 장애가 발생한 위치도 계층별로 확인할 수 있습니다.

| OSI 7계층 | TCP/IP 4계층 | 주요 역할 및 프로토콜 |
| --- | --- | --- |
| 응용·표현·세션 계층 | 애플리케이션 계층 | 사용자에게 네트워크 서비스 제공, HTTP·DNS |
| 전송 계층 | 전송 계층 | 프로세스 간 데이터 전송, TCP·UDP |
| 네트워크 계층 | 인터넷 계층 | IP 주소 지정과 라우팅, IP |
| 데이터 링크·물리 계층 | 네트워크 접근 계층 | 같은 네트워크 안에서 실제 데이터 전송, Ethernet·Wi-Fi |

- OSI 모델은 프로토콜의 역할을 설명하고 네트워크를 학습하거나 장애를 분석할 때 유용한 이론적 기준입니다.
- TCP/IP 모델은 OSI의 일부 계층을 합쳐 단순화했으며, 현대 인터넷의 실제 통신에 사용되는 프로토콜 체계를 설명합니다.
- 예를 들어 웹사이트에 접속하면 애플리케이션 계층의 HTTP 데이터가 전송 계층의 TCP, 인터넷 계층의 IP, 네트워크 접근 계층의 Ethernet이나 Wi-Fi를 거쳐 전달됩니다.

참고 자료:
- https://www.ibm.com/kr-ko/think/topics/osi-model
- https://www.cloudflare.com/ko-kr/learning/ddos/glossary/open-systems-interconnection-model-osi/

---

## [COMMON-082] TCP와 UDP의 차이에 대해 설명해 주세요.

답변:
- TCP와 UDP는 애플리케이션의 데이터를 목적지 프로세스까지 전달하는 전송 계층 프로토콜입니다.

| 구분 | TCP | UDP |
| --- | --- | --- |
| 연결 방식 | 연결 지향형 | 비연결형 |
| 신뢰성 | 순서·전달 여부를 확인하고 손실 시 재전송 | 전달·순서·중복 제거를 보장하지 않음 |
| 제어 기능 | 흐름 제어와 혼잡 제어 제공 | 기본적으로 제공하지 않음 |
| 데이터 형태 | 경계가 없는 바이트 스트림 | 메시지 경계를 유지하는 데이터그램 |
| 특징 | 제어 과정과 헤더로 인한 오버헤드가 있지만 신뢰성이 높음 | 오버헤드가 작아 낮은 지연 시간이 필요한 통신에 유리 |

- TCP는 연결 전에 Handshake를 수행하고, Sequence Number와 ACK를 이용해 데이터를 순서대로 전달합니다. 웹 통신, 파일 전송처럼 데이터의 정확성이 중요한 상황에 적합합니다.
- UDP는 연결 설정 없이 데이터그램을 바로 전송합니다. 실시간 게임, 음성·영상 통신, DNS처럼 일부 손실보다 지연 시간이 더 중요한 상황에 주로 사용됩니다.
- 다만 UDP 자체가 신뢰성을 제공하지 않을 뿐, QUIC처럼 애플리케이션 또는 상위 프로토콜에서 재전송과 순서 보장 기능을 구현할 수 있습니다.

참고 자료:
- https://www.cloudflare.com/ko-kr/learning/network-layer/internet-protocol/
- https://docs.aws.amazon.com/ko_kr/wellarchitected/2022-03-31/framework/perf_select_network_protocols.html

---

## [COMMON-083] TCP 3-way handshake가 무엇이고, 왜 필요한지 설명해 주세요.

답변:
- TCP 3-way Handshake는 데이터를 전송하기 전에 클라이언트와 서버가 **서로 통신 가능한 상태인지 확인하고 초기 Sequence Number를 동기화하는 연결 설정 과정**입니다.
- 연결 과정은 다음과 같습니다.
  1. 클라이언트가 초기 Sequence Number `x`를 담은 `SYN`을 서버에 보냅니다.
  2. 서버는 이를 확인하고 자신의 초기 Sequence Number `y`와 `ACK=x+1`을 담은 `SYN+ACK`를 보냅니다.
  3. 클라이언트가 `ACK=y+1`을 보내면 양쪽 모두 연결이 성립했음을 확인하고 데이터를 전송합니다.
- 두 번만 주고받으면 클라이언트는 서버의 응답을 확인할 수 있지만, 서버는 자신의 응답이 클라이언트에게 도착했는지 알 수 없습니다. 마지막 ACK까지 받아야 양방향 통신 가능 여부를 모두 확인할 수 있습니다.
- 초기 Sequence Number를 교환하면 이후 데이터의 순서와 손실 여부를 판단할 수 있고, 과거 연결에서 늦게 도착한 패킷을 현재 연결의 데이터로 잘못 처리하는 문제도 줄일 수 있습니다.

참고 자료:
- https://www.cloudflare.com/ko-kr/learning/ddos/what-is-an-ack-flood/
- https://www.cloudflare.com/ko-kr/learning/ddos/syn-flood-ddos-attack/

---

## [COMMON-084] TCP 4-way handshake가 무엇이고, 연결 종료 과정에서 TIME_WAIT이 필요한 이유를 설명해 주세요.

답변:
- TCP 4-way Handshake는 TCP 연결의 양방향 데이터 전송을 각각 종료하는 과정입니다. TCP는 Full-Duplex 통신을 사용하므로 한쪽의 전송 종료와 반대쪽의 전송 종료를 따로 처리합니다.
- 연결 종료 과정은 다음과 같습니다.
  1. 먼저 종료하려는 쪽이 더 보낼 데이터가 없다는 의미로 `FIN`을 보냅니다.
  2. 상대방은 `ACK`를 보내고, 아직 남은 데이터가 있다면 계속 전송합니다.
  3. 상대방도 전송을 마치면 `FIN`을 보냅니다.
  4. 처음 종료를 요청한 쪽이 마지막 `ACK`를 보낸 후 `TIME_WAIT` 상태로 들어갑니다.
- `FIN`과 `ACK`가 분리되는 이유는 상대방이 `FIN`을 받았더라도 아직 전송할 데이터가 남아 있을 수 있기 때문입니다.
- `TIME_WAIT`은 마지막 ACK가 유실됐을 때 상대방의 FIN 재전송을 받아 ACK를 다시 보내기 위해 필요합니다.
- 또한 과거 연결의 지연된 패킷이 네트워크에서 모두 사라질 때까지 기다려, 같은 IP와 Port 조합으로 만든 새로운 연결에 이전 패킷이 섞이는 것을 방지합니다. 일반적으로 최대 세그먼트 수명인 MSL의 두 배 동안 유지됩니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/windows/win32/api/mstcpip/ne-mstcpip-tcpstate
- https://learn.microsoft.com/ko-kr/azure/virtual-network/virtual-network-tcpip-performance-tuning

---

## [COMMON-085] TCP의 신뢰성 보장 방법에 대해 설명해 주세요.

답변:
- TCP는 다음과 같은 기능을 조합하여 데이터가 손실되거나 순서가 바뀔 수 있는 IP 네트워크 위에서 신뢰성 있는 전송을 제공합니다.
- **Sequence Number**를 이용하여 데이터를 올바른 순서로 재조립하고 중복된 데이터를 구분합니다.
- 수신자는 데이터를 받으면 **ACK**를 보내고, 송신자는 일정 시간 안에 ACK를 받지 못하거나 중복 ACK를 통해 손실을 감지하면 해당 데이터를 재전송합니다.
- **Checksum**을 이용해 전송 중 세그먼트의 내용이 손상됐는지 검사하고, 손상된 세그먼트는 폐기해 재전송되도록 합니다.
- **흐름 제어**는 수신 측의 버퍼 크기에 맞춰 송신량을 조절하여 수신자가 처리할 수 있는 양보다 많은 데이터가 전송되는 것을 막습니다.
- **혼잡 제어**는 네트워크 상태에 따라 전송량을 조절하여 네트워크가 감당하기 어려울 정도로 트래픽이 몰리는 것을 방지합니다.
- 예를 들어 세 개의 세그먼트 중 두 번째가 손실되면 수신자는 정상적인 순서로 처리하지 않고, 송신자는 ACK와 타이머를 통해 손실을 감지해 두 번째 세그먼트를 다시 보냅니다.

참고 자료:
- https://www.cloudflare.com/ko-kr/learning/network-layer/internet-protocol/
- https://learn.microsoft.com/ko-kr/azure/virtual-network/virtual-network-tcpip-performance-tuning

---

## [COMMON-086] HTTP가 무엇이고, HTTP의 특징을 설명해 주세요.

답변:
- HTTP(HyperText Transfer Protocol)는 클라이언트와 서버가 웹의 리소스를 주고받기 위해 사용하는 **애플리케이션 계층 프로토콜**입니다.
- 클라이언트가 Method, URL, Header, Body 등으로 구성된 요청을 보내면 서버는 Status Code, Header, Body 등으로 구성된 응답을 반환하는 Request-Response 구조입니다.
- HTTP는 **Stateless**하므로 프로토콜 자체는 이전 요청의 상태를 기억하지 않습니다. 로그인을 유지하려면 Cookie, Session, Token 같은 별도의 상태 관리 방법이 필요합니다.
- Header를 통해 인증, 캐시, 콘텐츠 형식 등을 확장할 수 있으며 HTML뿐 아니라 JSON, 이미지, 영상 등 다양한 데이터를 전송할 수 있습니다.
- HTTP 자체의 상태 비저장 특성과 전송 연결은 구분해야 합니다. HTTP/1.1의 Keep-Alive처럼 여러 요청에서 TCP 연결을 재사용할 수 있고, HTTP/3는 QUIC를 사용합니다.
- 예를 들어 클라이언트가 `GET /users/1`을 요청하면 서버는 `200 OK`와 함께 사용자 정보를 JSON으로 응답할 수 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP
- https://developer.mozilla.org/ko/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works

---

## [COMMON-087] HTTP와 HTTPS의 차이에 대해 설명해 주세요.

답변:
- HTTPS는 HTTP 통신에 TLS를 적용하여 **암호화, 무결성, 인증**을 제공하는 방식입니다.
- HTTP는 데이터가 암호화되지 않아 통신을 가로챈 공격자가 내용을 읽거나 변경할 수 있습니다. HTTPS는 세션 키로 요청과 응답을 암호화하여 기밀성을 보호합니다.
- TLS는 메시지가 전송 중에 변조됐는지 확인해 무결성을 보장하고, 인증 기관이 서명한 인증서를 통해 접속한 서버가 올바른 서버인지 검증합니다.
- 일반적으로 HTTP는 Port 80, HTTPS는 Port 443을 사용합니다.
- HTTPS를 사용해도 SQL Injection이나 XSS 같은 애플리케이션 취약점까지 자동으로 해결되는 것은 아니지만, 로그인 정보와 개인정보가 전송 중에 노출되거나 변조되는 위험을 줄일 수 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/Security/Defenses/Transport_Layer_Security
- https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/

---

## [COMMON-088] TLS/SSL Handshake가 어떤 역할을 하는지 설명해 주세요.

답변:
- TLS Handshake는 암호화 통신을 시작하기 전에 클라이언트와 서버가 **사용할 TLS 버전과 암호 알고리즘을 결정하고, 서버의 신원을 확인하며, 대칭키 암호화에 사용할 세션 키를 만드는 과정**입니다.
- 현재는 보안 취약점이 있는 SSL 대신 TLS를 사용하지만, 관습적으로 SSL 인증서나 SSL Handshake라는 표현을 사용하기도 합니다.
- TLS 1.3을 단순화한 흐름은 다음과 같습니다.
  1. 클라이언트가 지원하는 TLS 버전, 암호 알고리즘, Key Share 등을 `ClientHello`에 담아 보냅니다.
  2. 서버가 사용할 설정과 자신의 Key Share를 `ServerHello`로 보내고 인증서와 서명 정보를 제공합니다.
  3. 클라이언트는 신뢰할 수 있는 인증 기관의 서명, 인증서의 도메인과 유효 기간 등을 확인하여 서버를 인증합니다.
  4. 양쪽은 교환한 정보를 이용해 같은 세션 키를 만들고 `Finished` 메시지를 검증합니다.
  5. 이후 HTTP 데이터는 속도가 빠른 대칭키 암호 방식으로 암호화해 전송합니다.
- 공개키 암호 방식은 주로 서버 인증과 안전한 키 합의에 사용하고, 실제 대량의 데이터는 대칭키로 암호화하여 보안과 성능을 함께 확보합니다.

참고 자료:
- https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/
- https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/

---

## [COMMON-089] DNS가 무엇이고, 도메인 이름이 IP 주소로 변환되는 과정을 설명해 주세요.

답변:
- DNS(Domain Name System)는 사람이 기억하기 쉬운 `example.com` 같은 도메인 이름을 컴퓨터가 통신에 사용하는 IP 주소로 변환하는 분산 시스템입니다.
- 캐시가 없는 경우의 조회 과정은 다음과 같습니다.
  1. 브라우저와 운영체제의 DNS 캐시, Hosts 파일 등을 먼저 확인합니다.
  2. 정보가 없으면 설정된 **재귀 DNS Resolver**에 도메인 조회를 요청합니다.
  3. Resolver는 **Root Name Server**에 질의해 `.com` 같은 TLD Name Server의 위치를 얻습니다.
  4. **TLD Name Server**에 질의해 해당 도메인의 권한 있는 Name Server 위치를 얻습니다.
  5. **권한 있는 Name Server**로부터 A 레코드의 IPv4 주소나 AAAA 레코드의 IPv6 주소 등을 받습니다.
  6. Resolver는 결과를 클라이언트에 반환하고 레코드의 TTL 동안 캐시합니다.
- 실제로는 브라우저, 운영체제, Resolver 등에 결과가 캐시되어 있다면 중간 조회 과정을 생략하고 더 빠르게 IP 주소를 얻을 수 있습니다.

참고 자료:
- https://www.cloudflare.com/ko-kr/learning/dns/what-is-dns/
- https://www.cloudflare.com/ko-kr/learning/dns/what-is-a-dns-server/

---

## [COMMON-090] 브라우저에 URL을 입력했을 때 서버 응답을 받기까지의 흐름을 설명해 주세요.

답변:
- 브라우저에 URL을 입력하면 대략 다음 순서로 처리됩니다.
  1. 브라우저가 URL을 Scheme, Host, Port, Path 등으로 해석하고 캐시된 리소스나 HSTS 정책 등을 확인합니다.
  2. DNS 조회를 통해 도메인 이름에 해당하는 서버의 IP 주소를 얻습니다.
  3. 서버와 연결합니다. HTTP/1.1이나 HTTP/2라면 TCP 3-way Handshake를 수행하고, HTTPS라면 이어서 TLS Handshake를 수행합니다. HTTP/3는 TCP 대신 QUIC를 사용합니다.
  4. 브라우저가 `GET /path`와 Host, Cookie 등의 Header를 포함한 HTTP 요청을 보냅니다.
  5. 요청은 Load Balancer나 Reverse Proxy를 거쳐 애플리케이션 서버에 전달될 수 있습니다. 서버는 필요하면 Cache나 Database를 조회해 요청을 처리합니다.
  6. 서버가 상태 코드, Header, HTML이나 JSON 등의 Body를 담은 HTTP 응답을 반환합니다.
  7. 브라우저는 HTML을 파싱하고 필요한 CSS, JavaScript, 이미지 등을 추가로 요청한 뒤 화면을 렌더링합니다.
- DNS, TCP, TLS, HTTP 과정의 결과가 캐시되거나 기존 연결이 재사용되는 경우에는 일부 단계를 생략할 수 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works
- https://www.cloudflare.com/ko-kr/learning/network-layer/how-does-the-internet-work/

---

## [COMMON-091] HTTP 메서드 GET, POST, PUT, PATCH, DELETE의 차이를 설명해 주세요.

답변:
- HTTP Method는 클라이언트가 대상 리소스에 어떤 작업을 원하는지 나타냅니다.
- **GET**은 리소스를 조회합니다. 서버 상태를 변경하지 않는 안전한 Method이며, 같은 요청을 반복해도 의도한 결과가 같은 멱등성을 가집니다.
- **POST**는 일반적으로 새 리소스를 생성하거나 특정 작업을 처리할 때 사용합니다. 같은 요청을 반복하면 리소스가 여러 개 생성될 수 있으므로 기본적으로 멱등하지 않습니다.
- **PUT**은 지정한 URI의 리소스 전체를 요청 Body의 내용으로 교체하며, 리소스가 없다면 생성하도록 설계할 수도 있습니다. 동일한 요청을 반복해도 최종 상태가 같으므로 멱등합니다.
- **PATCH**는 리소스의 일부만 수정합니다. 처리 방식에 따라 결과가 누적될 수 있으므로 멱등성이 자동으로 보장되지는 않습니다.
- **DELETE**는 지정한 리소스를 삭제하며, 같은 삭제 요청을 반복해도 리소스가 삭제된 최종 상태는 같으므로 멱등합니다. 매번 같은 응답 코드가 반환된다는 의미는 아닙니다.
- 예를 들어 `/users/1`에 GET은 사용자 조회, PUT은 사용자 전체 교체, PATCH는 이름만 수정, DELETE는 사용자 삭제에 사용할 수 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Methods
- https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design

---

## [COMMON-092] HTTP 상태 코드 2xx, 3xx, 4xx, 5xx의 의미를 설명해 주세요.

답변:
- HTTP Status Code는 서버가 요청을 어떻게 처리했는지 세 자리 숫자로 나타냅니다.
- **2xx(Success)**는 요청을 정상적으로 처리했다는 의미입니다.
  - `200 OK`: 요청 성공
  - `201 Created`: 리소스 생성 성공
  - `204 No Content`: 성공했지만 반환할 Body가 없음
- **3xx(Redirection)**는 요청을 완료하려면 다른 위치로 이동하거나 캐시를 사용해야 한다는 의미입니다.
  - `301 Moved Permanently`: 영구 이동
  - `302 Found`: 임시 이동
  - `304 Not Modified`: 캐시된 리소스를 사용 가능
- **4xx(Client Error)**는 요청 형식, 인증, 권한, 대상 리소스 등 클라이언트 요청에 문제가 있다는 의미입니다.
  - `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`
- **5xx(Server Error)**는 유효한 요청을 서버가 처리하는 과정에서 문제가 발생했다는 의미입니다.
  - `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`
- `401`은 주로 인증 정보가 없거나 유효하지 않은 경우이고, `403`은 서버가 요청자를 알고 있더라도 해당 작업을 허용하지 않는 경우라는 차이가 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP/Status

---

## [COMMON-093] RESTful API가 무엇인지 설명해 주세요.

답변:
- REST는 네트워크의 데이터를 **Resource**로 보고 URI로 식별하며, HTTP의 표준 기능을 일관되게 사용하는 아키텍처 스타일입니다. REST의 제약 조건을 잘 따르도록 설계한 API를 RESTful API라고 합니다.
- 주요 특징은 다음과 같습니다.
  - **Client-Server**: 화면과 데이터 처리 책임을 분리합니다.
  - **Stateless**: 각 요청은 처리에 필요한 정보를 모두 포함하고, 서버는 클라이언트의 요청 문맥에 의존하지 않습니다.
  - **Cacheable**: 응답이 캐시 가능한지 명시하여 불필요한 요청을 줄일 수 있습니다.
  - **Uniform Interface**: 일관된 URI, HTTP Method, Status Code, 표현 형식을 사용합니다.
  - **Layered System**: 클라이언트는 Load Balancer나 Proxy 같은 중간 계층의 존재를 몰라도 됩니다.
- URI는 동사보다 리소스를 나타내는 명사를 사용하고, 작업은 HTTP Method로 표현하는 것이 일반적입니다.
- 예를 들어 사용자를 조회할 때 `/getUser?id=1`보다는 `GET /users/1`, 이름을 일부 수정할 때는 `PATCH /users/1`처럼 표현할 수 있습니다.
- REST가 반드시 JSON만 사용해야 한다는 의미는 아니며, HTTP를 사용한다고 해서 모든 API가 자동으로 RESTful한 것도 아닙니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design
- https://learn.microsoft.com/ko-kr/dotnet/maui/data-cloud/rest

---

## [COMMON-094] Cookie, Session, Token의 차이에 대해 설명해 주세요.

답변:
- Cookie, Session, Token은 완전히 같은 기준의 대체 기술은 아닙니다. **Cookie는 브라우저에 데이터를 저장하고 HTTP 요청에 실어 보내는 방식**이고, **Session과 Token은 로그인 상태나 인증 정보를 관리하는 방식**입니다.

| 구분 | Cookie | Session | Token |
| --- | --- | --- | --- |
| 주 저장 위치 | 클라이언트 브라우저 | 상태 정보는 서버, 클라이언트에는 주로 Session ID | 주로 클라이언트 |
| 전달 방식 | 조건에 맞는 요청에 브라우저가 자동 전송 | Session ID를 Cookie 등에 담아 전송 | Authorization Header나 Cookie 등에 담아 전송 |
| 서버 상태 | Cookie 자체는 클라이언트에 저장 | 서버에 Session 상태 저장 필요 | 자체 포함형 Token은 서버 상태 없이 검증 가능 |
| 특징 | 용량이 작고 매 요청에 포함될 수 있음 | 무효화와 사용자 상태 제어가 쉬움 | 분산 환경에서 확장하기 편리할 수 있음 |

- Session 방식은 로그인 후 서버가 Session을 만들고, 클라이언트는 Session ID만 보냅니다. 서버를 여러 대 사용한다면 Redis 같은 공용 Session 저장소나 Sticky Session 등이 필요할 수 있습니다.
- Token 방식은 서버가 발급한 Token을 클라이언트가 요청마다 제시합니다. JWT 같은 자체 포함형 Token은 서버 저장소를 조회하지 않고 검증할 수 있지만, 만료 전 즉시 폐기하거나 권한 변경을 반영하기 어렵습니다. 불투명 Token처럼 서버 조회가 필요한 Token도 있으므로 모든 Token이 Stateless한 것은 아닙니다.
- Session ID나 Token은 탈취되면 사용자 권한이 노출될 수 있으므로 HTTPS를 사용하고, Cookie에 저장한다면 `HttpOnly`, `Secure`, `SameSite`, 적절한 만료 시간을 설정해야 합니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/Cookies
- https://learn.microsoft.com/ko-kr/entra/identity-platform/security-tokens

---

## [COMMON-095] Stateless하다는 것이 무엇이고, HTTP가 Stateless한 것이 어떤 장단점을 가지는지 설명해 주세요.

답변:
- Stateless는 서버가 이전 요청에서 처리한 클라이언트의 상태를 기억하지 않고, **각 요청을 독립적으로 처리하는 특성**입니다.
- 따라서 클라이언트는 서버가 요청을 처리하는 데 필요한 인증 정보나 요청 내용을 매번 함께 보내야 합니다.
- 장점은 특정 서버의 메모리에 이전 요청 상태가 묶이지 않으므로 요청을 여러 서버에 분산하기 쉽고, 서버 장애 시 다른 서버가 요청을 처리하기도 쉬워 확장성과 가용성이 높다는 점입니다. 동일한 요청을 독립적으로 처리할 수 있어 캐시 활용에도 유리합니다.
- 단점은 인증 정보나 문맥을 요청마다 반복해 보내야 하므로 데이터가 중복될 수 있고, 로그인이나 장바구니처럼 연속된 상태가 필요한 기능은 Cookie, Session, Token, Database 등을 이용해 별도로 구현해야 한다는 점입니다.
- Stateless가 서버에 어떤 데이터도 저장하지 않는다는 뜻은 아닙니다. 서버는 Database에 사용자와 주문 데이터를 저장할 수 있지만, 특정 서버의 이전 요청 처리 문맥에 의존하지 않아야 한다는 의미입니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP
- https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/Cookies

---

## [COMMON-096] CORS가 무엇이고, 왜 발생하는지 설명해 주세요.

답변:
- CORS(Cross-Origin Resource Sharing)는 서버가 HTTP Header를 통해 **다른 Origin의 브라우저 코드에 자신의 리소스를 읽을 수 있는 권한을 부여하는 메커니즘**입니다.
- Origin은 `Scheme + Host + Port`의 조합입니다. 예를 들어 `https://example.com`과 `http://example.com`, `https://api.example.com`, `https://example.com:8080`은 서로 다른 Origin입니다.
- 브라우저는 보안을 위해 Same-Origin Policy를 적용하므로, 다른 Origin으로 요청한 응답을 JavaScript가 임의로 읽지 못하게 제한합니다. 서버 응답에 올바른 `Access-Control-Allow-Origin` 등의 Header가 없으면 브라우저에서 CORS 오류가 발생합니다.
- 단순 요청이 아닌 Method나 Header를 사용하면 브라우저는 실제 요청 전에 `OPTIONS` Preflight 요청을 보내 서버가 해당 Origin, Method, Header를 허용하는지 확인합니다.
- CORS는 브라우저가 적용하는 보안 정책이므로 일반적인 서버 간 통신이나 `curl`에는 동일하게 적용되지 않습니다. 또한 요청이 서버에 도달했더라도 브라우저가 응답을 JavaScript에 제공하지 않는 경우가 있습니다.
- 예를 들어 `http://localhost:3000`의 프론트엔드가 `http://localhost:8080`의 API를 호출하면 Port가 달라 Cross-Origin 요청이 되고, API 서버가 해당 Origin을 허용해야 합니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/CORS

---

## [COMMON-097] Blocking / Non-blocking과 Synchronous / Asynchronous의 차이에 대해 설명해 주세요.

답변:
- 두 개념은 비슷하게 사용되지만 구분 기준이 다릅니다. **Blocking / Non-blocking은 호출한 작업이 제어권을 언제 돌려주는지**, **Synchronous / Asynchronous는 작업의 완료와 결과를 누가 어떤 방식으로 확인하는지**에 관한 개념입니다.
- **Blocking**은 호출한 작업이 끝날 때까지 제어권이 돌아오지 않아 현재 스레드가 기다리는 방식입니다.
- **Non-blocking**은 작업이 완료되지 않았더라도 즉시 제어권을 돌려주며, 호출자는 다른 작업을 수행할 수 있습니다.
- **Synchronous**는 호출자가 작업의 완료를 직접 기다리거나 반복해서 확인하고, 완료된 결과를 이어서 처리하는 방식입니다.
- **Asynchronous**는 작업을 요청한 뒤 다른 일을 수행하고, 완료되면 Callback, Future, Event 등의 방식으로 결과를 전달받는 방식입니다.
- 예를 들어 Blocking Synchronous 파일 읽기는 데이터가 준비될 때까지 스레드가 기다린 뒤 결과를 반환합니다. Non-blocking Synchronous 방식은 준비되지 않았다면 즉시 반환하고 호출자가 다시 확인합니다. Asynchronous 방식은 읽기를 요청한 뒤 다른 작업을 수행하고, 완료 알림을 받았을 때 결과를 처리합니다.
- 비동기 API를 사용해도 내부 Worker Thread가 Blocking I/O를 수행할 수 있으므로, 비동기와 Non-blocking이 항상 같은 의미인 것은 아닙니다.

참고 자료:
- https://learn.microsoft.com/ko-kr/windows/win32/fileio/i-o-concepts
- https://learn.microsoft.com/ko-kr/dotnet/standard/io/asynchronous-file-i-o
- https://engineering.linecorp.com/ko/blog/do-not-block-the-event-loop-part1/

---

## [COMMON-098] I/O Multiplexing이 무엇이고, 왜 필요한지 설명해 주세요.

답변:
- I/O Multiplexing은 하나의 스레드가 여러 Socket이나 File Descriptor를 감시하다가, **읽기나 쓰기가 가능한 상태가 된 대상만 골라 처리하는 방식**입니다.
- 연결마다 하나의 스레드를 만들고 Blocking I/O를 수행하면 연결 수가 많아질수록 스레드의 Stack 메모리와 Context Switching 비용이 증가합니다.
- `select`, `poll`, Linux의 `epoll`, BSD 계열의 `kqueue`, Java NIO의 `Selector` 등을 이용하면 적은 수의 스레드로 많은 연결을 관리할 수 있어 대규모 동시 접속 서버에 유리합니다.
- `select`와 `poll`은 준비된 대상을 찾기 위해 등록된 File Descriptor를 반복해서 확인하는 비용이 커질 수 있습니다. `epoll`은 커널이 준비된 이벤트 목록을 알려주는 방식으로 많은 연결 중 일부만 활성화되는 상황에서 효율적입니다.
- 예를 들어 채팅 서버에 1만 개의 연결이 있어도 대부분은 메시지를 기다리고 있습니다. I/O Multiplexing을 사용하면 모든 연결에 스레드를 배정하지 않고, 실제 메시지가 도착한 Socket만 처리할 수 있습니다.
- I/O Multiplexing은 여러 I/O의 **준비 상태를 한 번에 감시하는 방식**이며 비동기 I/O 완료 자체와는 구분됩니다. 또한 Event Loop 안에서 오래 걸리는 연산이나 Blocking I/O를 실행하면 다른 연결도 함께 지연될 수 있습니다.

참고 자료:
- https://engineering.linecorp.com/ko/blog/do-not-block-the-event-loop-part1/
- https://engineering.linecorp.com/ko/blog/do-not-block-the-event-loop-part2/
- https://docs.python.org/ko/3.13/library/selectors.html

---

## [COMMON-099] WebSocket이 무엇이고, HTTP와 어떤 차이가 있는지 설명해 주세요.

답변:
- WebSocket은 클라이언트와 서버가 하나의 연결을 유지하면서 메시지를 양방향으로 주고받을 수 있는 **Full-Duplex 통신 프로토콜**입니다.
- 일반적인 HTTP 통신은 클라이언트가 요청해야 서버가 응답하는 Request-Response 방식입니다. WebSocket은 연결이 만들어진 뒤에는 서버도 클라이언트의 새 HTTP 요청을 기다리지 않고 메시지를 보낼 수 있습니다.
- 처음에는 HTTP Handshake를 통해 WebSocket 연결로 전환하고, 이후에는 작은 Frame 단위로 데이터를 주고받으므로 요청마다 HTTP Header를 반복해서 보내는 비용을 줄일 수 있습니다.
- 실시간 채팅, 주식 시세, 협업 편집, 게임처럼 서버의 변경 사항을 즉시 전달해야 하는 기능에 적합합니다.
- 연결을 계속 유지해야 하므로 서버와 Load Balancer가 많은 장기 연결을 관리해야 하고, 연결이 끊겼을 때 재연결, Heartbeat, 누락된 데이터 동기화 등을 애플리케이션에서 고려해야 합니다.
- 모든 API를 WebSocket으로 만들 필요는 없습니다. 단순 조회나 변경처럼 Request-Response로 충분한 기능은 HTTP가 구현, 캐시, 모니터링 측면에서 더 단순할 수 있습니다.

참고 자료:
- https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API

---

## [COMMON-100] Load Balancing이 무엇이고, 어떤 방식들이 있는지 설명해 주세요.

답변:
- Load Balancing은 여러 서버에 요청이나 네트워크 트래픽을 분산하여 한 서버에 부하가 집중되는 것을 막는 기술입니다.
- 서버를 수평으로 확장할 수 있고, Health Check로 장애가 발생한 서버를 제외해 처리량과 가용성을 높일 수 있습니다.
- 동작 계층에 따라 다음과 같이 나눌 수 있습니다.
  - **L4 Load Balancing**은 IP와 Port, TCP·UDP 정보 등을 기준으로 전달합니다. 패킷 수준에서 처리하므로 빠르지만 HTTP의 URL이나 Header를 기준으로 세밀하게 분기하기는 어렵습니다.
  - **L7 Load Balancing**은 HTTP의 Host, URL, Header, Cookie 등 애플리케이션 정보를 기준으로 전달합니다. `/images`는 이미지 서버, `/api`는 API 서버로 보내는 Content-Based Routing이 가능합니다.
- 대표적인 분산 알고리즘은 다음과 같습니다.
  - **Round Robin**: 서버를 순서대로 선택합니다.
  - **Weighted Round Robin**: 성능이 좋은 서버에 더 높은 가중치를 주어 요청을 더 많이 보냅니다.
  - **Least Connections**: 현재 연결 수가 가장 적은 서버를 선택합니다.
  - **Least Response Time**: 응답 시간과 연결 상태 등을 고려해 빠른 서버를 선택합니다.
  - **IP Hash**: 클라이언트 IP를 Hashing하여 같은 클라이언트를 같은 서버에 연결합니다.
- 예를 들어 세 대의 애플리케이션 서버 앞에 Load Balancer를 두면 요청을 분산하고, 한 서버의 Health Check가 실패했을 때 나머지 정상 서버로만 요청을 보낼 수 있습니다.

참고 자료:
- https://aws.amazon.com/ko/what-is/load-balancing/
- https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/application/load-balancer-target-groups.html

---

## 선택 질문

## [COMMON-101] TCP의 흐름 제어와 혼잡 제어의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-102] TCP에서 Head-of-Line Blocking이 무엇인지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-103] HTTP/1.1, HTTP/2, HTTP/3의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-104] Keep-Alive가 무엇이고, 어떤 장점이 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-105] Proxy, Forward Proxy, Reverse Proxy의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-106] CDN이 무엇이고, 어떤 상황에서 사용하는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-107] API Gateway가 무엇이고, 어떤 역할을 하는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-108] Rate Limiting이 무엇이고, 왜 필요한지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-109] Idempotent한 API란 무엇인지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-110] 멱등성이 중요한 API 예시를 들어 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-111] JWT가 무엇이고, Session 기반 인증과 비교해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-112] Refresh Token을 사용하는 이유와 주의할 점을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-113] OAuth의 기본 흐름을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-114] CSRF와 XSS의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-115] Long Polling, SSE, WebSocket의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-116] gRPC가 무엇이고, REST와 비교했을 때 어떤 특징이 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-117] 서버에서 파일 업로드 요청을 처리할 때 고려해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-118] 외부 API 호출이 느리거나 실패할 때 어떻게 대응할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-119] 타임아웃과 재시도 정책을 설계할 때 고려해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-120] Circuit Breaker 패턴이 무엇이고, 어떤 상황에서 사용하는지 설명해 주세요.

답변:

참고 자료:
