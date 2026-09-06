# Week 07 - Java / Spring Backend Role-Based Interview 2

---

## 제출 기준

- 필수 답변: ROLE-041 ~ ROLE-060
- 선택 답변: ROLE-061 ~ ROLE-080

---

## 필수 질문

## [ROLE-041] Java에서 객체 생성과 메모리 할당이 어떻게 이루어지는지 Heap과 Stack 관점에서 설명해 주세요.

답변:
- `new`로 만든 객체는 Heap에서 관리되고, Method를 호출할 때 생기는 지역 변수와 연산 정보는 Thread별 Stack Frame에서 관리됩니다.
- JVM의 논리적인 메모리 구조에서 Heap은 모든 Thread가 공유하며 Class Instance와 Array가 저장되는 영역입니다.
- Stack은 Thread마다 별도로 생성되고, Method가 호출될 때마다 Frame이 쌓입니다. Frame에는 지역 변수, Operand Stack, 반환에 필요한 정보 등이 들어갑니다.
- 예를 들어 `Member member = new Member();`를 Method 안에서 실행하면 지역 변수 `member`는 객체를 가리키는 참조값을 가지고, 실제 `Member` 객체는 Heap에서 관리됩니다.
- 객체를 생성할 때 JVM은 객체에 필요한 공간을 확보하고 Field를 기본값으로 초기화한 다음 Constructor를 실행합니다. 생성이 끝나면 객체의 참조값이 반환됩니다.
- 참조값이 항상 Stack에 있는 것은 아닙니다. 지역 변수라면 Stack Frame에 들어가지만, 다른 객체의 Field라면 그 객체와 함께 Heap에서 관리됩니다.
- JIT Compiler의 Escape Analysis에 따라 객체 할당이 제거되는 등의 최적화가 가능하므로, 실제 물리적 배치는 JVM 구현에 따라 달라질 수 있습니다. Heap과 Stack 설명은 JVM의 논리적인 실행 구조를 기준으로 이해하는 것이 좋습니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/26/docs/specs/jvms/jvms-2.html#jvms-2.5
- https://docs.oracle.com/javase/specs/jls/se26/html/jls-15.html#jls-15.9
- https://docs.oracle.com/en/java/javase/26/vm/java-hotspot-virtual-machine-performance-enhancements.html

---

## [ROLE-042] JVM Garbage Collector의 Young Generation과 Old Generation 개념을 설명해 주세요.

답변:
- 새로 생성된 객체는 주로 Young Generation에서 시작하고, GC에서 여러 번 살아남은 객체는 Old Generation으로 이동합니다.
- 세대별 GC는 대부분의 객체가 생성된 뒤 오래 살아남지 못한다는 약한 세대 가설(Weak Generational Hypothesis)을 이용합니다.
- Young Generation은 새 객체가 주로 할당되는 영역이며, 전통적으로 Eden과 Survivor 영역으로 설명합니다. 살아 있는 객체는 GC를 거치며 Survivor로 이동하고 일정 조건을 만족하면 Old Generation으로 승격됩니다.
- Old Generation은 비교적 오래 살아남았거나 크기가 큰 객체가 저장되는 영역입니다. 일반적으로 Young 영역보다 크고 수집 빈도는 낮지만, 수집할 데이터가 많아지면 더 큰 비용이 들 수 있습니다.
- 예를 들어 요청 처리 중 잠깐 사용하는 DTO는 Young 영역에서 빠르게 사라질 가능성이 높고, 오래 유지되는 Cache 객체는 Old 영역으로 이동할 가능성이 높습니다.
- G1 GC는 Heap을 물리적으로 연속된 Young/Old 공간으로 고정하지 않고 여러 Region 중 일부를 논리적인 Young 또는 Old 영역으로 사용합니다. 따라서 세부 구조와 승격 조건은 사용하는 GC에 따라 달라질 수 있습니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/17/gctuning/garbage-collector-implementation.html
- https://docs.oracle.com/en/java/javase/17/gctuning/garbage-first-g1-garbage-collector1.html
- https://d2.naver.com/helloworld/1329

---

## [ROLE-043] Minor GC와 Major GC 또는 Full GC의 차이를 설명해 주세요.

답변:
- Minor GC는 주로 Young Generation을 수집하고, Full GC는 일반적으로 Heap 전체를 대상으로 하므로 더 큰 정지 시간이 발생할 가능성이 높습니다.
- Minor GC 또는 Young GC는 Young Generation의 공간이 부족할 때 주로 발생합니다. 도달할 수 없는 객체를 제거하고 살아남은 객체를 Survivor 영역이나 Old Generation으로 이동시킵니다.
- Major GC는 보통 Old Generation 수집을 뜻하지만 JVM과 GC 문서마다 의미가 다르게 사용될 수 있습니다.
- Full GC는 일반적으로 Young과 Old를 포함한 전체 Heap을 수집하며, Collector에 따라 Class Metadata 정리 등이 함께 수행될 수 있습니다.
- Minor GC도 Application Thread를 잠시 멈추는 Stop-The-World 구간을 가질 수 있으므로, Minor GC는 무조건 정지가 없다고 보면 안 됩니다.
- G1 GC에는 Young Collection 외에도 Young Region과 일부 Old Region을 함께 수집하는 Mixed Collection이 있습니다. 따라서 `Minor = Young`, `Major = Old`, `Full = 전체`는 기본 개념으로 사용하되 실제 GC Log의 원인과 Collector 종류를 함께 확인해야 합니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/17/gctuning/garbage-collector-implementation.html
- https://docs.oracle.com/en/java/javase/17/gctuning/garbage-first-g1-garbage-collector1.html
- https://d2.naver.com/helloworld/37111

