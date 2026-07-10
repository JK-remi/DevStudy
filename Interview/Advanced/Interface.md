# Interface

## Interface란?

"이 기능을 반드시 구현해야 한다."는 계약입니다.

```csharp
interface IDamageable
{
    void Damage(int value);
}
```

---

## abstract class와 차이

abstract

- 공통 기능
- 공통 변수

interface

- 기능 계약만 제공

---

## 언제 사용하는가?

부모 클래스와 관계없이 같은 기능을 보장해야 할 때 사용합니다.
