# Rendering

## 🎯 Goal

- Rendering의 전체 흐름을 이해한다.
- CPU와 GPU의 역할을 이해한다.
- Draw Call과 Batching이 성능에 미치는 영향을 이해한다.
- Culling을 이용한 Rendering 최적화를 이해한다.
- Oracle War 프로젝트에 Rendering 최적화를 적용할 수 있다.

---

# 1. Rendering Basic

## Q1. Rendering이란 무엇인가요?

- GPU
- 화면 출력
- 3D → 2D

### 답변

Rendering은 **3D 데이터를 GPU를 통해 계산하여 2D 화면에 출력하는 과정**입니다.

---

## Q2. Rendering Pipeline이란 무엇인가요?

- CPU → GPU

### 답변

Rendering Pipeline은 **3D 데이터를 화면에 출력하기 위한 전체 처리 과정**입니다.

대표적인 흐름은 다음과 같습니다.

```text
Model
↓
Local Space
↓
World Space
↓
View(Camera) Space
↓
Projection
↓
Viewport
↓
Rasterization
↓
Fragment(Pixel)
↓
Frame Buffer
↓
Display
```

### 꼬리 질문

- Rendering Pipeline에서 Rasterization의 역할은 무엇인가요?
  - [Resterization](./Advanced/Rendering_Pipeline.md#6-rasterization)

---

## Q3. CPU와 GPU의 역할을 설명해주세요.

### 답변

CPU는 게임 로직, 물리 연산, AI, 스케줄링 등을 담당하며, 

GPU는 Rendering과 같은 대량의 그래픽 연산을 병렬 처리하는 데 특화되어 있습니다.

---

## Q4. Draw Call이란 무엇인가요?

- CPU → GPU: Rendering 명령

### 답변

Draw Call은 **CPU가 GPU에게 "이 오브젝트를 그려라"라고 전달하는 Rendering 명령**입니다.

Draw Call이 많아질수록 CPU가 GPU에게 전달해야 하는 명령 수가 증가합니다.

### 꼬리 질문

- Draw Call은 CPU와 GPU 중 어느 쪽의 부하에 더 큰 영향을 주나요?

---

## Q5. Draw Call이 많으면 왜 성능이 저하되나요?

- CPU 병목
- Rendering 명령

### 답변

Draw Call이 많아질수록 CPU가 GPU에 전달해야 하는 Rendering 명령이 증가하여 CPU 병목이 발생합니다.

결과적으로 프레임이 감소할 수 있습니다.

---

## Q6. Batching이란 무엇인가요?

- Draw Call 감소
- 같은 Material

### 답변

Batching은 **같은 Material을 사용하는 여러 오브젝트를 하나의 Draw Call로 묶어 Rendering하는 최적화 기법**입니다.

이를 통해 Draw Call을 감소시켜 Rendering 성능을 향상시킬 수 있습니다.

### 꼬리 질문

- Material 하나를 여러 오브젝트가 공유하면 어떤 장점이 있나요?

---

## Q7. Frustum Culling이란 무엇인가요?

- Camera 밖 (= 화면 밖)

### 답변

Frustum Culling은 **카메라 시야 밖에 있는 오브젝트를 Rendering하지 않는 최적화 기법**입니다.

카메라에 보이지 않는 오브젝트는 GPU가 그리지 않습니다.

---

## Q8. Occlusion Culling이란 무엇인가요?

- 가려진 오브젝트

### 답변

Occlusion Culling은 **다른 오브젝트에 의해 완전히 가려진 오브젝트를 Rendering하지 않는 최적화 기법**입니다.

카메라에 보이더라도 실제로 화면에 표시되지 않는 오브젝트는 Rendering 대상에서 제외합니다.

---

## Q9. Oracle War 프로젝트에서 Rendering 최적화를 적용한다면 어떻게 구현하시겠습니까?

### 답변

같은 종류의 Texture는 Texture Atlas를 사용하여 Batching을 유도하고 Draw Call을 감소시키겠습니다.

또한 LOD를 적용하여 거리에 따라 Polygon 수를 줄여 Rendering 비용을 감소시키겠습니다.

---

## Q10. Draw Call, Batching, Culling의 차이를 설명해주세요.

### 답변

| 항목 | 설명 |
|------|------|
| Draw Call | CPU가 GPU에게 보내는 Rendering 명령 |
| Batching | 여러 Draw Call을 하나로 합치는 최적화 |
| Culling | 보이지 않는 오브젝트를 Rendering하지 않는 최적화 |

### 꼬리 질문

- Draw Call을 줄이는 대표적인 방법에는 무엇이 있나요?

---

# 2. Shader

## Q1. Shader란 무엇인가요?

- GPU Program
- Vertex / Fragment(Pixel)

### 답변

Shader는 **GPU에서 실행되는 프로그램으로, 오브젝트를 어떻게 그릴지 결정하는 코드**입니다.

Vertex Shader는 정점의 위치를 계산하고,

Fragment(Pixel) Shader는 최종 Pixel의 색상을 계산합니다.

---

## Q2. Material과 Shader의 차이를 설명해주세요.

### 답변

Shader는 **렌더링 방식을 정의하는 코드**이고,

Material은 **Shader와 Texture, 색상 등의 속성을 묶어서 관리하는 데이터**입니다.

즉,

```text
Texture
        \
         Material
        /
Shader
```

구조로 생각하면 됩니다.

### 꼬리 질문

- Material 하나를 여러 오브젝트가 공유하면 어떤 장점이 있나요?

---

## Q3. Vertex Shader와 Fragment(Pixel) Shader의 차이를 설명해주세요.

- 위치 / 색
- [Vertex/Fragment(Pixel)]((./Advanced/Vetex_Fragment_Shader.md))

### 답변

Vertex Shader는 **정점(Vertex)의 위치를 계산**하는 단계입니다.

예를 들어 캐릭터 흔들림 / 깃발 펄럭임 / 물결 효과 등을 구현할 때 사용합니다.

Fragment(Pixel) Shader는 **최종 Pixel의 색상을 계산**하는 단계입니다.

예를 들어 조명 / 그림자 / 반사 / Rim Light / Emission 등을 구현할 때 사용합니다.

### 꼬리 질문

- Vertex Shader와 Fragment Shader 중 어느 쪽이 일반적으로 연산량이 더 많나요?

---

## Q4. Shader Graph란 무엇인가요?

- Visual Scripting

### 답변

Shader Graph는 **코드를 작성하지 않고 Node 기반으로 Shader를 제작할 수 있는 Unity 기능**입니다.

복잡한 Shader를 시각적으로 구성할 수 있어 개발 생산성이 높습니다.

---

## Q5. Built-in, URP, HDRP의 차이를 설명해주세요.

### 답변

| Pipeline | 특징 | 사용 환경 |
|-----------|------|-----------|
| Built-in | 기존 Unity Rendering Pipeline | 레거시 프로젝트 |
| URP | 성능 중심 Rendering Pipeline | 모바일, VR |
| HDRP | 고품질 Rendering Pipeline | PC, Console |

Oracle War처럼 모바일 기반 프로젝트라면 **URP가 적합**합니다.

### 꼬리 질문

- Oracle War 프로젝트라면 어떤 Rendering Pipeline을 선택하시겠습니까?

---

## Q6. 왜 Shader가 필요한가요?

- Rendering
- 표현

### 답변

Shader는 **오브젝트를 원하는 방식으로 표현하기 위해 필요합니다.**

같은 Texture라도 금속 / 물 / 유리 / 발광 등 다양한 재질 표현이 가능합니다.

---

## Q7. Material Instance란 무엇인가요?

- Material 복사
- 개별 속성

### 답변

Material Instance는 **기존 Material을 기반으로 개별 속성만 변경하여 사용하는 Material**입니다.

동일한 Shader를 사용하면서도 Texture나 Color 등을 개별적으로 변경할 수 있습니다.

---

## Q8. Oracle War 프로젝트에서 Shader를 사용한다면 어떤 곳에 적용하시겠습니까?

### 답변

물, 이펙트, Rim Light, Emission, 캐릭터 스킬 효과 등에 적용하여 게임의 연출을 강화하겠습니다.

---

## Q9. Shader Graph와 코드 Shader의 차이를 설명해주세요.

### 답변

| Shader Graph | 코드 Shader |
|---------------|-------------|
| Node 기반 | 코드 기반 |
| 제작이 쉬움 | 세부 제어 가능 |
| 빠른 프로토타이핑 | 최적화 및 복잡한 기능 구현 |

Shader Graph는 생산성이 높고,

코드 Shader는 성능 최적화와 세부 구현에 유리합니다.

---

## Q10. Shader, Material, Texture의 차이를 설명해주세요.

### 답변

| 항목 | 역할 |
|------|------|
| Texture | 오브젝트 표면에 사용하는 이미지 데이터 |
| Shader | GPU에서 실행되는 렌더링 코드 |
| Material | Shader와 Texture, 색상 등을 묶어 관리하는 데이터 |

구조는

```text
Texture
        \
         Material
        /
Shader
```

와 같습니다.

---

# 3. Texture & Lighting

## Q1. Texture란 무엇인가요?

- Image Data
- Rendering

### 답변

Texture는 **오브젝트 표면에 입히는 이미지 데이터**입니다.

Rendering 시 Material과 Shader를 통해 오브젝트의 외형을 표현하는 데 사용됩니다.

---

## Q2. Sprite Atlas란 무엇인가요?

- Texture 통합
- Batching
- Draw Call

### 답변

Sprite Atlas는 **여러 Texture를 하나의 큰 Texture로 합친 것**입니다.

같은 Material을 공유하도록 하여 Batching을 적용하기 쉽고, Draw Call을 감소시켜 Rendering 성능을 향상시킵니다.

---

## Q3. Mipmap이란 무엇인가요?

- 거리
- 작은 Texture
- GPU

### 답변

Mipmap은 **거리에 따라 작은 해상도의 Texture를 사용하는 기능**입니다.

멀리 있는 오브젝트에는 낮은 해상도의 Texture를 사용하여 GPU 부하를 줄이고, Texture 깜빡임(Aliasing)을 감소시킵니다.

### 꼬리 질문

- Mipmap을 사용하면 어떤 상황에서 성능 향상을 기대할 수 있나요?

---

## Q4. Texture Compression이란 무엇인가요?

- VRAM / Memory
- [Texture Compression](./Advanced/Texture_Compression.md)

### 답변

Texture Compression은 **Texture를 압축하여 VRAM과 메모리 사용량을 줄이는 기법**입니다.

압축된 Texture를 사용하면 GPU 메모리 사용량이 감소하여 Rendering 성능 향상에 도움이 됩니다.

---

## Q5. Directional Light와 Point Light의 차이를 설명해주세요.

### 답변

| 항목 | Directional Light | Point Light |
|------|-------------------|-------------|
| 특징 | 일정한 방향의 빛 | 한 점에서 사방으로 퍼지는 빛 |
| 예시 | 태양 | 전구 |

### 꼬리 질문

- Directional Light는 왜 거리의 영향을 받지 않나요?

---

## Q6. Realtime Lighting과 Baked Lighting의 차이를 설명해주세요.

- Dynamic / Static

### 답변

| 항목 | Realtime Lighting | Baked Lighting |
|------|-------------------|----------------|
| 계산 | 실시간 계산 | 미리 계산 |
| 대상 | 움직이는 오브젝트 | Static 오브젝트 |
| 장점 | 실시간 변화 가능 | 성능 우수 |
| 단점 | GPU 부하 증가 | 실시간 변화 불가 |

---

## Q7. Shadow Map이란 무엇인가요?

- Depth

### 답변

Shadow Map은 **빛 기준의 깊이(Depth) 정보를 저장하여 그림자를 계산하는 Texture**입니다.

---

## Q8. Opaque와 Transparent의 차이를 설명해주세요.

- [Z Buffer](./Advanced/Z_Buffer.md)

### 답변

| 항목 | Opaque | Transparent |
|------|---------|-------------|
| 특징 | 불투명 | 반투명 |
| Z Buffer | 사용 | 제한적 사용 |
| Rendering 순서 | 크게 중요하지 않음 | 매우 중요 |

Transparent는 뒤에 있는 오브젝트와 색상을 섞어야 하므로 **뒤에서 앞으로(Render Back to Front)** 그리는 것이 중요합니다.

### 꼬리 질문

- Transparent는 왜 Rendering 순서가 중요한가요?

---

## Q9. Alpha Blend란 무엇인가요?

### 답변

Alpha Blend는 **Alpha 값을 이용하여 앞 오브젝트와 뒤 오브젝트의 색상을 섞는 기법**입니다.

투명도 표현에 사용됩니다.

---

## Q10. Oracle War 프로젝트에서 Texture 최적화를 어떻게 적용하시겠습니까?

- Texture Atlas
- Compression
- Mipmap

### 답변

같이 사용하는 Texture는 Sprite Atlas를 적용하여 Batching을 유도하고 Draw Call을 감소시키겠습니다.

또한 Mipmap과 Texture Compression을 적용하여 VRAM 사용량을 줄이고 GPU 부하를 감소시키겠습니다.

---

## Q11. Oracle War 프로젝트에서 Lighting 최적화를 어떻게 적용하시겠습니까?

- Baked
- Realtime

### 답변

Oracle War는 던전 기반으로 시간 변화가 거의 없기 때문에 Static 오브젝트의 조명과 그림자는 Baked Lighting을 사용하겠습니다.

움직이는 캐릭터와 이펙트만 Realtime Lighting을 적용하여 성능과 품질의 균형을 맞추겠습니다.

---

# Related Concepts
- [Rendering Pipeline](./Advanced/Rendering_Pipeline.md)
- [Vertex/Fragment(Pixel)]((./Advanced/Vetex_Fragment_Shader.md))
- [Texture Compression](./Advanced/Texture_Compression.md)
- [Z Buffer](./Advanced/Z_Buffer.md)
