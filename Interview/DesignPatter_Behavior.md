# Behavioral Pattern (행동)

## 🎯 Goal
- Behavioral Pattern의 목적을 이해한다.
- 객체 간의 상호작용과 책임 분리 방식을 이해한다.
- Observer, Command, State, Strategy Pattern의 특징과 차이를 설명할 수 있다.
- 각 Pattern의 장단점과 적용 시점을 설명할 수 있다.
- Oracle War 프로젝트에 적용한(또는 적용 가능한) Behavioral Pattern을 설명할 수 있다.

---

# Observer Pattern

## Q1. Observer Pattern이란 무엇인가요?

- Event
- Publisher / Subscriber
- 느슨한 결합

### 추천 답변
Publisher와 Subscriber 사이에서 Event를 통해 객체 간의 직접적인 참조 없이 이벤트를 전달하는 행동 패턴입니다. Publisher는 Event를 발생시키고, Subscriber는 Event를 구독하여 필요한 동작을 수행합니다.

### 꼬리질문
- 결합도가 낮아진다는 것은 무엇을 의미하나요?

---

## Q2. Observer Pattern을 사용하는 이유는 무엇인가요?

- 결합도 감소
- 의존도 감소
- 이벤트 기반

### 추천 답변
객체 간의 결합도와 의존도를 낮추기 위해 사용합니다. Publisher는 Subscriber를 알 필요 없이 Event만 발생시키고, Subscriber는 필요한 Event만 구독하여 동작할 수 있습니다.

---

## Q3. Observer Pattern은 언제 사용하는 것이 적합한가요?

- 하나의 이벤트 : 여러 객체 반응
- UI / OnDead

### 추천 답변
하나의 이벤트 발생 시 여러 객체가 동시에 반응해야 하는 경우 사용합니다.

예)
- 캐릭터 사망
- UI 갱신
- Effect 생성
- Sound 재생

---

## Q4. Publisher와 Subscriber를 설명해주세요.

### 추천 답변
Publisher는 Event를 관리하고 발생시키는 객체이며, Subscriber는 해당 Event를 구독하여 필요한 동작을 수행하는 객체입니다.

---

## Q5. Unity에서 Observer Pattern의 대표적인 예는?

- OnDead: UI / Effect / Sound

### 추천 답변
캐릭터가 사망했을 때 UI, Effect, Sound 등이 동시에 반응해야 하는 경우 사용합니다.

---

## Q6. Observer Pattern의 장점은?

- 결합도 감소 / 유지보수 / 확장성

### 추천 답변
객체 간 결합도가 낮아지고 유지보수가 쉬워집니다. 새로운 Subscriber를 추가해도 Publisher를 수정할 필요가 없습니다.

---

## Q7. Observer Pattern의 단점은?

- 디버깅 / Event 해제 (Memory Leak)

### 추천 답변
Publisher와 Subscriber의 연결 관계를 추적하기 어려워 디버깅이 어려울 수 있습니다. 또한 Event를 해제하지 않으면 메모리 누수나 Null Reference 문제가 발생할 수 있습니다.

---

## Q8. Oracle War 프로젝트에서는?

### 추천 답변
프로젝트 구조상 직접 호출 방식을 유지했습니다. 만약 Observer Pattern을 적용한다면 캐릭터 사망, 퀘스트 완료, UI 갱신 등 이벤트 기반 구조로 변경할 수 있습니다.

---

# Command Pattern

## Q1. Command Pattern이란 무엇인가요?

- 명령 객체
- 요청 캡슐화

### 추천 답변
요청(명령)을 객체로 캡슐화하여 실행 요청과 실제 실행을 분리하는 행동 패턴입니다.

---

## Q2. Command Pattern을 사용하는 이유는 무엇인가요?

- Queue / Stack
- Undo / Redo
- 명령 관리

### 추천 답변
명령을 객체로 관리하여 실행 순서를 제어하거나 Queue, Stack 등에 저장하여 재사용 및 Undo/Redo 기능을 구현하기 쉽습니다.

### 꼬리질문
- Command를 객체로 만드는 이유는 무엇인가요?

---

## Q3. Command Pattern은 언제 사용하는 것이 적합한가요?

- 순차 실행
- 여러 객체

### 추천 답변
하나의 동작으로 여러 객체의 작업을 순차적으로 실행해야 하는 경우 사용합니다.

---

## Q4. Command, Invoker, Receiver를 설명해주세요.

### 추천 답변
- Command : 실행할 명령을 정의하는 인터페이스 또는 추상 클래스
- ConcreteCommand : 실제 명령을 구현한 객체
- Invoker : Command를 실행하는 객체
- Receiver : 실제 작업을 수행하는 객체

---

## Q5. Unity에서 사용할 수 있는 예는?

- Quest / UI / Reward / Network

### 추천 답변
퀘스트 완료 시 UI 갱신, 보상 지급, 사운드 재생, 서버 전송 등을 하나의 명령으로 순차 실행할 수 있습니다.

