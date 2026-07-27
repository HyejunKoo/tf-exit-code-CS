# Week 03 - Network / Web / I/O

---

## 제출 기준

- 필수 답변: COMMON-081 ~ COMMON-100
- 선택 답변: COMMON-101 ~ COMMON-120

---

## 필수 질문

## [COMMON-081] OSI 7계층과 TCP/IP 4계층을 비교해 설명해 주세요.

답변:

OSI 7계층은 네트워크 통신을 7단계로 구분한 이론적 참조 모델이고, TCP/IP 4계층은 인터넷 통신의 실제 표준 프로토콜입니다. OSI의 응용/표현/세션 계층이 TCP/IP의 응용 계층으로 합쳐지고, 물리/데이터링크 계층이 네트워크 인터페이스 계층으로 대응됩니다.

추가 설명)

| OSI 7계층 | TCP/IP 4계층 | 주요 프로토콜 |
|-----------|-------------|--------------|
| 7. 응용 / 6. 표현 / 5. 세션 | 응용 | HTTP, FTP, DNS, SMTP |
| 4. 전송 | 전송 | TCP, UDP |
| 3. 네트워크 | 인터넷 | IP, ICMP, ARP |
| 2. 데이터링크 / 1. 물리 | 네트워크 인터페이스 | Ethernet, Wi-Fi |

```
OSI 7계층               TCP/IP 4계층
┌────────────┐
│  7. 응용   │ ─┐
├────────────┤  │   ┌─────────────────┐
│  6. 표현   │  ├──▶│    응용 계층     │
├────────────┤  │   └─────────────────┘
│  5. 세션   │ ─┘
├────────────┤       ┌─────────────────┐
│  4. 전송   │ ─────▶│    전송 계층     │
├────────────┤       └─────────────────┘
│  3. 네트워크│ ─────▶│   인터넷 계층   │
├────────────┤       └─────────────────┘
│  2. 데이터링크│─┐  ┌─────────────────┐
├────────────┤  ├──▶│네트워크 인터페이스│
│  1. 물리   │ ─┘  └─────────────────┘
└────────────┘
```

- OSI는 이론적 표준화 목적, TCP/IP는 실제 인터넷 통신에 사용
- TCP/IP가 OSI보다 먼저 개발되어 계층이 정확히 대응되지 않음

참고 자료:
- https://velog.io/@yun8565/OSI-7%EA%B3%84%EC%B8%B5%EA%B3%BC-TCPIP-4%EA%B3%84%EC%B8%B5
- https://velog.io/@inyong_pang/OSI-7-%EA%B3%84%EC%B8%B5%EA%B3%BC-TCPIP-%EA%B3%84%EC%B8%B5

---

## [COMMON-082] TCP와 UDP의 차이에 대해 설명해 주세요.

답변:

TCP는 3-way handshake로 연결을 수립하고 순서 보장, 오류 제어, 흐름 제어를 제공하는 신뢰성 지향 프로토콜입니다. UDP는 연결 설정 없이 데이터를 전송하여 빠르지만 신뢰성이 없어 스트리밍, DNS, 게임처럼 속도가 중요한 경우에 사용합니다.

추가 설명)

| 구분 | TCP | UDP |
|------|-----|-----|
| 연결 방식 | 연결 지향 | 비연결 |
| 신뢰성 | 보장 | 미보장 |
| 순서 보장 | O | X |
| 오버헤드 | 높음 | 낮음 |
| 속도 | 느림 | 빠름 |
| 사용 사례 | HTTP, 파일 전송, 이메일 | DNS, 스트리밍, 온라인 게임 |

참고 자료:
- https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
- https://velog.io/@devharrypmw/TCPUDP-TCP%EC%99%80-UDP%EC%9D%98-%ED%8A%B9%EC%A7%95%EA%B3%BC-%EC%B0%A8%EC%9D%B4

---

## [COMMON-083] TCP 3-way handshake가 무엇이고, 왜 필요한지 설명해 주세요.

답변:

TCP 3-way handshake는 연결 수립 전 클라이언트와 서버가 서로의 존재를 확인하고 통신 준비를 마치는 과정입니다. SYN → SYN+ACK → ACK 3단계를 거쳐 양방향 통신 채널을 열고, 두 단계로는 서버 → 클라이언트 방향의 신뢰성을 보장할 수 없어 3번이 필요합니다.

