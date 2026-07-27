# Game AI - Decision Making

## 🎯 Goal
- FSM(Finite State Machine)의 개념과 사용 목적을 이해한다.
- Behavior Tree의 구조와 동작 방식을 이해한다.
- GOAP의 Planning 과정을 설명할 수 있다.
- Utility AI의 Score 기반 의사결정을 설명할 수 있다.
- 행동 AI(FSM / Behavior Tree / GOAP / Utility AI)의 차이를 설명할 수 있다.
- Oracle War 프로젝트에 적용한(또는 적용 가능한) 행동 AI를 설명할 수 있다.

---

# FSM (Finite State Machine)

---

## Q1. FSM(Finite State Machine)이란 무엇인가요?

- State / Transition

### 추천 답변
FSM(Finite State Machine)은 객체의 동작을 유한한(State) 상태로 나누고, 특정 조건(Transition)에 따라 상태를 전환하며 동작을 제어하는 AI 설계 방식입니다.  
현실의 행동은 무한하지만, 게임에서는 필요한 행동만 상태로 정의하여 관리함으로써 구현을 단순화할 수 있습니다.  
예를 들어 NPC는 Idle → Walk → Attack → Dead 와 같은 상태를 가지며, 조건에 따라 상태를 전환합니다.  

---

## Q2. FSM을 사용하는 이유는 무엇인가요?

### 추천 답변
상태가 명확하게 구분되고, 상태 전환 조건이 비교적 단순한 AI를 구현하기 위해 사용합니다.  
예를 들어 Idle / Walk / Attack / Dead 처럼 상태가 정해져 있고, 상태 전환만으로 대부분의 행동을 구현할 수 있는 경우 FSM이 적합합니다.  

### 꼬리 질문
- FSM이 적합하지 않은 경우는 언제인가요?

---

## Q3. FSM의 구성 요소(State, Transition)를 설명해주세요.

### 추천 답변

- **State**: 객체가 현재 수행 중인 행동이나 상태를 의미합니다.  
- **Transition**: 특정 조건을 만족했을 때 다른 State로 전환하는 규칙입니다.  
FSM은 현재 State를 기준으로 Transition 조건을 검사하고, 조건을 만족하면 다음 State로 변경됩니다.  

---

## Q4. Unity에서 FSM을 사용할 수 있는 대표적인 예를 설명해주세요.

- Animator (Mecanim)

### 추천 답변
Unity Animator(Mecanim)는 대표적인 FSM 구조입니다.  
Idle → Walk → Run → Jump → Attack 과 같은 Animation State를 가지고 있으며, Parameter와 Transition 조건을 이용하여 Animation State를 전환합니다.  
AI에서도 Idle, Patrol, Chase, Attack 등의 상태를 FSM으로 구현할 수 있습니다.  

### 꼬리 질문
- Animator State Machine과 AI FSM은 어떤 차이가 있나요?

---

## Q5. FSM의 장점과 단점은 무엇인가요?

### 추천 답변

#### 장점
- 구조가 단순하여 구현과 이해가 쉽습니다.
- 상태와 전환 조건이 명확합니다.
- 상태가 적은 AI에서는 매우 효율적입니다.

#### 단점
- 상태가 많아질수록 Transition이 기하급수적으로 증가합니다.
- switch문이나 if문 기반 구현은 결합도가 높아 유지보수가 어려워질 수 있습니다.

---

## Q6. Oracle War 프로젝트에서는 FSM을 어떻게 구현했나요?

### 추천 답변
Oracle War에서는 AiBase 내부에서 switch문을 이용하여 FSM을 구현했습니다.  
Idle, Walk, Patrol, Attack, Skill 등의 상태를 switch문으로 관리했고, 조건에 따라 상태를 변경하는 구조였습니다.  
프로토타입을 빠르게 제작해야 했기 때문에 switch문을 선택했습니다.  
이후에는 State Pattern으로 상태를 객체화하고, Behavior Tree 기반으로 발전시키는 방향을 고려했습니다.  

### 꼬리 질문
- switch문으로 구현하면서 가장 불편했던 점은 무엇이었나요?

---

## Q7. State Pattern과 FSM의 차이를 설명해주세요.

### 추천 답변
FSM은 상태(State)와 상태 전환(Transition)을 이용하여 AI를 구현하는 개념입니다.  
State Pattern은 이러한 FSM을 객체지향적으로 구현하는 대표적인 Design Pattern입니다.  
즉,  
- FSM = AI의 동작 방식  
- State Pattern = FSM 구현 방식  
이라는 차이가 있습니다.

