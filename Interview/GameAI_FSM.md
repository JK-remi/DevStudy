# FSM (Finite State Machine)

## 🎯 Goal
- FSM(Finite State Machine)의 개념과 동작 방식을 이해한다.
- State와 Transition의 역할을 설명할 수 있다.
- FSM의 장단점과 적용 시점을 설명할 수 있다.
- Unity에서 FSM을 구현하는 방법을 설명할 수 있다.
- Oracle War 프로젝트에서 사용한 FSM 구조를 설명할 수 있다.
- State Pattern과 FSM의 관계 및 차이를 설명할 수 있다.
- Behavior Tree와 FSM의 차이를 설명할 수 있다.

---

## Q1. FSM(Finite State Machine)이란 무엇인가요?
사람의 행동은 무한하지만 그렇게 동작하기에는 비용이 크므로, 유한한 상태를 두고, 그 내부의 상태 전환으로 움직임 구현.  
### 수정안
FSM은 객체의 행동을 유한한 상태(State)로 나누고, 조건(Transition)에 따라 상태를 전환하면서 동작을 구현하는 AI 기법입니다. 복잡한 행동을 여러상태로 분리하여 구현과 관리가 쉬워집니다.  

## Q2. FSM을 사용하는 이유는 무엇인가요?  (언제 사용하는 것이 적합한가요?까지 포함)
유한의 상태가 정해져 있고, 상태 전환으로 동작 구현 가능할 때  
- Feedback: 명확한 상태로 구분 가능  
  - ex. idle, move, attack  
### 수정안
행동을 상태별로 분리하여 구현을 단순화하기 위해 사용합니다. Idle, Move, Attack처럼 명확하게 구분 가능한 AI에서 적합합니다.  
### 꼬리 질문
* FSM이 적합하지 않은 경우는 언제인가요?
  * State / Transition 수
  * Behavior Tree / Utility AI

## Q3. FSM의 구성 요소(State, Transition)를 설명해주세요.
state: 상태, transition: 조건에 따른 상태 전환  
- Feedback
  - State: 현재 Ai가 수행하는 행동
  - Transition: 특정조건을 만족했을 때 다음 State로 변경하는 규칙  
### 수정안
FSM은 크게 State와 Transition으로 구성됩니다. State는 AI가 현재 수행하고 있는 행동을 의미합니다. Transition은 특정 조건을 만족했을 때 현재 State에서 다른 State로 변경하는 규칙입니다.

## Q4. Unity에서 FSM을 사용할 수 있는 대표적인 예를 설명해주세요.
메카님(animator)를 활용한 상태 전환  
### 수정안
NPC AI(Idle, Patrol, Chase, Attack), 플레이어 상태, Animator State Machine 등이 대표적인 예입니다.
### 꼬리질문
Animator State Machine과 AI FSM은 어떤 차이가 있나요? 

## Q5. FSM의 장점과 단점은 무엇인가요?
복잡하지 않은 조건 안에서 유한한 상태를 유려하게 전환 가능.  
상태 많아지면 관리 복잡해짐. State Pattern 고려 안 한 구현은 결합도가 매우 높기 때문에 유지 보수 어려움.  
- Feedback
  - 장점: 구현 / 디버깅 쉬움, 상태 적을 때 관리하기 좋음
  - 단점: 상태 많아질수록 Transition 증가 / 복잡한 AI = 유지보수 어려움
### 수정안
FSM은 상태를 명확하게 구분하기 때문에 구현이 단순하고 디버깅이 쉽다는장점이 있습니다. 또한 상태별로 동작을 나누어 관리할 수 있어 비교적 이해하기 쉽습니다.  
하지만 상태가 많아질수록 상태 전환 조건(Transition)이 급격하게 증가하여 관리가 어려워집니다. 따라서 복잡한 AI에서는 코드가 비대해지고 유지보수가 어려워질 수 있으며, 이런 경우에는 Behavior Tree와 같은 구조를 사용하는 것이 더 적합합니다.  

