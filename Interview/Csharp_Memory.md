# C# Memory

## 🎯 Goal

* Stack과 Heap 메모리 구조 이해
* Value Type과 Reference Type 차이 이해
* struct와 class 차이 이해
* Boxing / Unboxing 이해
* Garbage Collection(GC) 이해
* GC 최적화 방법 이해

---

# Memory

## Q1. Stack과 Heap의 차이는?

- Stack
- Heap
- Local Variable
- Object

### 답변

Stack은 함수 호출 시 생성되는 지역 변수와 함수 호출 정보를 저장하는 메모리 영역입니다.
메서드가 종료되면 자동으로 제거되며 접근 속도가 매우 빠릅니다.

Heap은 `new`로 생성한 객체(Object)가 저장되는 메모리 영역입니다.
객체는 함수가 종료되어도 바로 제거되지 않으며, 더 이상 참조되지 않을 때 Garbage Collector가 회수합니다.

### 꼬리 질문

> 왜 Stack이 Heap보다 접근 속도가 빠를까요?

---

## Q2. Value Type과 Reference Type의 차이는?

- Value Copy
- Reference Copy
- struct
- class

### 답변

Value Type은 값을 복사합니다.
따라서 복사한 값을 변경해도 원본에는 영향을 주지 않습니다.

Reference Type은 객체의 주소(Reference)를 복사합니다.
같은 객체를 참조하기 때문에 한쪽에서 객체를 변경하면 다른 쪽에서도 동일한 결과를 확인할 수 있습니다.

대표적으로 struct는 Value Type이고 class는 Reference Type입니다.

### 꼬리 질문

```csharp
Player a = new Player();
Player b = a;

b.hp = 100;
```

> a.hp는 어떻게 될까요?

---

## Q3. struct와 class의 차이는?

- Value Type
- Reference Type
- Inheritance
- Interface

### 답변

가장 큰 차이는 Value Type과 Reference Type입니다.

struct는 Value Type이기 때문에 값을 복사하며,
class는 Reference Type이기 때문에 객체의 주소를 복사합니다.

또한 struct는 상속이 불가능하지만 인터페이스 구현은 가능합니다.
반면 class는 상속과 다형성을 지원하므로 대부분의 게임 객체를 구현할 때 사용합니다.

### 꼬리 질문

> Unity에서 Vector3는 왜 struct로 구현되어 있을까요?

---

## Q4. struct를 사용하는 경우는?

- Small Data
- GC
- Value Type

### 답변

struct는 크기가 작고 자주 생성되는 데이터를 표현할 때 사용합니다.

대표적으로

- Vector2
- Vector3
- Quaternion
- Color
- Rect

등이 있습니다.

Value Type이므로 GC 부담이 적고 복사 비용이 작은 데이터에 적합합니다.

---

## Q5. Boxing과 Unboxing이란?

- Value Type
- Reference Type
- Heap Allocation
- Casting

### 답변

Boxing은 Value Type을 object와 같은 Reference Type으로 변환하는 과정입니다.

예를 들어

```csharp
int value = 10;

object obj = value;
```

이 과정에서 Heap에 새로운 object가 생성됩니다.

Unboxing은 반대로 object 안에 저장된 Value Type을 다시 꺼내오는 과정입니다.

```csharp
object obj = 10;

int value = (int)obj;
```

Unboxing은 반드시 명시적인 캐스팅이 필요합니다.

### 꼬리 질문

```csharp
object obj = 10;

float value = (float)obj;
```

> 어떻게 될까요?

---

## Q6. Boxing이 성능에 좋지 않은 이유는?

- Heap Allocation
- Garbage Collection
- Performance

### 답변

Boxing은 Heap에 새로운 객체를 생성하기 때문에 Heap Allocation이 발생합니다.

Heap Allocation이 많아질수록 Garbage Collection이 자주 실행되고, 게임에서는 Hitching이나 Frame Drop이 발생할 수 있습니다.

따라서 반복적으로 호출되는 코드에서는 Boxing을 피하는 것이 좋습니다.

---

## Q7. Garbage Collection(GC)이란?

- Heap
- Garbage Collector
- Memory Management

### 답변

Garbage Collection은 Heap에 생성된 객체 중 더 이상 참조되지 않는 객체를 자동으로 회수하는 메모리 관리 기능입니다.

개발자가 직접 메모리를 해제하지 않아도 되므로 편리하지만, 실행 시 성능 저하가 발생할 수 있습니다.

### 꼬리 질문

> GC는 개발자가 직접 실행할 수도 있을까요?

---

## Q8. GC가 발생하면 어떤 문제가 생기는가?

- Stop The World
- Hitching
- Frame Drop

### 답변

GC가 실행되는 동안에는 관리 대상 객체를 검사하기 위해 프로그램이 잠시 멈추는 Stop The World가 발생합니다.

게임에서는 이로 인해 Hitching과 Frame Drop이 발생하여 순간적으로 화면이 끊기는 현상이 나타날 수 있습니다.

### 꼬리 질문

> 게임에서 GC를 특히 조심해야 하는 이유는 무엇일까요?

---

## Q9. GC를 줄이는 방법은?

- Object Pool
- StringBuilder
- List Reuse
- RaycastNonAlloc
- Heap Allocation

### 답변

GC를 줄이기 위해서는 Heap Allocation을 최소화해야 합니다.

대표적인 방법은

- Object Pool
- StringBuilder
- List 재사용
- RaycastNonAlloc
- Update에서 반복적인 Heap Allocation 최소화

등이 있습니다.

### 꼬리 질문

> Object Pool이 GC를 줄이는 이유는 무엇인가요?

---

## Q10. Oracle War에서 GC를 줄이기 위해 했던 작업이 있는가?

- Object Pool
- Reuse

### 답변

Oracle War에서는 Object Pool을 사용하여 자주 생성되는 객체를 재사용했습니다.

매번 생성과 삭제를 반복하지 않고 미리 생성해 둔 객체를 활성화/비활성화하는 방식으로 Heap Allocation을 줄였고, 그 결과 Garbage Collection 발생 빈도도 줄일 수 있었습니다.

### 꼬리 질문

> Object Pool은 어떤 자료구조(Stack, Queue 등)로 구현하셨나요?

---

# Related Concepts

- [Memory Structure](Advanced/Memory_Structure.md)