---

# Behavior Tree

---

## Q1. Behavior Tree란 무엇인가요?

- Node / Tree

### 추천 답변
Behavior Tree는 NPC의 행동을 Tree 구조로 구성하여 현재 상황에 맞는 행동(Action)을 선택하는 AI 설계 방식입니다.  
FSM처럼 상태를 전환하는 것이 아니라, Root Node부터 Tree를 순회(Evaluate)하여 현재 가장 적절한 행동을 결정합니다.

---

## Q2. Behavior Tree를 사용하는 이유는 무엇인가요?

### 추천 답변
행동 종류가 많고, 상황에 따라 행동 우선순위가 계속 바뀌는 AI를 구현하기 위해 사용합니다.  
FSM보다 확장성이 뛰어나며, 새로운 행동을 추가하거나 수정할 때 기존 구조를 크게 변경하지 않아도 됩니다.

### 꼬리 질문
- Behavior Tree가 FSM보다 적합한 상황은 어떤 경우인가요?

---

## Q3. Behavior Tree의 구성 요소를 설명해주세요.
(Composite / Decorator / Leaf)

### 추천 답변

**Composite**: 자식 Node를 실행하는 Node입니다. 대표적으로 Selector / Sequence 가 Composite입니다.
**Decorator**: Child Node 실행 여부를 결정하거나, 실행 결과를 변경하는 Node입니다.  
예) HP < 30% / Enemy Visible / Cooldown Check 등 조건을 검사한 뒤 Child를 실행합니다.
**Leaf**: 실제로 행동(Action)을 수행하거나 조건(Condition)을 검사하는 Node입니다.  
예) Action(Move / Attack / Heal) / Condition(Enemy Visible / HP Low)

### 꼬리 질문
- Decorator는 어떤 상황에서 사용하는 것이 좋나요?

---

## Q4. Selector와 Sequence의 차이를 설명해주세요.

### 추천 답변
#### Selector
우선순위대로 Child를 실행하다가 Success가 나오면 즉시 종료합니다.  
즉 "가능한 행동 하나를 선택" 하는 Node입니다.  
예)  
```
Enemy Near? → Attack → Heal → Idle
```
```
Attack 성공 → 끝  
```
#### Sequence
Child를 순서대로 실행하며, 하나라도 Failure가 발생하면 즉시 종료합니다.  
즉 "순서대로 수행해야 하는 행동" 을 표현하는 Node입니다.  
예)  
```
Move → Aim → Attack
```
```
Move 실패 → Attack 실행 안 함
```

### 꼬리 질문
- Selector 안에 Sequence가 들어갈 수도 있나요?

---

## Q5. Behavior Tree의 장점과 단점은 무엇인가요?

### 추천 답변

#### 장점
- 행동을 Node 단위로 분리하여 재사용하기 쉽습니다.
- FSM보다 확장성이 좋습니다.
- 복잡한 AI를 자연스럽게 표현할 수 있습니다.

#### 단점
- 매 Tick마다 Tree를 순회(Evaluate)하므로 성능 부담이 발생할 수 있습니다.
- Tree가 커질수록 구조를 이해하기 어려워질 수 있습니다.

---

## Q6. Unity에서 Behavior Tree를 사용할 수 있는 대표적인 예를 설명해주세요.

- Boss AI / NPC AI

### 추천 답변
Boss AI나 NPC AI처럼 행동 종류가 많은 AI에서 자주 사용합니다.  
  
예를 들어 Melee / Range / Magic 등 공격 방식마다 Behavior Tree를 따로 구성하거나,  
Strategy Pattern을 이용하여 AttackType에 따라 Behavior Tree를 교체할 수 있습니다.

---

## Q7. Oracle War 프로젝트를 Behavior Tree로 변경한다면 어떻게 구현하시겠습니까?

### 추천 답변
기존에는 AttackType(Melee / Range / Magic)마다 switch문으로 행동을 구현했습니다.  
Behavior Tree를 적용한다면 AttackType별 Behavior Tree를 만들고,  
Strategy Pattern을 이용하여 캐릭터 생성 시 AttackType에 맞는 Behavior Tree를 할당하도록 설계하겠습니다.  

### 꼬리 질문
- 가장 먼저 어떤 부분부터 Behavior Tree로 변경하시겠습니까?  

---

## Q8. FSM과 Behavior Tree의 차이를 설명해주세요.