## Q6. Oracle War 프로젝트에서는 FSM을 어떻게 구현했나요?
AiBase에서 switch문으로 상태 조정. idle-walk-run-attack-skill-patrol의 상태가 있던 걸로 기억.
### 수정안
OracleWar에서는AiBase 내부에서 enum과 switch문을 이용해 FSM을 구현했습니다. Idle, Patrol, Chase, Attack 등의 상태를 관리했으며, 플레이어와의 거리나 감지 여부 등에 따라 상태를 전환했습니다. 
### 꼬리질문
switch문으로 구현하면서 가장 불편했던 점은 무엇이었나요? 

## Q7. State Pattern과 FSM의 차이를 설명해주세요.
state pattern은 FSM 제작의 한 방식(많은 개발자들에 의해 검증된).
### 수정안
FSM은 상태 머신 알고리즘이고, State Pattern은 FSM을 객체지향적으로 구현하는 대표적인 설계 패턴 입니다. 
### 꼬리질문
State Pattern으로 FSM을구현하면 어떤 장점이 있나요?


# Behavior Tree

## 🎯 Goal
- Behavior Tree의 개념과 동작 방식을 이해한다.
- Node(Composite, Decorator, Leaf)의 역할을 설명할 수 있다.
- Selector와 Sequence의 차이를 설명할 수 있다.
- Behavior Tree의 장단점과 적용 시점을 설명할 수 있다.
- FSM과 Behavior Tree의 차이를 설명할 수 있다.
- Oracle War 프로젝트를 Behavior Tree 구조로 변경한다면 어떻게 설계할지 설명할 수 있다.

---

## Q1. Behavior Tree란 무엇인가요?
NPC의 동작 방식을 Tree형태로 구성한 것
- Feedback: 노드 순회, 행동 결정
### 수정안
Behavior Tree는 AI의 행동을 트리 구조로 표현하고, 루트 노드부터 각 노드를 순차적으로 평가하면서 현재 상황에 맞는행동을 선택하는 AI 알고리즘입니다.  
### 꼬리 질문
Behavior Tree는 매 프레임 어떤 방식으로 동작하나요?

## Q2. Behavior Tree를 사용하는 이유는 무엇인가요? (언제 사용하는 것이 적합한가요? 포함)
NPC 종류에 따른 State가 다른 경우
- Feedback: 상황에 따른 행동 변화
### 수정안
AI의 행동이 복잡하고 조건이 많아질 때 사용합니다. 여러 행동을 우선순위와 조건에 따라 자연스럽게 선택해야 하는 NPC에 적합합니다.

## Q3. Behavior Tree의 구성 요소를 설명해주세요. (Composite / Decorator / Leaf)
leaf: state, 나머지는 모르겠음.(state 전환 조건 & 전환 시 행동 ?)
- Feedback
  - Composite: 자식 노드 실행 노드 (Selector, Sequence)
  - Decorator: 자식 노드의 실행 조건 or 결과 변경 노드
  - Leaf: 실제 행동(Action)이나 조건(Condition) 수행 노드 
### 수정안
Behavior Tree는 크게 Composite, Decorator, Leaf 노드로 구성됩니다.  
Composite는 여러 자식 노드를 관리하는 노드이며, 대표적으로 Selector와 Sequence가 있습니다.  
Decorator는 자식 노드의 실행조건이나 실행 결과를 변경하는 역할을 합니다. 예를 들어특정 조건이 만족될 때만 실행하거나, 성공과 실패 결과를 반대로 만드는 기능 등을 수행합니다.  
Leaf는 실제 행동(Action)이나 조건(Condition)을 수행하는 노드입니다. 예를 들어 Move, Attack…과 같은 행동이나 EnemyInRange와 같은 조건 검사를 담당합니다.   
### 꼬리질문
Decorator는 어떤상황에서 사용하는 것이 좋나요?  
  - Decorator: 규칙 (child 동작 여부 결정)
  - Condition Leaf: 질문 (단순 조건 check)

