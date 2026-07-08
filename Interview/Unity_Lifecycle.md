# Unity Lifecycle

## 🎯 Goal

- Unity Execution Order를 이해한다.
- MonoBehaviour Lifecycle을 설명할 수 있다.
- Coroutine의 동작 원리를 이해한다.
- 실무 경험과 연결하여 설명할 수 있다.

---

# Basic Questions

## Q1. Unity Lifecycle 순서를 설명해주세요.

### 답변

Awake → OnEnable → Start → FixedUpdate → Update → LateUpdate → OnDisable → OnDestroy 순으로 실행됩니다.

- Awake : 객체 생성 시 초기화
- OnEnable : 활성화될 때 호출
- Start : 모든 Awake 이후 한 번 호출
- FixedUpdate : 물리 연산
- Update : 매 프레임 호출
- LateUpdate : 모든 Update 이후 호출
- OnDisable : 비활성화
- OnDestroy : 객체 삭제

### 꼬리 질문

- Script Execution Order란?

### 참고

Unity 공식 문서

https://docs.unity3d.com/kr/2022.3/Manual/ExecutionOrder.html

---

## Q2. Awake와 Start의 차이는 무엇인가요?

### 답변

Awake는 객체 내부 초기화에 사용합니다. 모든 객체가 생성되는 즉시 호출되므로 다른 객체의 초기화 여부를 보장할 수 없습니다.

Start는 모든 Awake가 끝난 이후 호출되므로 다른 객체를 참조하거나 초기화가 완료된 Manager를 가져올 때 사용합니다.

### 실무 경험

Oracle War에서는 Singleton Manager를 Awake에서 생성하고, 다른 시스템에서는 Start에서 Manager를 참조하는 방식으로 구현했습니다.

### 꼬리 질문

- 다른 객체를 Awake에서 참조하면 어떤 문제가 발생할까요?

---

## Q3. Update / FixedUpdate / LateUpdate 차이는?

### 답변

- FixedUpdate : 물리 연산
- Update : 일반적인 게임 로직
- LateUpdate : 모든 Update 이후 최종 위치를 기준으로 처리

### 실무 경험

Oracle War에서는 Crowd Control(CC)과 카메라 연출을 구현할 때 처음에는 Update에서 처리했습니다. 하지만 상태 이상으로 인해 캐릭터 위치가 변경되는 과정에서 카메라가 이전 위치를 참조하는 문제가 발생했습니다. 이를 LateUpdate로 변경하여 모든 이동이 끝난 후 최종 위치를 기준으로 연출을 처리했고, 훨씬 자연스러운 화면을 만들 수 있었습니다.

### 배운 점

Unity Lifecycle은 단순한 호출 순서가 아니라 플레이어가 체감하는 연출 품질에도 영향을 준다는 점을 경험했습니다.

---

## Q4. Awake와 OnEnable 차이는?

### 답변

Awake는 객체가 생성될 때 한 번만 호출됩니다.

OnEnable은 객체가 활성화될 때마다 호출됩니다.

### 꼬리 질문

Object Pool에서는 어떤 함수를 주로 사용할까요?

→ OnEnable / OnDisable

---

## Q5. OnDisable과 OnDestroy 차이는?

### 답변

OnDisable은 객체가 비활성화될 때 호출됩니다.

OnDestroy는 객체가 메모리에서 제거될 때 호출됩니다.

---

## Q6. deltaTime이란?

### 답변

deltaTime은 이전 프레임과 현재 프레임 사이의 시간입니다.

프레임 속도와 관계없이 동일한 속도로 움직이도록 만들기 위해 사용합니다.

### 예시

```csharp
transform.Translate(Vector3.forward * speed * Time.deltaTime);
```

### 꼬리 질문

프레임 독립적인 로직은 어떻게 구현하나요?

→ deltaTime을 곱하여 구현합니다.

---

## Q7. targetFrameRate와 VSync 차이는?

### 답변

모바일에서는 targetFrameRate를 사용하여 목표 FPS를 설정합니다.

PC에서는 모니터 주사율과 동기화하기 위해 VSync를 사용하는 경우가 많습니다.

### 꼬리 질문

Screen Tearing이란?

→ Rendering 챕터 참고

---

# Coroutine

## Q13. Coroutine은 Thread인가요?

### 답변

아닙니다.

Coroutine은 Main Thread에서 실행되는 비동기 실행 방식입니다.

Thread처럼 보이지만 실제 병렬 처리는 하지 않습니다.

