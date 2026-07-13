# Unity Animation

## 🎯 Goal

- Unity Animation 시스템의 구조를 이해한다.
- Mechanim(Animator)의 핵심 개념(State, Transition, Blend Tree 등)을 이해한다.
- Animation 재생 방식과 이벤트 처리 방식을 이해한다.
- Character Animation(Root Motion, Layer, Avatar Mask, IK)의 역할을 이해한다.
- Animation 최적화 방법과 Legacy Animation과의 차이를 이해한다.

---

# Animation Clip

## Q1. Animation Clip이란?

- Animation 데이터
- Bone Transform
- Asset

### 답변 

Animation Clip은 캐릭터의 Bone Transform 정보를 저장하는 Asset입니다. 걷기, 뛰기, 공격과 같은 하나의 Animation 데이터를 보관하며, Animator가 이를 재생합니다.

---

# Animator

## Q2. Animator란?

- Component
- Animation 실행
- Parameter 관리
- State 변경

### 답변 

Animator는 GameObject에 부착되는 Component로, Animator Controller를 실행하여 현재 상태에 맞는 Animation을 재생합니다. 또한 Parameter를 변경하고 State를 전환하는 역할을 담당합니다.

---

## Q3. Animator Controller란?

- Asset
- State Machine
- Transition
- Parameter
- Blend Tree

### 답변 

Animator Controller는 Animation의 흐름을 정의하는 Asset입니다. State, Transition, Parameter, Blend Tree 등을 설정하여 어떤 Animation을 언제 재생할지를 결정합니다.

---

## Q4. Animator와 Animator Controller의 차이는?

### 답변 

Animator는 GameObject에 부착되어 Animation을 실제로 실행하는 Component이고, Animator Controller는 Animation의 구조와 흐름을 정의하는 Asset입니다. 즉, Controller가 설계도라면 Animator는 이를 실행하는 Component입니다.

---

# State Machine

## Q5. State란?

- Animation 상태

### 답변 

State는 하나의 Animation이 재생되는 상태를 의미합니다. Idle, Walk, Attack처럼 각각의 Animation은 하나의 State로 구성됩니다.

---

## Q6. Transition이란?

- State 이동
- Parameter 조건

### 답변 

Transition은 하나의 State에서 다른 State로 이동하는 기능입니다. Parameter 조건을 만족하면 다음 State로 전환됩니다.

---

## Q7. Parameter란?

- Transition 조건 (Bool/Float/Int/Trigger)

### 답변 

Parameter는 State Transition의 조건으로 사용하는 변수입니다.

- Bool : 상태 표현
- Float : 연속적인 값
- Int : 단계 표현
- Trigger : 한 번만 발생하는 이벤트

---

## Q8. Bool과 Trigger의 차이는?

### 답변 

Bool은 일정 시간 유지되는 상태를 표현할 때 사용하며, Trigger는 한 번만 발생해야 하는 이벤트를 표현할 때 사용합니다. 예를 들어 `isDead`, `isMove`는 Bool이 적합하고, `Attack`, `Hit`은 Trigger를 사용하는 것이 적합합니다.

---

## Q9. Blend Tree란?

- Animation Blend
- Parameter
- 하나의 State

### 답변 

Blend Tree는 하나의 State 안에서 Parameter 값에 따라 여러 Animation을 자연스럽게 혼합하는 기능입니다. 대표적으로 Speed 값을 이용하여 Idle, Walk, Run을 자연스럽게 연결할 때 사용합니다.

---

## Q10. Blend Tree와 State Transition의 차이는?

### 답변 

State Transition은 하나의 State에서 다른 State로 이동하는 기능이며, Blend Tree는 하나의 State 안에서 여러 Animation을 Parameter 값에 따라 혼합하는 기능입니다. 즉, Transition은 State를 변경하고, Blend Tree는 State를 변경하지 않고 Animation만 Blend합니다.

---

# Animation Playback

## Q11. Play와 CrossFade의 차이는?

### 답변 

Play는 현재 Animation을 즉시 중단하고 새로운 Animation을 재생합니다. CrossFade는 지정한 시간 동안 이전 Animation과 새로운 Animation을 자연스럽게 Blend하여 전환합니다.

---

## Q12. CrossFade와 Transition의 차이는?

### 답변 

CrossFade는 코드에서 직접 Animation을 변경하는 기능이고, Transition은 Animator Controller에 설정된 조건에 따라 자동으로 State를 변경하는 기능입니다.

---

# Animation Event

## Q13. Animation Event란?

- Timeline
- 함수 호출

### 답변 

