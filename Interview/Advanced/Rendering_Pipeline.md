# Rendering Pipeline

## 정의

Rendering Pipeline은 **3D 데이터를 화면에 출력하기까지의 전체 처리 과정**이다.

즉,

> 3D 모델 → 2D 화면

으로 변환하는 과정이다.

---

## 전체 흐름

```text
Model
    │
    ▼
Local Space
    │
    ▼
World Space
    │
    ▼
View(Camera) Space
    │
    ▼
Projection
    │
    ▼
Viewport
    │
    ▼
Rasterization
    │
    ▼
Fragment(Pixel)
    │
    ▼
Frame Buffer
    │
    ▼
Display
```

---

## 각 단계

### 1. Local Space

모델 자체의 좌표

예)

```text
Sword

(0,0,0)
```

---

### 2. World Space

씬에 배치된 좌표

예)

```text
Player

(10,0,5)
```

---

### 3. View Space

카메라 기준 좌표

카메라에서

얼마나 멀리 있는지 계산

---

### 4. Projection

3D 좌표를

2D 화면으로 투영

여기서

Perspective

원근감이 적용된다.

---

### 5. Viewport

정규화 좌표를

실제 화면 해상도로 변환

예)

```text
1920 × 1080
```

---

### 6. Rasterization

Triangle을

Pixel(Fragment) 단위로 분할

```text
Triangle

↓

Pixel 생성
```

---

### 7. Fragment(Pixel)

Pixel 색 계산

- Texture
- Lighting
- Shadow
- Reflection

---

### 8. Frame Buffer

최종 색 저장

모니터 출력 직전 단계

---

### 9. Display

모니터 출력

---

## 핵심

```text
3D

↓

2D

↓

Pixel 생성

↓

색 계산

↓

출력
```

---

## 면접 포인트

Rendering Pipeline은

> "3D 데이터를 화면에 출력하는 전체 과정"

이라는 정도만 이해하면 충분하다.
