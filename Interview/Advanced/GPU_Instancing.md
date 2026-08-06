# GPU Instancing (심화)

## 정의

GPU Instancing은 동일한 Mesh와 Material을 사용하는 여러 오브젝트를 한 번의 Draw Call로 Rendering하는 기술이다.

CPU가 GPU에 보내는 명령(Draw Call)을 크게 줄일 수 있다.

---

## 기존 Rendering 방식

예를 들어 나무가 1000개 있다면

```text
Tree

↓

Draw Call

↓

Tree

↓

Draw Call

...

1000번
```

같은 모델이어도 CPU는 GPU에게 1000번 명령을 보낸다.

---

## GPU Instancing 적용

```text
Tree × 1000

↓

Draw Call 1번

↓

GPU 내부에서
1000개 Rendering
```

CPU 부하가 크게 감소한다.

---

## 왜 Shader가 지원해야 하는가?

GPU Instancing에서는 Material은 같지만 각각의 Transform은 다르다.

```text
Tree A
Position (0,0,0)

Tree B
Position (5,0,3)

Tree C
Position (8,0,2)
```

GPU는 하나의 Draw Call 안에서 여러 개의 Transform을 각각 계산해야 한다.

따라서 Shader는 "여러 개의 오브젝트를 동시에 처리할 수 있다."는 기능을 지원해야 한다.

Unity에서는 Shader가 GPU Instancing을 지원해야 Material Inspector에서

```
Enable GPU Instancing
```

옵션이 활성화된다.

---

## 적용 조건

GPU Instancing이 적용되기 위해서는

- 같은 Mesh
- 같은 Material
- 같은 Shader
- Shader가 GPU Instancing 지원
- Material에서 Enable GPU Instancing 체크

를 모두 만족해야 한다.

---

## 적용되지 않는 경우

다음과 같은 경우에는 Instancing이 불가능하다.

- Mesh가 다른 경우
- Material이 다른 경우
- Shader가 다른 경우
- Shader가 GPU Instancing을 지원하지 않는 경우
- Enable GPU Instancing을 체크하지 않은 경우

---

## 언제 사용하는가?

동일한 모델이 대량으로 등장하는 경우 가장 효과적이다.

대표적인 예

- 나무
- 풀
- 바위
- 총알
- 동일 NPC
- 반복되는 Effect

---

## Batching와의 차이

### Batching

CPU에서 오브젝트를 묶어서 GPU에 전달한다.

### GPU Instancing

CPU는 한 번만 명령을 보내고, GPU가 내부에서 여러 개를 처리한다.

즉, Instancing은 동일 Mesh를 대량으로 그릴 때 Batching보다 더 효율적이다.

---

## Oracle War에 적용한다면

Oracle War에서는 동일한 몬스터가 수십 마리 등장하거나 동일한 이펙트가 반복 생성되는 경우 GPU Instancing을 고려할 수 있다.

다만 프로젝트에서는 최대 10마리 내외의 몬스터가 동시에 등장했기 때문에 Draw Call이 큰 병목이 아니었고, Texture Atlas와 Batching 정도로도 충분한 성능을 확보할 수 있었다.
