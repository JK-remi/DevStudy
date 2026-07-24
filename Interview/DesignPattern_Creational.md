# Creational Pattern (생성)

## 🎯 Goal
- Creational Pattern의 목적을 이해한다.
- 객체 생성 과정과 객체 사용을 분리하는 이유를 이해한다.
- Singleton, Object Pool, Factory Pattern의 특징과 차이점을 설명할 수 있다.
- 각 패턴의 장단점과 적용 시점을 설명할 수 있다.
- Oracle War 프로젝트에 적용한(또는 적용 가능한) Creational Pattern을 설명할 수 있다.

---

# Singleton Pattern

## Q1. Singleton Pattern이란 무엇인가요?

- 하나의 객체
- 전역 접근
- Creational Pattern

### 답변
프로그램에서 하나만 존재해야 하는 객체를 전역에서 접근 가능하도록 관리하는 생성(Creational) 패턴입니다.

---

## Q2. Singleton Pattern을 사용하는 이유는 무엇인가요?

- 중앙 관리
- 하나의 객체
- Manager

### 답변
프로그램 전체에서 하나의 객체만 유지해야 하는 경우 중앙에서 상태를 관리하기 위해 사용합니다.

---

## Q3. Unity에서 Singleton을 사용하는 대표적인 예를 설명해 주세요.

### 답변
Unity에서는 프로젝트 전역에서 하나만 존재해야 하는 객체를 관리할 때 Singleton을 자주 사용합니다. 대표적으로 Game, UI, Sound, Resource, Save Manager 등이 있으며 여러 시스템에서 공통으로 접근해야 하는 기능을 중앙에서 관리할 때 사용합니다.

---

## Q4. Singleton을 구현하는 방법을 설명해 주세요.

- private 생성자
- static Instance
- Instance 접근

### 답변
Singleton은 생성자를 private으로 만들어 외부 생성을 막고, static Instance를 제공합니다. Instance가 처음 호출될 때 객체를 생성하고 이후에는 동일한 객체를 반환하여 프로그램 전체에서 하나의 객체만 사용하도록 구현합니다.

### 꼬리질문
- 생성자를 private으로 만드는 이유는 무엇인가요?
- Lazy Initialization과 Eager Initialization의 차이를 설명해 주세요.
  - [Lazy/Eager Initialization](./Advanced/Lazy_Eager_Init.md)
- Unity에서는 MonoBehaviour Singleton을 일반 C# Singleton과 어떻게 다르게 구현하나요?

---

## Q5. Singleton의 장점은 무엇인가요?

- 전역 접근
- 상태 일관성
- 구현 단순

### 답변
Singleton은 프로그램 어디에서나 동일한 객체에 접근할 수 있어 관리가 쉽고, 하나의 객체에서 상태를 일관되게 관리할 수 있습니다. 또한 구현이 비교적 단순하여 전역 시스템을 구성하기에 적합합니다.

---

## Q6. Singleton의 단점은 무엇인가요?

- 결합도 증가
- 테스트 어려움
- 유지보수 어려움

### 답변
전역 객체가 되어 결합도가 높아지고 테스트가 어려우며, 의존성이 많아질수록 유지보수가 어려워질 수 있습니다.