추가 설명)

```
Client                       Server
  │──── SYN(seq=x) ─────────▶│  연결 요청
  │◀─── SYN+ACK(seq=y,ack=x+1)│  수락 + 서버도 연결 요청
  │──── ACK(ack=y+1) ─────────▶│  확인
  │          연결 완료          │
```

- SYN: Synchronize, 연결 요청 플래그
- ACK: Acknowledge, 수신 확인 플래그
- 2-way로는 서버가 클라이언트의 수신 능력을 확인할 수 없음
- 3번째 ACK로 클라이언트가 서버 응답을 수신했음을 확인

참고 자료:
- https://velog.io/@skyepodium/3-way-handshaking-4-way-handshaking
- https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake

---

## [COMMON-084] TCP 4-way handshake가 무엇이고, 연결 종료 과정에서 TIME_WAIT이 필요한 이유를 설명해 주세요.

답변:

4-way handshake는 TCP 연결을 종료하는 과정으로 FIN → ACK → FIN → ACK 4단계를 거칩니다. 3-way가 아닌 4-way인 이유는 서버가 FIN을 받아도 아직 전송할 데이터가 남아 있을 수 있어 ACK와 FIN을 분리 전송하기 때문입니다. TIME_WAIT은 마지막 ACK가 유실됐을 때 상대방이 재전송하는 FIN에 응답하기 위해 일정 시간 소켓을 유지하는 상태입니다.

추가 설명)

```
Client                       Server
  │──── FIN ────────────────▶│  종료 요청
  │◀─── ACK ─────────────────│  확인 (서버는 아직 데이터 전송 중)
  │◀─── FIN ─────────────────│  서버도 종료 준비 완료
  │──── ACK ────────────────▶│  확인
  │ [TIME_WAIT: 2MSL 대기]    │
  │         연결 종료          │
```

- TIME_WAIT 기간: 보통 60~240초, OS마다 상이
- TIME_WAIT 없이 포트를 재사용하면 이전 연결의 지연 패킷이 새 연결에 섞일 수 있음

참고 자료:
- https://velog.io/@yeseolee/3-Way-4-Way-HandshakeTCP-%EC%97%B0%EA%B2%B0-%EC%84%A4%EC%A0%95%EA%B3%BC-%ED%95%B4%EC%A0%9C
- https://velog.io/@skyepodium/3-way-handshaking-4-way-handshaking

---

## [COMMON-085] TCP의 신뢰성 보장 방법에 대해 설명해 주세요.

답변:

TCP는 오류 제어, 흐름 제어, 혼잡 제어 세 가지 메커니즘으로 신뢰성을 보장합니다. 오류 제어는 ARQ 기반 재전송, 흐름 제어는 수신 버퍼 크기 기반 전송량 조절, 혼잡 제어는 네트워크 상태에 따른 전송 속도 조절입니다.

추가 설명)

| 메커니즘 | 목적 | 기준 |
|---------|------|------|
| 오류 제어 | 손실/오류 패킷 복구 | Sequence Number, ACK |
| 흐름 제어 | 수신 측 버퍼 보호 | 수신 윈도우 크기 |
| 혼잡 제어 | 네트워크 혼잡 방지 | 네트워크 상태 감지 |

- **오류 제어**: Sequence Number + ACK로 손실 감지 후 ARQ 재전송
- **흐름 제어**: Sliding Window 방식, 수신 측이 처리 가능한 크기만큼만 전송
- **혼잡 제어**: Slow Start → Congestion Avoidance → Fast Retransmit 단계적 조절

참고 자료:
- https://velog.io/@jsj3282/TCP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4
- https://velog.io/@hammii/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPIP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4-%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4

---

## [COMMON-086] HTTP가 무엇이고, HTTP의 특징을 설명해 주세요.

답변:

HTTP는 클라이언트-서버 구조로 동작하는 애플리케이션 계층 프로토콜입니다. 핵심 특징은 무상태성과 비연결성으로, 서버가 클라이언트 상태를 저장하지 않아 수평 확장이 용이하지만 상태 유지가 필요한 경우 Cookie/Session을 추가로 사용해야 합니다.

추가 설명)