→ 자세한 내용 : Advanced/Thread.md

---

## Q14. Coroutine과 Update는 언제 사용하나요?

### 답변

Update는 객체가 살아있는 동안 계속 실행되는 로직에 적합합니다.

Coroutine은 일정 시간 동안 순차적으로 실행되는 이벤트 처리에 적합합니다.

### 실무 경험

Animation Sequence System에서는 처음에 WaitForSeconds를 사용하여 이벤트를 실행했습니다. 하지만 프레임 드랍이나 애니메이션 속도 변경 시 이벤트 타이밍이 어긋나는 문제가 발생했습니다.

그래서 WaitForSeconds 대신 yield return null을 사용하여 매 프레임 AnimationState.time을 확인하고, 현재 재생 시간이 이벤트 시점에 도달하면 이벤트를 실행하도록 수정했습니다.

이를 통해 프레임 드랍이나 애니메이션 속도 변경 상황에서도 정확한 타이밍에 이벤트가 실행되도록 개선했습니다.

---

## Q15. Coroutine 장단점

### 답변

장점

- 순차적인 이벤트 처리에 적합하다.
- 코드 가독성이 높다.
- 시간 기반 로직 구현이 쉽다.

단점

- 실제 병렬 처리가 아니다.
- Main Thread에서 실행된다.
- Coroutine이 너무 많아지면 관리가 어려워질 수 있다.

---

## Q16. Time.timeScale = 0이면 Coroutine은 어떻게 되나요?

### 답변

WaitForSeconds는 Time.timeScale의 영향을 받으므로 멈춥니다.

yield return null은 다음 프레임까지 대기하기 때문에 계속 호출되지만, AnimationState.time이나 deltaTime은 멈추므로 대부분의 게임 로직은 함께 정지합니다.

---

## Q17. WaitForSeconds와 WaitForSecondsRealtime 차이는?

### 답변

WaitForSeconds는 Time.timeScale의 영향을 받습니다.

WaitForSecondsRealtime은 실제 시간을 기준으로 동작합니다.

---

## Q18. 게임 Pause 상태에서는 Coroutine을 어떻게 처리하나요?

### 답변

게임 로직과 함께 멈춰야 하는 Coroutine은 WaitForSeconds를 사용합니다.

Pause와 상관없이 실행되어야 하는 UI나 로딩은 WaitForSecondsRealtime을 사용합니다.

필요한 경우 Coroutine Handle을 저장한 뒤 StopCoroutine으로 직접 중지하기도 합니다.

---

# Project Experience

## Q19. Lifecycle을 고려하여 구현했던 경험이 있나요?

### 답변

Oracle War에서 Crowd Control(CC)과 카메라 연출을 구현할 때 처음에는 Update에서 처리했습니다. 하지만 상태 이상으로 인해 캐릭터 위치가 변경되는 과정에서 카메라가 이전 위치를 참조하는 문제가 발생했습니다. 이를 LateUpdate로 변경하여 모든 이동이 끝난 뒤 최종 위치를 기준으로 연출을 처리했고, 훨씬 자연스러운 화면을 만들 수 있었습니다.

이 경험을 통해 Unity Lifecycle은 단순한 호출 순서가 아니라 플레이어가 체감하는 연출 품질에도 영향을 준다는 점을 배웠습니다.

---

## Q20. Animation Sequence System에서 Lifecycle을 고려했던 부분이 있었나요?

### 답변

Animation Sequence Editor 자체는 Unity Editor에서 동작하는 인하우스 툴이었기 때문에 Lifecycle을 고려할 필요는 없었습니다.

반면 Runtime에서 동작하는 Animation Sequence System은 Coroutine을 이용해 이벤트를 순차적으로 처리했습니다.

처음에는 WaitForSeconds를 사용했지만 프레임 드랍이나 애니메이션 속도 변경 시 이벤트 타이밍이 어긋나는 문제가 발생했습니다.

이를 AnimationState.time을 매 프레임 확인하는 방식으로 수정하여 항상 정확한 시점에 이벤트가 실행되도록 개선했습니다.

---

# Related Concepts

- [Thread & Coroutine](Advanced/Thread.md)
- [DOTS (ECS / Job System / Burst)](Advanced/DOTS(Dependency_Injection).md)
- [Dependency Injection](Advanced/DI.md)
- [NavMesh 최적화](Advanced/NavMesh.md)
- [Memory & GC](Advanced/Memory.md)
