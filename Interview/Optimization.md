# Optimization

## 🎯 Goal
- Profiling을 통해 병목(Bottleneck)을 분석할 수 있다.
- CPU Bound와 GPU Bound를 구분하고 원인을 파악할 수 있다.
- GC, Object Pool, ScriptableObject 등을 활용한 CPU / Memory 최적화를 설명할 수 있다.
- Draw Call, Batching, Culling, LOD 등을 활용한 Rendering 최적화를 설명할 수 있다.
- 실제 프로젝트(Oracle War)에 적용했던 최적화 방법을 설명할 수 있다.

---

# 1. Profiling & Bottleneck

## Q1. Profiling이란 무엇인가요?

- Runtime 분석
- 성능 측정
- 병목 분석

### 답변

Profiling은 프로그램 실행 중 CPU, GPU, Memory 등의 성능 정보를 측정하여 병목(Bottleneck)을 분석하는 과정이다. 성능 최적화는 추측이 아니라 Profiling을 통해 원인을 찾은 후 진행해야 한다.

### 꼬리 질문

- Memory Leak이 계속 발생하면 어떤 문제가 생기나요?

---

## Q2. Unity Profiler란 무엇인가요?

- CPU / GPU Usage
- Memory
- Rendering (Draw Call)

### 답변

Unity에서 제공하는 성능 분석 도구이다. CPU Usage, GPU Usage, Memory, Rendering, Draw Call, SetPass Call 등을 확인하여 성능 병목이 발생하는 구간을 찾는다.

---

## Q3. Bottleneck(병목)이란 무엇인가요?

- Frame Drop (렉)
- CPU / GPU Bound

### 답변

특정 작업이 전체 프레임 속도를 제한하는 현상이다. CPU 연산, Rendering, Memory 관리 등에서 병목이 발생하면 Frame Drop이나 렉이 발생한다.

---

## Q4. CPU Bound와 GPU Bound의 차이를 설명해주세요.

### 답변

CPU Bound는 CPU가 프레임 생성을 따라가지 못하는 상황이며, GC, AI 연산, Physics 등이 원인이 된다. GPU Bound는 GPU가 Rendering을 따라가지 못하는 상황이며, Draw Call 증가, 무거운 Shader, Post Processing 등이 원인이 된다.

---

## Q5. 성능 문제가 발생하면 어떤 순서로 원인을 분석하시겠습니까?

- Profiler
- CPU/GPU Bound
- 병목

### 답변

먼저 Unity Profiler를 실행하여 CPU Usage와 GPU Usage를 확인한다. CPU Bound인지 GPU Bound인지 구분한 후 병목이 발생한 구간을 확인하고, 해당 시스템(Rendering, Memory, AI 등)을 중심으로 최적화를 진행한다.

---

## Q6. Deep Profile이란 무엇인가요?

- 함수 단위 상세 분석
- Overhead

### 답변

일반 Profiler보다 함수 단위까지 세부적으로 분석하는 기능이다. 어떤 함수에서 시간이 많이 소모되는지 확인할 수 있지만, 분석을 위해 추가적인 연산이 발생하므로 실제 성능보다 느리게 측정될 수 있다.

### 꼬리 질문

- GC를 개발자가 직접 호출하는 것이 적절한 상황은 언제인가요?
- Deep Profile은 왜 전체 성능을 왜곡할 수 있나요?

---

# 2. CPU Optimization

---

## Q1. Object Pooling이란 무엇인가요?

### 답변

객체를 생성(Instantiate)과 삭제(Destroy)하지 않고 재사용하는 기법이다. 자주 생성되는 객체를 삭제하지 않고 비활성화한 뒤 다시 초기화하여 사용함으로써 GC 발생을 줄이고 CPU 부하를 감소시킨다.

### 꼬리 질문

- Object Pool의 크기는 어떻게 결정하시겠습니까?

---

## Q2. Garbage Collection(GC)이란 무엇인가요?

- 메모리 해제 (참조 제거)
- Frame Drop

### 답변

더 이상 참조되지 않는 객체를 탐지하여 메모리를 해제하는 과정이다. GC가 실행되는 동안 CPU가 일시적으로 멈출 수 있으므로 Frame Drop의 원인이 될 수 있다. 일반적으로 메모리 부족이나 일정 조건에서 자동으로 실행되며, Loading 화면과 같이 사용자 입력이 없는 구간에서는 개발자가 직접 호출하기도 한다.

---

## Q3. Boxing / Unboxing이란 무엇인가요?

- Value - Reference(object) Type
- 메모리 할당

### 답변

Boxing은 Value Type을 Reference Type(object)으로 변환하는 과정이며, Unboxing은 다시 Value Type으로 변환하는 과정이다. Boxing 시 추가 메모리 할당이 발생하므로 빈번하게 발생하면 GC 증가와 성능 저하의 원인이 될 수 있다.

### 꼬리 질문

- C#에서 Boxing이 발생하는 대표적인 예를 하나 들어보세요.

---

## Q4. Update 최적화 방법에는 어떤 것들이 있나요?