---

## [ROLE-044] Java에서 OutOfMemoryError가 발생할 수 있는 원인과 대응 방법을 설명해 주세요.

답변:
- OutOfMemoryError는 JVM이 필요한 메모리나 Native Resource를 더 이상 확보할 수 없을 때 발생하며, 먼저 오류의 상세 메시지로 부족한 영역을 구분해야 합니다.
- 대표적인 원인은 해제되지 않는 객체가 계속 쌓이는 Memory Leak, 처리량에 비해 작은 Heap, 한 번에 지나치게 큰 객체나 배열을 만드는 경우입니다.
- Class를 과도하게 동적으로 생성하거나 ClassLoader가 해제되지 않으면 `Metaspace`, Direct Buffer를 과도하게 사용하면 `Direct buffer memory`, Thread를 지나치게 생성하면 `unable to create native thread` 오류가 발생할 수 있습니다.
- 대응할 때는 먼저 OOM 상세 메시지, Heap 사용량, GC Log, Thread 수와 Native Memory를 확인하여 어느 영역이 부족한지 구분합니다.
- Heap 문제라면 `-XX:+HeapDumpOnOutOfMemoryError`로 Heap Dump를 남기고 Eclipse MAT 등의 도구로 어떤 객체가 많이 남아 있으며 누가 참조하고 있는지 분석할 수 있습니다.
- 원인이 Memory Leak이면 불필요한 참조, 무제한 Collection이나 Cache 등을 수정하고, 정상적인 사용량에 비해 메모리 크기가 부족한 경우에만 측정 결과를 근거로 `-Xmx` 등의 설정을 조정합니다. 단순히 Heap만 늘리면 문제 발생 시점만 늦출 수 있습니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/26/troubleshoot/troubleshooting-memory-leaks.html
- https://docs.oracle.com/en/java/javase/24/troubleshoot/prepare-java-troubleshooting.html
- https://docs.oracle.com/en/java/javase/26/docs/specs/man/java.html
- https://pkgonan.github.io/2019/03/java-jvm-heap-space-out-of-memory-error-troubleshooting

---

## [ROLE-045] Java에서 동시성 문제를 해결하기 위해 Atomic 클래스, synchronized, Lock을 각각 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
- 단일 값의 원자적 변경에는 Atomic 클래스, 단순한 임계 영역에는 `synchronized`, 세밀한 잠금 제어가 필요할 때는 `Lock`을 사용할 수 있습니다.
- Atomic 클래스는 CAS(Compare-And-Set)를 이용하여 `incrementAndGet()` 같은 단일 변수의 연산을 원자적으로 수행합니다. Counter처럼 한 값의 단순한 변경에는 편리하지만 여러 변수의 상태를 한 번에 맞춰야 하는 복합 로직에는 부족할 수 있습니다.
- `synchronized`는 한 Thread만 임계 영역에 들어가게 하고, 잠금을 획득하고 해제하는 과정에서 메모리 가시성도 보장합니다. 문법이 단순하고 Block을 벗어나면 JVM이 잠금을 자동으로 해제하므로 일반적인 상호 배제에 적합합니다.
- `Lock`, 대표적으로 `ReentrantLock`은 `tryLock()`, 대기 시간 제한, Interrupt 가능한 잠금, 여러 `Condition`과 같은 기능이 필요할 때 사용합니다. 대신 `finally`에서 반드시 `unlock()`해야 합니다.
- 예를 들어 단순 방문 횟수 증가는 `AtomicLong`, 계좌 잔액 확인과 차감을 하나의 임계 영역으로 묶을 때는 `synchronized`, 일정 시간만 잠금을 기다리고 실패 처리해야 한다면 `ReentrantLock`을 고려할 수 있습니다.
- 어떤 방법이든 공유 상태의 범위와 임계 영역을 먼저 줄이고, 단일 JVM의 잠금만으로 여러 서버가 공유하는 DB 데이터까지 보호할 수 없다는 점을 고려해야 합니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/concurrent/atomic/package-summary.html
- https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/concurrent/locks/Lock.html
- https://docs.oracle.com/javase/specs/jls/se26/html/jls-17.html
- https://sunghyun98.tistory.com/399

---

## [ROLE-046] ConcurrentHashMap이 HashMap과 어떤 차이를 가지며, 멀티스레드 환경에서 왜 필요한지 설명해 주세요.

