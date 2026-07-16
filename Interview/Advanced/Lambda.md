# Lambda & Capture

## Lambda(Expression)

Lambda는 **익명 함수(Anonymous Function)** 를 만드는 문법입니다.

함수를 따로 만들지 않고 필요한 위치에서 바로 작성할 수 있으며, Delegate나 Event에 전달하는 용도로 자주 사용합니다.

특히 **매개변수가 필요한 함수를 AddListener에 등록할 때** 많이 사용합니다.

### 예시

```csharp
button.onClick.AddListener(() => OnClickItem(itemId));
```

---

## Capture

Lambda는 외부 변수를 **복사하는 것이 아니라 참조(Capture)** 합니다.

즉, Lambda 내부에서는 외부 변수 자체를 참조하여 사용합니다.

---

### Capture 주의점

for문의 반복 변수는 하나의 변수를 계속 변경하면서 사용합니다.

Lambda는 이 변수를 Capture하기 때문에 모든 Lambda가 마지막 값만 참조하는 문제가 발생할 수 있습니다.

### 잘못된 예

```csharp
for (int i = 0; i < 3; i++)
{
    button.onClick.AddListener(() => Debug.Log(i));
}
```

출력

```
3
3
3
```

---

### 올바른 예

```csharp
for (int i = 0; i < 3; i++)
{
    int index = i;

    button.onClick.AddListener(() => Debug.Log(index));
}
```

출력

```
0
1
2
```

반복마다 새로운 지역 변수(index)를 생성했기 때문에 각각의 값을 정상적으로 Capture합니다.

---

## RemoveListener와 Lambda

일반 함수는 등록한 메서드를 그대로 제거할 수 있습니다.

```csharp
button.onClick.AddListener(OnClickButton);
button.onClick.RemoveListener(OnClickButton);
```

하지만 Lambda는 **익명 함수**이므로 동일한 Delegate 인스턴스를 다시 만들 수 없습니다.

```csharp
button.onClick.AddListener(() => Attack(id));
```

이렇게 등록한 Lambda는

```csharp
button.onClick.RemoveListener(() => Attack(id)); // 제거되지 않음
```

처럼 제거할 수 없습니다.

RemoveListener는 **등록했던 동일한 Delegate 인스턴스**가 필요하기 때문입니다.

---

## Object Pool에서의 사용

Object Pool에서는 UI가 재사용되므로 기존 Listener를 제거해야 합니다.

Lambda를 사용한 경우 개별 RemoveListener가 어렵기 때문에 일반적으로

```csharp
button.onClick.RemoveAllListeners();
```

를 호출한 뒤 필요한 Listener를 다시 등록하는 방식을 사용합니다.

---

## 핵심 정리

- Lambda는 익명 함수를 만드는 문법이다.
- Delegate나 Event 등록 시 자주 사용한다.
- 매개변수가 필요한 AddListener에서 많이 사용한다.
- Lambda는 외부 변수를 복사하지 않고 Capture(참조)한다.
- for문에서는 새로운 지역 변수를 만들어 Capture 문제를 방지한다.
- Lambda는 RemoveListener가 어렵기 때문에 Object Pool에서는 RemoveAllListeners()를 사용하는 경우가 많다.
