# Unity Physics

## 🎯 Goal

* Rigidbody와 Collider의 역할 이해
* Collision / Trigger 차이 이해
* Raycast와 Physics Query 활용
* Force와 Rigidbody 이동 방식 이해
* Physics 최적화 방법 이해

---

# Rigidbody

## Q1. Rigidbody란 무엇인가?

- Physics Engine
- 물리 계산

### 답변

Rigidbody는 Unity Physics 엔진이 해당 GameObject를 물리 객체로 인식하도록 하는 Component입니다.  
중력, 힘(AddForce), 속도(Velocity), 충돌 반응 등을 Physics 엔진이 계산하여 적용합니다.  

### 예상 꼬리 질문
- Rigidbody 없이도 충돌이 발생할 수 있나요?

---

## Q2. Collider만 있는 객체와 Rigidbody가 있는 객체의 차이는?

- Collider: 충돌 영역  
- Rigidbody: 물리 계산 대상  
- Physics  

### 답변

Collider는 충돌 영역만 정의하는 Component입니다.  
Rigidbody가 추가되면 Unity Physics 엔진이 해당 객체를 물리 객체로 인식하여 중력, 힘, 충돌 반응 등을 계산합니다.  

---

## Q3. isKinematic은 언제 사용하는가?

- Physics 비활성화  
- Script 제어  
- Animation 제어  

### 답변

isKinematic은 Rigidbody를 Physics 엔진이 직접 움직이지 않도록 하는 옵션입니다.  
중력이나 AddForce의 영향을 받지 않고 Animation이나 Script에서 직접 이동을 제어할 때 사용합니다.  

### 예상 꼬리 질문
- isKinematic인데 Trigger는 발생하나요?

---

## Q4. Use Gravity는 어떤 옵션인가?

### 답변

Rigidbody에 중력을 적용할지 여부를 결정하는 옵션입니다.  
활성화하면 Physics 엔진의 중력 영향을 받고, 비활성화하면 중력의 영향을 받지 않습니다.  

---

## Q5. Constraints(Freeze Position / Rotation)는 언제 사용하는가?

### 답변

Rigidbody의 Position 또는 Rotation 축을 고정하는 기능입니다.  
불필요한 물리 움직임을 막거나 특정 방향으로만 이동하도록 제한할 때 사용합니다.  

---

## Q6. MovePosition과 transform.position의 차이는?

- Physics 이동
- 직접 이동

### 답변

MovePosition은 Physics 엔진을 통해 이동하여 Collision과 Rigidbody 계산이 정상적으로 수행됩니다.  
transform.position은 Physics를 거치지 않고 즉시 위치를 변경하므로 Collision 오차나 Physics 계산 충돌이 발생할 수 있습니다.  

### 예상 꼬리 질문
- Rigidbody가 있는데 transform.position을 계속 사용하면 어떤 문제가 발생할까요?

---

## Q7. MoveRotation과 transform.rotation의 차이는?

### 답변

MoveRotation은 Physics 엔진을 통해 회전을 적용하는 방식입니다.  
transform.rotation은 Physics 계산 없이 즉시 회전값을 변경하므로 Rigidbody 객체에서는 예상하지 못한 움직임이 발생할 수 있습니다.  

---

# Collider

## Q8. Collider란 무엇인가?

- 충돌 영역(Hit Box)

### 답변
 
Collider는 물체의 충돌 영역(Hit Box)을 정의하는 Component입니다.  
자체적으로 물리 계산은 하지 않으며 Rigidbody와 함께 Collision 또는 Trigger 판정을 수행합니다.  

---

## Q9. Collision과 Trigger의 차이는?

- Physics
- Is Trigger

### 답변

Collision은 실제 물리 충돌이 발생하며 Physics 엔진이 충돌 반응을 계산합니다.  
Trigger는 물리 반응 없이 충돌 여부만 감지하여 이벤트만 발생합니다.  
둘의 차이는 Collider의 **Is Trigger 옵션**입니다.  

### 예상 꼬리 질문
- Projectile는 Collision과 Trigger 중 무엇을 사용하시겠습니까?

---

## Q10. OnCollisionEnter와 OnTriggerEnter의 차이는?

### 답변

