# Dynamic Binding (동적 바인딩)

## 핵심 키워드

- virtual
- override
- Runtime
- 실제 객체

---

## Dynamic Binding이란?

부모 타입으로 참조하더라도 **실제 객체의 메서드를 런타임에 결정하여 호출하는 것**입니다.  

즉
```text
컴파일

↓

부모 타입만 확인

↓

실행(Runtime)

↓

실제 객체 확인

↓

override 호출
```

---

## 예제

```csharp
class Character
{
    public virtual void Attack()
    {
        Debug.Log("Character");
    }
}

class Player : Character
{
    public override void Attack()
    {
        Debug.Log("Player");
    }
}
```

```csharp
Character c = new Player();

c.Attack();
```

출력

```text
Player
```

부모 타입인데 Player가 실행됩니다.  

---

## 왜?

컴파일 시
```text
Character
```
이지만  

실행 시
```text
Player
```
객체라는 것을 알기 때문입니다.

---

## Static Binding과 차이

### Static Binding

컴파일 시 결정
```text
컴파일

↓

호출 대상 결정
```

예)

```text
private

static

sealed
```

---

### Dynamic Binding

런타임 결정
```text
실행

↓

실제 객체 확인

↓

override 호출
```

---

## Dynamic Binding이 필요한 이유

다형성 구현
예)
```csharp
List<Character>

↓

Player

Monster

Boss
```

```csharp
foreach(Character c in characters)
{
    c.Attack();
}
```

각 객체마다 다른 Attack이 실행됩니다.

---

## override와 new 차이

override
```text
Dynamic Binding

↓

자식 호출
```

---

new
```text
부모 메서드 숨김

↓

부모 타입으로 호출하면 부모 실행
```

---

## 면접 답변

> Dynamic Binding은 부모 타입으로 객체를 참조하더라도 런타임에 실제 객체를 확인하여 override된 메서드를 호출하는 방식입니다.
>
> 이를 통해 하나의 부모 타입으로 여러 자식 객체를 동일하게 관리하면서도 객체별 다른 동작을 수행할 수 있습니다.
