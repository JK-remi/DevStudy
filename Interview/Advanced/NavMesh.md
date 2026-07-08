# NavMesh & AI Optimization

## 경험

서바이벌 프로젝트에서 약 200마리 이상의 몬스터를 동시에 동작시키면서 심각한 성능 저하를 경험했습니다.

처음에는 Coroutine이 원인이라고 생각했지만 이후 분석해보니 NavMeshAgent의 업데이트 비용이 훨씬 큰 영향을 준다는 것을 알게 되었습니다.

---

## 적용한 방법

- Spawn 시간 분산
- AI Update 분산
- 멀리 있는 몬스터 Disable

---

## 추가적으로 가능한 최적화

### AI Update 분산

매 프레임 모든 몬스터를 업데이트하지 않습니다.

예시

Frame1

Monster 1~20

Frame2

Monster 21~40

Frame3

Monster 41~60

---

### Destination 분산

SetDestination도 일정 프레임마다 나누어 호출합니다.

---

### LOD AI

멀리 있는 몬스터는

- AI 중지
- Animation 중지
- Update 중지

---

### NavMeshSurface

예전에는 Scene 하나에 Bake했지만 현재는 지역별 Surface 생성이 가능합니다.

---

### NavMeshLink

지역과 지역을 연결하는 기능입니다.

---

## 대규모 AI

수천 개 이상의 NPC에서는 NavMeshAgent 대신

- Flow Field
- Steering Behavior
- Boids

등을 사용하는 경우가 많습니다.

---

## 면접 답변

프로젝트에서 약 200마리 이상의 몬스터를 동시에 동작시키며 성능 문제를 경험했습니다. 초기에는 Coroutine이 원인이라고 생각했지만 분석 결과 NavMeshAgent의 업데이트 비용이 더 큰 병목이라는 것을 확인했습니다. Spawn과 AI Update를 분산시키는 방식으로 개선했고 이후 Unity의 Job System과 ECS가 등장한 이유도 이해하게 되었습니다.
