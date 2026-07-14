# DOTS (Data-Oriented Technology Stack)

## DOTS란?

DOTS는 Unity가 기존 GameObject + MonoBehaviour 구조의 성능 한계를 해결하기 위해 만든 고성능 병렬 처리 프레임워크입니다.

기존 구조는 객체(Object) 중심으로 동작하기 때문에 수천 개 이상의 오브젝트를 동시에 처리하면 CPU Cache 효율이 떨어지고 Main Thread에 부하가 집중됩니다.

DOTS는 데이터를 중심(Data-Oriented)으로 관리하여 Cache 효율을 높이고 여러 CPU Core를 활용할 수 있도록 설계되었습니다.

|장점|단점|
|---|---|
|병렬 처리|기존 Unity API 사용 제한|
|Cache Friendly|학습 난이도 높음|
|대량 객체 처리에 강함|소규모 프로젝트에서는 오히려 개발 비용 증가|

---

## 구성

- ECS (Entity Component System)
- C# Job System
- Burst Compiler

---

## ECS

**기존**

GameObject  
↓  
Transform  
↓  
Renderer  
↓  
AI  
↓  
HP  

객체 중심

---

**ECS**

Transform Data  
HP Data  
AI Data  

처럼 데이터 종류별로 연속 저장합니다.

**장점**
- CPU Cache 효율 증가
- 반복 처리 속도 향상
- 동일한 Component를 한 번에 처리하기 때문에 Branch가 적다.

---

## Job System

CPU 여러 Core를 이용하여 작업을 자동 분산합니다.

예시

- AI 계산
- 거리 계산
- 충돌 계산
- Boids
- Flow Field

주의

Unity API는 사용할 수 없습니다.

---

## Burst Compiler

Job으로 작성한 C# 코드를 LLVM 기반 Native Code로 컴파일하여 실행 속도를 높입니다.

---

## DOTS가 등장한 이유

수천 개 이상의 NPC를 동시에 처리하기 위해 등장했습니다.

2018년 이후 Unity에서 적극적으로 개발되었습니다.

### 언제 사용하는가?

- Bullet Hell 게임
- Projectile 수만~수십만 개
- RTS 유닛 수천 개
- Crowd Simulation
- Boids
- 대규모 AI

### 언제 사용하지 않는가?

일반적인 규모의 게임에서는 기존 MonoBehaviour + GameObject 구조가 개발과 유지보수 측면에서 더 유리합니다.  
객체 수가 매우 많아 성능 병목이 발생하는 경우에만 DOTS를 고려합니다.  

---

## 면접 답변

Unity는 기존 GameObject + MonoBehaviour 구조의 성능 한계를 해결하기 위해 DOTS(ECS, Job System, Burst Compiler)를 도입했습니다.  
ECS는 데이터를 연속된 메모리에 저장하여 CPU Cache 효율을 높이고, Job System은 여러 CPU Core를 활용하여 병렬 계산을 수행합니다.  
다만 일반적인 게임에서는 MonoBehaviour 구조가 유지보수에 더 적합하며, 객체 수가 매우 많은 Bullet Hell이나 RTS 같은 프로젝트에서 DOTS를 고려하는 것이 적절하다고 생각합니다.