답변:
- `HashMap`은 동시 수정을 보장하지 않지만 `ConcurrentHashMap`은 여러 Thread가 안전하게 조회하고 수정할 수 있도록 설계된 Map입니다.
- `HashMap`을 여러 Thread가 동시에 수정하면 갱신이 유실되거나 내부 상태가 일관되지 않을 수 있으므로 외부에서 동기화해야 합니다.
- `ConcurrentHashMap`은 조회 작업의 높은 동시성을 지원하고, 갱신 시 전체 Map 하나를 항상 잠그는 대신 CAS와 필요한 구간의 잠금을 활용합니다. 세부 구현은 JDK 버전에 따라 달라질 수 있습니다.
- `get()`, `put()`, `remove()` 같은 각 연산은 Thread-Safe하지만 여러 호출을 조합한 로직 전체가 자동으로 원자적이 되는 것은 아닙니다. `get()` 후 `put()`하는 Check-Then-Act 대신 `putIfAbsent()`, `compute()`, `merge()` 같은 원자적 복합 연산을 사용해야 합니다.
- `ConcurrentHashMap`은 `null` Key와 Value를 허용하지 않으며, 반복자는 순회 중 변경을 허용하는 Weakly Consistent 특성을 가집니다.
- 예를 들어 여러 요청 Thread가 같은 사용자별 요청 횟수를 기록한다면 `compute()`를 이용해 값을 안전하게 갱신할 수 있습니다.

참고 자료:
- https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html
- https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/HashMap.html
- https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/concurrent/package-summary.html
- https://jolocal.tistory.com/33

---

## [ROLE-047] Spring에서 Bean Scope에는 어떤 것들이 있고, Singleton Scope 사용 시 주의할 점을 설명해 주세요.

답변:
- Bean Scope는 Spring이 Bean Instance를 생성하고 공유하는 범위를 정하며, 기본값인 Singleton Bean은 여러 요청이 함께 사용하므로 가급적 상태를 가지지 않아야 합니다.
- singleton은 하나의 Spring IoC Container에서 Bean 이름 하나당 Instance 하나를 사용합니다. Java의 Singleton Pattern처럼 JVM 전체에 무조건 하나라는 뜻은 아닙니다.
- prototype은 Bean을 요청할 때마다 새 Instance를 생성합니다. Spring은 생성과 의존성 주입까지 담당하지만 생성 후 Prototype Bean의 완전한 생명주기 관리는 호출자 책임입니다.
- Web 환경에서는 HTTP 요청마다 생성되는 request, Session마다 생성되는 session, ServletContext 단위의 application, WebSocket Session 단위의 websocket Scope를 사용할 수 있습니다.
- Singleton Bean의 변경 가능한 Field는 여러 Thread가 동시에 접근할 수 있어 Race Condition이 발생할 수 있습니다. 따라서 Controller와 Service는 요청별 데이터를 Field에 저장하지 않고 지역 변수로 처리하는 Stateless 구조가 좋습니다.
- Scope가 짧은 Bean을 Singleton Bean에 주입하면 생성 시점과 수명이 맞지 않을 수 있으므로 Scoped Proxy나 `ObjectProvider` 같은 방법을 고려해야 합니다.

참고 자료:
- https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html
- https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-singleton
- https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-other-injection
- https://halfmoonbearlog.tistory.com/89

---

## [ROLE-048] Spring에서 Circular Dependency가 무엇이고, 왜 문제가 되는지 설명해 주세요.

답변:
- Circular Dependency는 Bean A가 Bean B를 필요로 하고 Bean B도 다시 Bean A를 필요로 하여 의존관계가 원형으로 연결된 상태입니다.
- 예를 들어 `OrderService`의 Constructor가 `PaymentService`를 받고, `PaymentService`의 Constructor가 다시 `OrderService`를 받으면 어느 객체를 먼저 완성해야 하는지 결정할 수 없습니다.
- Constructor Injection에서 이러한 순환 참조가 생기면 Spring은 Bean을 생성하지 못하고 일반적으로 `BeanCurrentlyInCreationException`을 발생시킵니다.
- Setter나 Field Injection, `@Lazy` 등으로 일부 상황을 우회할 수 있지만 완전히 초기화되지 않은 Bean 노출, Proxy 동작의 혼란, 늦은 Runtime 오류가 생길 수 있어 근본적인 해결로 보기 어렵습니다.
- 순환 의존성은 두 Class의 책임이 강하게 결합되었거나 책임 분리가 부족하다는 신호일 수 있으며 단위 테스트와 유지보수도 어렵게 만듭니다.
- 가장 좋은 해결은 공통 책임을 제3의 Service로 분리하거나 단방향 의존관계로 재설계하고, 필요한 경우 Event를 통해 직접 의존성을 끊는 것입니다.

참고 자료:
- https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html#beans-factory-collaborators
- https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html#beans-dependency-resolution
- https://docs.spring.io/spring-boot/reference/using/spring-beans-and-dependency-injection.html

---

