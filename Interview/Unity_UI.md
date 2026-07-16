# Unity UI

## 🎯 Goal

* Unity UI 시스템의 구조 이해
* Canvas와 Render Mode 이해
* RectTransform과 Anchor/Pivot 이해
* EventSystem과 UI 이벤트 처리 이해
* Layout 및 ScrollView 구조 이해
* Canvas Rebuild와 UI 최적화 이해
* Sprite Atlas와 Draw Call 최적화 이해
* UI Pooling 및 Inventory UI 설계 이해

---

# Canvas

## Q1. Canvas란 무엇인가?

- UI 최상위 부모
- UI 렌더링

### 답변

Canvas는 Unity UI가 표시되는 최상위 부모(Component)입니다.  
모든 UI는 Canvas 아래에서 렌더링되며, Canvas를 기준으로 화면에 출력됩니다.

---

## Q2. Canvas의 Render Mode 3가지를 설명해 보세요.

- Overlay
- Camera
- World Space

### 답변

### Screen Space - Overlay

- 화면(Viewport)에 직접 출력
- 가장 많이 사용하는 방식
- HUD / 메뉴 UI

### Screen Space - Camera

- 특정 Camera 기준 출력
- Camera 효과(Post Processing 등)의 영향을 받을 수 있음
- 카메라 연출이 필요한 UI

### World Space

- 씬 공간에 존재하는 UI
- NPC 이름 / 체력바 / 월드 오브젝트 UI

### 예상 꼬리 질문

- HUD란 무엇인가?
  - 플레이 중 항상 화면에 표시되는 UI
  - ex. HP/MP/EXP/미니맵…


---

## Q3. RectTransform과 Transform의 차이는?

### 답변

Transform은 월드 좌표를 기준으로 위치를 관리합니다.  
RectTransform은 Canvas(UI) 기준 위치를 관리하며, Anchor / Pivot / Width / Height 등 UI 전용 기능을 제공합니다.

---

## Q4. Anchor와 Pivot의 차이는?

### 답변

Anchor는 부모(Canvas) 기준으로 UI가 고정되는 기준입니다.
화면 해상도가 변경되어도 UI 위치를 유지하도록 사용합니다.    
Pivot은 UI 자신의 회전 및 크기 변경의 중심점입니다.

---

## Q5. Canvas Scaler는 왜 사용하는가?

- Resolution
- Responsive UI

### 답변

Canvas Scaler는 화면 해상도에 따라 UI 크기를 일정하게 유지하기 위한 Component입니다.

---

# Event System

## Q6. Graphic Raycaster의 역할은 무엇인가?

- UI Raycast
- Input

### 답변

Graphic Raycaster는 UI에서 마우스나 터치 입력을 감지하는 Component입니다.
Physics Raycast가 아닌 UI 영역만 검사합니다.

### 예상 꼬리 질문

- Physics Raycast와 차이는?

---

## Q7. EventSystem은 어떤 역할을 하는가?

- UI Input
- Event

### 답변

EventSystem은 Button Click / Drag / Drop / Pointer 등 UI 입력 이벤트를 관리하는 시스템입니다.  
Graphic Raycaster와 함께 동작합니다.

---

## Q8. UI Button 클릭 이벤트는 어떻게 처리하는가?

- Inspector
- AddListener

### 답변

### Inspector
Button > OnClick() > Object 등록 > Method 등록

### Code
```csharp
button.onClick.AddListener(OnClickButton);
button.onClick.RemoveListener(OnClickButton);
button.onClick.RemoveAllListeners();
```

매개변수가 필요한 경우 Lambda를 사용합니다.
```csharp
button.onClick.AddListener(() => OnClickItem(itemId));
```

### 예상 꼬리 질문

- Lambda 사용 시 RemoveListener가 어려운 이유는?  
[Lambda](Advanced/Lambda.md)

---

# Layout

## Q9. Layout Group은 언제 사용하는가?

### 답변

자식 UI를 자동으로 정렬하기 위해 사용합니다. 대표적으로
- Vertical Layout Group
- Horizontal Layout Group
- Grid Layout Group  
이 있습니다.

---

## Q10. Content Size Fitter란?

- Auto Resize

### 답변

Content의 크기를 자식 UI 크기에 맞게 자동으로 변경하는 Component입니다.

---

# Scroll View

## Q11. Scroll View는 어떤 구조로 되어 있는가?

- ScrollRect
- Viewport
- Content

### 답변

ScrollView는 ScrollRect / Viewport / Content로 구성됩니다.  
ScrollRect는 스크롤 입력을 처리하고,
Viewport는 실제 보이는 영역이며,
Content에는 Item들이 배치됩니다.  
Viewport에는 일반적으로 RectMask2D가 적용됩니다.