### 추천 답변
FSM은 상태(State)와 상태 전환을 중심으로 AI를 구현합니다.  
Behavior Tree는 현재 상황을 평가하여 조건에 맞는 행동(Action)을 선택합니다.  
  
FSM은 상태가 적고 상태 전환이 단순한 AI에 적합하며,  
Behavior Tree는 행동 종류가 많고 상황에 따라 행동이 계속 달라지는 AI에 적합합니다.

---

# GOAP (Goal Oriented Action Planning)

---

## Q1. GOAP(Goal Oriented Action Planning)이란 무엇인가요?

- Goal / Planning
- Action / World State

### 추천 답변
GOAP(Goal Oriented Action Planning)은 목표(Goal)를 달성하기 위해 현재 World State를 기반으로 가능한 Action들을 조합하여 최적의 행동 계획(Planning)을 생성하는 AI 설계 방식입니다.  
Behavior Tree처럼 미리 정의된 행동 순서를 사용하는 것이 아니라, 현재 상황에 따라 Goal까지 도달할 수 있는 최적의 Action Sequence를 매번 생성합니다.

---

## Q2. GOAP를 사용하는 이유는 무엇인가요? (언제 사용하는 것이 적합한가요? 포함)

### 추천 답변
목표는 같지만 현재 상황(World State)에 따라 행동 순서가 달라질 수 있는 AI를 구현할 때 적합합니다.  
예를 들어 Goal = Enemy Dead 이라면 무기 유무, 탄약, 적 위치 등에 따라  
```
Move → Attack

또는

Reload → Move → Attack
```
처럼 Planning이 달라질 수 있습니다.  

### 꼬리 질문
- GOAP가 Behavior Tree보다 적합한 상황은 어떤 경우인가요?

---

## Q3. GOAP의 구성 요소(Goal, World State, Action)를 설명해주세요.

### 추천 답변

**Goal**: AI가 최종적으로 달성하고자 하는 목표입니다.  
예) Enemy Dead / Eat Food / Escape  
**World State**: 현재 게임 내 상태를 의미합니다.  
예) Enemy Visible / Has Weapon / HP Low / Ammo Empty  
**Action**: Goal을 달성하기 위해 수행할 수 있는 행동입니다.  
각 Action은 Preconditions(전제조건) / Effects(결과) 를 가지고 있습니다.  
- Move (Enemy Visible(Pre) / Near Enemy(Post))
- Attack (Near Enemy(Pre) / Enemy Dead(Post))

---

## Q4. GOAP는 계획(Planning)을 어떻게 생성하나요?

- Preconditions / Effects

### 추천 답변
GOAP는 현재 World State에서 Goal을 만족시키기 위해 Action들의 Preconditions와 Effects를 비교하며 가능한 Action들을 탐색합니다.  
즉, 현재 상태에서 시작하여 Goal을 만족할 수 있는 Action Sequence를 생성하며,  
여러 Action이 존재하는 경우 Goal까지 도달할 수 있는 가장 효율적인 계획을 선택합니다.  

예)  
Goal  
```
Enemy Dead
```
↓  
Planning  
```
Move → Reload → Attack
```

### 꼬리 질문
- Planning은 언제 다시 수행해야 하나요?

---

## Q5. GOAP의 장점과 단점은 무엇인가요?

### 추천 답변

#### 장점
- 상황에 따라 Action을 유연하게 변경할 수 있습니다.
- 개발자가 모든 행동 흐름을 직접 정의하지 않아도 됩니다.
- Goal만 정의하면 Planning이 자동으로 생성됩니다.

#### 단점
- Planning 비용이 발생합니다.
- Action이 많아질수록 탐색 비용이 증가합니다.
- 구현 난이도가 FSM이나 Behavior Tree보다 높습니다.

---

## Q6. Unity에서 GOAP를 사용할 수 있는 대표적인 예를 설명해주세요.

- NPC / Simulation

### 추천 답변
오픈월드 NPC처럼 목표를 달성하기 위해 다양한 행동을 선택해야 하는 AI에 적합합니다.  

예를 들어
```
배고픔 → 음식 찾기 → 이동 → 먹기
```
또는
```
Enemy Dead → 무기 획득 → 이동 → 공격
```
처럼 상황에 따라 Planning이 달라지는 AI에서 주로 사용됩니다.

---

## Q7. Oracle War 프로젝트에 GOAP를 적용한다면 어떤 구조로 설계하시겠습니까?

- Quest / Auto Play

