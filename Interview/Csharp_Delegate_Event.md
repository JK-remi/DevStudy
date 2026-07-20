# C# Delegate / Event

## 🎯 Goal
- Delegate 개념 이해
- Delegate 선언 및 사용
- Multicast Delegate 이해
- Action / Func / Predicate 차이 이해
- Event 개념 이해
- Delegate와 Event 차이 이해
- Reflection 활용 방식 이해
- Callback과 Delegate 관계 이해

---

# Part 1. Delegate

## Q1. Delegate란 무엇인가?

- Method Reference
- Reference Type
- 함수 저장 및 전달

### 답변
Delegate는 메소드를 참조하고 호출할 수 있는 **Reference Type**입니다. 함수를 변수처럼 저장하고 전달하기 위해 사용하는 객체입니다.

---

## Q2. Delegate를 사용하는 이유는 무엇인가?

- Loose Coupling
- Callback
- Runtime 결정

### 답변
실행할 메소드를 런타임에 결정하거나, 호출하는 객체와 호출되는 객체를 분리하기 위해 사용합니다.

---

## Q3. Delegate를 선언하고 사용하는 방법을 설명해 주세요.

### 답변
먼저 `delegate` 키워드로 메소드 시그니처를 선언합니다. 이후 Delegate 변수에 메소드를 등록하고 Delegate를 호출하면 등록된 메소드가 실행됩니다. 이를 통해 실행할 메소드를 런타임에 변경하거나 전달할 수 있습니다.

```csharp
public delegate void MessageDelegate();

void Hello()
{
    Debug.Log("Hello");
}

MessageDelegate message = Hello;

message();
```

---

## Q4. Delegate는 왜 "함수를 변수처럼 저장한다"고 하나요?

### 답변
Delegate 내부에는 메소드에 대한 참조(Method Reference)가 저장됩니다. 따라서 변수처럼 저장하거나 전달한 뒤 나중에 호출할 수 있습니다.

---

## Q5. Multicast Delegate란 무엇인가?

### 답변
하나의 Delegate에 여러 메소드를 등록하여 순서대로 호출하는 Delegate입니다.

```csharp
public delegate void Calc();

Calc calc = Plus;
calc += Minus;

calc();
```

> 반환형이 있는 Delegate도 Multicast가 가능하지만 마지막 메소드의 반환값만 사용할 수 있으므로 반환값이 필요한 경우에는 잘 사용하지 않습니다.

---

## Q6. Action, Func, Predicate의 차이를 설명해 주세요.

### 답변

- **Action**
  - 반환값이 없는 Delegate
  - `Action<int, float>` → `void Func(int, float)`

- **Func**
  - 반환값이 있는 Delegate
  - 마지막 Generic이 반환형
  - `Func<int, float, bool>` → `bool Func(int, float)`

- **Predicate**
  - bool을 반환하는 Delegate
  - `Predicate<int>` → `bool Func(int)`

---

## Q7. Delegate와 일반 메서드 호출의 차이는 무엇인가?

- Runtime
- 메소드 전달

### 답변
일반 메소드는 호출 대상을 컴파일 시 알고 있지만, Delegate는 메소드를 객체처럼 전달하여 런타임에 어떤 메소드를 실행할지 결정할 수 있습니다.

---

## Q8. Delegate를 Unity에서 사용한 예를 설명해 주세요.

### 답변
Unity에서는 Button.onClick, Sequence 완료 후 다음 동작을 연결하는 Callback 등에 사용할 수 있습니다.

---

## Q9. Lambda와 Delegate의 관계를 설명해 주세요.

- Lambda = Delegate 문법

### 답변
Lambda는 Delegate를 간결하게 작성하기 위한 문법입니다. 결국 Lambda도 Delegate에 저장되어 실행됩니다.

---

## Q10. Delegate 사용 시 주의할 점은 무엇인가?

### 답변
Delegate는 호출 흐름을 추적하기 어려울 수 있습니다. 또한 등록 해제를 하지 않거나 null 체크를 하지 않으면 예기치 않은 동작이 발생할 수 있습니다.

---

# Part 2. Event

## Q11. Event란 무엇인가?

- 캡슐화

### 답변
Event는 Delegate를 기반으로 만든 이벤트 처리 기능입니다. 외부에서는 등록과 해제만 가능하며 실제 호출은 선언한 클래스 내부에서만 가능합니다.