### 꼬리질문
- Singleton의 결합도를 낮추는 방법은 무엇인가요?
  - [DI](./Advanced/DI.md)
  - [Service Locator](./Advanced/Service_Locator.md)
  - [Event](./Csharp_Delegate_Event.md#part-2-event)
- 테스트가 어려운 이유는 무엇인가요?
  - Mock 객체

---

## Q7. Singleton을 남용하면 어떤 문제가 발생하나요?

- 초기화 순서
- 순환 참조
- 의존성 증가

### 답변
Singleton을 많이 사용하면 객체들이 서로 Singleton에 의존하게 되어 결합도가 높아지고 유지보수가 어려워집니다. 또한 Unity에서는 Awake 초기화 순서에 따라 아직 생성되지 않은 Singleton을 참조하거나 순환 의존 문제가 발생할 수 있습니다.

### 꼬리질문
- Unity에서 Singleton 초기화 순서 문제를 어떻게 해결하시겠습니까?

---

## Q8. Oracle War 프로젝트에서 Singleton을 사용한 사례를 설명해 주세요.

### 답변
Oracle War에서는 Game, UI, Sound, ObjectPool Manager 등을 Singleton으로 구현하여 게임 전역에서 동일한 객체를 사용하도록 관리했습니다.

---

## Q9. Singleton과 Static Class의 차이를 설명해 주세요.

- Singleton = 객체
- Static Class = 클래스
- 객체지향

### 답변
Singleton은 하나의 객체를 생성하여 전역에서 공유하는 패턴이고, Static Class는 객체를 생성하지 않고 클래스 자체를 사용하는 방식입니다. Singleton은 인터페이스 구현, 상속, 다형성 같은 객체지향 기능을 사용할 수 있지만 Static Class는 사용할 수 없습니다.

### 꼬리질문
- Singleton이 Static Class보다 객체지향적인 이유는 무엇인가요?

---

# Object Pool Pattern

## Q1. Object Pool Pattern이란 무엇인가요?

- 객체 재사용
- Creational Pattern

### 답변
객체를 생성과 삭제하지 않고 미리 생성해 둔 객체를 재사용하는 생성(Creational) 패턴입니다.

---

## Q2. Object Pool Pattern을 사용하는 이유는 무엇인가요?

- Instantiate 감소
- Destroy 감소
- GC 감소
- 프레임 안정

### 답변
객체 생성과 삭제 비용을 줄이고 GC 발생을 최소화하여 프레임 드랍을 방지하기 위해 사용합니다.

### 꼬리질문
- Instantiate가 비싼 이유는 무엇인가요?

---

## Q3. Object Pool은 언제 사용하는 것이 적합한가요?

- 반복 생성/삭제

### 답변
짧은 시간 안에 반복적으로 생성되고 삭제되는 객체가 많을 때 사용합니다.

---

## Q4. Unity에서 Object Pool을 사용하는 대표적인 예를 설명해 주세요.

### 답변
Unity에서는 Projectile, Effect, Damage Text, Monster, UI처럼 생성과 삭제가 빈번한 객체를 재사용하기 위해 Object Pool을 사용합니다.

---

## Q5. Object Pool의 동작 과정을 설명해 주세요.

- 생성
- 반환
- 재사용

### 답변
초기에 객체를 생성하여 Pool에 저장합니다. 객체가 필요하면 Pool에서 가져와 사용하고, 사용이 끝나면 삭제하지 않고 Pool에 반환합니다. 다시 사용할 때는 필요한 상태만 초기화하여 재사용합니다.

### 꼬리질문
- 재사용 시 무엇을 초기화해야 하나요?

---

## Q6. Object Pool의 장점은 무엇인가요?

- 생성 비용 감소
- 삭제 비용 감소
- GC 감소
- 성능 안정

### 답변
Object Pool은 객체를 반복적으로 생성하고 삭제하지 않고 재사용하기 때문에 메모리 할당과 해제 비용을 줄일 수 있습니다. 또한 GC 발생을 감소시켜 프레임 드랍을 줄이고 안정적인 성능을 유지할 수 있습니다.

### 꼬리질문
- Object Pool을 사용하면 GC가 완전히 발생하지 않나요?

---

## Q7. Object Pool의 단점은 무엇인가요?

- 재초기화
- 메모리 선점
- Pool 크기

### 답변
객체를 재사용하기 때문에 이전 상태를 완전히 초기화하지 않으면 버그가 발생할 수 있습니다. 또한 Pool 크기를 잘못 설정하면 메모리를 낭비하거나 런타임에 추가 생성이 발생할 수 있습니다.

### 꼬리질문
- Pool 크기는 어떻게 결정하시겠습니까?

---

## Q8. Queue를 사용하는 이유는 무엇인가요?

- 순차 관리

### 답변
반환된 객체를 순차적으로 관리하기 쉽기 때문입니다.

---

## Q9. Oracle War 프로젝트에서 Object Pool을 어떻게 구현했나요?

### 답변
Oracle War에서는 ObjectPoolManager가 객체 종류별 Queue를 관리했습니다. Queue에서 객체를 가져와 활성화하고, 사용이 끝난 객체는 비활성화 후 다시 Queue에 반환했습니다. 재사용 시에는 OnEnable에서 필요한 데이터를 초기화하여 이전 상태가 남지 않도록 처리했습니다.

---

# Factory Pattern

## Q1. Factory Pattern이란 무엇인가요?

- 생성 책임 분리
- Creational Pattern

### 답변
Factory Pattern은 객체를 직접 생성하지 않고 객체 생성 책임을 Factory 클래스에 위임하여 객체 생성 과정을 관리하는 생성 패턴입니다.

---

## Q2. Factory Pattern을 사용하는 이유는 무엇인가요?

- 생성 책임 분리
- 코드 중복 제거
- 결합도 감소

### 답변
객체 생성 코드를 한 곳에서 관리하여 생성 로직의 중복을 줄이고 객체 생성 방식이 변경되더라도 사용하는 코드를 수정하지 않도록 하기 위해 사용합니다.

### 꼬리질문
- 객체를 직접 new 하지 않는 이유는 무엇인가요?

---

## Q3. Factory Pattern은 언제 사용하는 것이 적합한가요?

- 생성 객체 종류 다양
- 생성 과정 복잡

### 답변
생성해야 하는 객체 종류가 많거나 생성 과정이 복잡한 경우 생성 책임을 분리하여 관리하고 싶을 때 사용합니다.

---

## Q4. Unity에서 Factory Pattern을 사용할 수 있는 예를 설명해 주세요.

### 답변
Unity에서는 MonsterSpawner나 ProjectileSpawner처럼 객체를 생성하는 시스템에서 Factory Pattern을 적용할 수 있습니다. 또한 Object Pool의 초기 생성이나 Pool이 부족할 때 새로운 객체를 생성하는 부분도 Factory를 통해 분리하여 관리할 수 있습니다.

### 꼬리질문
- MonsterSpawner와 Factory는 어떤 관계인가요?

---

## Q5. Oracle War 프로젝트에서 Factory Pattern을 적용했거나 적용할 수 있는 부분을 설명해 주세요.

### 답변
Oracle War에서는 Object Pool의 초기 객체 생성과 Pool 부족 시 새로운 객체 생성 부분에 Factory Pattern을 적용할 수 있습니다. 또한 플레이어와 몬스터 생성도 Factory를 통해 생성 책임을 분리하여 관리할 수 있습니다.

---

## Q6. Factory Pattern과 Object Pool Pattern의 차이를 설명해 주세요.

- Factory = 생성
- Object Pool = 재사용

### 답변
Factory Pattern은 객체를 생성하는 역할을 담당하고, Object Pool Pattern은 이미 생성된 객체를 재사용하는 역할을 담당합니다. 실제 프로젝트에서는 Factory가 객체를 생성하고 Object Pool이 생성된 객체를 관리하는 형태로 함께 사용하는 경우가 많습니다.

### 꼬리질문
- Factory와 Object Pool을 같이 사용할 수 있나요?

---

## Q7. Factory Pattern과 Singleton Pattern의 차이를 설명해 주세요.

- Factory = 생성
- Singleton = 하나의 객체

### 답변
Factory Pattern은 객체 생성 책임을 담당하는 패턴이고, Singleton Pattern은 하나의 객체만 존재하도록 관리하는 패턴입니다. Factory는 여러 객체를 생성할 수 있지만 Singleton은 하나의 인스턴스만 유지하는 것이 목적입니다.

### 꼬리질문
- Factory도 Singleton으로 만들 수 있나요?

---

# Related Concepts
- [Lazy/Eager Initialization](./Advanced/Lazy_Eager_Init.md)
- [DI](./Advanced/DI.md)
- [Service Locator](./Advanced/Service_Locator.md)
- [Event](./Csharp_Delegate_Event.md#part-2-event)