### 추천 답변
Auto Play 시스템에 적용할 수 있습니다.  
Quest 진행을 Goal로 설정하고, 현재 World State(Quest 진행 여부, HP, MP, 몬스터 위치 등)를 기반으로 Planning을 생성합니다.  
예를 들어
```
Quest 진행 → Move → Attack → Quest Complete → Return
```
또는
```
Quest 진행 중 HP가 낮다면 → Heal → Move → Attack
```
처럼 상황에 따라 Planning이 변경되도록 설계할 수 있습니다.

### 꼬리 질문
- Auto Play에서 Goal은 어떤 기준으로 변경하시겠습니까?

---

# Utility AI

---

## Q1. Utility AI란 무엇인가요?

- Utility (Score)

### 추천 답변
Utility AI는 현재 상황을 점수(Score)로 평가하여 가장 높은 점수를 가진 행동(Action)을 선택하는 의사결정 AI입니다.  
FSM처럼 상태를 전환하거나, GOAP처럼 계획을 생성하는 것이 아니라, 현재 상황에서 가장 가치 있는 행동을 선택하는 방식입니다.

---

## Q2. Utility AI를 사용하는 이유는 무엇인가요? (언제 사용하는 것이 적합한가요? 포함)

- Score (Priority)

### 추천 답변
현재 상황에 따라 행동 우선순위가 계속 달라지는 AI를 구현할 때 적합합니다.  
예를 들어 HP / MP / 적과의 거리 / 스킬 쿨타임 등에 따라 각 행동의 Score를 계산하고,  
가장 높은 점수의 행동을 선택합니다.

### 꼬리 질문
- Utility AI가 Behavior Tree보다 적합한 상황은 어떤 경우인가요?

---

## Q3. Utility AI는 점수(Score)를 어떻게 계산하나요?

- Utility Function

### 추천 답변
각 행동(Action)에 대해 현재 World State를 기반으로 Utility Function을 이용하여 점수를 계산합니다.

예를 들어
```
Heal
HP 20% → 95점
Attack → 40점
Run → 20점
```
처럼 각 행동의 Score를 계산한 뒤, 가장 높은 점수의 행동을 선택합니다.

### 꼬리 질문
- Score가 동일한 행동이 여러 개라면 어떻게 처리하시겠습니까?

---

## Q4. Utility Function이란 무엇인가요?

### 추천 답변
Utility Function은 현재 상황을 입력받아 특정 행동의 Score를 계산하는 함수입니다.  

예를 들어
```
HP ↓ → Heal Score ↑
Enemy Distance ↓ → Attack Score ↑
```
처럼 상황에 따라 행동의 점수를 계산하는 역할을 합니다.

---

## Q5. Utility AI의 장점과 단점은 무엇인가요?

### 추천 답변

#### 장점
- 현재 상황에 맞는 행동을 자연스럽게 선택할 수 있습니다.
- 행동 우선순위가 계속 변경되는 AI에 적합합니다.
- 새로운 행동을 추가하기 쉽습니다.

#### 단점
- Utility Function 설계가 어렵습니다.
- Score 조정(Balancing)에 시간이 많이 소요됩니다.
- 행동 수가 많아질수록 Score 계산 비용이 증가할 수 있습니다.

---

## Q6. Unity에서 Utility AI를 사용할 수 있는 대표적인 예를 설명해주세요.

- Boss AI / Auto Play

### 추천 답변
Boss AI나 Auto Play 시스템에서 사용할 수 있습니다.  
예를 들어
- HP가 낮으면 Heal 점수가 높아지고
- 적이 가까우면 Attack 점수가 높아지며
- 스킬 쿨타임이 끝났다면 Skill 사용 점수가 높아집니다.
현재 상황에서 모든 행동의 Score를 계산한 뒤, 가장 높은 점수의 행동을 선택하여 실행합니다.

---

## Q7. Oracle War 프로젝트에 Utility AI를 적용한다면 어떤 구조로 설계하시겠습니까?

### 추천 답변
Auto Play 시스템에 적용할 수 있습니다.  
Quest 진행 여부, HP, MP, 적의 수, 스킬 쿨타임 등을 기반으로 각 행동의 Score를 계산합니다.  

예를 들어  
- Quest 완료 → Return Score 증가
- HP 부족 → Heal Score 증가
- Quest 진행 중 → Monster Search Score 증가  
처럼 현재 상황에 맞는 행동을 선택하도록 설계하겠습니다.

### 꼬리 질문
- Auto Play에서 어떤 요소를 Score 계산에 사용할 것인가요?

---

# Related Concepts
- [Comparison_Combination](./Advanced/GameAI_Decision_Ref.md)