## [ROLE-049] Spring AOP의 Proxy 기반 동작 방식과 Self Invocation 문제가 무엇인지 설명해 주세요.

답변:
- Spring AOP는 대상 Bean을 감싼 Proxy가 Method 호출을 가로채 부가 기능을 실행하며, 같은 객체 내부에서 `this`로 호출하면 Proxy를 거치지 않아 AOP가 적용되지 않을 수 있습니다.
- Spring은 Interface가 있으면 JDK Dynamic Proxy를 사용할 수 있고, Class 기반 Proxy가 필요하면 CGLIB Proxy를 사용할 수 있습니다.
- 외부 객체가 Proxy Bean의 Method를 호출하면 Proxy가 먼저 호출을 받아 Transaction, Logging 같은 Advice를 실행한 뒤 실제 Target Method를 호출합니다.
- 반면 Target Method 안에서 같은 Class의 다른 Method를 `this.otherMethod()`로 호출하면 외부 Proxy를 다시 통과하지 않습니다. 이를 Self Invocation 문제라고 하며 `@Transactional`, `@Async`, `@Cacheable` 같은 Proxy 기반 기능이 기대대로 동작하지 않을 수 있습니다.
- Private Method는 Proxy가 Override하거나 Interface를 통해 가로챌 수 없으므로 일반적인 Proxy 기반 AOP의 적용 대상이 되기 어렵습니다.
- 해결 방법으로 AOP가 필요한 Method를 별도의 Bean으로 분리하여 외부 호출로 만들거나, 필요하면 `TransactionTemplate` 같은 Programmatic 방식을 사용할 수 있습니다. 자기 자신을 주입받는 방식은 구조를 복잡하게 만들 수 있어 신중해야 합니다.

참고 자료:
- https://docs.spring.io/spring-framework/reference/core/aop/proxying.html
- https://docs.spring.io/spring-framework/reference/core/aop/introduction-proxies.html
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html
- https://www.ttukttak-coding.dev/posts/42-spring-aop-self-invocation

---

## [ROLE-050] @Transactional이 적용되지 않는 대표적인 상황과 그 이유를 설명해 주세요.

답변:
- `@Transactional`은 일반적으로 Spring Proxy를 통과한 호출에 적용되므로, Proxy를 우회하거나 Spring이 관리하지 않는 객체를 호출하면 동작하지 않습니다.
- 같은 Class 안에서 `this.method()`로 호출하는 Self Invocation은 Proxy를 거치지 않으므로 호출된 Method의 `@Transactional` 설정이 새로 적용되지 않습니다.
- `new`로 직접 생성한 객체는 Spring Bean이 아니므로 Spring이 Transaction Proxy를 만들지 못합니다.
- Private Method는 Proxy 방식에서 가로챌 수 없습니다. Method Visibility와 Proxy 종류에 따라 적용 범위가 달라질 수 있으므로 보통 외부에서 호출되는 Public Service Method에 Transaction 경계를 둡니다.
- 기본 Rollback 규칙은 `RuntimeException`과 `Error`이며 Checked Exception은 자동 Rollback 대상이 아닙니다. 필요하면 `rollbackFor`를 지정해야 합니다. 또한 Exception을 Method 안에서 잡고 밖으로 전달하지 않으면 Proxy가 실패를 알지 못해 Commit될 수 있습니다.
- 새 Thread나 `@Async` 작업에는 기존 Thread의 Transaction Context가 자동으로 전파되지 않습니다. 사용할 TransactionManager가 여러 개인 경우 잘못된 Manager를 선택해도 기대한 Resource에 Transaction이 적용되지 않을 수 있습니다.
- 실제 적용 여부는 Transaction Log와 Integration Test로 확인하고, Service의 Public Method를 다른 Bean에서 호출하는 구조로 Transaction 경계를 명확히 하는 것이 좋습니다.

참고 자료:
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/rolling-back.html
- https://docs.spring.io/spring-framework/reference/core/aop/proxying.html
- https://www.ttukttak-coding.dev/posts/42-spring-aop-self-invocation

---

## [ROLE-051] @Transactional의 propagation 옵션이 무엇이고, REQUIRED와 REQUIRES_NEW의 차이를 설명해 주세요.

답변:
- Propagation은 Transaction이 있는 상태에서 다른 Transactional Method를 호출했을 때 기존 Transaction에 참여할지 새 Transaction을 만들지 정하는 옵션입니다.
- 기본값인 `REQUIRED`는 기존 Transaction이 있으면 참여하고, 없으면 새 Transaction을 시작합니다. 여러 작업을 하나의 원자적인 업무 단위로 처리할 때 적합합니다.
- 내부 `REQUIRED` 작업이 Rollback-Only로 표시되면 같은 물리 Transaction을 사용하는 외부 작업도 Commit할 수 없으며, 외부에서 `UnexpectedRollbackException`을 받을 수 있습니다.
- `REQUIRES_NEW`는 항상 독립된 물리 Transaction을 시작하고, 기존 Transaction이 있으면 잠시 중단합니다. 내부 Transaction의 Commit과 Rollback은 외부 Transaction과 독립적입니다.
- 예를 들어 주문 Transaction의 성공 여부와 무관하게 감사 Log를 별도로 저장해야 한다면 별도 Bean의 Method에 `REQUIRES_NEW`를 적용하는 방식을 고려할 수 있습니다.
- `REQUIRES_NEW`는 외부 Transaction이 Connection을 잡은 상태에서 내부 Transaction용 Connection을 하나 더 요구할 수 있습니다. 과도하게 사용하면 Connection Pool 고갈이나 Lock 대기 문제가 생길 수 있으므로 Pool 크기와 호출 구조를 함께 검토해야 합니다.
- 이 옵션도 Proxy를 통과해야 적용되므로 같은 Class 내부 호출로 분리하면 새 Transaction이 생성되지 않을 수 있습니다.