| 특징 | 설명 | 단점 보완 |
|------|------|----------|
| 무상태성 | 서버가 이전 요청 상태를 저장하지 않음 | Cookie / Session / Token |
| 비연결성 | 요청-응답 후 연결 종료 | Keep-Alive로 연결 재사용 |
| 클라이언트-서버 | 요청은 항상 클라이언트가 시작 | WebSocket으로 서버 Push 가능 |
| 텍스트 기반 | 사람이 읽을 수 있는 형식 | HTTP/2 이후 바이너리 프레임 |

참고 자료:
- https://velog.io/@duarufp06/HTTP-Stateless-Connectionless-HTTP-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B0%9C%EB%85%90
- https://velog.io/@jiwoow00/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%B4%9D-%EC%A0%95%EB%A6%ACStateless-%EB%A7%A4%EC%84%9C%EB%93%9C-%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C-%ED%97%A4%EB%8D%94-keep-alive-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B4%EB%8B%9D-http-%EB%B2%84%EC%A0%84%EB%B3%84-%ED%8A%B9%EC%A7%95

---

## [COMMON-087] HTTP와 HTTPS의 차이에 대해 설명해 주세요.

답변:

HTTP는 평문으로 데이터를 전송하고, HTTPS는 TLS/SSL로 암호화하여 전송합니다. HTTPS는 데이터 암호화, 서버 신원 인증, 데이터 무결성을 보장하며 포트는 각각 80, 443을 사용합니다.

추가 설명)

| 구분 | HTTP | HTTPS |
|------|------|-------|
| 포트 | 80 | 443 |
| 암호화 | X | TLS/SSL |
| 인증서 | 불필요 | CA 발급 인증서 필요 |
| 속도 | 빠름 | 핸드셰이크로 약간 느림 |
| 용도 | 일반 웹 | 로그인, 결제, 개인정보 |

```
HTTP:  Client ───[평문 데이터]────▶ Server
HTTPS: Client ───[암호화 데이터]──▶ Server
       TLS 핸드셰이크로 세션 키 교환 후 대칭 암호화 통신
```

참고 자료:
- https://velog.io/@leedohyung28/http-vs.-https-%EA%B7%B8%EB%A6%AC%EA%B3%A0-SSL-%EC%9D%B8%EC%A6%9D%EC%84%9C
- https://velog.io/@eeeve/HTTP%EC%99%80-HTTPS

---

## [COMMON-088] TLS/SSL Handshake가 어떤 역할을 하는지 설명해 주세요.

답변:

TLS 핸드셰이크는 클라이언트와 서버가 암호화 통신을 시작하기 전 사용할 암호화 알고리즘과 세션 키를 협상하는 과정입니다. 비대칭 암호화로 세션 키를 안전하게 교환한 뒤, 실제 데이터는 해당 세션 키로 대칭 암호화하여 전송합니다.

추가 설명)

```
Client                              Server
  │── Client Hello (지원 암호화 목록) ──▶│
  │◀── Server Hello (선택된 암호화) ─────│
  │◀──── 서버 인증서 (공개키 포함) ───────│
  │─ 세션 키 생성 후 공개키로 암호화 전송 ─▶│
  │       서버: 개인키로 복호화            │
  │◀────── Finished (암호화 확인) ────────│
  │════════ 이후 대칭 암호화 통신 ═════════│
```

- 비대칭 암호화: 세션 키 교환 시 사용, 느리지만 안전
- 대칭 암호화: 실제 데이터 통신에 사용, 빠름
- CA(Certificate Authority)가 서버 인증서의 신뢰성 보증

참고 자료:
- https://velog.io/@minh0518/TLSSSL-HandShake%EA%B0%80-%EB%8F%84%EB%8C%80%EC%B2%B4-%EB%AD%98%EA%B9%8C
- https://velog.io/@ann0905/HTTPS-2.-HTTPS%EC%99%80-SSL-Handshake

---

## [COMMON-089] DNS가 무엇이고, 도메인 이름이 IP 주소로 변환되는 과정을 설명해 주세요.

답변:

DNS는 사람이 읽을 수 있는 도메인 이름을 컴퓨터가 통신에 사용하는 IP 주소로 변환하는 분산 계층 시스템입니다. 브라우저 캐시 → OS 캐시 → 로컬 DNS 서버 → Root → TLD → 권한 DNS 서버 순으로 반복 질의하여 IP를 찾습니다.

추가 설명)

