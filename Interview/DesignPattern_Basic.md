# Design Pattern 개요

## 🎯 Goal
- Design Pattern이 필요한 이유를 이해한다.
- Design Pattern의 분류(생성 / 구조 / 행동)를 이해한다.
- SOLID 원칙과 Design Pattern의 관계를 이해한다.
- Unity에서 자주 사용하는 Design Pattern을 설명할 수 있다.
- Oracle War 프로젝트에 적용한(또는 적용 가능한) Design Pattern을 설명할 수 있다.

---

## Q1. Design Pattern이란 무엇인가요?

- 반복되는 설계 문제
- 검증된 해결 방법
- 설계 패턴

### 답변
소프트웨어를 설계하면서 반복적으로 발생하는 문제에 대해 검증된 해결 방법을 정리해 놓은 설계 패턴입니다.

---

## Q2. 왜 Design Pattern을 사용하는가?

- 유지보수
- 재사용성
- 결합도 감소
- 확장성

### 답변
반복되는 설계 문제를 검증된 방식으로 해결하여 유지보수성과 확장성을 높이고 객체 간 결합도를 낮추기 위해 사용합니다.

---

## Q3. Design Pattern은 언제 적용해야 하나요?

- 반복되는 구조
- 객체 관계
- 설계 필요

### 답변
반복되는 구조가 발생하거나 객체 간 관계를 효율적으로 설계해야 할 때 적용합니다.

---

## Q4. Design Pattern을 무조건 사용하는 것이 좋은가요?

- 문제 해결 도구
- 적절한 상황
- 불필요한 복잡성

### 답변
아닙니다. Design Pattern은 문제를 해결하기 위한 도구이므로 현재 구조에 적합할 때만 사용하는 것이 좋습니다. 불필요하게 적용하면 오히려 구조가 복잡해질 수 있습니다.

---

## Q5. Design Pattern의 3가지 분류를 설명해 주세요.

- 생성(Creational)
- 구조(Structural)
- 행동(Behavioral)

### 답변
생성 패턴은 객체를 어떻게 생성하고 관리할지, 구조 패턴은 객체들을 어떻게 연결하고 구성할지, 행동 패턴은 객체들이 어떻게 협력하고 동작할지를 정의하는 패턴입니다.

---

## Q6. 생성(Creational) 패턴이란 무엇인가요?

- 객체 생성
- 객체 관리

### 답변
객체를 어떤 방식으로 생성하고 관리할 것인지에 대한 패턴입니다.

---

## Q7. 구조(Structural) 패턴이란 무엇인가요?

- 객체 연결
- 객체 구성

### 답변
객체와 객체를 어떤 구조로 연결할 것인지에 대한 패턴입니다.

---

## Q8. 행동(Behavioral) 패턴이란 무엇인가요?

- 책임 분배
- 통신
- 동작 방식

### 답변
객체 간의 책임 분배와 통신, 동작 방식을 정의하는 패턴입니다.

---

## Q9. SOLID 원칙과 Design Pattern의 관계를 설명해 주세요.

- SOLID = 설계 원칙
  - [SOLID](./Adbanced/SOLID.md)
- Pattern = 구현 방법

### 답변
SOLID는 객체지향 설계 원칙이고, Design Pattern은 이러한 원칙을 실제 코드에서 구현하는 대표적인 방법입니다.

---

## Q10. Oracle War 프로젝트에서 적용했거나 적용하면 좋을 것 같은 Design Pattern을 설명해 주세요.

- State(FSM)
- Observer(Sequence)
- Singleton
- Object Pool

### 답변
Oracle War에서는 FSM(State), Object Pool, Singleton을 적용했고, Sequence System에는 Observer Pattern을 적용하거나 적용하기 적합한 구조였습니다.

---

# Related Concepts
- [SOLID](./Adbanced/SOLID.md)