---

## Q12. Mask와 RectMask2D의 차이는?

- Stencil
- Performance

### 답변

Mask
- Stencil Buffer 사용
- Image 필요
- 복잡한 모양 가능

RectMask2D
- 사각형만 가능
- Image 필요 없음
- Stencil 미사용
- 성능 우수

### 예상 꼬리 질문

- ScrollView에서는 왜 RectMask2D를 많이 사용할까요?

---

# UI Optimization

## Q13. Canvas를 여러 개로 나누는 이유는 무엇인가?

- Canvas Rebuild
- Optimization

### 답변

Canvas는 내부 UI 하나만 변경되어도 Canvas 전체를 다시 계산(Rebuild)합니다.  
따라서 HUD / Popup / Inventory등을 각각 Canvas로 분리하여 변경된 Canvas만 Rebuild하도록 구성합니다.  
이렇게 하면 CPU 비용을 줄일 수 있습니다.

---

## Q14. Canvas Rebuild란 무엇인가?

- Layout Recalculate
- CPU Cost

### 답변

UI 위치, 크기, Layout 등이 변경될 때 Canvas 전체를 다시 계산하는 작업입니다.  
CPU 비용이 큰 작업이므로 최소화하는 것이 좋습니다.

### 예상 꼬리 질문

- Rebuild를 많이 발생시키는 Component는?

---

## Q15. Draw Call이란 무엇이며 UI에서는 어떻게 줄이는가?

- GPU  
- Sprite Atlas  

### 답변

Draw Call은 GPU에게 렌더링을 요청하는 횟수입니다.  
UI에서는 Sprite Atlas / Material 공유 등으로 Draw Call을 줄일 수 있습니다.

---

## Q16. Sprite Atlas를 사용하는 이유는 무엇인가?

### 답변

여러 Sprite를 하나의 Texture로 묶는 기능입니다.  
같은 Atlas를 사용하면 Texture 변경이 줄어 Draw Call이 감소합니다.

---

## Q17. Inventory UI를 구현한다면 어떻게 구성하시겠습니까?

### 답변

ScrollView를 사용하여 Content에 Item을 배치합니다.
Item은 Object Pool을 사용하여 재사용하고, 클릭 시 Tooltip, Drag & Drop으로 장비창과 연동합니다.  
Item 수가 많다면 Virtualization을 적용하여 화면에 보이는 Item만 생성합니다.

### 예상 꼬리 질문

- Item이 수천 개라면?

---

## Q18. UI Pooling이 필요한 이유는 무엇인가?

- Object Pool
- GC

### 답변

UI는 반복 생성되는 경우가 많기 때문에 Object Pool을 사용합니다.
이를 통해 Instantiate 감소 / Destroy 감소 / GC 감소 효과를 얻을 수 있습니다.  
Oracle War에서도 UI Manager가 Stack 형태로 UI를 관리하여 재사용했습니다.

---

## Q19. TextMeshPro를 사용하는 이유는 무엇인가?

- SDF

### 답변

TextMeshPro는 Signed Distance Field(SDF)를 사용합니다.

따라서 확대해도 글자가 깨지지 않고 / Outline / Shadow / RichText 등을 효율적으로 사용할 수 있습니다.  

### 예상 꼬리 질문

- SDF란?
  - 글자의 윤곽선까지의 거리를 저장한 데이터.
  - 확대/축소 시 글자 깨지지 않고 선명하게 표현.


---

# Oracle War

## Q20. Oracle War에서 UI를 구현하며 고려했던 점이나 개선하고 싶은 점이 있다면?

- UX
- Async
- Duplicate Request

### 답변

UI Designer가 제작한 Prefab에 기능을 연결하는 작업을 주로 담당했습니다.  
대표적인 사례는 로그인 UI였습니다.  
버튼을 연타하면 서버에 Request가 여러 번 전송되는 문제가 있었기 때문에 Flag 사용 / Loading Panel 표시를 통해 응답이 올 때까지 입력을 막았습니다.  
일반적으로 UI를 구현할 때는 중복 입력 방지 / 비동기 처리 / Object Pool / Canvas 분리 / Virtualization 등을 함께 고려해야 한다고 생각합니다.

---

# UI 최적화 체크리스트

- Canvas 분리
- Sprite Atlas
- Object Pool
- Virtualization
- RectMask2D 사용
- Layout Group 최소화
- Content Size Fitter 최소화
- Canvas Rebuild 최소화

---

# Related Concepts

- [Lambda](Advanced/Lambda.md)  