---

## Q6. Command Pattern의 장점은?

- 명령 관리
- Queue / Stack

### 추천 답변
명령 실행을 한 곳에서 관리할 수 있으며, Queue, Stack 등을 이용한 관리가 쉽습니다.

---

## Q7. Command Pattern의 단점은?

- 클래스 증가

### 추천 답변
명령마다 클래스를 만들어야 하므로 클래스 수가 증가합니다.

---

## Q8. Oracle War 프로젝트에서는?

### 추천 답변
직접 적용하지는 않았지만 퀘스트 완료, 스킬 사용 등의 작업을 하나의 Command 객체로 분리하면 관리성이 향상될 것으로 생각합니다.

---

# State Pattern

## Q1. State Pattern이란 무엇인가요?

- 상태 객체
- 상태 전환

### 추천 답변
객체의 상태(State)를 각각의 클래스로 분리하여 상태가 변경될 때 객체의 동작도 함께 변경되도록 하는 행동 패턴입니다.

---

## Q2. State Pattern을 사용하는 이유와 적용 시점은?

- 조건문 감소
- FSM
- 상태별 동작

### 추천 답변
상태에 따른 동작을 하나의 클래스에서 모두 처리하면 조건문이 많아지고 유지보수가 어려워집니다. 객체의 상태에 따라 동작이 달라지는 경우 사용합니다.

예)
- FSM
- 플레이어 상태
- UI 상태

---

## Q3. Context, State, ConcreteState를 설명해주세요.

### 추천 답변
- Context : 현재 State를 보관하고 상태 전환을 관리하는 객체
- State : 공통 인터페이스 또는 추상 클래스
- ConcreteState : 실제 상태를 구현한 클래스

### 꼬리질문
- State 객체는 누가 생성하고 관리하나요?

---

## Q4. State Pattern의 장점은?

- 유지보수 / 상태 분리 / 확장성

### 추천 답변
상태별 로직을 분리하여 유지보수가 쉬우며 새로운 상태를 추가하기 쉽습니다.

---

## Q5. State Pattern의 단점은?

- 클래스 증가 / 상태 관리

### 추천 답변
상태마다 클래스를 생성해야 하므로 클래스 수가 증가하고 상태가 많아질수록 관리가 어려워집니다.

---

## Q6. Oracle War 프로젝트에서는?

### 추천 답변
AIBase 내부에서 FSM을 구현했지만 State 클래스로 분리하지는 않았습니다. 일정상 기존 구조를 유지했으며 이후에는 IdleState, ChaseState, AttackState처럼 분리하는 방향을 고려했습니다.

---

## Q7. State Pattern과 FSM의 관계는?

### 추천 답변
FSM은 상태 머신 알고리즘이며 State Pattern은 이를 객체지향적으로 구현하는 대표적인 설계 패턴입니다.

---

# Strategy Pattern

## Q1. Strategy Pattern이란 무엇인가요?

- 알고리즘 교체
- Strategy 객체

### 추천 답변
동일한 기능을 수행하지만 내부 알고리즘이 다른 경우 알고리즘을 객체로 분리하여 필요에 따라 교체하는 행동 패턴입니다.

---

## Q2. Strategy Pattern을 사용하는 이유와 적용 시점은?

- 알고리즘 교체
- Melee / Range / Magic

### 추천 답변
동일한 기능을 여러 알고리즘으로 수행해야 하는 경우 사용합니다.

예)
- Melee Attack
- Range Attack
- Magic Attack

### 꼬리질문
- Strategy를 객체로 분리하는 이유는 무엇인가요?

---

## Q3. Context, Strategy, ConcreteStrategy를 설명해주세요.

### 추천 답변
- Context : 현재 사용할 Strategy를 보관하고 실행하는 객체
- Strategy : 공통 인터페이스 또는 추상 클래스
- ConcreteStrategy : 실제 알고리즘을 구현한 객체

### 꼬리질문
- Context는 Strategy를 어떻게 교체하나요?

---

## Q4. Strategy Pattern의 장점은?

- 알고리즘 교체 / 확장성 / 유지보수

### 추천 답변
Context 수정 없이 Strategy만 교체하여 알고리즘을 변경할 수 있으며 확장성과 유지보수성이 높습니다.

---

## Q5. Strategy Pattern의 단점은?

- 클래스 증가

### 추천 답변
Strategy마다 클래스를 만들어야 하므로 클래스 수가 증가하고 관리가 어려워질 수 있습니다.

---

## Q6. Oracle War 프로젝트에서는?

### 추천 답변
Strategy 객체를 따로 만들지는 않았지만 AttackType(enum)에 따라 공격 알고리즘을 분기했습니다. Strategy Pattern을 적용한다면 MeleeStrategy, RangeStrategy, MagicStrategy처럼 각각을 클래스로 분리하여 Context에서 교체하는 구조로 개선할 수 있습니다.

---

# Related Concepts
- [GoF Observer](./Advanced/Interface_Observer_GoF.md)
