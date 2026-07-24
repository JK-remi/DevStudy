# GoF & Interface 기반 Observer Pattern

---

# GoF (Gang of Four)

## GoF란?

GoF(Gang of Four)는 객체지향 설계에서 자주 사용하는 Design Pattern을 체계적으로 정리한 4명의 저자를 의미한다.

- Erich Gamma
- Richard Helm
- Ralph Johnson
- John Vlissides

이들이 1994년에 출판한

> Design Patterns: Elements of Reusable Object-Oriented Software

에서 **23개의 Design Pattern**을 정의하였다.

현재 우리가 공부하는 대부분의 Design Pattern은 GoF Design Pattern을 의미한다.

---

## GoF Design Pattern 분류

### Creational (생성)

객체 생성 방법에 관한 패턴

- Singleton
- Factory Method
- Abstract Factory
- Builder
- Prototype

---

### Structural (구조)

객체 구조에 관한 패턴

- Adapter
- Decorator
- Facade
- Composite
- Proxy

---

### Behavioral (행동)

객체 간 상호작용에 관한 패턴

- Observer
- Command
- State
- Strategy
- Iterator
- Mediator
- Visitor
- Template Method

---

# GoF Observer Pattern

## 개념

GoF에서 정의한 Observer Pattern은

> **한 객체(Publisher)의 상태가 변경되면 이를 구독하고 있는 여러 객체(Subscriber)에게 자동으로 알리는 행동 패턴이다.**

C#의 Event는 Observer Pattern을 구현하는 방법 중 하나일 뿐이며,
GoF에서는 Interface 기반으로 구현한다.

---

# Interface 기반 Observer Pattern

## 구조

```text
                IObserver
             +-------------+
             | Notify()    |
             +-------------+
                    ▲
          ┌─────────┴─────────┐
          │                   │
     UIObserver         SoundObserver

                Register()

                Remove()

          +--------------------+
          |     Publisher      |
          |--------------------|
          | List<IObserver>    |
          | Register()         |
          | Remove()           |
          | Notify()           |
          +--------------------+
```

---

## 동작 과정

1. Subscriber가 Publisher에 Register()
2. Publisher가 Observer를 List로 관리
3. 상태 변화 발생
4. Publisher.Notify()
5. 등록된 모든 Observer의 Notify() 호출

---

## 예시 코드

### Observer Interface

```csharp
public interface IObserver
{
    void Notify();
}
```

---

### Publisher

```csharp
public class Publisher
{
    private readonly List<IObserver> observers = new();

    public void Register(IObserver observer)
    {
        observers.Add(observer);
    }

    public void Remove(IObserver observer)
    {
        observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (var observer in observers)
        {
            observer.Notify();
        }
    }
}
```

---

### Subscriber

```csharp
public class UIObserver : IObserver
{
    public void Notify()
    {
        Debug.Log("UI Update");
    }
}
```

---

### 사용 예시

```csharp
Publisher publisher = new Publisher();

publisher.Register(new UIObserver());
publisher.Register(new SoundObserver());

publisher.Notify();
```

---

# Interface Observer vs C# Event

| Interface 방식 | Event 방식 |
|---------------|-----------|
| Register() | += |
| Remove() | -= |
| Notify() | Invoke() |
| List<IObserver> 직접 관리 | Event가 내부적으로 Delegate 관리 |

---

# 면접 포인트

## Q. Observer Pattern과 Event는 같은 개념인가요?

**추천 답변**

> 아닙니다. Observer Pattern은 객체 간 상태 변경을 알리는 설계 패턴이고, Event는 C#에서 Observer Pattern을 구현하기 위한 문법입니다. GoF에서는 Interface 기반으로 Observer Pattern을 설명하지만, C#에서는 Delegate와 Event를 이용해 더 간단하게 구현하는 경우가 대부분입니다.

---

## 핵심 정리

- GoF = Design Pattern을 정리한 4명의 저자
- Observer Pattern = 행동(Behavioral) 패턴
- GoF 원본 구현 = Interface 기반
- Unity 실무 = C# Event / UnityEvent 기반 구현이 대부분
- Event는 Observer Pattern을 구현하는 방법 중 하나