---

## Q12. Delegate와 Event의 차이는 무엇인가?

- 호출 제한

### 답변
Delegate는 외부에서도 직접 호출할 수 있지만 Event는 외부에서는 등록과 해제만 가능하며 호출은 선언한 클래스 내부에서만 가능합니다.

---

## Q13. 왜 event 키워드를 사용하는가?

### 답변
Delegate를 외부에서 마음대로 호출하지 못하도록 막기 위해 사용합니다. 이벤트 발생은 내부에서만 하고 외부는 구독만 가능하도록 캡슐화합니다.

---

## Q14. += 와 -= 는 왜 사용하는가?

### 답변
`+=`는 이벤트를 구독하기 위해 사용하고, `-=`는 더 이상 이벤트를 받을 필요가 없을 때 구독을 해제하기 위해 사용합니다.

---

## Q15. Event를 사용하면 어떤 장점이 있는가?

### 답변
이벤트를 구독하는 객체와 발생시키는 객체를 분리할 수 있습니다. 또한 외부에서 임의로 이벤트를 발생시키지 못하기 때문에 안정성이 높습니다.

---

## Q16. Unity에서 Event를 사용하는 예를 설명해 주세요.

### 답변
UI 버튼 클릭, 플레이어 HP 변경, 몬스터 사망, 퀘스트 완료, 씬 전환 등 여러 객체에 동시에 알릴 필요가 있는 경우 사용합니다.

---

## Q17. Observer Pattern과 Event의 관계를 설명해 주세요.

### 답변
Observer Pattern은 이벤트를 발생시키는 객체와 이를 구독하는 객체를 분리하는 디자인 패턴입니다. C# Event는 Observer Pattern을 쉽게 구현하기 위한 기능입니다.

---

## Q18. Event를 사용할 때 주의할 점은 무엇인가?

### 답변
구독 해제를 반드시 관리해야 합니다. Unity에서는 이벤트를 구독한 객체가 Destroy되더라도 해제하지 않으면 참조가 남아 예상치 못한 동작이 발생할 수 있습니다. 또한 이벤트 호출 여부를 추적하기 어려워 디버깅이 복잡해질 수 있습니다.

---

## Q19. UnityEvent와 C# Event의 차이는 무엇인가?

### 답변
UnityEvent는 Inspector에서 연결할 수 있도록 Unity가 제공하는 Event이며, 내부 목적은 C# Event와 동일하게 이벤트를 안전하게 관리하는 것입니다.

---

# Part 3. 심화

## Q20. Reflection과 Delegate는 언제 각각 선택하시겠습니까?

- Runtime
- Tool

### 답변
Reflection은 런타임에 클래스나 메소드 정보를 조사하거나 실행해야 하는 Tool이나 Editor에서 사용합니다.

Oracle War에서는 `GetMethods()`와 `GetParameters()`를 이용해 Sequence Editor에서 Animation Event를 자동 등록하기 위해 Reflection을 사용했습니다.

Delegate는 호출할 메소드의 시그니처가 이미 정해져 있고 Callback이나 Event처럼 런타임에 실행할 메소드만 바뀌는 경우 사용합니다.

---

## Q21. Oracle War의 Sequence System을 지금 다시 만든다면 어떻게 개선하시겠습니까?

### 답변
Sequence Editor에서는 Reflection으로 메소드를 자동 수집하고 파라미터 UI를 생성했습니다.

실행부는 Invoke를 사용했던 것으로 기억하지만 파라미터 처리 문제 때문에 switch 기반으로 구현했던 것으로 기억합니다.

현재 다시 만든다면

- `Dictionary<string, Action<AniEvent>>`를 사용하여 switch 제거
- `[SequenceAction]` Attribute를 사용하여 등록 가능한 메소드만 자동 관리

하는 방향으로 개선할 수 있을 것 같습니다.

---

## Q22. Callback과 Delegate의 관계를 설명해 주세요.

### 답변
Callback은 특정 작업이 끝난 뒤 나중에 호출되는 함수를 의미하는 개념입니다.

Delegate는 이러한 Callback을 구현하기 위한 C#의 함수 참조 타입입니다.

즉 **Callback은 개념**, **Delegate는 Callback을 저장하고 호출하기 위한 도구**입니다.