참고 자료:
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/tx-propagation.html
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Propagation.html
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html
- https://yooniversal.github.io/study/post288/

---

## [ROLE-052] @Transactional의 isolation 옵션이 무엇이고, DB 격리 수준과 어떤 관계가 있는지 설명해 주세요.

답변:
- Isolation은 동시에 실행되는 Transaction이 서로의 변경 내용을 어느 정도까지 볼 수 있는지 정하며, Spring 설정은 실제 DB의 격리 기능을 사용합니다.
- `DEFAULT`는 사용하는 DB의 기본 격리 수준을 따릅니다.
- `READ_UNCOMMITTED`는 Commit되지 않은 값을 읽는 Dirty Read가 가능하며 가장 낮은 격리 수준입니다.
- `READ_COMMITTED`는 Dirty Read를 막지만 같은 Transaction에서 같은 Row를 다시 읽었을 때 값이 달라지는 Non-Repeatable Read가 발생할 수 있습니다.
- `REPEATABLE_READ`는 일반적으로 Dirty Read와 Non-Repeatable Read를 막지만 표준상 조건에 맞는 Row가 새로 나타나는 Phantom Read가 발생할 수 있습니다.
- `SERIALIZABLE`은 Transaction을 순차 실행한 것처럼 가장 강하게 격리하지만 Lock 대기, 충돌, 처리량 저하가 커질 수 있습니다.
- 실제 동작은 MySQL InnoDB의 MVCC와 Gap Lock처럼 DB 구현에 따라 달라질 수 있습니다. 또한 Isolation 설정은 보통 새로 시작하는 Transaction에 적용되므로 `REQUIRED`로 기존 Transaction에 참여하면 내부 Method의 다른 설정이 반영되지 않을 수 있습니다.

참고 자료:
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Isolation.html
- https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html
- https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html
- https://mangkyu.tistory.com/169

---

## [ROLE-053] JPA에서 Entity 생명주기와 영속, 준영속, 비영속, 삭제 상태를 설명해 주세요.

답변:
- Entity는 Persistence Context와의 관계에 따라 비영속, 영속, 준영속, 삭제 상태로 나뉩니다.
- 비영속(Transient)은 `new`로 만들었지만 Persistence Context가 관리하지 않는 상태입니다. 값을 바꿔도 DB에 자동 반영되지 않습니다.
- 영속(Managed/Persistent)은 `persist()`하거나 조회하여 Persistence Context가 관리하는 상태입니다. 변경 감지와 1차 Cache의 대상이 됩니다.
- 준영속(Detached)은 한때 영속 상태였지만 `detach()`, `clear()`, `close()` 등으로 Persistence Context의 관리를 벗어난 상태입니다. 값을 변경해도 자동으로 Update되지 않습니다.
- 삭제(Removed)는 `remove()`가 호출되어 삭제 예정인 상태이며 Flush 시점에 DELETE SQL이 실행됩니다.
- `merge()`는 전달받은 준영속 객체 자체를 다시 영속 상태로 만드는 것이 아니라 그 값을 복사한 영속 객체를 반환합니다. 따라서 반환값을 사용해야 합니다.
- `flush()`는 Persistence Context의 변경 내용을 DB에 동기화하는 작업이며 Transaction Commit과 같은 의미는 아닙니다. 실제 영구 반영 여부는 Transaction Commit에 달려 있습니다.

참고 자료:
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#pc
- https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2#entity-instance-s-life-cycle
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#pc-working-with-detached-data
- https://winwin0219.tistory.com/316

---

## [ROLE-054] JPA Dirty Checking이 무엇이고, 어떤 조건에서 동작하는지 설명해 주세요.

