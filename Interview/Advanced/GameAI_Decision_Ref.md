# GameAI_Decision_Reference

---

# 핵심 정리

### FSM
- 상태(State) 중심
- 상태 전환으로 행동 결정

### Behavior Tree
- 현재 상황(Condition) 중심
- Tree를 순회하여 행동 선택

### GOAP
- 목표(Goal) 중심
- 목표 달성을 위한 계획 생성

### Utility AI
- 점수(Score) 중심
- 현재 가장 가치 있는 행동 선택

---

# 행동 AI 비교

| 구분 | FSM | Behavior Tree | GOAP | Utility AI |
|------|-----|---------------|-------|------------|
| 의사결정 기준 | 현재 상태(State) | 현재 상황(Condition) | 목표(Goal) | 점수(Score) |
| 핵심 개념 | 상태 전환 | Tree 순회 후 행동 선택 | 목표 달성을 위한 계획 생성 | 가장 높은 점수의 행동 선택 |
| 결과 | 다음 State | Action | Action Sequence (Planning) | Action |
| 장점 | 구현 단순, 직관적 | 확장성 우수, 재사용성 높음 | 상황에 따라 유연한 계획 생성 | 자연스러운 행동 선택 |
| 단점 | 상태 증가 시 관리 어려움 | Tree가 커질수록 관리 및 성능 부담 | Planning 비용 큼 | Score 설계 및 밸런싱 어려움 |
| 적합한 상황 | 상태가 적고 Transition이 단순 | 행동과 조건이 복잡 | 목표는 같지만 행동 순서가 달라지는 경우 | 행동 우선순위가 계속 변하는 경우 |
| Oracle War 적용 | 일반 몬스터 AI | Boss AI, 복잡한 NPC AI | Auto Play (Quest 기반) | Auto Play (행동 우선순위 결정) |

---

# 행동 AI 조합

## Matrix

| Main \ Sub | FSM | Behavior Tree | GOAP | Utility AI |
|-------------|:---:|:-------------:|:-----:|:----------:|
| **FSM** | - | [FSM > BT](#fsm--behavior-tree) | [FSM > GOAP](#fsm--goap) | [FSM > Utility AI](#fsm--utility-ai) |
| **Behavior Tree** | - | - | - | [BT > Utility AI](#behavior-tree--utility-ai) |
| **GOAP** | - | [GOAP > BT](#goap--behavior-tree) | - | - |
| **Utility AI** | - | - | [Utility AI > GOAP](#utility-ai--goap) | - |

---

# FSM > Behavior Tree

### 구조

```text
FSM
├─ Idle
├─ Combat
│   └─ Behavior Tree
└─ Dead
```

### 설명

FSM이 큰 상태(State)를 관리하고,

Combat 같은 특정 State에 진입하면 해당 State 내부에서 Behavior Tree가 현재 상황을 평가하여 행동(Action)을 선택합니다.

### 사용 예시

- 일반 몬스터 AI
- Boss AI
  
[MATRIX](#Matrix)

---

# FSM > GOAP

### 구조

```text
FSM
└─ Combat
    └─ GOAP Planning
```

### 설명

FSM이 현재 State를 결정한 뒤,

Combat State 내부에서 GOAP가 Goal을 기반으로 Planning(Action Sequence)을 생성합니다.

### 사용 예시

- Combat AI
- Survival AI
  
[MATRIX](#Matrix)

---

# FSM > Utility AI

### 구조

```text
FSM
└─ Combat
    └─ Utility AI
```

### 설명

Combat 상태 안에서

Heal, Attack, Run 등의 행동 점수를 계산하여

가장 높은 점수의 행동을 선택합니다.

### 사용 예시

- Combat AI
  
[MATRIX](#Matrix)

---

# Behavior Tree > Utility AI

### 구조

```text
Behavior Tree
    │
    ▼
Combat Node
    │
    ▼
Utility AI
```

### 설명

Behavior Tree가 현재 행동 그룹을 선택하고,

Utility AI가 해당 그룹 내부에서 가장 적합한 행동을 선택합니다.

### 사용 예시

- Boss AI
- Companion AI
  
[MATRIX](#Matrix)

---

# GOAP > Behavior Tree

### 구조

```text
Goal
 │
 ▼
Planning
 │
 ▼
Action
 │
 ▼
Behavior Tree
```

### 설명

GOAP가 Goal을 달성하기 위한 Action Sequence를 생성하고,

각 Action 내부의 세부 행동은 Behavior Tree가 수행합니다.

### 사용 예시

- 오픈월드 NPC
- 생존 AI
  
[MATRIX](#Matrix)

---

# Utility AI > GOAP

### 구조

```text
Utility AI
    │
    ▼
Goal 선택
    │
    ▼
GOAP Planning
```

### 설명

Utility AI가 현재 상황을 평가하여 가장 높은 점수의 Goal을 선택하고,

GOAP가 해당 Goal을 달성하기 위한 Planning을 생성합니다.

실무에서는 자주 사용되는 구조는 아니지만 특정 시스템에서는 활용 가능합니다.

### 사용 예시

- 특수 NPC
- 전략 시뮬레이션 AI
  
[MATRIX](#Matrix)