- Coroutine / Timer / n Frame

### 답변

매 프레임 실행할 필요가 없는 로직은 Update 대신 Coroutine이나 Timer를 사용하거나, 일정 시간 또는 n Frame마다 실행하도록 변경하여 CPU 사용량을 줄일 수 있다.

---

## Q5. Coroutine과 Update의 차이를 설명해주세요.

- 매 프레임 / 비동기 & 순차 실행

### 답변

Update는 매 프레임 호출되는 함수이며, Coroutine은 특정 이벤트나 시간 흐름에 따라 순차적으로 실행되는 함수이다. 지속적인 상태 확인은 Update가 적합하고, 시간 지연이나 순차적인 처리는 Coroutine이 적합하다.

### 꼬리 질문

- Coroutine을 사용하면 적합하지 않은 경우는 언제인가요?

---

## Q6. foreach와 for의 차이를 설명해주세요.

- Enumerator / Index

### 답변

for는 Index 기반 반복문이며, foreach는 Enumerator를 사용하여 컬렉션을 순회한다. 현대 C#에서는 성능 차이가 크지 않지만, 성능이 중요한 구간에서는 일반적으로 for를 사용하는 경우가 많다.

---

## Q7. LINQ를 자주 사용하지 않는 이유는 무엇인가요?

- Language Integrated Query
  - 컬렉션 조회 / 가독성
- Enumerator / Alloc / GC

### 답변

LINQ(Language Integrated Query)는 컬렉션(List, Array, Dictionary 등)을 SQL과 비슷한 문법으로 조회하고 가공할 수 있도록 제공하는 C# 기능이다.  
대표적으로 Where(), Select(), OrderBy(), FirstOrDefault() 등을 사용하여 데이터를 간결하게 처리할 수 있다.  
다만 내부적으로 Enumerator 생성이나 추가 메모리 할당(Alloc)이 발생할 수 있으므로 런타임 성능이 중요한 게임 플레이 로직에서는 사용을 최소화하고, Editor Tool이나 개발 편의 기능에서는 자주 사용한다.  

---

## Q8. Oracle War 프로젝트에서 CPU 최적화를 적용한다면 어떻게 구현하시겠습니까?

### 답변

자주 생성되는 객체는 Object Pool을 사용하여 GC 발생을 최소화하고, 시간 기반 처리는 Coroutine을 사용하여 Update 호출을 줄인다. 또한 반복문은 성능이 중요한 구간에서는 for문을 사용하고, 불필요한 연산은 가능한 한 줄이도록 구현한다.

### 꼬리 질문

- Oracle War 프로젝트에서 가장 효과가 컸던 CPU 최적화는 무엇이라고 생각하시나요?

---

# 3. Memory Optimization

---

## Q1. Memory Leak이란 무엇인가요?



- 참조 유지 / OOM / GC

### 답변

Memory Leak은 더 이상 사용하지 않는 객체가 메모리에서 해제되지 않고 계속 남아 있는 현상이다. 메모리 사용량이 지속적으로 증가하며, 심한 경우 OOM(Out Of Memory)이나 잦은 GC로 인해 Frame Drop이 발생할 수 있다.

### 꼬리질문

- Memory Leak을 발견하면 어떤 순서로 원인을 찾으시겠습니까?

---

## Q2. Unity에서 Memory Leak이 발생하는 대표적인 원인은 무엇인가요?

- Event / Delegate / static
- 참조 유지

### 답변

대표적인 원인은 Event 또는 Delegate를 해제하지 않아 참조가 유지되는 경우와 static 변수에 객체를 계속 보관하는 경우이다. 또한 사용하지 않는 객체를 참조하고 있는 경우에도 Memory Leak이 발생할 수 있다.

---

## Q3. ScriptableObject를 사용하는 이유는 무엇인가요?

- 공유 데이터
- 데이터 관리
- 메모리 절약
- [Scriptable Object](./Advanced/ScriptableObject.md)

### 답변

ScriptableObject는 데이터를 여러 객체가 공유할 수 있도록 만든 객체이다. GameObject나 MonoBehaviour를 생성하지 않아도 되므로 메모리 사용량을 줄이고 데이터 관리도 편리해진다.

---

## Q4. ScriptableObject가 메모리를 절약하는 이유는 무엇인가요?

- 데이터 공유
- 인스턴스 공유

### 답변

동일한 데이터를 여러 객체가 하나의 ScriptableObject 인스턴스를 공유하기 때문이다. 동일 데이터를 객체마다 복사하지 않으므로 메모리 사용량이 감소한다.

---

## Q5. Resources.Load를 자주 사용하지 않는 이유는 무엇인가요?

- Resources
- 메모리
- 관리

### 답변

Resources 폴더의 Asset은 빌드 시 모두 포함되므로 프로젝트 규모가 커질수록 메모리 관리와 Asset 관리가 어려워진다. 또한 런타임에서 반복적으로 로드하는 것은 권장되지 않는다.

---

## Q6. Addressables란 무엇인가요?

