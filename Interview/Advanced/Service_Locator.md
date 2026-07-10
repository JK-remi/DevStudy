# Service Locator

## Service Locator란?

Service Locator는 필요한 객체를 직접 생성하거나 Singleton에 직접 접근하지 않고, 중앙 관리 객체를 통해 필요한 서비스를 요청하는 구조입니다.

Singleton과 DI(Dependency Injection)의 중간 형태로 생각하면 이해하기 쉽습니다.

---

## 왜 사용하는가?

Singleton: Manager를 직접 알고 있어야 합니다.

Service Locator: 구현체를 몰라도 됩니다.
```csharp
ServiceLocator.Get<IAudioService>().Play("Attack");
```

---

## 장점

- Singleton보다 결합도가 낮다.
- Interface를 통해 의존성을 줄일 수 있다.
- 구현체 교체가 쉽다.

---

## 단점

- 결국 전역 객체(ServiceLocator)가 존재한다.
- 너무 많이 사용하면 또 다른 Singleton이 된다.

---

## OracleWar에서는?

OracleWar는 프로젝트 규모가 크지 않았고 Manager 수도 제한적이었기 때문에 Singleton 구조가 더 단순하고 효율적이었습니다.

프로젝트가 더 커지고 테스트 코드가 필요한 환경이라면 Service Locator 또는 DI를 고려했을 것입니다.