답변:
- Dirty Checking은 영속 상태 Entity의 변경을 JPA 구현체가 감지하여 Flush할 때 UPDATE SQL로 반영하는 기능입니다.
- Entity를 조회할 때 Persistence Context는 비교에 필요한 초기 상태를 관리합니다. Flush 시 현재 상태와 비교하여 변경된 Entity가 있으면 Update를 수행합니다. Hibernate는 Bytecode Enhancement를 통한 방식도 지원합니다.
- Dirty Checking이 동작하려면 Entity가 현재 Persistence Context에서 영속 상태여야 하며, 일반적으로 Transaction 안에서 변경한 뒤 Flush 또는 Commit이 일어나야 합니다.
- 예를 들어 Transactional Service에서 `member.setName("oilwoo")`만 호출해도 Managed Entity라면 별도의 `save()` 호출 없이 Commit 과정에서 Update될 수 있습니다.
- 준영속 또는 비영속 Entity를 변경하면 자동 반영되지 않습니다. `readOnly` Transaction이나 Entity를 Read-Only로 관리하는 설정에서는 변경 감지가 생략되거나 반영되지 않을 수 있습니다.
- JPQL Bulk Update는 Persistence Context를 거치지 않고 DB를 직접 변경하므로 이미 관리 중인 Entity와 DB 상태가 달라질 수 있습니다. 필요하면 Bulk 연산 뒤 `clear()`하여 오래된 상태를 제거해야 합니다.

참고 자료:
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#pc-managed-state
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#pc-bytecode-enhancement
- https://docs.spring.io/spring-data/jpa/reference/jpa/transactions.html
- https://yoonsm45.tistory.com/27

---

## [ROLE-055] JPA에서 연관관계의 주인이 무엇이고, 왜 필요한지 설명해 주세요.

답변:
- 양방향 연관관계에서 외래 키의 값을 실제로 변경하는 쪽을 연관관계의 주인이라고 합니다.
- 객체에서는 `Team.members`와 `Member.team`처럼 양쪽에서 서로를 참조할 수 있지만, 관계형 DB에서는 보통 하나의 외래 키로 관계를 표현합니다. JPA는 어느 쪽의 변경을 외래 키에 반영할지 정해야 합니다.
- 일반적인 `OneToMany`와 `ManyToOne` 양방향 관계에서는 외래 키를 가진 `ManyToOne` 쪽이 주인이 됩니다.
- 주인이 아닌 쪽에는 `mappedBy`를 지정합니다. 이쪽은 조회를 위한 반대 방향 참조이며, 해당 Collection만 변경해도 외래 키가 Update되지 않습니다.
- 예를 들어 `team.getMembers().add(member)`만 호출하고 주인인 `member.setTeam(team)`을 호출하지 않으면 DB의 `team_id`가 기대대로 바뀌지 않을 수 있습니다.
- 객체 상태도 일관되게 유지하기 위해 `member.changeTeam(team)` 같은 연관관계 편의 Method에서 양쪽 참조를 함께 갱신하는 것이 좋습니다.
- 연관관계의 주인은 업무적으로 더 중요한 Entity를 뜻하는 것이 아니라 외래 키 Update를 관리하는 Mapping 개념입니다.

참고 자료:
- https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2#bidirectional-many-to-one-one-to-many-relationships
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#associations-one-to-many-bidirectional
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#associations-many-to-one
- https://tecoble.techcourse.co.kr/post/2021-07-30-jpa-mapping/

---

## [ROLE-056] JPA에서 Cascade와 Orphan Removal의 차이를 설명해 주세요.