- [Addressables](./Advanced/Addressables_AssetBundle.md#Addressables)
- 동적 로드

### 답변

Addressables는 Asset을 주소(Address) 기반으로 관리하고 동적으로 로드하는 Unity 시스템이다. 내부적으로 AssetBundle을 자동 생성 및 관리하며, 개발자는 주소만 이용하여 Asset을 로드할 수 있다.

---

## Q7. AssetBundle과 Addressables의 차이를 설명해주세요.

- [Addressables vs AssetBundle](./Advanced/Addressables_AssetBundle.md#assetBundle-vs-addressables)
- 자동 관리

### 답변

AssetBundle은 개발자가 직접 생성하고 로드 및 의존성을 관리해야 하는 패키지 파일이다. Addressables는 AssetBundle을 내부적으로 자동 생성하고 의존성 관리, 로드, 메모리 관리 등을 Unity가 지원하는 시스템이다.

---

## Q8. Oracle War 프로젝트에서 메모리 최적화를 적용한다면 어떻게 구현하시겠습니까?

### 답변

자주 생성되는 객체는 Object Pool을 사용하여 GC 발생을 줄이고, 공유 데이터는 ScriptableObject를 사용하여 메모리 사용량을 감소시킨다. 또한 Event와 Delegate의 등록 및 해제를 명확하게 관리하여 Memory Leak이 발생하지 않도록 구현한다.

---

# 4. Rendering Optimization

## Q1. Draw Call을 줄이는 방법에는 어떤 것들이 있나요?

- Batching (Texture Atlas)
- GPU Instancing
- Culling

### 답변

Draw Call을 줄이는 대표적인 방법은 Static/Dynamic Batching, Texture Atlas, GPU Instancing, Frustum Culling, Occlusion Culling 등을 사용하는 것이다.

---

## Q2. Static Batching과 Dynamic Batching의 차이를 설명해주세요.

### 답변

Static Batching은 움직이지 않는 오브젝트를 미리 묶어 Draw Call을 줄이는 방식이다. Dynamic Batching은 움직이는 오브젝트를 런타임에서 묶는 방식이며, Vertex 수 등의 제한이 있다.

---

## Q3. GPU Instancing이란 무엇인가요?

- [GPU Instancing](./Advanced/GPU_Instancing.md)
- 동일 Mesh / Material
- Draw Call

### 답변

동일한 Mesh와 Material을 사용하는 여러 오브젝트를 GPU에서 한 번의 Draw Call로 처리하는 기술이다. CPU가 GPU에 전달하는 Rendering 명령을 줄여 성능을 향상시킨다.

---

## Q4. SRP Batcher란 무엇인가요?

- SetPass Call
- CPU Rendering

### 답변

동일한 Shader를 사용하는 Material을 효율적으로 처리하여 SetPass Call을 줄이고 CPU Rendering 비용을 감소시키는 기능이다. URP와 HDRP에서 사용할 수 있다.

---

## Q5. LOD(Level of Detail)란 무엇인가요?

- Mesh (Polygon)

### 답변

카메라와의 거리에 따라 Polygon 수가 적은 Mesh로 교체하여 Rendering 비용을 줄이는 기술이다. 가까운 오브젝트는 고품질 Mesh를 사용하고, 멀어질수록 단순한 Mesh를 사용한다.

### 꼬리질문

- LOD와 Mipmap의 차이는 무엇인가요?

---

## Q6. Frustum Culling과 Occlusion Culling의 차이를 설명해주세요.

### 답변

Frustum Culling은 카메라 시야 밖의 오브젝트를 Rendering하지 않는 기법이다. Occlusion Culling은 카메라 시야 안에 있더라도 다른 오브젝트에 가려진 오브젝트를 Rendering하지 않는 기법이다.

---

## Q7. Rendering 최적화를 진행한다면 가장 먼저 무엇을 확인하시겠습니까?

- Profiler
- Draw Call
- SetPass Call
- Polygon

### 답변

Unity Profiler를 통해 Draw Call, SetPass Call, Polygon 수를 확인한다. 이후 GPU Bound 여부를 판단하고 Batching, Culling, LOD 등의 최적화를 적용한다.

### 꼬리질문

- SetPass Call이 높다면 어떤 부분을 먼저 의심하시겠습니까?

---

## Q8. Oracle War 프로젝트에서 Rendering 최적화를 적용한다면 어떻게 구현하시겠습니까?

- [Texture Compression](./Advanced/Texture_Compression.md)

### 답변

Texture Atlas를 사용하여 Batching이 잘 이루어지도록 구성하고, Texture Compression을 적용하여 VRAM 사용량을 줄인다. 또한 정적인 오브젝트에는 Baked Lighting을 적용하고, Frustum Culling과 Occlusion Culling을 사용하여 불필요한 Rendering을 줄인다.

---

# Related Concepts

- [Scriptable Object](./Advanced/ScriptableObject.md)
- [Addressables, AssetBundle](./Advanced/Addressables_AssetBundle.md)
- [GPU Instancing Adavanced](./Advanced/GPU_Instancing.md)
- [Texture Compression](./Advanced/Texture_Compression.md)
