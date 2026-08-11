# Week 05 - Software Engineering / Design Pattern + Mock Interview

---

## 제출 기준

- 필수 답변: COMMON-161 ~ COMMON-180
- 선택 답변: COMMON-181 ~ COMMON-200

---

## 필수 질문

## [COMMON-161] 객체지향 프로그래밍이 무엇이고, 주요 특징을 설명해 주세요.

답변:
객체지향 프로그래밍은 프로그램을 데이터와 동작을 함께 가진 객체들의 협력으로 설계하는 방식이다. 절차 중심으로 로직을 길게 나열하기보다, 현실의 개념이나 도메인 역할을 객체로 나누고 각 객체가 자신의 책임을 수행하도록 만든다.

주요 특징은 캡슐화, 상속, 다형성, 추상화이다. 캡슐화는 내부 구현을 숨기고 필요한 기능만 공개하는 것이고, 상속은 공통 기능을 재사용하거나 확장하는 방식이다. 다형성은 같은 메시지나 인터페이스에 대해 객체마다 다른 동작을 하게 하는 것이며, 추상화는 복잡한 세부사항을 감추고 핵심 개념만 모델링하는 것이다. 이 특징들을 통해 코드 재사용성, 변경 용이성, 유지보수성을 높일 수 있다.

참고 자료: [Oracle Java Tutorials - Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html)

---

## [COMMON-162] 캡슐화, 상속, 다형성, 추상화에 대해 설명해 주세요.

답변:
캡슐화는 객체의 상태와 동작을 하나로 묶고, 외부에서는 공개된 메서드나 인터페이스를 통해서만 접근하게 하는 것이다. 상속은 기존 클래스의 속성과 동작을 하위 클래스가 물려받아 재사용하거나 확장하는 방식이고, 다형성은 부모 타입이나 인터페이스 타입으로 여러 구현체를 동일하게 다루되 실제 실행은 각 구현체에 맞게 달라지는 성질이다.

추상화는 문제 해결에 필요한 핵심 속성과 행위만 남기고 불필요한 구현 세부사항을 감추는 것이다. 예를 들어 결제 시스템에서 `PaymentMethod`라는 공통 인터페이스만 보고 카드 결제, 간편 결제, 계좌 이체를 처리하면 호출자는 각 결제 방식의 내부 구현을 몰라도 된다. 결국 네 가지 개념은 변경 영향을 줄이고 객체 간 협력을 명확히 하기 위한 도구이다.

참고 자료: [Oracle Java Tutorials - Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html)

---

## [COMMON-163] SOLID 원칙에 대해 설명해 주세요.

답변:
SOLID는 객체지향 설계를 더 유지보수하기 쉽게 만들기 위한 다섯 가지 원칙이다. 단일 책임 원칙은 하나의 클래스가 하나의 변경 이유만 가지게 하자는 것이고, 개방-폐쇄 원칙은 기존 코드를 수정하기보다 확장을 통해 기능을 추가하자는 원칙이다. 리스코프 치환 원칙은 하위 타입이 상위 타입을 대체해도 프로그램의 올바른 동작이 깨지면 안 된다는 의미이다.

인터페이스 분리 원칙은 클라이언트가 사용하지 않는 메서드에 의존하지 않도록 인터페이스를 작게 나누자는 것이고, 의존성 역전 원칙은 상위 정책 코드가 구체 구현이 아니라 추상화에 의존하게 하자는 원칙이다. SOLID의 목적은 원칙 자체를 외우는 것이 아니라 변경이 생겼을 때 수정 범위를 줄이고, 테스트하기 쉽고, 확장 가능한 구조를 만드는 데 있다.

