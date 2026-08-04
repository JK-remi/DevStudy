# Vertex Shader vs Fragment Shader

## Shader란?

GPU에서 실행되는 프로그램이다.

Rendering 과정에서 Vertex와 Pixel을 처리한다.

---

# Vertex Shader

## 입력

```text
Vertex

Position

Normal

UV
```

## 역할

Vertex 위치 계산

```text
Local

↓

World

↓

Camera

↓

Projection
```

### 예시

- Water 흔들림
- Grass 흔들림
- Character 흔들림

---

# Fragment(Pixel) Shader

## 입력

```text
Pixel

Texture

Lighting

Shadow
```

## 역할

Pixel 색 계산

### 예시

- Diffuse
- Specular
- Reflection
- Rim Light
- Emission

---

# 실행 순서

```text
Mesh

↓

Vertex Shader

↓

Triangle

↓

Rasterization

↓

Fragment Shader

↓

Frame Buffer
```

---

# 차이

| 항목 | Vertex | Fragment |
|------|---------|----------|
| 대상 | Vertex | Pixel |
| 역할 | 위치 계산 | 색 계산 |
| 실행 횟수 | Vertex 수 | Pixel 수 |
| 연산량 | 적음 | 많음 |

---

## 왜 Fragment가 더 무거운가?

예)

```text
Vertex 100개

↓

Triangle 200개

↓

Pixel 2,000,000개
```

Fragment Shader는 Pixel마다 실행된다.

그래서 Lighting, Reflection, Shadow 계산이 훨씬 무겁다.