OnCollisionEnter는 실제 물리 충돌이 발생했을 때 호출됩니다.  
OnTriggerEnter는 Trigger 영역에 들어왔을 때 물리 반응 없이 호출됩니다.  

---

## Q11. OnCollisionStay / Exit는 언제 사용하는가?

### 답변

OnCollisionStay는 충돌이 유지되는 동안 지속적으로 호출됩니다.  
OnCollisionExit는 충돌이 종료되는 순간 한 번 호출됩니다.  

---

## Q12. OnTriggerStay / Exit는 언제 사용하는가?

### 답변

OnTriggerStay는 Trigger 영역 안에 있는 동안 지속적으로 호출됩니다.  
OnTriggerExit는 Trigger 영역을 벗어나는 순간 한 번 호출됩니다.  

---

## Q13. Collision이 발생하려면 어떤 조건이 필요한가?

### 답변
- 두 객체 모두 Collider가 있어야 합니다.  
- 둘 중 하나에는 Rigidbody가 있어야 합니다.  
- 두 Collider 모두 Is Trigger가 꺼져 있어야 합니다.  

---

## Q14. Trigger가 발생하려면 어떤 조건이 필요한가?

### 답변
- 두 객체 모두 Collider가 있어야 합니다.  
- 둘 중 하나에는 Rigidbody가 있어야 합니다.  
- 둘 중 하나 이상의 Collider에서 Is Trigger가 체크되어 있어야 합니다.  

---

## Q15. Physics Material이란?

- Friction
- Bounciness

### 답변

Physics Material은 Collider의 마찰(Friction)과 반발력(Bounciness)을 설정하는 Asset입니다.  
얼음처럼 미끄럽게 하거나, 공처럼 튀는 정도를 조절할 때 사용합니다.  

---

# Raycast

## Q16. Raycast란?

- 대표적인 Physics Query
- Collider 검사 O
- Physics 계산 X

### 답변

Raycast는 한 점에서 특정 방향으로 Ray를 발사하여 Collider와의 충돌 여부를 검사하는 기능입니다.  
실제 물리 충돌은 발생하지 않으며, 충돌한 객체의 정보(RaycastHit)를 반환합니다.  

---

## Q17. Raycast를 사용하는 대표적인 예는?

### 답변

대표적으로 다음과 같은 경우 사용합니다.  
- FPS Hit Scan 총알
- 마우스 클릭 오브젝트 선택
- Ground Check
- AI의 Line of Sight(장애물 확인)

### 예상 꼬리 질문
- AI 시야는 Trigger와 Raycast 중 무엇을 사용하시겠습니까?
- Trigger와 OverlapSphere 중 무엇을 사용하시겠습니까?

---

## Q18. Raycast와 Trigger를 언제 각각 사용하는가?

- Raycast → 직접 검사
- Trigger → 영역 감지

### 답변

Raycast는 특정 시점에 원하는 방향으로 충돌 여부를 직접 검사할 때 사용합니다.  
Trigger는 Collider 영역 안에 들어왔는지를 지속적으로 감지할 때 사용합니다.  
즉, Raycast → 직접 검사 / Trigger → 영역 감지에 적합합니다.  

---

## Q19. LayerMask는 왜 사용하는가?

- Filtering
- 최적화

### 답변

LayerMask는 Physics 검사 대상을 제한하기 위해 사용합니다.  
예를 들어  
- Enemy만 검사
- Ground만 검사
- Projectile끼리는 무시  
등 불필요한 충돌 계산을 줄여 Physics 성능을 향상시킬 수 있습니다.  

---

## Q20. RaycastNonAlloc을 아는가?

- GC
- 배열 재사용

### 답변

RaycastNonAlloc은 결과를 미리 준비한 배열에 저장하여 새로운 메모리를 할당하지 않는 Raycast입니다.  
반복 호출 시 GC를 줄일 수 있어 성능 최적화에 사용합니다.  

---

# Force

## Q21. AddForce()란?

### 답변

AddForce는 Rigidbody에 힘(Force)을 가하여 Physics 엔진을 통해 자연스럽게 움직이도록 하는 함수입니다.  
힘의 방향과 크기에 따라 가속, 감속, 낙하 등이 Physics 계산에 의해 결정됩니다.  

---

## Q22. ForceMode 종류와 차이는?

### 답변