Animation Event는 Animation Timeline의 특정 시점에 지정한 함수를 호출하는 기능입니다.

|장점|단점|
|---|---|
|구현이 간단하다.|Animation과 Event가 결합된다.|
||Animation 재사용이 어렵다.|
||유지보수가 어려워질 수 있다.|  

### Oracle War 경험 

Oracle War에서는 이러한 문제 때문에 Animation과 Event를 분리한 Animation Sequence System을 구현했습니다.

### 예상 꼬리 질문 
- 왜 Animation Event를 사용하지 않았나요?
- Animation Sequence System은 어떻게 만들었나요?
- Animation Event의 제약은 무엇이었나요?

---

## Q14. time과 normalizedTime의 차이는?

### 답변 

time은 실제 재생 시간을 초(second) 단위로 표현한 값이고, normalizedTime은 Animation 전체를 0~1로 정규화한 값입니다. Loop Animation의 경우 normalizedTime은 2, 3처럼 계속 증가합니다.

### 예상 꼬리 질문 
- 왜 WaitForSeconds를 안 썼나요?
- Animation 속도 변경은 어떻게 대응했나요?
- timeScale이 변하면?

---

## Q15. Animation Sequence System이란?

### 답변

Animation Sequence System은 Animation과 Event를 분리하여 관리하는 시스템입니다. Animation의 재사용성을 높이고 Event를 독립적으로 관리하기 위해 사용합니다.

### 예상 꼬리 질문
- CSV 구조
- Event 저장 방식
- Invoke 사용 이유
- delegate를 안 쓴 이유
- 지금 다시 만든다면?

---

# Character Animation

## Q16. Root Motion이란?

- Root Bone

### 답변

Root Motion은 Animation에 포함된 Root Bone의 이동 정보를 이용하여 Animation의 이동량과 Character의 실제 이동을 동기화할 수 있습니다.

### Oracle War 경험

Unity Root Motion 대신 Root Bone 이동량을 추출하여 Animation Sequence Data에 저장하고 Runtime에서 적용했습니다.

### 예상 꼬리 질문
- 왜 Unity Root Motion 안 썼나요?
- Runtime 계산과 차이점은?

---

## Q17. Animation Layer란?

- 동시 재생

### 답변

Animation Layer는 여러 Animation을 동시에 재생하기 위한 기능입니다. 대표적으로 상체는 공격하고 하체는 달리는 Animation을 동시에 재생할 때 사용합니다.

---

## Q18. Avatar Mask란?

- Bone 선택
- Layer

### 답변

Avatar Mask는 특정 Bone에만 Animation을 적용하는 기능입니다. Animation Layer와 함께 사용하여 원하는 Bone에만 Animation을 적용합니다.

---

## Q19. IK(Inverse Kinematics)란?

- 목표 위치
- 관절 계산

### 답변

IK는 손이나 발의 목표 위치를 지정하면 관절의 회전을 자동으로 계산하여 자연스러운 자세를 만드는 기능입니다.  

---

# Animator Advanced

## Q20. Any State란?

- 즉시 전환
- 현재 State 무관

### 답변

Any State는 현재 어떤 State에 있든 관계없이 특정 State로 즉시 전환할 수 있는 기능입니다. Hit, Death처럼 언제든 실행되어야 하는 Animation에 주로 사용합니다.

---

## Q21. Exit Time(Has Exit Time)이란?

- Animation 종료
- Transition 조건

### 답변

Exit Time은 현재 Animation이 일정 비율 이상 재생된 후 Transition이 가능하도록 하는 옵션입니다. Attack이나 Skill처럼 Animation을 끝까지 재생해야 하는 경우 사용하며, Hit이나 Death처럼 즉시 전환해야 하는 경우에는 사용하지 않습니다.

---

## Q22. StateMachineBehaviour란?

- State 전용 스크립트 (OnStateEnter/OnStateUpdate/OnStateExit)  

### 답변

StateMachineBehaviour는 특정 Animation State에 부착되는 스크립트입니다. State 진입, 업데이트, 종료 시점을 제어할 수 있으며, GameObject 단위로 동작하는 MonoBehaviour와 달리 Animation State 단위로 동작합니다.

---

## Q23. Animator Override Controller란?

- Clip 교체
- Controller 재사용

### 답변

Animator Override Controller는 기존 Animator Controller의 State와 Transition 구조는 그대로 유지하면서 Animation Clip만 교체하는 기능입니다. 여러 캐릭터가 동일한 Animation 구조를 사용하지만 Animation만 다른 경우 Controller를 재사용할 수 있습니다.

