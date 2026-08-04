# Vertex Shader & Fragment Shader

## Shader란?

GPU에서 실행되는 프로그램. 오브젝트를 어떻게 그릴지 결정한다.

---

# Vertex Shader

## 역할

Vertex 위치 계산

즉

```text
Vertex

↓

어디에 그릴 것인가?
```

---

## 대표 예시

- 캐릭터 흔들림
- Grass 흔들림
- 물결
- 깃발

---

# Fragment(Pixel) Shader

## 역할

Pixel 색 계산

즉

```text
Pixel

↓

무슨 색으로 그릴 것인가?
```

---

## 대표 예시

- Lighting
- Shadow
- Reflection
- Rim Light
- Emission
- Transparency

---

# 처리 순서

```text
Vertex Shader

↓

Rasterization

↓

Fragment Shader

↓

Frame Buffer
```

---

# 차이

| Vertex | Fragment |
|---------|----------|
| 위치 계산 | 색 계산 |
| Vertex 개수만큼 실행 | Pixel 개수만큼 실행 |
| 상대적으로 가벼움 | 상대적으로 무거움 |

---

## 면접 포인트

Vertex

=

위치

Fragment

=

색
