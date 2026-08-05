# Rendering Pipeline

## 정의

Rendering Pipeline은 **3D 모델 데이터를 GPU가 화면에 출력하기까지의 전체 처리 과정**이다.

즉, **3D 공간의 데이터를 2D 화면에 표현하는 과정** 이라고 볼 수 있다.

Rendering Pipeline은 좌표를 변환하고, 화면에 보이는 Pixel을 생성한 후, 최종 색을 계산하여 화면에 출력한다.

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

## 1. Model

게임에서 사용하는 3D 모델 데이터이다.  
Model은 Vertex / Triangle / Normal / UV 등의 정보를 가지고 있다.

예)

```text
Player
Sword
Monster
```

---

## 2. Local Space

모델 자체가 가지고 있는 좌표계이다. 아직 월드에 배치되지 않은 상태이다.

예)

```text
Sword (0,0,0)
```

이 좌표는 "검 자체의 기준" 이다.

```text
Vertex Shader (Local → World 변환)
```

---

## 3. World Space

Scene에 배치된 좌표이다. Transform(Position / Rotation / Scale)이 적용된다.

예)

```text
Player (10,0,5)
```

같은 모델이라도 Transform이 다르면 World Space 좌표가 달라진다.

```text
Vertex Shader (World Matrix 적용)
```

---

## 4. View (Camera) Space

카메라를 기준으로 좌표를 변환한다.  
즉, "카메라에서 얼마나 떨어져 있는가"를 계산하는 단계이다.  
카메라가 움직이면 View Space도 함께 변경된다.

```text
Vertex Shader (View Matrix 적용)
```

---

## 5. Projection

3D 좌표를 2D 화면으로 투영한다.  
Perspective Camera의 경우 원근감이 적용된다.  

예)

```text
가까운 물체 → 크게

먼 물체 → 작게
```

```text
Vertex Shader (Projection Matrix 적용)
```

---

## 6. Viewport

Projection 이후의 좌표를 실제 화면 해상도로 변환한다.

예)

```text
1920 × 1080
2560 × 1440
```

같은 좌표라도 모니터 해상도에 맞게 변경된다.

```text
Vertex Shader 마지막 단계
```

---

## 7. Rasterization

Vertex로 구성된 Triangle을 Pixel(Fragment) 단위로 분할한다.

예)

```text
Triangle

↓

□□□□□□□□□
Pixel 생성
```

이 단계부터는 화면에 실제 그릴 Pixel들이 만들어진다.

```text
Vertex Shader 종료
↓
Fragment Shader 시작
```

---

## 8. Fragment (Pixel)

생성된 Pixel마다 최종 색상을 계산한다.  
여기서

- Texture
- Lighting
- Shadow
- Reflection
- Rim Light

등이 계산된다.  
즉, "무슨 색으로 그릴 것인가" 를 결정하는 단계이다.

```text
Fragment(Pixel) Shader
```

---

## 9. Frame Buffer

최종 계산된 Pixel 정보를 저장한다.  
Frame Buffer가 완성되면 모니터로 출력할 준비가 끝난 상태이다.

```text
Fragment Shader 종료
```

---

## 10. Display

Frame Buffer의 내용을 모니터에 출력한다. 우리가 실제로 보는 화면이다.

---

# 핵심 정리

```text
===========
Vertex Shader
(좌표 변환)

Local

↓

World

↓

View

↓

Projection

↓

Viewport
===========

↓

Rasterization
(Pixel 생성)

↓

===========
Fragment Shader
(Pixel 색 계산)
===========

↓

Frame Buffer
```

---

# 면접 포인트

- Rendering Pipeline은 **3D 데이터를 화면에 출력하는 전체 과정**이다.
- Local → World → View → Projection → Viewport 순으로 좌표가 변환된다.
- Rasterization에서 Triangle이 Pixel(Fragment)로 분할된다.
- Fragment 단계에서 Texture, Lighting 등을 이용해 최종 색상을 계산한다.
- Vertex Shader는 **좌표 변환**, Fragment Shader는 **색상 계산**을 담당한다.
