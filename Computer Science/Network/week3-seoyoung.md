# Week 03 - Network / Web / I/O

---

## 제출 기준

- 필수 답변: COMMON-081 ~ COMMON-100
- 선택 답변: COMMON-101 ~ COMMON-120

---

## 필수 질문

## [COMMON-081] OSI 7계층과 TCP/IP 4계층 비교

답변:

OSI 7계층은 네트워크 통신 과정을 7단계로 나눈 이론적 참조 모델이다. 물리, 데이터링크, 네트워크, 전송, 세션, 표현, 응용 계층으로 구성된다.

TCP/IP 4계층은 실제 인터넷 통신에 사용되는 모델이다. OSI의 세션/표현/응용 계층은 TCP/IP의 응용 계층으로, 물리/데이터링크 계층은 네트워크 인터페이스 계층으로 대응된다. OSI는 개념 이해에, TCP/IP는 실제 구현 설명에 더 적합하다.

참고 자료:
- [IBM - What is the OSI model?](https://www.ibm.com/think/topics/osi-model)

---

## [COMMON-082] TCP와 UDP의 차이

답변:

TCP는 연결 지향형 프로토콜로, 연결 수립 후 데이터의 순서와 전달 여부를 확인하고 손실 시 재전송한다. 그래서 신뢰성이 높지만 제어 과정이 많아 UDP보다 느릴 수 있다.

UDP는 비연결형 프로토콜로, 연결 설정 없이 데이터를 보낸다. 전달 보장과 순서 보장은 없지만 오버헤드가 작고 빠르기 때문에 DNS, 스트리밍, 온라인 게임처럼 속도가 중요한 경우에 사용된다.

참고 자료:
- [MDN - UDP](https://developer.mozilla.org/en-US/docs/Glossary/UDP)

---

## [COMMON-083] TCP 3-way handshake와 필요성

답변:

TCP 3-way handshake는 TCP 연결을 시작하기 전에 클라이언트와 서버가 서로 통신 가능한 상태인지 확인하는 과정이다. 순서는 `SYN -> SYN + ACK -> ACK`이다.

이 과정이 필요한 이유는 양쪽의 송수신 가능 여부를 확인하고 초기 순서 번호를 교환하기 위해서다. TCP는 신뢰성 있는 스트림 전송을 제공해야 하므로 데이터를 보내기 전에 연결 상태를 명확히 맞춘다.

참고 자료:
- [RFC 9293 - Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc9293.html)

---

## [COMMON-084] TCP 4-way handshake와 TIME_WAIT

답변:

TCP 4-way handshake는 TCP 연결 종료 절차다. 한쪽이 `FIN`을 보내면 상대방이 `ACK`를 보내고, 상대방도 종료 준비가 끝나면 `FIN`을 보내며, 마지막으로 처음 종료 요청자가 `ACK`를 보내 연결이 종료된다.

`TIME_WAIT`은 마지막 `ACK`를 보낸 쪽이 일정 시간 기다리는 상태다. 마지막 `ACK`가 유실되었을 때 재전송되는 `FIN`에 응답하고, 이전 연결의 지연 패킷이 새 연결에 섞이는 것을 막기 위해 필요하다.

참고 자료:
- [RFC 9293 - Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc9293.html)

---

## [COMMON-085] TCP의 신뢰성 보장 방법

답변:

TCP는 순서 번호, ACK, 재전송, 체크섬을 통해 데이터의 손실과 오류를 감지하고 복구한다. 수신자는 받은 데이터에 대해 ACK를 보내고, 송신자는 ACK가 오지 않으면 데이터를 다시 전송한다.

또한 흐름 제어로 수신자의 처리 능력을 넘지 않게 조절하고, 혼잡 제어로 네트워크 상황에 따라 전송 속도를 조절한다. 이런 기능들이 합쳐져 TCP의 신뢰성이 보장된다.

참고 자료:
- [RFC 9293 - Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc9293.html)

---

## [COMMON-086] HTTP와 HTTP의 특징

답변:

HTTP는 웹에서 클라이언트와 서버가 요청과 응답을 주고받기 위한 애플리케이션 계층 프로토콜이다. 클라이언트가 요청을 보내고 서버가 응답하는 구조로 동작한다.

HTTP의 대표 특징은 무상태성과 비연결성이다. 서버는 기본적으로 이전 요청 상태를 기억하지 않기 때문에 확장성이 좋지만, 로그인 같은 상태 유지가 필요한 기능은 쿠키, 세션, 토큰으로 보완해야 한다.

참고 자료:
- [MDN - Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)

---

## [COMMON-087] HTTP와 HTTPS의 차이

답변:

HTTP는 데이터를 평문으로 전송하고, HTTPS는 HTTP에 TLS를 적용해 데이터를 암호화해서 전송한다. 그래서 HTTP는 중간에서 내용을 볼 수 있지만 HTTPS는 기밀성과 무결성을 보장한다.

HTTPS는 인증서를 통해 서버의 신뢰성도 확인한다. 일반적으로 HTTP는 80번 포트, HTTPS는 443번 포트를 사용하며, 현재 웹 서비스에서는 HTTPS 사용이 기본에 가깝다.

참고 자료:
- [MDN - HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS)

---

## [COMMON-088] TLS/SSL Handshake의 역할

답변:

TLS/SSL Handshake는 클라이언트와 서버가 암호화 통신을 시작하기 전에 서로를 인증하고 사용할 암호화 방식과 키를 협상하는 과정이다.

서버는 인증서로 자신의 신원을 증명하고, 양쪽은 이후 데이터 암호화에 사용할 세션 키를 만든다. 실제 데이터 전송은 이 세션 키를 이용한 대칭키 암호화로 처리된다.

참고 자료:
- [Cloudflare - What happens in a TLS handshake?](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)

---

## [COMMON-089] DNS와 도메인 이름 변환 과정

답변:

DNS는 사람이 읽기 쉬운 도메인 이름을 IP 주소로 변환하는 시스템이다. 사용자가 도메인을 입력하면 브라우저와 OS 캐시를 먼저 확인하고, 없으면 DNS Resolver에 질의한다.

Resolver는 Root DNS, TLD DNS, Authoritative DNS 서버를 순서대로 조회해 최종 IP 주소를 얻는다. 이후 브라우저는 얻은 IP 주소로 서버에 연결해 HTTP 또는 HTTPS 요청을 보낸다.

참고 자료:
- [Cloudflare - What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)

---

## [COMMON-090] URL 입력 후 서버 응답까지의 흐름

답변:

브라우저에 URL을 입력하면 먼저 URL을 해석하고 DNS 조회로 서버 IP를 찾는다. 이후 TCP 3-way handshake로 연결을 맺고, HTTPS라면 TLS handshake까지 수행한 뒤 HTTP 요청을 보낸다.

서버는 요청을 처리해 응답을 반환하고, 브라우저는 HTML, CSS, JavaScript 등을 파싱한다. 이후 DOM과 CSSOM을 만들고 렌더링 과정을 거쳐 화면을 구성한다.

참고 자료:
- [MDN - Populating the page: how browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/How_browsers_work)

---

## [COMMON-091] HTTP 메서드 GET, POST, PUT, PATCH, DELETE의 차이

답변:

GET은 리소스 조회, POST는 리소스 생성이나 처리 요청, PUT은 리소스 전체 교체, PATCH는 리소스 일부 수정, DELETE는 리소스 삭제에 사용된다.

GET은 안전하고 멱등하다. PUT과 DELETE는 리소스 상태를 바꾸지만 같은 요청을 여러 번 보내도 최종 결과가 같아 멱등하다. POST는 호출할 때마다 새 리소스가 생길 수 있어 일반적으로 멱등하지 않다.

참고 자료:
- [MDN - HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)

---

## [COMMON-092] HTTP 상태 코드 2xx, 3xx, 4xx, 5xx의 의미

답변:

2xx는 요청 성공, 3xx는 리다이렉션, 4xx는 클라이언트 요청 오류, 5xx는 서버 처리 오류를 의미한다.

예를 들어 `200 OK`는 성공, `301`은 영구 이동, `404`는 리소스 없음, `500`은 서버 내부 오류다. 4xx는 요청 자체의 문제에 가깝고, 5xx는 서버가 요청을 처리하지 못한 문제에 가깝다.

참고 자료:
- [MDN - HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)

---

## [COMMON-093] RESTful API

답변:

RESTful API는 REST 아키텍처 스타일을 따르는 API다. 자원은 URI로 표현하고, 자원에 대한 행위는 HTTP 메서드로 표현한다.

예를 들어 `GET /users`는 사용자 목록 조회, `POST /users`는 사용자 생성, `DELETE /users/1`은 특정 사용자 삭제를 의미한다. 핵심은 무상태성, 클라이언트-서버 분리, 일관된 인터페이스다.

참고 자료:
- [AWS - What is RESTful API?](https://aws.amazon.com/what-is/restful-api/)

---

## [COMMON-094] Cookie, Session, Token의 차이

답변:

Cookie는 클라이언트 브라우저에 저장되는 작은 데이터다. Session은 사용자 상태를 서버에 저장하고, 클라이언트는 보통 세션 ID만 쿠키로 들고 있는 방식이다.

Token은 인증 또는 인가 정보를 담은 문자열이다. 세션은 서버가 상태를 관리해 제어가 쉽지만 확장 시 세션 공유가 필요하다. 토큰은 서버가 상태를 덜 가져 확장성이 좋지만 탈취 시 만료 전까지 위험할 수 있다.

참고 자료:
- [MDN - Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)

---

## [COMMON-095] Stateless와 HTTP의 장단점

답변:

Stateless는 서버가 클라이언트의 이전 요청 상태를 저장하지 않고 각 요청을 독립적으로 처리한다는 의미다. HTTP는 기본적으로 Stateless한 프로토콜이다.

장점은 서버 확장이 쉽고 어떤 서버든 요청을 처리할 수 있다는 점이다. 단점은 매 요청마다 인증 정보나 필요한 상태 정보를 함께 보내야 하며, 로그인이나 장바구니 같은 기능은 쿠키, 세션, 토큰으로 보완해야 한다는 점이다.

참고 자료:
- [MDN - Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)

---

## [COMMON-096] CORS와 발생 이유

답변:

CORS는 Cross-Origin Resource Sharing의 약자로, 브라우저가 다른 출처의 리소스 접근을 허용할지 확인하는 메커니즘이다. Origin은 프로토콜, 호스트, 포트의 조합이다.

브라우저는 보안상 Same-Origin Policy를 적용하기 때문에 다른 Origin으로 요청할 때 서버의 허용 헤더를 확인한다. `Access-Control-Allow-Origin` 같은 헤더가 없거나 맞지 않으면 CORS 오류가 발생한다.

참고 자료:
- [MDN - Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

---

## [COMMON-097] Blocking/Non-blocking과 Synchronous/Asynchronous의 차이

답변:

Blocking과 Non-blocking은 제어권 반환 여부의 차이다. Blocking은 작업이 끝날 때까지 기다리고, Non-blocking은 작업 완료와 관계없이 바로 제어권을 돌려받는다.

Synchronous와 Asynchronous는 결과 처리 방식의 차이다. Synchronous는 호출자가 작업 완료를 직접 확인하고, Asynchronous는 작업을 맡긴 뒤 완료되면 콜백이나 이벤트로 결과를 받는다.

참고 자료:
- [InterConnection - Blocking or Non-Blocking, Synchronous and Asynchronous](https://interconnection.tistory.com/141)

---

## [COMMON-098] I/O Multiplexing과 필요성

답변:

I/O Multiplexing은 하나의 스레드나 프로세스가 여러 I/O 이벤트를 동시에 감시하고, 준비된 I/O만 처리하는 방식이다. 대표적으로 `select`, `poll`, `epoll`이 있다.

연결마다 스레드를 만들면 연결 수가 많아질수록 메모리와 컨텍스트 스위칭 비용이 커진다. I/O Multiplexing은 적은 수의 스레드로 많은 연결을 처리할 수 있어 대규모 네트워크 서버에 필요하다.

참고 자료:
- [man7 - select(2)](https://man7.org/linux/man-pages/man2/select.2.html)

---

## [COMMON-099] WebSocket과 HTTP의 차이

답변:

WebSocket은 클라이언트와 서버가 하나의 연결을 유지하면서 양방향으로 메시지를 주고받는 프로토콜이다. 초기 연결은 HTTP Upgrade 요청으로 시작된다.

HTTP는 기본적으로 클라이언트 요청에 서버가 응답하는 구조라 서버가 먼저 데이터를 보내기 어렵다. WebSocket은 서버도 즉시 메시지를 보낼 수 있어 채팅, 실시간 알림, 게임 같은 기능에 적합하다.

참고 자료:
- [MDN - WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

---

## [COMMON-100] Load Balancing과 방식

답변:

Load Balancing은 여러 서버에 트래픽을 분산해 특정 서버에 부하가 집중되지 않도록 하는 기술이다. 이를 통해 성능, 가용성, 확장성을 높인다.

대표 방식으로는 순서대로 분배하는 Round Robin, 서버 성능에 따라 분배하는 Weighted Round Robin, 연결 수가 적은 서버로 보내는 Least Connections, 클라이언트 IP 기준으로 서버를 고정하는 IP Hash가 있다.

참고 자료:
- [F5 - What Is a Load Balancer?](https://www.f5.com/glossary/load-balancer)

---

## 선택 질문

## [COMMON-101] TCP 흐름 제어와 혼잡 제어의 차이

답변:

흐름 제어는 송신자가 수신자의 처리 능력을 초과해 데이터를 보내지 않도록 조절하는 기능이다. 수신자의 버퍼 상태를 기준으로 전송량을 조절하므로 목적은 수신자 보호다.

혼잡 제어는 네트워크 전체의 혼잡 상태를 보고 송신 속도를 조절하는 기능이다. 패킷 손실과 지연을 줄이고 네트워크 붕괴를 막는 것이 목적이다.

참고 자료:
- [GeeksforGeeks - Difference between Flow Control and Congestion Control](https://www.geeksforgeeks.org/computer-networks/difference-between-flow-control-and-congestion-control/)

---

## [COMMON-102] TCP의 Head-of-Line Blocking

답변:

Head-of-Line Blocking은 앞의 데이터가 손실되거나 지연되면 뒤의 데이터가 도착해도 먼저 처리되지 못하고 기다리는 현상이다.

TCP는 순서 보장을 제공하므로 중간 패킷 하나가 손실되면 그 뒤의 패킷도 애플리케이션에 전달되지 못한다. HTTP/2도 TCP 위에서 동작하므로 TCP 레벨의 HOL Blocking 문제는 남아 있고, HTTP/3는 QUIC을 사용해 이를 줄인다.

참고 자료:
- [Cloudflare - What is HTTP/3?](https://www.cloudflare.com/learning/performance/what-is-http3/)

---

## [COMMON-103] HTTP/1.1, HTTP/2, HTTP/3의 차이

답변:

HTTP/1.1은 지속 연결을 지원하지만 요청과 응답 처리에서 Head-of-Line Blocking 문제가 있었다. HTTP/2는 바이너리 프레임, 헤더 압축, 멀티플렉싱을 도입해 하나의 TCP 연결에서 여러 요청을 동시에 처리할 수 있게 했다.

HTTP/3는 TCP 대신 UDP 기반 QUIC을 사용한다. 연결 설정이 빠르고, 스트림 단위로 손실 영향을 줄일 수 있어 HTTP/2의 TCP 레벨 HOL Blocking을 완화한다.

참고 자료:
- [MDN - Evolution of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Evolution_of_HTTP)

---

## [COMMON-104] Keep-Alive와 장점

답변:

Keep-Alive는 하나의 TCP 연결을 요청마다 닫지 않고 재사용하는 방식이다. 여러 HTTP 요청과 응답을 같은 연결에서 처리할 수 있게 한다.

장점은 TCP handshake와 TLS handshake 비용을 줄여 지연 시간을 낮출 수 있다는 점이다. 다만 연결을 오래 유지하면 서버 자원을 점유하므로 timeout과 최대 요청 수를 적절히 설정해야 한다.

참고 자료:
- [MDN - Connection header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Connection)

---

## [COMMON-105] Proxy, Forward Proxy, Reverse Proxy의 차이

답변:

Proxy는 클라이언트와 서버 사이에서 요청과 응답을 대신 전달하는 중간 서버다. 캐싱, 필터링, 보안, 로깅 등의 목적으로 사용된다.

Forward Proxy는 클라이언트 앞에서 클라이언트를 대신해 외부 서버에 요청한다. Reverse Proxy는 서버 앞에서 외부 요청을 내부 서버로 전달한다. Forward Proxy는 클라이언트를 숨기고, Reverse Proxy는 서버를 숨기는 역할에 가깝다.

참고 자료:
- [MDN - Proxy servers and tunneling](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Proxy_servers_and_tunneling)

---

## [COMMON-106] CDN과 사용 상황

답변:

CDN은 Content Delivery Network의 약자로, 전 세계에 분산된 서버를 통해 사용자와 가까운 위치에서 콘텐츠를 제공하는 네트워크다.

이미지, CSS, JavaScript, 동영상 같은 정적 리소스를 캐시해 응답 속도를 높이고 원본 서버 부하를 줄인다. 글로벌 서비스, 정적 파일 요청이 많은 서비스, 트래픽 급증에 대비해야 하는 서비스에서 사용된다.

참고 자료:
- [Cloudflare - What is a CDN?](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)

---

## [COMMON-107] API Gateway와 역할

답변:

API Gateway는 클라이언트와 여러 백엔드 서비스 사이에 위치하는 API 진입점이다. 클라이언트는 각 서비스에 직접 접근하지 않고 Gateway를 통해 요청한다.

주요 역할은 라우팅, 인증과 인가, Rate Limiting, 로깅, 모니터링, 요청/응답 변환, API 버전 관리다. 마이크로서비스 구조에서 공통 정책을 한 곳에 모으고 클라이언트 복잡도를 줄인다.

참고 자료:
- [AWS Docs - What is Amazon API Gateway?](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

---

## [COMMON-108] Rate Limiting과 필요성

답변:

Rate Limiting은 사용자, IP, API Key 같은 기준으로 일정 시간 동안 허용되는 요청 수를 제한하는 방식이다.

서버 자원을 보호하고 과도한 요청이 전체 서비스 품질을 떨어뜨리는 것을 막기 위해 필요하다. 브루트포스 공격, API 남용, 크롤링, DDoS성 트래픽 완화에도 사용된다.

참고 자료:
- [Cloudflare - What is rate limiting?](https://www.cloudflare.com/learning/bots/what-is-rate-limiting/)

---

## [COMMON-109] Idempotent한 API

답변:

Idempotent한 API는 같은 요청을 여러 번 보내도 서버의 최종 상태가 한 번 보냈을 때와 같은 API다.

예를 들어 GET은 조회만 하므로 멱등하고, PUT은 같은 값으로 전체 교체하므로 멱등하다. DELETE도 이미 삭제된 상태가 유지되므로 최종 상태 관점에서 멱등하다. 반면 POST 생성 요청은 호출마다 새 리소스가 생길 수 있어 일반적으로 멱등하지 않다.

참고 자료:
- [MDN - Idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)

---

## [COMMON-110] 멱등성이 중요한 API 예시

답변:

결제 API는 멱등성이 중요하다. 결제 요청 후 네트워크 타임아웃이 발생하면 클라이언트는 결제가 성공했는지 실패했는지 알기 어렵다.

이때 같은 요청을 다시 보냈을 때 결제가 중복 처리되면 큰 문제가 된다. 그래서 `Idempotency-Key` 같은 고유 키를 사용해 같은 요청의 재시도는 이전 결과를 반환하도록 설계한다.

참고 자료:
- [Stripe Docs - Idempotent requests](https://docs.stripe.com/api/idempotent_requests)

---

## [COMMON-111] JWT와 Session 기반 인증 비교

답변:

JWT는 인증 또는 인가 정보를 JSON Claim으로 담고 서명한 토큰이다. Header, Payload, Signature로 구성되며, 서버는 서명을 검증해 토큰의 위변조 여부를 확인한다.

Session 인증은 서버가 사용자 상태를 저장하고 클라이언트는 세션 ID만 가진다. JWT는 서버가 세션 상태를 저장하지 않아 확장성이 좋지만, 탈취 시 만료 전까지 위험하고 즉시 무효화가 어렵다. Session은 제어가 쉽지만 서버 확장 시 세션 공유가 필요하다.

참고 자료:
- [JWT.io - Introduction to JSON Web Tokens](https://www.jwt.io/introduction)

---

## [COMMON-112] Refresh Token 사용 이유와 주의점

답변:

Refresh Token은 Access Token이 만료되었을 때 새 Access Token을 발급받기 위해 사용한다. Access Token의 만료 시간을 짧게 가져가면서도 사용자가 계속 로그인 상태를 유지할 수 있게 한다.

Refresh Token은 수명이 길어 탈취 위험이 크다. 따라서 안전한 저장소에 보관하고, HttpOnly/Secure 쿠키, 토큰 회전, 재사용 탐지, 만료 정책, 로그아웃 시 폐기 등을 고려해야 한다.

참고 자료:
- [Auth0 Docs - Refresh Tokens](https://auth0.com/docs/secure/tokens/refresh-tokens)

---

## [COMMON-113] OAuth의 기본 흐름

답변:

OAuth는 사용자가 비밀번호를 직접 넘기지 않고 제3자 애플리케이션에 제한된 접근 권한을 부여하는 인가 프로토콜이다.

대표적인 Authorization Code Flow는 사용자를 인증 서버로 보내 로그인과 동의를 받고, 인증 서버가 Authorization Code를 클라이언트에 돌려주는 방식이다. 클라이언트는 이 코드를 Access Token으로 교환하고, 토큰으로 리소스 서버 API를 호출한다.

참고 자료:
- [OAuth.com - Authorization Code Grant](https://www.oauth.com/oauth2-servers/server-side-apps/authorization-code/)

---

## [COMMON-114] CSRF와 XSS의 차이

답변:

CSRF는 사용자가 로그인한 상태를 악용해 원하지 않는 요청을 보내게 만드는 공격이다. 사용자의 인증 쿠키가 자동으로 포함되는 점을 이용한다.

XSS는 웹 페이지에 악성 스크립트를 삽입해 사용자의 브라우저에서 실행시키는 공격이다. CSRF는 요청 위조가 핵심이고, XSS는 스크립트 실행이 핵심이다.

참고 자료:
- [PortSwigger - XSS vs CSRF](https://portswigger.net/web-security/csrf/xss-vs-csrf)

---

## [COMMON-115] Long Polling, SSE, WebSocket의 차이

답변:

Long Polling은 클라이언트가 요청을 보내고 서버가 새 데이터가 생길 때까지 응답을 지연시키는 방식이다. 구현은 쉽지만 요청이 반복된다.

SSE는 하나의 HTTP 연결로 서버가 클라이언트에 단방향 이벤트를 계속 보내는 방식이다. WebSocket은 하나의 연결에서 클라이언트와 서버가 양방향으로 메시지를 주고받는 방식이다. 단방향 알림은 SSE, 양방향 실시간 통신은 WebSocket이 적합하다.

참고 자료:
- [ByteByteGo - Short/long polling, SSE, WebSocket](https://bytebytego.com/guides/shortlong-polling-sse-websocket/)

---

## [COMMON-116] gRPC와 REST 비교

답변:

gRPC는 Protocol Buffers로 서비스 인터페이스와 메시지를 정의하고 HTTP/2 위에서 동작하는 RPC 프레임워크다. 원격 함수를 호출하듯 API를 사용한다.

REST는 URI와 HTTP 메서드 중심으로 리소스를 다루는 방식이고, gRPC는 명확한 인터페이스 정의와 코드 생성, 바이너리 직렬화, 스트리밍에 강점이 있다. 내부 마이크로서비스 통신에는 gRPC가 유리하고, 외부 공개 API에는 REST가 더 단순하고 친숙한 경우가 많다.

참고 자료:
- [gRPC Docs - Introduction to gRPC](https://grpc.io/docs/what-is-grpc/introduction/)

---

## [COMMON-117] 파일 업로드 처리 시 고려할 점

답변:

파일 업로드는 보안과 자원 관리가 중요하다. 파일 크기 제한, 확장자와 MIME 타입 검증, 파일 시그니처 확인, 안전한 파일명 재생성, 저장 경로 분리, 실행 권한 차단이 필요하다.

또한 악성 파일 검사, 이미지 재인코딩, 메타데이터 제거, 대용량 파일의 스트리밍 처리, 객체 스토리지 사용도 고려해야 한다. 업로드 파일은 사용자가 보내는 입력이므로 신뢰하면 안 된다.

참고 자료:
- [OWASP Cheat Sheet - File Upload](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)

---

## [COMMON-118] 외부 API 호출이 느리거나 실패할 때의 대응

답변:

외부 API 호출에는 반드시 연결 타임아웃과 응답 타임아웃을 설정해야 한다. 일시적 장애는 제한된 횟수로 재시도하되, 즉시 반복하지 않고 Exponential Backoff와 Jitter를 적용한다.

실패가 계속되면 Circuit Breaker로 호출을 잠시 차단하고 fallback, 캐시, 기본값, 비동기 큐 처리 등을 사용한다. 무조건 재시도하면 장애가 난 외부 시스템에 더 큰 부하를 줄 수 있다.

참고 자료:
- [AWS - 시간 제한, 재시도 및 지터를 사용한 백오프](https://aws.amazon.com/ko/builders-library/timeouts-retries-and-backoff-with-jitter/)

---

## [COMMON-119] 타임아웃과 재시도 정책 설계 고려점

답변:

타임아웃은 너무 짧으면 정상 요청도 실패하고, 너무 길면 스레드나 커넥션 같은 자원이 오래 묶인다. 따라서 외부 서비스의 지연 시간, 사용자 경험, 시스템 자원 한계를 함께 보고 정해야 한다.

재시도는 네트워크 오류, `408`, `429`, `5xx` 같은 일시적 오류에만 제한적으로 적용하는 것이 좋다. 재시도 횟수와 전체 시간을 제한하고, Backoff와 Jitter를 적용해야 하며, 부작용이 있는 요청은 멱등성이 보장될 때만 재시도해야 한다.

참고 자료:
- [Google Cloud Docs - Retry strategy](https://cloud.google.com/storage/docs/retry-strategy)

---

## [COMMON-120] Circuit Breaker 패턴과 사용 상황

답변:

Circuit Breaker는 외부 서비스 호출 실패가 계속될 때 호출을 잠시 차단해 장애 전파를 막는 패턴이다. 실패 임계치를 넘으면 Open 상태가 되어 요청을 보내지 않고 즉시 실패나 fallback을 반환한다.

일정 시간이 지나면 Half-Open 상태에서 일부 요청으로 회복 여부를 확인하고, 성공하면 Closed로 돌아간다. 외부 API, 결제 서비스, 인증 서버처럼 의존 서비스 장애가 전체 시스템으로 번질 수 있는 상황에서 사용한다.

참고 자료:
- [Martin Fowler - Circuit Breaker](https://martinfowler.com/bliki/CircuitBreaker.html)