```
브라우저 캐시 없음
      ↓
OS hosts 파일 캐시 없음
      ↓
로컬 DNS 서버 (ISP 제공)
      ↓
Root DNS 서버 (.com 담당 TLD 주소 반환)
      ↓
TLD DNS 서버 (example.com 담당 권한 DNS 주소 반환)
      ↓
권한 DNS 서버 → IP 주소 반환
      ↓
브라우저: 획득한 IP로 TCP 연결
```

- TTL: DNS 캐시 유효 시간, 만료 전까지 재조회 불필요
- 반복 질의: 로컬 DNS가 루트부터 순차적으로 직접 질의
- 재귀 질의: 로컬 DNS가 전체 과정 대신 처리 후 결과만 반환

참고 자료:
- https://velog.io/@park-moen/devcourse-study01
- https://velog.io/@invidam/DNS-%EC%84%9C%EB%B2%84%EA%B0%80-IP%EC%A3%BC%EC%86%8C%EB%A5%BC-%EA%B0%80%EC%A0%B8%EC%98%A4%EB%8A%94-%EB%B0%A9%EB%B2%95

---

## [COMMON-090] 브라우저에 URL을 입력했을 때 서버 응답을 받기까지의 흐름을 설명해 주세요.

답변:

URL을 입력하면 DNS로 IP를 조회하고, TCP 3-way handshake로 연결을 맺은 뒤 HTTP 요청을 전송합니다. HTTPS라면 TLS 핸드셰이크가 추가되며, 서버 응답을 받은 브라우저가 HTML을 파싱하고 CSS/JS를 로드하여 화면을 렌더링합니다.

추가 설명)

```
1. URL 입력 및 파싱
2. DNS 조회 → IP 주소 획득
3. TCP 3-way Handshake
   (HTTPS: TLS Handshake 추가)
4. HTTP 요청 전송 (GET /index.html HTTP/1.1)
5. 서버 처리 → HTTP 응답 (200 OK + HTML)
6. 브라우저 HTML 파싱 → DOM 생성
7. CSS 파싱 → CSSOM 생성
8. Render Tree 구성 → Layout → Paint → Composite
```

참고 자료:
- https://velog.io/@harperkwon/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-URL%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%98%EB%A9%B4-%EC%96%B4%EB%96%A4-%EC%9D%BC%EC%9D%B4-%EC%9D%BC%EC%96%B4%EB%82%A0%EA%B9%8C
- https://velog.io/@khy226/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-url%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%98%EB%A9%B4-%EC%96%B4%EB%96%A4%EC%9D%BC%EC%9D%B4-%EB%B2%8C%EC%96%B4%EC%A7%88%EA%B9%8C

---

## [COMMON-091] HTTP 메서드 GET, POST, PUT, PATCH, DELETE의 차이를 설명해 주세요.

답변:

GET은 리소스 조회, POST는 새 리소스 생성, PUT은 리소스 전체 교체, PATCH는 리소스 부분 수정, DELETE는 리소스 삭제입니다. GET/DELETE는 멱등성을 가지고, POST는 멱등하지 않으며, PUT은 멱등이지만 PATCH는 구현에 따라 다릅니다.

추가 설명)

| 메서드 | 역할 | 바디 | 멱등 | 안전 |
|--------|------|------|------|------|
| GET | 조회 | X | O | O |
| POST | 생성 | O | X | X |
| PUT | 전체 교체 | O | O | X |
| PATCH | 부분 수정 | O | 구현 의존 | X |
| DELETE | 삭제 | X | O | X |

- 멱등: 같은 요청을 여러 번 해도 결과가 동일
- 안전: 리소스를 변경하지 않음
- PUT vs PATCH: PUT은 명시 안 된 필드를 null로 덮어씀, PATCH는 명시한 필드만 수정

참고 자료:
- https://velog.io/@galong/HTTP-%EB%A9%94%EC%84%9C%EB%93%9C-GET-POST-PUT-PATCH-DELETE
- https://velog.io/@woply/HTTP-%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-5%EA%B0%80%EC%A7%80-%EC%A0%95%EB%A6%ACGET-POST-PUT-PATCH-DELETE

---

## [COMMON-092] HTTP 상태 코드 2xx, 3xx, 4xx, 5xx의 의미를 설명해 주세요.

답변:

2xx는 성공, 3xx는 리다이렉션, 4xx는 클라이언트 오류, 5xx는 서버 오류를 의미합니다. 4xx와 5xx의 차이는 잘못된 요청을 보낸 쪽이 클라이언트인지 서버인지입니다.

추가 설명)

| 코드 | 의미 | 설명 |
|------|------|------|
| 200 | OK | 요청 성공 |
| 201 | Created | 리소스 생성 성공 (POST 응답) |
| 204 | No Content | 성공, 응답 바디 없음 (DELETE 응답) |
| 301 | Moved Permanently | 영구 리다이렉션 (URL 변경) |
| 302 | Found | 임시 리다이렉션 |
| 304 | Not Modified | 캐시 사용 가능 |
| 400 | Bad Request | 잘못된 요청 문법 |
| 401 | Unauthorized | 인증 필요 |
| 403 | Forbidden | 인가 없음 (권한 부족) |
| 404 | Not Found | 리소스 없음 |
| 500 | Internal Server Error | 서버 내부 오류 |
| 503 | Service Unavailable | 서버 과부하/점검 |

참고 자료:
- https://velog.io/@xxziiko/HTTP-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%99%9C%EC%9A%A9-%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C

---

## [COMMON-093] RESTful API가 무엇인지 설명해 주세요.

답변:

REST는 자원을 URI로 표현하고, HTTP 메서드로 행위를 정의하는 아키텍처 스타일입니다. RESTful API는 무상태성, 통일된 인터페이스, 클라이언트-서버 분리 등 REST 제약 조건을 따르는 API를 말합니다.

추가 설명)

REST 주요 제약 조건:
1. **클라이언트-서버 분리**: UI와 데이터 저장 관심사 분리
2. **무상태성**: 서버가 클라이언트 상태 저장 안 함
3. **캐시 가능**: 응답에 캐시 가능 여부 명시
4. **통일된 인터페이스**: URI + HTTP 메서드 표준화
5. **계층형 시스템**: 중간 서버 존재 가능

URI 설계 원칙:
```
GET    /users         사용자 목록 조회
GET    /users/1       사용자 1 조회
POST   /users         사용자 생성
PUT    /users/1       사용자 1 전체 수정
DELETE /users/1       사용자 1 삭제
```

- URI는 명사 사용, 동사 사용 지양 (/getUser X → /users O)
- 계층 관계는 슬래시로 표현 (/users/1/orders)

참고 자료:
- https://velog.io/@rlaclgns321/REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
- https://velog.io/@joje/API-%EC%84%9C%EB%B2%84%EC%99%80-REST

---

## [COMMON-094] Cookie, Session, Token의 차이에 대해 설명해 주세요.

답변:

Cookie는 클라이언트 브라우저에 저장되는 데이터 조각이고, Session은 인증 정보를 서버에 저장하고 클라이언트는 세션 ID만 Cookie에 보관하는 방식입니다. Token은 서버가 상태를 저장하지 않고 클라이언트가 토큰 자체에 인증 정보를 담아 관리하는 무상태 방식입니다.

추가 설명)

| 구분 | 저장 위치 | 서버 부하 | 보안 | 확장성 |
|------|----------|----------|------|--------|
| Cookie | 클라이언트 | 없음 | 낮음 (탈취 취약) | 높음 |
| Session | 서버 | 있음 | 높음 | 낮음 (서버 종속) |
| Token(JWT) | 클라이언트 | 없음 | 중간 (서명으로 위변조 방지) | 높음 |

- Session은 서버 메모리 사용, 스케일 아웃 시 세션 공유 문제 발생
- JWT는 Stateless라 서버 확장에 유리하지만, 토큰 탈취 시 만료 전 무효화 어려움

참고 자료:
- https://velog.io/@park-moen/%EC%9D%B8%EC%A6%9D-%EB%B0%A9%EC%8B%9D-Cookie-vs-Cookie-Session-vs-JWT
- https://velog.io/@kimdy0915/%EC%9D%B8%EC%A6%9D-%EB%B0%A9%EC%8B%9D%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98-JWT%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90

---

## [COMMON-095] Stateless하다는 것이 무엇이고, HTTP가 Stateless한 것이 어떤 장단점을 가지는지 설명해 주세요.

답변:

Stateless는 서버가 클라이언트의 이전 요청 상태를 저장하지 않는 설계 방식입니다. HTTP는 기본적으로 Stateless하여 서버를 수평 확장하기 쉽고 특정 서버에 종속되지 않지만, 매 요청마다 인증 정보 등을 포함해야 하는 단점이 있습니다.

추가 설명)

| 구분 | Stateless | Stateful |
|------|----------|---------|
| 상태 저장 | 클라이언트가 관리 | 서버가 관리 |
| 확장성 | 수평 확장 용이 | 확장 시 세션 동기화 필요 |
| 장애 | 서버 교체 자유 | 서버 장애 시 세션 손실 |
| 예시 | HTTP, REST | TCP 연결 유지, FTP |

장점:
- 서버 수평 확장 용이, 아무 서버나 요청 처리 가능
- 서버 자원 효율적 사용

단점:
- 매 요청에 인증 정보 포함 필요 → Cookie/Session/Token으로 보완

참고 자료:
- https://velog.io/@jrk9204/Stateless
- https://velog.io/@rmaomina/network-http

---

## [COMMON-096] CORS가 무엇이고, 왜 발생하는지 설명해 주세요.

답변:

CORS는 브라우저의 동일 출처 정책에 의해 다른 Origin의 리소스 요청이 차단될 때 발생합니다. Origin은 프로토콜 + 호스트 + 포트의 조합이며, 셋 중 하나라도 다르면 Cross-Origin입니다. 서버가 응답 헤더에 Access-Control-Allow-Origin을 설정하여 허용된 출처를 명시하면 해결됩니다.

추가 설명)

```
SOP (Same-Origin Policy):
https://frontend.com ──요청──▶ https://api.backend.com
                                다른 Origin → 브라우저가 차단 (CORS 오류)

해결: 서버에서 응답 헤더 추가
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
```

- Preflight 요청: 브라우저가 실제 요청 전 OPTIONS 메서드로 서버에 허용 여부 확인
- Simple Request: 일부 GET, POST는 Preflight 없이 바로 전송
- CORS는 서버가 아닌 브라우저 보안 정책, curl/서버 간 통신에는 미적용

참고 자료:
- https://velog.io/@asj1966/CORS-%EC%98%A4%EB%A5%98
- https://velog.io/@dessin/CORS-error%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C

---

## [COMMON-097] Blocking / Non-blocking과 Synchronous / Asynchronous의 차이에 대해 설명해 주세요.

답변:

Blocking/Non-blocking은 함수 호출 시 제어권을 즉시 반환하는지 여부이고, Synchronous/Asynchronous는 작업 완료 확인을 호출자가 하는지 피호출자가 알리는지의 차이입니다. Blocking은 완료까지 대기하고, Non-blocking은 바로 반환하며, Async는 완료 시 콜백으로 통보합니다.

추가 설명)

| 구분 | 의미 | 핵심 기준 |
|------|------|----------|
| Blocking | 호출 후 완료까지 대기 | 제어권 반환 여부 |
| Non-blocking | 호출 후 즉시 반환 | 제어권 반환 여부 |
| Synchronous | 호출자가 완료 여부 확인 | 완료 확인 주체 |
| Asynchronous | 완료 시 콜백/이벤트 통보 | 완료 확인 주체 |

4가지 조합:
```
Sync  + Blocking    : 전화 통화 (응답까지 기다림)
Sync  + Non-blocking: 주기적으로 직접 완료 확인
Async + Non-blocking: 이메일 (보내고 다른 일, 답장 오면 알림)
Async + Blocking    : 잘 사용하지 않음
```

참고 자료:
- https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx
- https://velog.io/@maketheworldwise/SyncAsync-BlockingNon-Blocking-%EB%AC%B4%EC%8A%A8-%EC%B0%A8%EC%9D%B4%EC%9D%BC%EA%B9%8C

---

## [COMMON-098] I/O Multiplexing이 무엇이고, 왜 필요한지 설명해 주세요.

답변:

I/O Multiplexing은 하나의 스레드로 여러 I/O 이벤트를 동시에 감시하여 처리하는 기법입니다. select, poll, epoll 같은 시스템 콜로 여러 소켓/파일 디스크립터를 모니터링하고 이벤트가 발생한 것만 처리합니다. 스레드 생성 비용 없이 동시에 많은 연결을 처리할 수 있어 C10K 문제 해결에 활용됩니다.

추가 설명)