---

# Optimization

## Q24. Animator.StringToHash()를 사용하는 이유는?

- 문자열 비교 제거
- Hash 비교
- 성능 향상

### 답변

Animator 내부에서는 Parameter를 Hash 값으로 관리합니다. StringToHash()를 사용하면 문자열 비교 대신 정수(Hash) 비교를 수행하므로 Parameter 접근 비용을 줄일 수 있습니다. 
Update처럼 자주 호출되는 경우 미리 Hash를 저장하여 사용하는 것이 좋습니다.

---

## Q25. Animation Compression이란?

- Keyframe 압축
- 메모리 절약

### 답변

Animation Compression은 사람의 눈으로 차이가 거의 없는 Keyframe을 제거하여 Animation 데이터를 압축하는 기능입니다. 메모리 사용량과 Animation Asset의 크기를 줄일 수 있습니다.

---

## Q26. Animation Culling Mode란?

- 화면 밖 최적화
- Animation 계산

### 답변

Animation Culling Mode는 화면 밖에 있는 Animator의 계산 여부를 결정하는 기능입니다.

- ***Always Animate*** : 항상 Animation을 계산합니다.
- ***Cull Update Transforms*** : 화면 밖에서는 Transform 계산을 최소화합니다.
- ***Cull Completely*** : 화면 밖에서는 Animator 계산을 중지합니다.

---

## Q27. Animator Update Mode란?

### 답변

Animator Update Mode는 Animator가 어떤 Update 주기에 맞춰 동작할지를 결정합니다.

- ***Normal*** : 일반 Update에서 동작합니다.
- ***Animate Physics*** : FixedUpdate와 동기화되어 물리 연산과 함께 동작합니다.
- ***Unscaled Time*** : Time.timeScale의 영향을 받지 않고 Animation을 재생합니다.

---

## Q28. Animation 최적화 방법은?

- Compression
- Layer
- Avatar Mask
- Culling
- StringToHash
- Event 최소화

### 답변

Animation 최적화를 위해 Animation Compression으로 메모리 사용량을 줄이고, 
Avatar Mask와 Layer를 활용하여 필요한 Bone만 계산하도록 구성합니다. 
또한 화면 밖의 캐릭터는 Culling Mode를 사용하여 Animation 계산을 줄이고, 
Parameter 접근 시 StringToHash를 사용하여 문자열 비교 비용을 줄입니다. 
Animation Event 역시 필요한 경우에만 사용하는 것이 유지보수와 성능 측면에서 유리합니다.  
NPC 수가 매우 많은 경우에는 Animator 자체를 줄이거나 DOTS(Animation) 또는 단순 Sprite/Bone 갱신 방식으로 대체하는 것도 고려합니다.  

---

## Q29. Animator가 많아질수록 성능 문제가 발생하는 이유는?

- Bone 계산
- State Machine
- Blend Tree
- Layer
- IK

### 답변

Animator는 매 프레임 Bone Transform 계산과 State Machine, Blend Tree, Layer, IK 등을 처리합니다. 따라서 동시에 동작하는 Animator가 많아질수록 CPU 사용량이 증가하며, NPC가 많은 프로젝트에서는 최적화가 중요합니다.

---

## Q30. Animation Event를 많이 사용하지 않는 이유는?

- 결합도
- 재사용성
- 유지보수

### 답변

Animation Event를 많이 사용하면 Animation Clip과 게임 로직이 강하게 결합됩니다. 같은 Animation을 다른 상황에서 재사용하기 어렵고, 호출되는 함수가 Animation에 분산되어 유지보수와 디버깅이 어려워질 수 있습니다.

---

## Q31. Legacy Animation과 Mechanim(Animator)의 차이는?

- Legacy
- Mechanim
- State Machine

### 답변

Legacy Animation은 코드에서 직접 Animation을 제어하는 방식으로 구조가 단순하지만 State 관리 기능이 부족합니다. Mechanim은 Animator Controller를 기반으로 State Machine, Transition, Blend Tree, Layer 등의 기능을 제공하여 복잡한 Animation을 효율적으로 관리할 수 있습니다. 현재 Unity에서는 대부분 Mechanim 사용을 권장합니다.

### Oracle War 경험

Oracle War는 Legacy 기반이라 AIBase에서 Animation을 직접 관리했습니다. 현재 다시 만든다면 Mechanim을 사용하여 Transition과 Blend Tree를 적극 활용할 것 같습니다.

### 예상 꼬리 질문
- Legacy를 선택한 이유는?
- Mechanim으로 다시 만든다면?
- 장단점은?
