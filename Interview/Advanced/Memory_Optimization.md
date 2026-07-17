# Memory Optimization

## 메모리 관련 경험

### Animation Resource Optimization

기존에는 동일한 Animation Clip을 스킬마다 복사하여 사용했습니다.

문제점

- Animation Clip 중복
- Event 데이터 중복
- 수정 비용 증가

Animation Sequence Editor를 통해 Animation과 Event를 분리하고 하나의 Animation Clip을 여러 스킬이 공유하도록 변경했습니다.

효과

- 리소스 관리 효율 향상
- 유지보수 향상

---

## Object Pool

Instantiate / Destroy를 반복하지 않고 오브젝트를 재사용합니다.

사용 예시

- Effect
- Projectile
- Monster

장점

- GC 감소
- CPU 부하 감소
- 생성 비용 감소

---

## GC

GC는 사용하지 않는 객체를 자동으로 회수합니다.

Instantiate / Destroy가 반복되면 GC가 자주 발생하여 프레임 드랍이 생길 수 있습니다.

---

## Unity Profiler

성능 분석 시

- CPU Usage
- Memory Usage
- GC Alloc

등을 확인합니다.

---

## 면접 답변

프로젝트에서는 Animation Sequence Editor를 통해 Animation과 Event 데이터를 분리하여 하나의 Animation Clip을 여러 스킬이 공유하도록 구조를 변경했습니다. 또한 자주 생성되는 오브젝트는 Object Pool을 사용하는 것이 일반적인 GC 최적화 방법이라는 점도 이해하고 있습니다.