| ForceMode | 설명 |
| ---------- | ---- |
| Force | 지속적으로 힘을 가합니다. 질량의 영향을 받습니다. |
| Acceleration | 지속적으로 가속도를 적용합니다. 질량의 영향을 받지 않습니다. |
| Impulse | 순간적인 힘을 가합니다. 질량의 영향을 받습니다. (점프, 폭발) |
| VelocityChange | 순간적으로 속도를 변경합니다. 질량의 영향을 받지 않습니다. |

---

## Q23. velocity를 직접 변경하는 것과 AddForce의 차이는?

### 답변

velocity는 Rigidbody의 속도를 직접 변경하는 방식입니다. 즉시 원하는 속도를 적용할 수 있습니다.  
반면 AddForce는 Physics 엔진을 통해 힘을 가하는 방식으로 가속도와 질량 등이 함께 계산됩니다.  

---

## Q24. Projectile 구현 시 velocity를 사용하는 경우와 AddForce를 사용하는 경우의 차이는?

### 답변

Projectile가 일정한 속도로 직선 이동하면 되는 경우에는 velocity를 사용합니다.  
반면 중력, 감속, 폭발 등의 Physics 반응이 필요한 Projectile는 AddForce를 사용하는 것이 적합합니다.  

---

# Physics Update

## Q25. 왜 Physics는 FixedUpdate에서 처리해야 하는가?

- Fixed Timestep

### 답변

Physics 엔진은 FixedUpdate 주기에 맞춰 일정한 시간 간격(Time.fixedDeltaTime)으로 계산됩니다.  
따라서 Rigidbody를 제어하는 코드는 FixedUpdate에서 실행해야 Physics 계산과 동기화되어 안정적인 충돌과 움직임을 보장할 수 있습니다.  

---

## Q26. Update에서 Rigidbody를 움직이면 어떤 문제가 발생하는가?

### 답변

Update는 프레임마다 호출되고 Physics는 FixedUpdate마다 계산됩니다.  
따라서 Update에서 Rigidbody를 움직이면 Physics 계산과 동기화되지 않아  
- Jitter
- Collision 오차
- 예상하지 못한 움직임  
등의 문제가 발생할 수 있습니다.

---

## Q27. Interpolation은 무엇인가?

### 답변

Interpolation은 Physics 업데이트 사이의 위치를 보간하여 화면에서 더 부드럽게 보이도록 하는 기능입니다.  
Physics 계산 횟수를 늘리는 것이 아니라 렌더링 시 움직임을 자연스럽게 보이게 합니다.  

---

## Q28. Continuous Collision Detection(CCD)은 언제 사용하는가?

- Fast Projectile
- Tunneling

### 답변

Continuous Collision Detection(CCD)은 매우 빠르게 이동하는 Rigidbody가 Collider를 통과하는(Tunneling) 현상을 방지하기 위한 기능입니다.  
총알이나 매우 빠른 Projectile 등에 사용합니다.  

---

## Q29. Physics 최적화 방법은?

### 답변

Physics 최적화를 위해
- 불필요한 Rigidbody를 사용하지 않고
- 필요한 경우에만 Collision을 사용하며
- LayerMask로 충돌 대상을 제한하고
- RaycastNonAlloc 등을 사용하여 GC를 줄이고
- Physics 계산이 필요 없는 경우 Trigger나 Raycast를 사용하는 것이 좋습니다.

---

## Q30. Projectile이 수천 개 있는 게임이라면 Physics를 어떻게 최적화하겠는가?

- Object Pool
- Collision/Trigger/Raycast
- **LayerMask**

### 답변

Projectile의 특성에 따라 Physics 적용 여부를 구분합니다.  
단순 충돌 판정만 필요한 Projectile는 Trigger나 Raycast를 사용하고,  
실제 물리 반응이 필요한 Projectile만 Collision을 사용합니다.  

또한 Object Pool을 사용하여 생성/삭제 비용을 줄이고,  
LayerMask를 통해 불필요한 충돌 계산을 최소화합니다.  

Projectile 수가 수만~수십만 개 수준까지 증가한다면 ECS(Entity Component System)와 DOTS(Data-Oriented Technology Stack)를 고려하겠습니다.  

### 꼬리 질문
- Projectile이 10만 개라면 어떻게 구현하시겠습니까?

---

# Related Concepts

- [DOTS](Advanced/DOTS.md)  