```
단순 Blocking 처리 (연결당 스레드):
연결1 ──스레드1── 처리
연결2 ──스레드2── 처리   ← 스레드 수만큼 자원 소모
...

I/O Multiplexing:
         ┌── 연결1 이벤트
select ──┤── 연결2 이벤트  → 이벤트 발생한 것만 처리
         └── 연결3 이벤트
         (하나의 스레드로 모두 감시)
```

| 함수 | 특징 | FD 제한 |
|------|------|--------|
| select | 가장 오래됨, 포터블 | 1024개 |
| poll | select 개선 | 제한 없음 |
| epoll | Linux 최적화, 변경된 FD만 반환 | 제한 없음 |

참고 자료:
- https://velog.io/@kafkaaaa/IO-Multiplexing-%EC%9E%85%EC%B6%9C%EB%A0%A5-%EB%8B%A4%EC%A4%91%ED%99%94
- https://velog.io/@jsj3282/Blocking-IO-vs-Non-Blocking-IO-synchronous-IO-vs-asynchronous-IO-10k-problem

---

## [COMMON-099] WebSocket이 무엇이고, HTTP와 어떤 차이가 있는지 설명해 주세요.

답변:

HTTP는 클라이언트가 요청하면 서버가 응답하는 단방향 구조인 반면, WebSocket은 한 번 연결 후 클라이언트와 서버가 자유롭게 양방향으로 메시지를 주고받는 Full-Duplex 프로토콜입니다. 초기 연결은 HTTP Upgrade 요청으로 시작하고, 연결 후에는 ws:// 또는 wss://로 통신합니다.

추가 설명)

| 구분 | HTTP | WebSocket |
|------|------|-----------|
| 통신 방식 | 단방향 (요청-응답) | 양방향 |
| 연결 유지 | 비연결 (응답 후 종료) | 연결 유지 |
| 서버 Push | 불가 | 가능 |
| 오버헤드 | 매 요청마다 헤더 | 최초 연결 시 1회 |
| 사용 사례 | 일반 API | 채팅, 주식 시세, 게임 |

```
HTTP:
Client ──GET /──▶ Server
Client ◀──200── Server  (연결 종료)

WebSocket:
Client ──Upgrade: websocket──▶ Server  (최초 HTTP 핸드셰이크)
Client ◀════════ 연결 유지 ════════ Server
Client ──message──▶ Server
Server ──push────▶ Client
```

참고 자료:
- https://velog.io/@sj_yun/Web-Socket%EC%9D%B4%EB%9E%80
- https://velog.io/@rhdmstj17/%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC-2

---

## [COMMON-100] Load Balancing이 무엇이고, 어떤 방식들이 있는지 설명해 주세요.

답변:

Load Balancing은 여러 서버에 트래픽을 분산하여 단일 서버의 과부하를 방지하고 가용성과 확장성을 높이는 기술입니다. 로드 밸런서가 클라이언트와 서버 사이에 위치하여 요청을 여러 서버에 분배합니다.

추가 설명)

| 방식 | 설명 | 적합한 상황 |
|------|------|-----------|
| Round Robin | 순서대로 순환 분배 | 서버 성능이 동일할 때 |
| Weighted Round Robin | 성능 비율로 분배 | 서버 사양이 다를 때 |
| Least Connection | 현재 연결 수가 가장 적은 서버 | 요청 처리 시간이 다를 때 |
| IP Hash | 클라이언트 IP 해시로 고정 서버 연결 | 세션 유지가 필요할 때 |

```
         ┌───────────────────┐
Client ──▶   Load Balancer   │
         └──┬────┬────┬──────┘
            ▼    ▼    ▼
         Server1 Server2 Server3
```

- L4 로드밸런싱: IP/포트 기반, 전송 계층에서 동작
- L7 로드밸런싱: URL/HTTP 헤더 기반, 응용 계층에서 동작, 더 세밀한 라우팅 가능

참고 자료:
- https://velog.io/@bagt/%EB%A1%9C%EB%93%9C-%EB%B0%B8%EB%9F%B0%EC%8B%B1-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EA%B3%BC-Load-Balancer%EC%9D%98-%EC%A2%85%EB%A5%98
- https://velog.io/@sh93/%EB%A1%9C%EB%93%9C-%EB%B0%B8%EB%9F%B0%EC%8B%B1Load-balancing

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
