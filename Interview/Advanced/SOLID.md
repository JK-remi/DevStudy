# SOLID 원칙

---

# SOLID란?

SOLID는 객체지향 프로그래밍(OOP)에서 **유지보수성과 확장성을 높이기 위한 5가지 설계 원칙**입니다.

Design Pattern이 "구현 방법"이라면,

SOLID는 **좋은 설계를 하기 위한 기준**입니다.

즉,

```text
SOLID

↓

좋은 설계 원칙

↓

Design Pattern

↓

그 원칙을 구현하는 대표적인 방법
```

이라고 이해하면 됩니다.

---

# S - Single Responsibility Principle

## 단일 책임 원칙

> **하나의 클래스는 하나의 책임만 가져야 한다.**

---

## 의미

클래스가 여러 가지 역할을 수행하면

변경 이유도 여러 개가 됩니다.

그러면

```text
UI 수정

↓

Game Logic 버그
```

같은 문제가 발생할 수 있습니다.

---

## 나쁜 예

```csharp
class Player
{
    Move();

    Attack();

    Save();

    PlaySound();

    UIUpdate();
}
```

Player가

- 이동
- 전투
- 저장
- 사운드
- UI

모두 담당하고 있습니다.

---

## 좋은 예

```text
Player

↓

PlayerMove

↓

PlayerCombat

↓

PlayerSave

↓

PlayerUI
```

역할을 분리합니다.

---

## Unity 예시

```text
PlayerController

↓

입력 처리

PlayerStat

↓

체력

PlayerAnimator

↓

애니메이션
```

---

## Oracle War

Oracle War에서도

```text
Sequence System

Dialog

Camera

Effect
```

를 각각 따로 분리했습니다.

---

# O - Open Closed Principle

## 개방 폐쇄 원칙

> **확장에는 열려 있고 수정에는 닫혀 있어야 한다.**

---

## 의미

새로운 기능이 추가될 때

기존 코드를 수정하지 않고

새로운 코드를 추가하는 형태가 되어야 합니다.

---

## 나쁜 예

```csharp
if(type == Fire)
...

else if(type == Ice)
...

else if(type == Wind)
...
```

속성이 추가될 때마다

기존 코드를 수정합니다.

---

## 좋은 예

```text
Skill

↓

FireSkill

↓

IceSkill

↓

WindSkill
```

새로운 클래스를 추가합니다.

---

## Unity 예시

```text
Enemy

↓

Slime

Goblin

Boss
```

---

## Oracle War

FSM의

```text
Idle

Move

Attack
```

각 State를

새 클래스로 추가하는 구조도

OCP를 만족합니다.

---

# L - Liskov Substitution Principle

## 리스코프 치환 원칙

> **부모 객체를 자식 객체로 교체해도 프로그램이 정상 동작해야 한다.**

---

## 의미

업캐스팅 이후에도

동일한 동작을 해야 합니다.

---

## 예시

```csharp
Animal animal = new Dog();

animal.Move();
```

Dog가 들어와도

Animal처럼 정상 동작해야 합니다.

---

## Unity 예시

```text
EnemyBase

↓

Slime

↓

Boss
```

Spawner는

EnemyBase만 알아도 됩니다.

---

# I - Interface Segregation Principle

## 인터페이스 분리 원칙

> **사용하지 않는 기능을 강제로 구현하게 하면 안 된다.**

---

## 나쁜 예

```csharp
interface IEnemy
{
    Move();

    Attack();

    Fly();
}
```

Slime도 Fly를 구현해야 합니다.

---

## 좋은 예

```text
IMove

IAttack

IFly
```

필요한 것만 구현합니다.

---

## Unity 예시

```text
IDamageable

IInteractable

IPoolable
```

---

# D - Dependency Inversion Principle

## 의존성 역전 원칙

> **구체 클래스가 아니라 추상화에 의존해야 한다.**

---

## 의미

직접

```text
new

Singleton

구체 클래스
```

를 참조하지 않고

Interface를 사용합니다.

---

## 나쁜 예

```csharp
Player

↓

SoundManager.Instance
```

직접 참조

---

## 좋은 예

```text
Player

↓

ISoundManager
```

Interface 참조

---

## Unity 예시

DI

Zenject

Event

Observer

Service Locator

등이

대표적인 해결 방법입니다.

---

## Oracle War

현재는

```text
Singleton Manager
```

를 많이 사용했지만

추후에는

```text
Observer

DI

Event
```

구조로 변경하면

DIP를 더 잘 만족할 수 있습니다.

---

# SOLID와 Design Pattern 관계

SOLID는

```text
좋은 설계 원칙
```

이고

Design Pattern은

```text
그 원칙을 구현하는 대표적인 방법
```

입니다.

예를 들어

| SOLID | 대표 Pattern |
|--------|--------------|
| SRP | State |
| OCP | Strategy / State |
| LSP | 상속 |
| ISP | Interface |
| DIP | Observer / DI / Service Locator |

---

# Oracle War에서 적용 사례

| 원칙 | 사례 |
|------|------|
| SRP | Sequence / Dialog / Camera 분리 |
| OCP | FSM State 추가 |
| LSP | EnemyBase 상속 |
| ISP | Interface 분리 가능 |
| DIP | Singleton 사용 중 → Event / Observer로 개선 가능 |

---

# 면접 답변

## Q. SOLID 원칙이란 무엇인가요?

> **SOLID는 객체지향 설계에서 유지보수성과 확장성을 높이기 위한 5가지 설계 원칙입니다. 각각 단일 책임(SRP), 개방 폐쇄(OCP), 리스코프 치환(LSP), 인터페이스 분리(ISP), 의존성 역전(DIP) 원칙으로 구성되며, Design Pattern은 이러한 원칙을 실제 코드에서 구현하는 대표적인 방법입니다.**

---

## Q. Design Pattern과 SOLID의 관계를 설명해 주세요.

> **SOLID는 좋은 객체지향 설계를 위한 원칙이고, Design Pattern은 이러한 원칙을 실제 코드에서 구현하기 위해 자주 사용하는 설계 방법입니다. 즉, SOLID가 '왜 이렇게 설계해야 하는가'를 설명한다면, Design Pattern은 '어떻게 구현할 것인가'를 제시하는 역할을 합니다.**

---

# 자주 나오는 꼬리 질문

### Q. Oracle War에서 SOLID를 적용한 사례를 설명해 주세요.

---

### Q. 가장 중요하다고 생각하는 SOLID 원칙은 무엇인가요?

---

### Q. DIP를 실제 프로젝트에서 어떻게 적용할 수 있나요?

---

### Q. Singleton은 SOLID를 위반하나요?

> 상황에 따라 DIP를 위반할 수 있으므로 남용은 지양해야 합니다.
