# Composition (조합)

## 핵심 키워드

- Has-A 관계
- Component
- 낮은 결합도
- 높은 유연성
- Unity

---

## Composition이란?

Composition은 **객체를 상속(Inheritance)하지 않고 필요한 객체를 포함(조합)하여 사용하는 방식**입니다.  
즉, **"Is-A"가 아니라 "Has-A" 관계**를 의미합니다.  

예를 들어
```text
Player
 ├── Weapon
 ├── Inventory
 ├── HPComponent
 └── SkillComponent
```
Player는 Weapon이 아닙니다. Player는 **Weapon을 가지고(Has-A)** 있습니다.  

---

## 상속과의 차이

### 상속 (Inheritance)

```text
Character
   │
   ├── Player
   ├── Monster
   └── NPC
```

관계
```text
Player IS A Character
```

즉 Player는 Character입니다.  

---

### Composition

```text
Player

 ├── Weapon
 ├── Inventory
 ├── Skill
 └── HP
```

관계
```text
Player HAS A Weapon
```

Player는 Weapon이 아닙니다. Weapon을 가지고 있습니다.  

---

## Composition을 사용하는 이유

### 1. 결합도 감소

상속은
```text
부모 변경

↓

모든 자식 영향
```
을 받습니다.  

반면 Composition은 Weapon만 교체하면 됩니다. Player는 수정하지 않습니다.

---

### 2. 재사용성 증가

Weapon Component

↓

Monster도 사용 가능

↓

Player도 사용 가능

↓

NPC도 사용 가능

---

### 3. 런타임 교체 가능

예)

```text
Sword

↓

Gun

↓

Bow
```
교체 가능  

Player는 그대로 유지됩니다.

---

## Unity가 Composition을 사용하는 이유

Unity는 거의 모든 시스템이 Composition입니다.  

예)
```text
GameObject

+

Transform

+

Renderer

+

Collider

+

Rigidbody
```
필요한 Component만 붙입니다.  

상속으로
```text
PlayerWithRigidBody

PlayerWithRenderer

PlayerWithCollider
```
를 만들지 않습니다.

---

## Oracle War에서의 예

예)

Monster

↓

FSM

↓

HP

↓

Animation

↓

AI

각 기능을 Component로 분리하면 새로운 Monster를 만들 때 필요한 Component만 붙이면 됩니다.

---

## Composition의 장점

- 결합도 감소
- 유지보수 쉬움
- 기능 재사용 가능
- 런타임 변경 가능
- Unity 스타일

---

## Composition의 단점

- 객체 수 증가
- 구조 파악 어려움
- 너무 잘게 나누면 관리 비용 증가
