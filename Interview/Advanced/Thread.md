# Thread & Coroutine

## Thread란?

Thread는 CPU가 실제로 동시에 작업을 수행하는 실행 단위입니다.
여러 Thread를 생성하면 CPU의 여러 Core를 사용하여 병렬 작업을 수행할 수 있습니다.

예시
- 파일 입출력
- 네트워크 통신
- 대량의 수학 계산

---

## Coroutine과 Thread의 차이

| Coroutine | Thread |
|------------|--------|
| Main Thread에서 실행 | 별도의 Thread에서 실행 |
| 병렬 처리 아님 | 실제 병렬 처리 |
| yield를 이용하여 실행을 잠시 중단 | CPU가 동시에 실행 |
| Unity API 사용 가능 | Unity API 사용 불가 |

Coroutine은 Thread처럼 보이지만 실제로는 Main Thread에서 순차적으로 실행됩니다.

---

## Unity에서 Thread 사용 시 주의사항

Thread에서는 Unity API를 사용할 수 없습니다.

사용 불가능한 예시

- Transform
- Instantiate()
- Destroy()
- Animator
- NavMeshAgent

Thread에서는 순수 계산만 수행해야 합니다.

---

## Unity에서 병렬 처리

기존에는 일반 Thread를 사용했지만 현재는 Job System 사용이 권장됩니다.

자세한 내용은 DOTS 문서를 참고합니다.

→ [DOTS.md](DOTS.md)