## Q4. Selector와 Sequence의 차이를 설명해주세요.
selector는 분기 있어서 선택, sequence는 순서대로 진행
- Feedback
  - Selector: 왼쪽부터 실행하여 성공하는 노드 나오면 즉시 종료
  - Sequence: 왼쪽부터 실행하며 하나라도 실패하면 즉시 종료
### 수정안
Selector는 여러 행동 중 하나를 선택하기 위한 노드입니다 자식 노드를 왼쪽부터 실행하여 성공하는 노드가 나오면 즉시 실행을 종료하고 성공을 반환합니다. 따라서 우선순위가 높은 행동을 먼저 시도하는데 적합합니다.  
Sequence는 여러 행동을 순서대로 수행하기 위한 노드입니다. 자식노드를 순서대로 실행하며 하나라도 실패하면 즉시 실패를 반환합니다. 따라서 여러 행동을 순차적으로모두 수행해야 하는경우에 사용됩니다.   
### 꼬리 질문
Selector 안에 Sequence가 들어갈 수도 있나요?

## Q5. Behavior Tree의 장점과 단점은 무엇인가요?
개발자 편의대로 행동 양식 구현 가능.  
행동 양식마다 늘어남?  
- Feedback
|장점|단점|
|---|---|
|행동 추가 쉬움|Tick마다 Tree순회 = 성능 비용|
|상태 폭발(State Explosion) 적음|트리 커질수록 구조 복잡해짐|
|우선순위 기반 AI 구현 쉬움 ||
### 수정안
Behavior Tree는 행동을 트리 구조로 분리하기 때문에 새로운행동을 추가하거나 기존 행동을수정하기 쉽고, 상태가 많아져도 FSM보다 구조를관리하기 수월합니다. 또한 상황에 따라 행동을 유연하게 선택할 수 있기 때문에 복잡한 NPC AI를 구현하는데 적합합니다.  
하지만 매 Tick마다 트리를 순회하여 현재 행동을 결정하기 때문에 FSM보다 연산량이 많을 수 있습니다. 또한 트리가 지나치게 커질경우 구조를 이해하거나 관리하기 어려워질 수 있습니다.      

## Q6. Unity에서 Behavior Tree를 사용할 수 있는 대표적인 예를 설명해주세요.
animator의 behavior tree 같은? NPC(Melee, Magic, Range) 같은 Strategy적 접근?
- Feedback: 상황 맞춰 행동 선택
### 수정안
적 Ai, 보스 AI, 동료 AI처럼 상황에 따라 행동 선택해야 하는 NPC에 사용합니다.
### 꼬리질문
Unity에서 Behavior Tree 직접 구현 해봤나요?

## Q7. Oracle War 프로젝트를 Behavior Tree로 변경한다면 어떻게 구현하시겠습니까?
원래는 행동 양식 달라지면 다 따로 switch문 구현. NPC(Melee, Magic, Range) 같은 Strategy적 접근
### 수정안
현재는 switch문으로 상태를 관리했지만, Behavior Tree를 적용한다면 Root아래에 Selector를 두고 Attack, Chase, Patrol 등의행동을 자식 노드로 구성하겠습니다. 각 행동은 Condition을 통해 실행 여부를 판단하도록 구현할 것입니다.    

## Q8. FSM과 Behavior Tree의 차이를 설명해주세요.
행동양식 고정 or 개발자 편의대로 구현
- Feedback
  - FSM: 상태 중심
  - Behavior Tree: 행동 선택 중심
### 수정안
FSM은 현재 상태(State)를 기준으로 Transition 조건을 만족하면 다른 상태로 전환하는방식입니다. 따라서 상태가 적고 행동이 단순한AI에 적합합니다.  
Behavior Tree는 현재 상태를저장하기보다는 매 Tick마다 트리를순회하면서 현재 상황에 가장적합한행동을 선택합니다. 따라서 조건이 많고 행동이 복잡한 AI에서도 유연하게 확장할 수 있으며, 복잡한 NPC AI 구현에 더 적합합니다.      
### 꼬리질문	
FSM과 Behavior Tree를 함께 사용하는 경우도 있나요?
