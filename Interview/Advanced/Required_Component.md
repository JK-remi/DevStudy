# RequireComponent

## RequireComponent란?

`RequireComponent`는 특정 Component가 반드시 함께 존재해야 하는 경우 자동으로 추가해주는 Attribute입니다.

GameObject에 스크립트를 추가할 때 필요한 Component가 없다면 Unity가 자동으로 생성해줍니다.

---

## 왜 사용하는가?

필수 Component 누락으로 인한 NullReferenceException이나 런타임 오류를 방지하기 위해 사용합니다.

또한 개발자가 Prefab을 생성할 때 실수로 Component를 빼먹는 것을 예방할 수 있습니다.

---

## 기본 사용법

```csharp
[RequireComponent(typeof(Collider))]
public class Projectile : MonoBehaviour
{

}
```

Projectile을 GameObject에 추가하면 Collider가 없을 경우 자동으로 생성됩니다.

---

## 여러 개도 가능할까요?

가능합니다.

```csharp
[RequireComponent(typeof(Rigidbody))]
[RequireComponent(typeof(Collider))]
public class Projectile : MonoBehaviour
{

}
```

또는

```csharp
[RequireComponent(typeof(Rigidbody), typeof(Collider))]
public class Projectile : MonoBehaviour
{

}
```

---

## 언제 사용하는가?

필수 Component가 있어야 정상적으로 동작하는 경우 사용합니다.

대표적인 예

- Projectile → Collider
- CharacterController → CapsuleCollider
- Rigidbody를 반드시 사용하는 이동 객체

---

## Oracle War에서는?

Projectile이나 특정 AI Object처럼 반드시 Collider가 필요한 객체에서 사용했습니다.

Component를 추가할 때 자동으로 Collider가 생성되므로 Prefab 제작 과정에서 발생할 수 있는 실수를 줄일 수 있었습니다.

---

## 장점

- 필수 Component 누락 방지
- NullReferenceException 예방
- Prefab 제작 실수 감소
- 유지보수 향상

---

## 단점

- 이미 붙어있는 Component를 제거하지는 않습니다.
- 스크립트를 추가하는 순간에만 동작합니다.
- 런타임 중 제거된 Component는 다시 생성하지 않습니다.

---

## 면접 답변

### Q. RequireComponent는 언제 사용하시나요?

A.

필수 Component가 반드시 존재해야 하는 Script에서 사용합니다.

예를 들어 Projectile이 Collider를 반드시 필요로 한다면 RequireComponent를 사용하여 스크립트를 추가하는 순간 Collider를 자동 생성하도록 만들 수 있습니다.

이를 통해 Prefab 제작 과정에서 Component 누락으로 발생하는 오류를 예방할 수 있습니다.

---

## Related Concepts

- [Unity_Architecture](../Unity_Architecture.md)