참고 자료: [Microsoft Learn - Dangers of Violating SOLID Principles in C#](https://learn.microsoft.com/en-us/archive/msdn-magazine/2014/may/csharp-best-practices-dangers-of-violating-solid-principles-in-csharp)

---

## [COMMON-164] 단일 책임 원칙이 무엇이고, 왜 중요한지 설명해 주세요.

답변:
단일 책임 원칙은 하나의 클래스나 모듈이 하나의 책임만 가져야 하며, 하나의 변경 이유만 가져야 한다는 원칙이다. 여기서 책임은 단순히 메서드 개수가 적다는 뜻이 아니라, 변경을 요구하는 주체나 이유가 하나로 모여 있어야 한다는 의미에 가깝다.

이 원칙이 중요한 이유는 서로 다른 관심사가 한 클래스에 섞이면 한 기능을 수정하다가 다른 기능까지 깨뜨릴 가능성이 커지기 때문이다. 예를 들어 주문 계산, DB 저장, 이메일 발송이 한 클래스에 모두 있으면 가격 정책 변경, 저장 방식 변경, 알림 방식 변경이 모두 같은 코드를 흔든다. 책임을 분리하면 테스트 단위가 작아지고 변경 범위가 명확해져 유지보수성이 좋아진다.

참고 자료: [Microsoft Learn - Dangers of Violating SOLID Principles in C#](https://learn.microsoft.com/en-us/archive/msdn-magazine/2014/may/csharp-best-practices-dangers-of-violating-solid-principles-in-csharp)

---

## [COMMON-165] 의존성 역전 원칙이 무엇이고, 어떤 장점이 있는지 설명해 주세요.

답변:
의존성 역전 원칙은 상위 수준의 정책이나 비즈니스 로직이 하위 수준의 구체 구현에 직접 의존하지 않고, 둘 다 인터페이스 같은 추상화에 의존해야 한다는 원칙이다. 예를 들어 주문 서비스가 `MySqlOrderRepository`를 직접 생성해서 사용하기보다 `OrderRepository` 인터페이스에 의존하고 실제 구현체는 외부에서 주입받는 식이다.

장점은 구현 교체가 쉬워지고 테스트가 쉬워진다는 점이다. DB, 외부 API, 파일 시스템 같은 세부 구현을 가짜 객체나 다른 구현체로 바꿔도 상위 로직은 거의 수정하지 않아도 된다. 또한 비즈니스 규칙이 인프라 세부사항에 끌려가지 않기 때문에 계층 간 결합도가 낮아지고 구조가 더 유연해진다.

참고 자료: [Microsoft Learn - Designing the microservice application layer and Web API](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/microservice-application-layer-web-api-design)

---

## [COMMON-166] 인터페이스를 사용하는 이유와 장단점을 설명해 주세요.

답변:
인터페이스는 어떤 객체가 제공해야 하는 기능의 계약을 정의하기 위해 사용한다. 호출자는 구체 클래스가 무엇인지 몰라도 인터페이스에 정의된 메서드만 믿고 사용할 수 있기 때문에, 구현체 교체와 테스트 대역 사용이 쉬워지고 다형성을 활용할 수 있다. 예를 들어 알림 기능을 `NotificationSender` 인터페이스로 두면 이메일, SMS, 푸시 알림 구현체를 상황에 따라 바꿔 사용할 수 있다.

장점은 낮은 결합도, 구현 교체 용이성, 테스트 편의성, 여러 구현체를 같은 방식으로 다룰 수 있다는 점이다. 단점은 필요 이상으로 인터페이스를 만들면 추상화가 과해져 코드 추적이 어려워지고, 인터페이스 설계가 잘못되면 오히려 모든 구현체가 불필요한 메서드를 구현해야 한다는 점이다. 따라서 변화 가능성이 있거나 경계를 분리해야 할 때 사용하는 것이 좋다.

참고 자료: [Microsoft Learn - Interfaces in C#](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces)

---

## [COMMON-167] 추상 클래스와 인터페이스의 차이에 대해 설명해 주세요.

답변:
추상 클래스와 인터페이스는 모두 하위 타입이 따라야 할 계약을 정의할 수 있지만 목적이 다르다. 추상 클래스는 관련 있는 클래스들이 공통 상태나 공통 구현을 공유할 때 적합하고, 인터페이스는 서로 다른 계층의 타입들이 공통 능력이나 역할을 가져야 할 때 적합하다. 일반적으로 클래스 상속은 하나만 가능하지만 인터페이스는 여러 개를 구현할 수 있다는 차이도 있다.

예를 들어 여러 결제 수단이 공통으로 결제 로그 저장 로직과 상태를 공유한다면 추상 클래스를 고려할 수 있고, 결제 가능하다는 능력만 표현하고 싶다면 `Payable` 같은 인터페이스가 더 자연스럽다. 면접에서는 "공통 구현과 상태를 공유해야 하면 추상 클래스, 구현 교체와 역할 계약이 중요하면 인터페이스"라고 정리하면 좋다.

참고 자료: [Microsoft Learn - Interfaces vs. abstract classes](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces#interfaces-vs-abstract-classes)

---

## [COMMON-168] 디자인 패턴이 무엇이고, 왜 사용하는지 설명해 주세요.

답변:
디자인 패턴은 자주 반복되는 설계 문제에 대해 검증된 해결 구조를 이름 붙여 정리한 것이다. 특정 언어나 프레임워크의 코드 조각이라기보다 객체 생성, 객체 간 협력, 책임 분리 같은 문제를 해결하는 설계 아이디어에 가깝다.

디자인 패턴을 사용하면 이미 알려진 장단점을 바탕으로 설계할 수 있고, 개발자 사이의 의사소통이 쉬워진다. 예를 들어 "여기는 Strategy 패턴으로 분리하겠다"라고 하면 조건문으로 분기하던 알고리즘을 교체 가능한 객체로 나누겠다는 의도를 빠르게 전달할 수 있다. 다만 패턴을 억지로 적용하면 구조가 불필요하게 복잡해질 수 있으므로, 실제 변경 가능성과 문제 맥락이 있을 때 선택해야 한다.

참고 자료: [Refactoring.Guru - Design Patterns Catalog](https://refactoring.guru/design-patterns/catalog)

---

## [COMMON-169] Singleton Pattern이 무엇이고, 장단점은 무엇인지 설명해 주세요.

답변:
Singleton Pattern은 특정 클래스의 인스턴스가 애플리케이션에서 하나만 생성되도록 보장하고, 그 인스턴스에 전역적으로 접근할 수 있게 하는 생성 패턴이다. 설정 객체, 로거, 캐시처럼 하나의 공유 인스턴스가 자연스러운 경우에 사용할 수 있다.

장점은 인스턴스 생성을 제어하고 공유 자원을 일관되게 사용할 수 있다는 점이다. 단점은 전역 상태를 만들기 쉬워 테스트가 어려워지고, 클래스 간 숨은 의존성이 생길 수 있다는 점이다. 또한 멀티스레드 환경에서는 인스턴스 생성이 안전한지 신경 써야 한다. 그래서 싱글턴은 편리하지만 남용하면 결합도를 높이므로, 의존성 주입으로 대체 가능한지 함께 고려하는 것이 좋다.

참고 자료: [Refactoring.Guru - Singleton](https://refactoring.guru/design-patterns/singleton)

---

## [COMMON-170] Factory Method Pattern이 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
Factory Method Pattern은 객체 생성 로직을 직접 `new`로 고정하지 않고, 객체를 생성하는 메서드를 상위 타입에 정의한 뒤 하위 클래스나 구현체가 실제 생성할 객체를 결정하게 하는 생성 패턴이다. 핵심은 클라이언트 코드가 구체 제품 클래스에 강하게 의존하지 않도록 생성 책임을 분리하는 것이다.

예를 들어 운영체제별 버튼, 결제 수단별 결제 처리기, 파일 형식별 파서처럼 상황에 따라 생성해야 하는 객체 종류가 달라질 때 사용할 수 있다. 새 제품 타입이 추가되어도 기존 클라이언트 로직을 크게 수정하지 않고 팩토리 구현만 확장할 수 있어 개방-폐쇄 원칙을 지키는 데 도움이 된다. 다만 클래스 수가 늘어날 수 있으므로 단순한 생성에는 오히려 과할 수 있다.

참고 자료: [Refactoring.Guru - Factory Method](https://refactoring.guru/design-patterns/factory-method)

---

## [COMMON-171] Strategy Pattern이 무엇이고, 어떤 문제를 해결할 수 있는지 설명해 주세요.

답변:
Strategy Pattern은 알고리즘이나 정책을 각각 별도의 객체로 캡슐화하고, 실행 시점에 필요한 전략을 선택해서 사용할 수 있게 하는 행동 패턴이다. 즉, 같은 목적을 가진 여러 처리 방식을 공통 인터페이스로 묶고, 클라이언트는 구체 알고리즘이 무엇인지 몰라도 사용할 수 있게 만든다.

이 패턴은 조건문이 계속 늘어나는 문제를 해결하는 데 유용하다. 예를 들어 할인 정책, 정렬 방식, 결제 방식이 여러 종류라면 `if-else`나 `switch`로 모든 경우를 처리하기보다 전략 객체로 분리할 수 있다. 이렇게 하면 새 정책을 추가할 때 기존 코드를 덜 수정하게 되고, 각 전략을 독립적으로 테스트할 수 있다.

참고 자료: [Refactoring.Guru - Strategy](https://refactoring.guru/design-patterns/strategy)

---

## [COMMON-172] Observer Pattern이 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
Observer Pattern은 어떤 객체의 상태 변화가 생겼을 때 그 객체를 구독하고 있는 여러 객체에게 자동으로 알림을 보내는 행동 패턴이다. 상태를 가진 객체를 Subject, 알림을 받는 객체를 Observer라고 부르며, 일대다 관계를 느슨하게 연결하는 데 사용한다.

대표적으로 UI 이벤트 처리, 알림 시스템, 게시-구독 구조, 데이터 변경에 따른 화면 갱신에서 사용할 수 있다. 장점은 발행자가 구독자들의 구체 구현을 몰라도 알림을 보낼 수 있어 결합도가 낮아진다는 점이다. 단점은 알림 순서, 중복 이벤트, 구독 해제 누락으로 인한 메모리 문제를 관리해야 한다는 점이다.

참고 자료: [Refactoring.Guru - Observer](https://refactoring.guru/design-patterns/observer)

---

## [COMMON-173] MVC 패턴이 무엇이고, 각 구성 요소의 역할을 설명해 주세요.

답변:
MVC는 애플리케이션을 Model, View, Controller로 나누는 아키텍처 패턴이다. Model은 데이터와 비즈니스 규칙을 담당하고, View는 사용자에게 보여지는 화면 표현을 담당하며, Controller는 사용자의 입력이나 요청을 받아 적절한 Model 작업을 호출하고 어떤 View를 반환할지 결정한다.

이렇게 나누면 화면 로직과 비즈니스 로직이 섞이는 것을 줄일 수 있고, 각 영역을 독립적으로 변경하거나 테스트하기 쉬워진다. 예를 들어 화면 디자인이 바뀌어도 Model의 핵심 규칙은 유지할 수 있고, 요청 흐름이 바뀌어도 View를 재사용할 수 있다. MVC의 핵심 가치는 관심사의 분리를 통해 유지보수성을 높이는 것이다.

참고 자료: [Microsoft .NET - ASP.NET MVC Pattern](https://dotnet.microsoft.com/en-us/apps/aspnet/mvc)

---

## [COMMON-174] Layered Architecture가 무엇이고, 어떤 장단점이 있는지 설명해 주세요.

답변:
Layered Architecture는 시스템을 역할별 계층으로 나누고, 각 계층이 정해진 방향으로만 의존하도록 구성하는 구조이다. 일반적으로 Presentation, Application 또는 Service, Domain, Infrastructure 또는 Data Access 계층처럼 나눌 수 있다. 각 계층은 자신의 책임에 집중하고 다른 계층의 세부 구현을 직접 알지 않도록 설계한다.

장점은 관심사 분리가 명확하고, 계층별 테스트와 교체가 쉬우며, 팀이 역할별로 작업하기 좋다는 점이다. 단점은 단순한 기능도 여러 계층을 지나야 해서 코드량이 늘고, 잘못 설계하면 의미 없는 전달 코드가 많아질 수 있다는 점이다. 또한 모든 요청이 계층을 순서대로 거치면 성능이나 개발 속도 측면의 비용이 생길 수 있다.

참고 자료: [Azure Architecture Center - N-tier architecture style](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)

---

## [COMMON-175] 관심사의 분리가 무엇이고, 좋은 코드 구조에 왜 중요한지 설명해 주세요.

답변:
관심사의 분리는 서로 다른 목적이나 변경 이유를 가진 코드를 분리해서 배치하는 설계 원칙이다. 예를 들어 화면 표시, 비즈니스 규칙, 데이터 저장, 인증, 로깅 같은 관심사가 한 코드에 뒤섞이지 않도록 나누는 것이다.

좋은 코드 구조에서 관심사의 분리가 중요한 이유는 변경 영향을 줄이기 때문이다. UI가 바뀌었는데 비즈니스 로직까지 수정해야 하거나, DB 저장 방식이 바뀌었는데 화면 코드가 흔들린다면 구조가 강하게 결합된 것이다. 관심사를 잘 나누면 코드를 읽는 사람이 역할을 빠르게 파악할 수 있고, 테스트와 재사용도 쉬워진다.

참고 자료: [Microsoft Learn - Architectural principles](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles)

---

## [COMMON-176] 결합도와 응집도가 무엇이고, 좋은 설계에서는 각각 어떻게 관리되어야 하는지 설명해 주세요.

답변:
결합도는 한 모듈이 다른 모듈에 얼마나 의존하는지를 나타내고, 응집도는 한 모듈 내부의 요소들이 하나의 목적을 위해 얼마나 밀접하게 모여 있는지를 나타낸다. 좋은 설계는 보통 낮은 결합도와 높은 응집도를 지향한다. 즉, 모듈 내부는 하나의 책임에 집중하고, 모듈 간에는 필요한 계약만 통해 협력하도록 만드는 것이다.

결합도는 구체 클래스 직접 의존을 줄이고 인터페이스, 이벤트, 의존성 주입 등을 활용해 낮출 수 있다. 응집도는 관련 없는 기능을 한 클래스에 몰아넣지 않고 책임별로 나누는 방식으로 높일 수 있다. 다만 무조건 작게 쪼개는 것이 답은 아니며, 함께 변경되는 것들은 같은 곳에 두고 독립적으로 변경되는 것들은 분리하는 균형이 중요하다.

참고 자료: [Microsoft Learn - Patterns in Practice: Cohesion And Coupling](https://learn.microsoft.com/en-us/archive/msdn-magazine/2008/october/patterns-in-practice-cohesion-and-coupling)

---

## [COMMON-177] 테스트 코드가 필요한 이유와 단위 테스트, 통합 테스트의 차이를 설명해 주세요.

답변:
테스트 코드는 기능이 의도대로 동작하는지 빠르게 확인하고, 변경이나 리팩토링으로 기존 기능이 깨지는 회귀 버그를 줄이기 위해 필요하다. 또한 테스트는 코드의 사용 예시이자 실행 가능한 문서 역할도 한다. 테스트하기 어려운 코드는 보통 책임 분리나 의존성 관리가 잘 안 된 경우가 많아서, 테스트 코드는 설계 품질을 확인하는 신호가 되기도 한다.

단위 테스트는 하나의 함수, 클래스, 작은 컴포넌트처럼 좁은 범위를 외부 의존성과 분리해 빠르게 검증하는 테스트이다. 통합 테스트는 여러 모듈, DB, 네트워크, 외부 시스템 등이 함께 연결되었을 때 제대로 동작하는지 확인한다. 단위 테스트는 빠르고 원인 파악이 쉽지만 실제 연결 문제를 놓칠 수 있고, 통합 테스트는 현실에 가깝지만 느리고 실패 원인 분석이 더 복잡할 수 있다.

참고 자료: [Microsoft Learn - Testing in .NET](https://learn.microsoft.com/en-us/dotnet/core/testing/)

---

## [COMMON-178] 리팩토링이 무엇이고, 리팩토링 시 주의해야 할 점을 설명해 주세요.

답변:
리팩토링은 외부에서 보이는 동작은 바꾸지 않으면서 코드의 내부 구조를 개선하는 작업이다. 목적은 기능 추가가 아니라 가독성, 중복 제거, 책임 분리, 변경 용이성 개선이다. 예를 들어 긴 메서드를 작은 메서드로 나누거나, 중복 로직을 공통 함수로 추출하는 작업이 리팩토링에 해당한다.

주의할 점은 동작을 바꾸지 않는다는 원칙을 지키는 것이다. 가능하면 테스트를 먼저 확보하고, 작은 단위로 변경하며, 기능 변경과 리팩토링을 한 커밋에 섞지 않는 것이 좋다. 또한 단순히 마음에 들지 않는 코드를 전부 고치는 것이 아니라, 실제 변경 비용을 줄이는 방향인지 판단해야 한다.

참고 자료: [Martin Fowler - Definition Of Refactoring](https://martinfowler.com/bliki/DefinitionOfRefactoring.html)

---

## [COMMON-179] 기술 선택 시 trade-off를 설명한다는 것은 어떤 의미인지 설명해 주세요.

답변:
기술 선택에서 trade-off를 설명한다는 것은 어떤 기술이 무조건 좋다고 말하는 것이 아니라, 특정 요구사항과 제약 안에서 무엇을 얻고 무엇을 포기했는지 설명하는 것이다. 예를 들어 캐시를 도입하면 응답 속도는 좋아질 수 있지만 데이터 정합성 관리와 캐시 무효화 복잡도가 생긴다. MSA를 선택하면 독립 배포와 확장성은 좋아질 수 있지만 운영 복잡도와 네트워크 장애 가능성이 늘어난다.

면접에서는 문제 상황, 고려한 대안, 판단 기준, 선택한 이유, 감수한 단점, 보완책을 함께 말하는 것이 좋다. 이렇게 답하면 단순히 기술명을 아는 사람이 아니라 요구사항과 비용을 비교해 의사결정할 수 있는 사람이라는 인상을 줄 수 있다.

참고 자료: [AWS Well-Architected Framework - Evaluate how trade-offs impact customers and architecture efficiency](https://docs.aws.amazon.com/wellarchitected/latest/framework/perf_architecture_evaluate_trade_offs.html)

---

## [COMMON-180] 본인의 프로젝트에서 기술적 의사결정을 설명할 때 어떤 구조로 답변하면 좋을지 설명해 주세요.

답변:
기술적 의사결정은 "문제 상황 - 제약 조건 - 대안 - 선택 기준 - 결정 - 결과 - 아쉬운 점" 순서로 말하면 좋다. 먼저 어떤 문제가 있었고 어떤 요구사항이나 제약이 있었는지 설명한 뒤, 비교했던 대안들을 간단히 말한다. 그 다음 성능, 비용, 팀 숙련도, 유지보수성, 일정 같은 기준으로 왜 해당 기술을 선택했는지 설명한다.

마지막에는 선택 이후의 결과를 수치나 구체 사례로 말하고, 아쉬운 점과 다음에 개선할 점까지 덧붙이면 좋다. 예를 들어 "초기 개발 속도를 위해 모놀리식 구조를 선택했고, 배포와 트랜잭션 관리가 단순해졌지만 기능이 커지면서 모듈 경계가 흐려져 이후 도메인별 패키지 분리를 진행했다"처럼 장점과 한계를 함께 말하는 방식이 좋다.

참고 자료: [Microsoft Azure Well-Architected Framework - Maintain an architecture decision record](https://learn.microsoft.com/en-us/azure/well-architected/architect-role/architecture-decision-record)

---

## 선택 질문

## [COMMON-181] Open-Closed Principle을 예시와 함께 설명해 주세요.

답변:
Open-Closed Principle은 소프트웨어 요소가 확장에는 열려 있고 수정에는 닫혀 있어야 한다는 원칙이다. 새로운 요구사항이 생길 때 기존 코드를 계속 수정하기보다, 새로운 구현체를 추가하는 방식으로 기능을 확장할 수 있어야 한다는 뜻이다.

예를 들어 할인 계산 로직을 `if (vip)`, `if (coupon)`처럼 계속 추가하면 새 할인 정책이 생길 때마다 기존 코드를 수정해야 한다. 대신 `DiscountPolicy` 인터페이스를 만들고 정액 할인, 정률 할인, 쿠폰 할인 정책을 각각 구현하면 새로운 할인 정책을 클래스로 추가할 수 있다. 이렇게 하면 기존 로직의 회귀 위험을 줄이고 테스트 범위도 명확해진다.

참고 자료: [Microsoft Learn - Dangers of Violating SOLID Principles in C#](https://learn.microsoft.com/en-us/archive/msdn-magazine/2014/may/csharp-best-practices-dangers-of-violating-solid-principles-in-csharp)

---

## [COMMON-182] Liskov Substitution Principle을 예시와 함께 설명해 주세요.

답변:
Liskov Substitution Principle은 하위 타입 객체가 상위 타입 객체를 대체해도 프로그램의 기대 동작이 깨지면 안 된다는 원칙이다. 단순히 문법적으로 상속 관계가 있다고 해서 좋은 상속은 아니며, 상위 타입이 보장하던 규칙과 의미를 하위 타입도 지켜야 한다.

대표 예시로 `Rectangle`을 상속한 `Square`가 있다. 직사각형은 너비와 높이를 독립적으로 변경할 수 있다고 기대하는데, 정사각형은 너비를 바꾸면 높이도 같이 바뀌어야 한다. 이 경우 `Rectangle`을 사용하는 코드에 `Square`를 넣으면 면적 계산 같은 기대가 깨질 수 있어 LSP 위반이다. 이런 경우 상속보다 공통 인터페이스나 합성을 고려하는 것이 좋다.

참고 자료: [Liskov & Wing - A Behavioral Notion of Subtyping](https://www.cs.cmu.edu/~wing/publications/LiskovWing94.pdf)

---

## [COMMON-183] Interface Segregation Principle을 예시와 함께 설명해 주세요.

답변:
Interface Segregation Principle은 클라이언트가 자신이 사용하지 않는 메서드에 의존하도록 강제하면 안 된다는 원칙이다. 하나의 거대한 인터페이스보다 클라이언트 목적에 맞는 작은 인터페이스 여러 개로 나누는 것이 좋다는 의미이다.

예를 들어 `Machine` 인터페이스에 `print`, `scan`, `fax`가 모두 있으면 단순 프린터도 사용하지 않는 `scan`, `fax`를 구현해야 한다. 대신 `Printer`, `Scanner`, `Fax` 인터페이스로 나누면 필요한 기능만 구현할 수 있다. 이렇게 하면 불필요한 의존이 줄고, 특정 기능 변경이 관련 없는 구현체에 영향을 덜 준다.

참고 자료: [Microsoft Learn - Dangers of Violating SOLID Principles in C#](https://learn.microsoft.com/en-us/archive/msdn-magazine/2014/may/csharp-best-practices-dangers-of-violating-solid-principles-in-csharp)

---

## [COMMON-184] Dependency Injection이 무엇이고, 어떤 장점이 있는지 설명해 주세요.

답변:
Dependency Injection은 객체가 필요한 의존성을 내부에서 직접 생성하지 않고 외부에서 전달받도록 하는 방식이다. 보통 생성자 주입을 많이 사용하며, 서비스가 구체 구현이 아니라 인터페이스에 의존하도록 만들 때 함께 사용된다. 예를 들어 `OrderService`가 `new EmailSender()`를 직접 호출하지 않고 생성자로 `NotificationSender`를 전달받는 구조이다.

장점은 결합도가 낮아지고 테스트가 쉬워진다는 점이다. 실제 DB나 외부 API 대신 가짜 구현체를 주입해 단위 테스트를 할 수 있고, 구현체 교체도 설정이나 조립 코드에서 처리할 수 있다. 또한 객체 생성과 사용 책임이 분리되어 코드의 역할이 명확해진다. 다만 DI 컨테이너 설정이 복잡해지면 런타임 오류를 찾기 어려울 수 있어 구성 관리가 중요하다.

참고 자료: [Microsoft Learn - Dependency injection in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection/overview)

---

## [COMMON-185] Adapter Pattern이 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
Adapter Pattern은 서로 호환되지 않는 인터페이스를 함께 사용할 수 있도록 중간에서 변환해 주는 구조 패턴이다. 기존 클래스나 외부 라이브러리의 인터페이스가 현재 코드가 기대하는 인터페이스와 다를 때, 어댑터가 호출 형식이나 데이터 형식을 맞춰 준다.

예를 들어 우리 시스템은 `PaymentClient.pay(amount)`를 기대하는데 외부 결제 SDK는 `requestPayment(price, currency)`만 제공한다면, 어댑터 클래스가 `pay` 호출을 SDK 호출로 변환해 줄 수 있다. 이렇게 하면 기존 비즈니스 코드는 외부 SDK 세부사항에 직접 의존하지 않고, 나중에 SDK가 바뀌어도 어댑터만 수정하면 된다.

참고 자료: [Refactoring.Guru - Adapter](https://refactoring.guru/design-patterns/adapter)

---

## [COMMON-186] Proxy Pattern이 무엇이고, 어떤 상황에서 사용할 수 있는지 설명해 주세요.

답변:
Proxy Pattern은 실제 객체에 대한 접근을 대리 객체가 대신 제어하는 구조 패턴이다. 프록시는 실제 객체와 같은 인터페이스를 제공하면서 호출 전후에 접근 제어, 지연 로딩, 캐싱, 로깅, 원격 호출, 권한 검사 같은 부가 로직을 넣을 수 있다.

예를 들어 큰 이미지를 실제로 화면에 보여줄 때까지 로딩하지 않거나, 외부 API 응답을 캐싱하거나, 민감한 서비스 호출 전에 권한을 확인하는 경우에 프록시를 사용할 수 있다. 장점은 실제 객체의 핵심 로직을 수정하지 않고 접근 관련 관심사를 분리할 수 있다는 점이다. 단점은 호출 흐름이 한 단계 늘어나므로 디버깅과 구조 이해가 조금 더 복잡해질 수 있다.

참고 자료: [Refactoring.Guru - Proxy](https://refactoring.guru/design-patterns/proxy)

---

## [COMMON-187] Decorator Pattern이 무엇이고, 상속과 비교했을 때 어떤 장점이 있는지 설명해 주세요.

답변:
Decorator Pattern은 기존 객체를 감싸는 객체를 만들어 런타임에 기능을 동적으로 추가하는 구조 패턴이다. 데코레이터는 원본 객체와 같은 인터페이스를 구현하고, 요청을 원본 객체에 위임하면서 앞뒤로 추가 동작을 수행한다. 예를 들어 기본 커피 객체에 우유, 샷, 시럽 데코레이터를 감싸 가격과 설명을 확장할 수 있다.

상속과 비교했을 때 장점은 조합이 유연하다는 점이다. 기능 조합마다 하위 클래스를 만들면 클래스 수가 폭발할 수 있지만, 데코레이터는 필요한 기능을 순서대로 감싸 조합할 수 있다. 또한 기존 클래스를 수정하지 않고 기능을 추가할 수 있어 개방-폐쇄 원칙에도 잘 맞다. 다만 데코레이터가 많이 겹치면 객체 구조를 추적하기 어려워질 수 있다.

참고 자료: [Refactoring.Guru - Decorator](https://refactoring.guru/design-patterns/decorator)

---

## [COMMON-188] Facade Pattern이 무엇이고, 어떤 장점이 있는지 설명해 주세요.

답변:
Facade Pattern은 복잡한 서브시스템을 더 단순한 인터페이스로 감싸서 클라이언트가 쉽게 사용할 수 있게 하는 구조 패턴이다. 내부에는 여러 클래스와 호출 순서가 있더라도, 외부에는 자주 쓰는 기능을 하나의 진입점으로 제공하는 방식이다.

예를 들어 영상 변환 기능을 사용하려면 코덱 선택, 파일 읽기, 압축, 저장 같은 여러 단계를 거쳐야 할 수 있다. Facade가 `convertVideo()` 같은 단순한 메서드를 제공하면 클라이언트는 내부 복잡도를 몰라도 된다. 장점은 사용성이 좋아지고 클라이언트와 서브시스템 간 결합이 줄어든다는 점이다. 다만 Facade가 너무 많은 책임을 가지면 또 다른 거대한 클래스가 될 수 있어 범위를 관리해야 한다.

참고 자료: [Refactoring.Guru - Facade](https://refactoring.guru/design-patterns/facade)

---

## [COMMON-189] Template Method Pattern이 무엇인지 설명해 주세요.

답변:
Template Method Pattern은 알고리즘의 전체 흐름은 상위 클래스에서 정의하고, 일부 단계의 구체 구현은 하위 클래스가 오버라이드하도록 하는 행동 패턴이다. 즉, 변하지 않는 처리 순서는 고정하고 변하는 부분만 하위 클래스에 맡기는 방식이다.

예를 들어 데이터 처리 과정이 "파일 열기 - 데이터 파싱 - 검증 - 저장 - 정리" 순서로 항상 같지만 파싱 방식만 CSV, JSON, XML마다 다르다면 상위 클래스에 전체 흐름을 두고 파싱 단계만 하위 클래스에서 구현하게 할 수 있다. 장점은 중복되는 흐름을 제거하고 알고리즘 순서를 일관되게 유지할 수 있다는 점이다. 단점은 상속 기반이므로 하위 클래스가 상위 클래스 구조에 강하게 묶일 수 있다는 점이다.

참고 자료: [Refactoring.Guru - Template Method](https://refactoring.guru/design-patterns/template-method)

---

## [COMMON-190] 이벤트 기반 아키텍처가 무엇이고, 어떤 장단점이 있는지 설명해 주세요.

답변:
이벤트 기반 아키텍처는 서비스나 컴포넌트가 직접 서로를 호출하기보다, 상태 변화나 중요한 사실을 이벤트로 발행하고 관심 있는 소비자가 그 이벤트를 구독해 처리하는 구조이다. 예를 들어 주문이 생성되면 주문 서비스가 `OrderCreated` 이벤트를 발행하고, 결제, 알림, 재고 서비스가 각자 필요한 처리를 수행할 수 있다.

장점은 생산자와 소비자의 결합도가 낮아지고, 비동기 처리와 확장에 유리하며, 하나의 이벤트를 여러 소비자가 독립적으로 처리할 수 있다는 점이다. 단점은 최종 일관성을 고려해야 하고, 이벤트 순서, 중복 처리, 실패 재시도, 추적과 디버깅이 복잡해질 수 있다는 점이다. 따라서 멱등성, 관측 가능성, 재처리 전략을 함께 설계해야 한다.

참고 자료: [AWS - Event-Driven Architecture](https://aws.amazon.com/event-driven-architecture/)

---

## [COMMON-191] 동기 처리와 비동기 처리 중 어떤 방식을 선택할지 판단하는 기준을 설명해 주세요.

답변:
동기 처리는 호출자가 결과를 받을 때까지 기다리는 방식이고, 비동기 처리는 요청을 보낸 뒤 결과를 나중에 받거나 별도 흐름에서 처리하는 방식이다. 사용자가 즉시 결과를 알아야 하거나 강한 일관성이 필요한 짧은 작업이라면 동기 처리가 적합하다. 예를 들어 로그인 검증이나 재고 확인처럼 다음 단계 진행에 결과가 꼭 필요한 경우이다.

반대로 시간이 오래 걸리는 작업, 대량 처리, 알림 발송, 영상 변환, 외부 시스템 연동처럼 즉시 응답이 필요하지 않거나 장애 전파를 줄이고 싶은 경우에는 비동기 처리가 적합하다. 판단 기준은 사용자 경험, 응답 시간, 일관성 요구사항, 실패 처리 방식, 구현 복잡도이다. 비동기는 확장성과 장애 격리에 유리하지만 상태 추적과 재시도 설계가 더 중요해진다.

참고 자료: [Azure Architecture Center - Asynchronous Request-Reply pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/asynchronous-request-reply)

---

## [COMMON-192] Monolithic Architecture와 Microservices Architecture의 차이를 설명해 주세요.

답변:
Monolithic Architecture는 애플리케이션의 여러 기능이 하나의 코드베이스와 배포 단위 안에 함께 들어 있는 구조이다. 개발과 배포가 단순하고 트랜잭션 관리가 쉬우며 초기 프로젝트에 적합한 경우가 많다. 하지만 시스템이 커지면 코드 변경 영향 범위가 넓어지고, 일부 기능만 독립적으로 확장하거나 배포하기 어렵다.

Microservices Architecture는 시스템을 작은 서비스들로 나누고 각 서비스가 독립적으로 개발, 배포, 확장될 수 있도록 구성하는 구조이다. 서비스별로 도메인 책임을 나누고 팀 자율성을 높일 수 있지만, 네트워크 통신, 데이터 일관성, 장애 처리, 모니터링, 배포 자동화 등 운영 복잡도가 커진다. 따라서 규모, 팀 구조, 배포 빈도, 도메인 복잡도를 보고 선택해야 한다.

참고 자료: [AWS - Monolithic vs. Microservices Architecture](https://aws.amazon.com/compare/the-difference-between-monolithic-and-microservices-architecture/)

---

## [COMMON-193] MSA를 도입할 때 얻는 장점과 발생할 수 있는 문제를 설명해 주세요.

답변:
MSA를 도입하면 서비스별 독립 배포와 독립 확장이 가능하고, 팀이 도메인 단위로 책임을 나눠 빠르게 개발할 수 있다. 또한 한 서비스의 장애가 전체 시스템으로 번지는 것을 줄일 수 있고, 서비스별로 적합한 기술을 선택할 여지도 생긴다.

하지만 장점만 있는 것은 아니다. 서비스 간 통신이 네트워크를 거치면서 지연과 장애 가능성이 생기고, 분산 트랜잭션과 데이터 일관성 관리가 어려워진다. 또한 로그, 모니터링, 추적, 배포 파이프라인, API 버전 관리 등 운영 체계가 충분히 준비되어야 한다. 그래서 작은 팀이나 초기 제품에서는 잘 나눈 모듈형 모놀리스가 더 현실적인 선택일 수도 있다.

참고 자료: [AWS - What are Microservices?](https://aws.amazon.com/microservices/)

---

## [COMMON-194] 장애 격리와 장애 전파 방지 관점에서 시스템을 어떻게 설계할 수 있는지 설명해 주세요.

답변:
장애 격리는 한 컴포넌트의 실패가 정해진 경계 안에 머물도록 설계하는 것이다. 이를 위해 서비스나 인프라를 기능별, 도메인별, 가용 영역별로 나누고, 한쪽 장애가 전체 시스템 자원을 고갈시키지 않도록 bulkhead, rate limit, timeout, circuit breaker 같은 패턴을 사용할 수 있다.

장애 전파를 막으려면 외부 의존성 호출에 무한 재시도를 하지 않고, 재시도 횟수와 시간 제한을 두며, 실패 시 fallback이나 degraded mode를 제공해야 한다. 큐를 활용해 피크 트래픽을 완충하고, 멱등성을 보장해 재처리를 안전하게 만드는 것도 중요하다. 또한 모니터링과 알림을 통해 장애 경계를 빠르게 파악할 수 있어야 한다.

참고 자료: [AWS Well-Architected Reliability Pillar - Use fault isolation to protect your workload](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/use-fault-isolation-to-protect-your-workload.html)

---

## [COMMON-195] 로깅과 모니터링이 중요한 이유를 설명해 주세요.

답변:
로깅과 모니터링은 운영 중인 시스템에서 무슨 일이 일어나고 있는지 파악하기 위한 기본 수단이다. 로그는 특정 요청이나 오류의 맥락을 남겨 원인 분석에 도움을 주고, 모니터링은 지표와 알림을 통해 장애나 성능 저하를 빠르게 감지하게 해 준다.

특히 운영 환경에서는 사용자가 문제를 제보하기 전에 시스템이 먼저 이상 징후를 알려줘야 한다. 응답 지연, 트래픽, 에러율, 자원 포화도 같은 지표를 관찰하면 장애를 조기에 발견하고 영향 범위를 줄일 수 있다. 좋은 로깅과 모니터링은 단순히 기록을 남기는 것이 아니라, 빠른 복구와 재발 방지를 가능하게 하는 장치이다.

참고 자료: [Google SRE Book - Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)

---

## [COMMON-196] API 설계 시 고려해야 할 요소를 설명해 주세요.

답변:
API를 설계할 때는 먼저 리소스와 행위를 명확히 모델링해야 한다. URL, HTTP 메서드, 상태 코드, 요청과 응답 형식, 에러 형식이 일관되어야 하고, 클라이언트가 예측 가능하게 사용할 수 있어야 한다. 또한 인증, 권한, 입력 검증, rate limit, pagination, filtering, sorting, idempotency 같은 요소도 함께 고려해야 한다.

좋은 API는 내부 구현이 아니라 사용자 관점에서 이해하기 쉬워야 한다. 버전 관리와 하위 호환성을 고려해 기존 클라이언트가 갑자기 깨지지 않도록 해야 하고, 문서와 예제가 충분해야 한다. 정리하면 API 설계의 기준은 일관성, 안정성, 보안, 확장성, 사용성이다.

참고 자료: [Google Cloud - API Design Guide](https://docs.cloud.google.com/apis/design)

---

## [COMMON-197] 좋은 에러 처리 방식이란 무엇인지 설명해 주세요.

답변:
좋은 에러 처리는 오류를 숨기지 않고, 호출자가 이해하고 대응할 수 있는 형태로 전달하는 것이다. 내부에서는 원인 분석에 필요한 로그와 컨텍스트를 남기고, 외부 API 응답에서는 표준화된 상태 코드, 에러 코드, 메시지, 추적 ID 등을 제공하는 것이 좋다. 단, 민감한 내부 정보나 스택 트레이스를 그대로 노출하면 안 된다.

또한 복구 가능한 오류와 복구 불가능한 오류를 구분해야 한다. 입력값 오류처럼 사용자가 수정할 수 있는 문제는 명확한 메시지를 주고, 일시적 장애는 재시도 가능성을 고려하며, 예상하지 못한 예외는 공통 처리 계층에서 로깅하고 일관된 응답으로 변환하는 것이 좋다. 중요한 것은 에러를 단순히 `catch`해서 무시하지 않고, 시스템 안정성과 사용자 경험을 모두 고려하는 것이다.

참고 자료: [Google AIP-193 - Errors](https://google.aip.dev/193)

---

## [COMMON-198] 코드 리뷰에서 중점적으로 봐야 할 요소를 설명해 주세요.

답변:
코드 리뷰에서는 먼저 요구사항을 올바르게 해결했는지와 버그 가능성을 봐야 한다. 그 다음 설계가 적절한지, 책임이 잘 분리되었는지, 불필요하게 복잡하지 않은지, 기존 코드 스타일과 일관되는지 확인한다. 테스트가 충분한지, 에러 처리와 경계 조건이 빠지지 않았는지도 중요하다.

또한 보안, 성능, 동시성, 데이터 정합성처럼 문제 발생 시 영향이 큰 부분은 더 꼼꼼히 봐야 한다. 좋은 리뷰는 단순히 취향을 지적하는 것이 아니라 코드베이스의 품질을 장기적으로 유지하기 위한 피드백을 주는 과정이다. 리뷰 단위가 너무 크면 품질 있게 보기 어려우므로, 작은 변경으로 나누는 것도 중요하다.

참고 자료: [Google Engineering Practices - Code Review](https://google.github.io/eng-practices/review/)

---

## [COMMON-199] 유지보수하기 좋은 코드의 특징을 설명해 주세요.

답변:
유지보수하기 좋은 코드는 읽는 사람이 의도를 빠르게 파악할 수 있는 코드이다. 명확한 이름, 작은 함수, 높은 응집도, 낮은 결합도, 일관된 스타일, 중복 최소화, 예측 가능한 에러 처리, 충분한 테스트를 갖추고 있어야 한다. 또한 "무엇을 하는지"는 코드로 드러나고, "왜 그렇게 했는지"는 필요한 경우 짧은 주석이나 문서로 남겨져야 한다.

중요한 기준은 변경이 쉬운가이다. 요구사항이 바뀌었을 때 수정해야 하는 위치가 명확하고, 변경 영향이 제한되며, 테스트로 안전하게 확인할 수 있다면 유지보수성이 높은 코드라고 볼 수 있다. 반대로 똑똑해 보이지만 의도를 읽기 어렵거나, 여러 책임이 한곳에 섞여 있거나, 숨은 의존성이 많은 코드는 시간이 지날수록 비용이 커진다.

참고 자료: [Microsoft Learn - Architectural principles](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles)

---

## [COMMON-200] 프로젝트 경험을 기술면접에서 설명할 때 문제 상황, 선택 이유, 결과, 아쉬운 점을 어떻게 구조화하면 좋을지 설명해 주세요.

답변:
프로젝트 경험은 STAR 구조를 기술 면접에 맞게 변형해서 말하면 좋다. 먼저 Situation으로 프로젝트 배경과 문제 상황을 설명하고, Task로 본인이 맡은 역할과 목표를 말한다. 그 다음 Action에서 선택한 기술, 대안 비교, trade-off, 구현 과정, 문제 해결 방식을 구체적으로 설명한다.

마지막 Result에서는 성능 개선, 오류 감소, 개발 기간 단축, 사용자 피드백처럼 결과를 가능한 한 수치나 사례로 말한다. 이후 아쉬운 점과 다음에 개선할 점까지 덧붙이면 회고 능력을 보여줄 수 있다. 예를 들어 "당시에는 일정 때문에 빠른 구현을 선택했지만, 이후 테스트 자동화가 부족해 회귀 버그가 생겼고 다음 프로젝트에서는 핵심 도메인부터 단위 테스트를 작성했다"처럼 말하면 좋다.

참고 자료: [MIT CAPD - The STAR Method for Behavioral Interviews](https://capd.mit.edu/resources/the-star-method-for-behavioral-interviews/)