답변:
- Cascade는 부모에게 수행한 영속성 작업을 자식에게 전파하고, Orphan Removal은 부모와의 관계가 끊어진 자식을 삭제합니다.
- `cascade = PERSIST`는 부모를 저장할 때 자식도 저장하고, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`도 각각 해당 작업을 자식에게 전파합니다. `ALL`은 모든 Cascade 작업을 포함합니다.
- `orphanRemoval = true`는 부모 Collection에서 자식을 제거하거나 단일 연관관계를 `null`로 만들어 고아가 되면 해당 자식을 DB에서 삭제하도록 합니다.
- `CascadeType.REMOVE`는 부모 자체를 삭제할 때 연결된 자식 삭제를 전파합니다. 반면 Orphan Removal은 부모가 남아 있더라도 부모와 자식의 관계가 끊어지면 자식을 삭제할 수 있습니다.
- 예를 들어 주문과 주문 항목의 생명주기가 완전히 같다면 Cascade와 Orphan Removal을 함께 고려할 수 있습니다. 하지만 자식이 다른 부모와 공유되거나 독립적인 생명주기를 가진다면 자동 삭제로 데이터가 사라질 수 있어 주의해야 합니다.
- 특히 `ManyToMany` 관계에서 REMOVE Cascade를 사용하면 공유 중인 상대 Entity까지 삭제할 위험이 있으므로 일반적으로 신중하게 사용해야 합니다.

참고 자료:
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#pc-cascade
- https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#associations-one-to-many-orphan-removal
- https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2#orphanRemoval
- https://tecoble.techcourse.co.kr/post/2021-08-15-jpa-cascadetype-remove-vs-orphanremoval-true/

---

## [ROLE-057] Spring Security에서 SecurityFilterChain이 어떤 역할을 하는지 설명해 주세요.

답변:
- `SecurityFilterChain`은 HTTP 요청에 어떤 Security Filter들을 어떤 순서로 적용할지 정의합니다.
- Servlet Container의 `DelegatingFilterProxy`가 Spring Security의 `FilterChainProxy`로 요청을 전달하고, `FilterChainProxy`는 요청과 일치하는 `SecurityFilterChain`을 선택합니다.
- 선택된 Chain에는 CSRF 보호, 인증, Session 관리, Exception 처리, 인가 등을 담당하는 Filter들이 정해진 순서로 들어갑니다. 인증 Filter가 인가 Filter보다 먼저 동작해야 하는 것처럼 순서가 중요합니다.
- 보통 `@Bean`으로 `SecurityFilterChain`을 등록하고 `HttpSecurity`를 통해 URL별 접근 규칙, Login 방식, CSRF, CORS 등을 구성합니다.
- 여러 Chain을 만들 수도 있으며 `securityMatcher()`와 `@Order`를 이용해 API와 관리자 화면 등에 서로 다른 보안 정책을 적용할 수 있습니다. 여러 Chain 중 일반적으로 가장 먼저 일치한 Chain 하나가 사용됩니다.
- 예를 들어 `/api/**`에는 JWT Filter와 Stateless 정책을 적용하고, 나머지 Web 요청에는 Form Login과 Session 정책을 적용하도록 분리할 수 있습니다.

참고 자료:
- https://docs.spring.io/spring-security/reference/servlet/architecture.html
- https://docs.spring.io/spring-security/reference/servlet/configuration/java.html
- https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html
- https://anythingis.tistory.com/112

---

## [ROLE-058] 인증(Authentication)과 인가(Authorization)의 차이를 Spring Security 관점에서 설명해 주세요.

답변:
- 인증은 사용자가 누구인지 확인하는 과정이고, 인가는 인증된 사용자가 특정 Resource에 접근할 권한이 있는지 판단하는 과정입니다.
- Authentication 과정에서는 ID와 Password, Session, JWT 등의 Credential을 확인합니다. 인증에 성공하면 사용자와 권한 정보가 담긴 `Authentication` 객체가 생성되어 `SecurityContext`에 저장됩니다.
- Spring Security에서는 일반적으로 Authentication Filter가 요청에서 Credential을 추출하고 `AuthenticationManager`가 적절한 `AuthenticationProvider`에 인증을 위임합니다.
- Authorization 과정에서는 현재 `Authentication`이 가진 Authority와 요청 URL, Method 또는 Domain 규칙을 비교합니다. `AuthorizationManager`와 `authorizeHttpRequests`, Method Security 등을 사용할 수 있습니다.
- 예를 들어 Login하여 `oilwoo`라는 사용자를 확인하는 것은 인증이고, 해당 사용자가 `/admin`에 접근할 `ADMIN` 권한이 있는지 확인하는 것은 인가입니다.
- 인증 정보가 없거나 유효하지 않아 Login이 필요하면 일반적으로 401 응답을 사용하고, 인증은 되었지만 권한이 부족하면 403 응답을 사용합니다. Spring Security에서는 각각 `AuthenticationEntryPoint`와 `AccessDeniedHandler`가 관련 처리를 담당합니다.

참고 자료:
- https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html
- https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html
- https://docs.spring.io/spring-security/reference/servlet/architecture.html
- https://anythingis.tistory.com/112

---

## [ROLE-059] Refresh Token을 사용할 때 Access Token 탈취, Refresh Token 탈취에 각각 어떻게 대응할 수 있는지 설명해 주세요.

답변:
- Access Token은 짧게 사용하여 피해 시간을 줄이고, Refresh Token은 안전하게 보관하면서 Rotation과 재사용 탐지로 탈취에 대응해야 합니다.
- Access Token 탈취 대응으로 유효 시간을 짧게 설정하고 HTTPS를 사용하며, 필요한 Audience와 Scope만 부여하여 탈취 시 피해 범위를 줄입니다.
- Browser에서는 Token이 XSS로 노출되지 않도록 저장 위치와 CSP를 검토해야 합니다. Cookie를 사용한다면 `HttpOnly`, `Secure`, `SameSite`와 CSRF 방어를 함께 고려합니다.
- 이미 발급된 Stateless JWT를 즉시 무효화해야 한다면 짧은 만료 시간 외에 사용자 Token Version, Denylist 또는 인증 상태 조회 같은 별도 방안이 필요하지만 운영 비용과 성능을 고려해야 합니다.
- Refresh Token 탈취 대응으로 서버에는 원문 대신 Hash를 저장하고, Token을 사용할 때마다 새 Refresh Token을 발급하고 기존 Token을 폐기하는 Refresh Token Rotation을 적용할 수 있습니다.
- 폐기한 Refresh Token이 다시 사용되면 탈취 가능성이 있으므로 해당 Token Family나 사용자 Session 전체를 폐기하고 재로그인을 요구합니다. 로그아웃, Password 변경, 의심스러운 Device 탐지 시에도 Refresh Token을 폐기할 수 있어야 합니다.
- 더 높은 보안이 필요한 환경에서는 DPoP나 mTLS처럼 Token을 특정 Client의 Key에 묶는 Sender-Constrained Token도 고려할 수 있습니다.

참고 자료:
- https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html
- https://www.rfc-editor.org/rfc/rfc9700.html#name-refresh-token-protection
- https://datatracker.ietf.org/doc/html/rfc6750
- https://hoilog.tistory.com/772

---

## [ROLE-060] Spring 백엔드에서 동시 요청으로 인한 재고 차감 Race Condition을 어떻게 해결할 수 있는지 설명해 주세요.

답변:
- 여러 요청이 같은 재고를 동시에 읽고 수정하지 못하도록 DB의 원자적 Update나 Lock을 사용하고, 충돌 빈도와 서버 구조에 맞는 방법을 선택해야 합니다.
- 예를 들어 재고가 1개일 때 두 Transaction이 모두 1을 읽은 뒤 각각 0으로 저장하면 두 주문이 성공하거나 한 번의 차감이 유실될 수 있습니다. 이는 읽기와 쓰기가 분리된 Check-Then-Act 과정이 원자적이지 않기 때문입니다.
- 가장 단순한 방법은 `UPDATE stock SET quantity = quantity - 1 WHERE id = ? AND quantity > 0`처럼 조건부 감소를 하나의 SQL로 수행하고 영향받은 Row 수가 0이면 품절로 처리하는 것입니다.
- 낙관적 Lock은 `@Version`으로 충돌을 감지하고 실패한 Transaction을 제한적으로 재시도합니다. 충돌이 드물 때 유리하지만 요청이 몰리면 재시도 비용이 커질 수 있습니다.
- 비관적 Lock은 `SELECT ... FOR UPDATE` 등으로 Row를 먼저 잠가 순서대로 처리합니다. 충돌이 잦을 때 정합성을 지키기 쉽지만 Lock 대기, Timeout과 Deadlock을 관리해야 합니다.
- 여러 서버에서도 같은 DB Row의 Lock은 동작합니다. Redis 분산 Lock은 여러 Resource나 외부 작업을 포함하여 DB Transaction만으로 보호하기 어려울 때 고려하되, Lease 만료, 소유자 확인, Fencing Token과 장애 상황을 함께 설계해야 합니다.
- 중복 HTTP 요청까지 막으려면 Idempotency Key나 주문 고유 제약조건을 추가하고, Transaction 범위를 짧게 유지하며 동시성 Test로 최종 재고와 성공 주문 수가 일치하는지 검증해야 합니다. Java의 `synchronized`만 사용하면 여러 Application Instance 사이의 요청은 보호하지 못합니다.

참고 자료:
- https://docs.spring.io/spring-data/jpa/reference/jpa/locking.html
- https://dev.mysql.com/doc/refman/8.4/en/innodb-locking-reads.html
- https://leesg107.tistory.com/162

---

## 선택 질문

## [ROLE-061] Java Record가 무엇이고, DTO로 사용할 때의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-062] Java에서 equals()와 hashCode()를 함께 재정의해야 하는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-063] Java에서 불변 객체를 사용하는 이유와 불변 객체를 설계하는 방법을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-064] Spring에서 Filter, Interceptor, AOP의 차이와 각각 사용할 수 있는 상황을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-065] Controller Advice를 사용한 전역 예외 처리 구조를 설계할 때 고려해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-066] API 응답 형식을 공통 포맷으로 감싸는 방식의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-067] REST API에서 PUT과 PATCH를 Spring Controller에서 구현할 때 어떤 차이를 고려해야 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-068] Spring Validation에서 @Valid, @Validated, BindingResult의 역할을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-069] JPA에서 Fetch Join을 사용할 때 페이징 문제가 발생할 수 있는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-070] JPA에서 DTO Projection을 사용하는 이유와 Entity 조회와의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-071] QueryDSL을 사용하는 이유와 JPQL, Native Query와 비교했을 때의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-072] Spring Cache 추상화가 무엇이고, @Cacheable, @CachePut, @CacheEvict의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-073] Redis를 Spring 백엔드에서 캐시, 세션 저장소, 분산 락으로 사용할 때 각각의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-074] Spring에서 비동기 처리를 위해 @Async를 사용할 때 주의해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-075] Spring Scheduler를 사용할 때 단일 서버와 다중 서버 환경에서 각각 주의해야 할 점을 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-076] 대용량 트래픽을 받는 Spring API에서 Thread Pool, Connection Pool, Timeout을 어떻게 함께 고려해야 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-077] Spring Boot Actuator가 무엇이고, 운영 환경에서 어떤 지표를 확인할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-078] 로그 추적을 위해 Trace ID 또는 Correlation ID를 사용하는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-079] Spring 백엔드 프로젝트에서 성능 개선 경험을 설명할 때 어떤 지표와 근거를 제시해야 하는지 설명해 주세요.

답변:

참고 자료:

---

## [ROLE-080] Java/Spring 백엔드 면접에서 본인의 프로젝트 아키텍처를 설명할 때 어떤 순서로 답변하면 좋을지 설명해 주세요.

답변:

참고 자료:
