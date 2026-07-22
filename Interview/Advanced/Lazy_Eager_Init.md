# Lazy Initialization vs Eager Initialization

---

# 왜 필요한가?

Singleton은 프로그램에서 **객체를 하나만 생성**하여 사용하는 패턴입니다.

하지만 **언제 생성할 것인가?**에 따라 두 가지 방식으로 나뉩니다.

- Lazy Initialization (지연 초기화)
- Eager Initialization (즉시 초기화)

둘의 차이를 이해하는 것은 Singleton 면접에서 자주 나오는 질문입니다.

---

# 1. Lazy Initialization (지연 초기화)

## 정의

객체를 **실제로 처음 사용할 때 생성**하는 방식입니다.

즉, 프로그램이 시작될 때는 객체를 만들지 않고,
`Instance`가 처음 호출되는 순간 생성합니다.

---

## 구현

```csharp
public class GameManager
{
    private static GameManager instance;

    public static GameManager Instance
    {
        get
        {
            if (instance == null)
                instance = new GameManager();

            return instance;
        }
    }

    private GameManager() { }
}
```

### 동작 과정

```text
프로그램 시작

↓

Instance 없음

↓

처음 Instance 호출

↓

new GameManager()

↓

Instance 생성

↓

이후에는 같은 객체 반환
```

---

## 장점

### 1. 메모리 절약

사용하지 않는 객체는 생성하지 않습니다. 예를 들어
```text
DebugManager
```
가 실제 게임에서 한 번도 사용되지 않았다면
```text
메모리 사용 = 0
```
입니다.

---

### 2. 초기 로딩 속도 향상

프로그램 시작 시 생성하는 객체 수가 줄어듭니다.

게임 시작 시
```text
GameManager
SoundManager
ResourceManager
...
```
전부 생성하지 않아도 됩니다.

---

## 단점

### 1. 첫 접근 시 생성 비용 발생

처음 호출하는 순간
```text
메모리 할당

↓

생성자 호출

↓

초기화
```
가 일어나므로 순간적인 비용이 발생합니다.

---

### 2. Thread Safe 문제

Multi Thread 환경에서는
```text
Thread A

↓

instance == null

Thread B

↓

instance == null

↓

둘 다 new 수행
```
이 가능하므로 Singleton이 깨질 수 있습니다.

이를 해결하려면
- lock
- Lazy<T>
등을 사용해야 합니다.

---

## 사용하기 좋은 경우

- 사용 여부가 불확실한 객체
- 메모리를 아끼고 싶은 경우

예시

- SaveSystem
- NetworkManager
- DebugManager

---

# 2. Eager Initialization (즉시 초기화)

## 정의

프로그램이 시작될 때 객체를 **미리 생성**하는 방식입니다.  
Instance를 호출하기 전부터 이미 존재합니다.

---

## 구현

```csharp
public class GameManager
{
    private static readonly GameManager instance = new GameManager();

    public static GameManager Instance => instance;

    private GameManager() { }
}
```

---

## 동작 과정

```text
프로그램 시작

↓

new GameManager()

↓

Instance 생성 완료

↓

언제 호출해도 바로 반환
```

---

## 장점

### 1. 첫 접근이 빠름

이미 생성되어 있으므로
```text
Instance 호출

↓

즉시 반환
```
됩니다.

---

### 2. Thread Safe

CLR의 static 초기화 과정은 Thread Safe가 보장됩니다. 따라서 별도의 lock 없이도 안전합니다.

---

### 3. 구현이 단순

if문도 필요 없습니다.
```csharp
public static GameManager Instance => instance;
```
만으로 끝납니다.

---

## 단점

### 1. 사용하지 않아도 생성됨

예를 들어
```text
DebugManager
```
를 한 번도 사용하지 않아도 프로그램 시작 시 생성됩니다.

---

### 2. 메모리 선점

필요하지 않은 객체도 메모리를 차지합니다.

---

### 3. 초기 로딩 비용 증가

모든 Singleton을 미리 생성하면 게임 시작 시
```text
초기화 비용
```
이 증가할 수 있습니다.

---

## 사용하기 좋은 경우

항상 사용하는 객체

예)
- GameManager
- TimeManager
- SettingManager

---

# Lazy vs Eager 비교

| 항목 | Lazy | Eager |
|------|------|--------|
| 생성 시점 | 처음 사용할 때 | 프로그램 시작 시 |
| 메모리 | 필요할 때만 사용 | 처음부터 사용 |
| 초기 로딩 | 빠름 | 조금 느림 |
| 첫 접근 속도 | 생성 비용 발생 | 즉시 반환 |
| Thread Safe | 추가 처리 필요 | 기본적으로 안전 |
| 구현 난이도 | 조금 높음 | 단순 |

---

# Unity에서는?

Unity의 `MonoBehaviour`는
```csharp
new GameManager();
```
를 사용할 수 없습니다. 
따라서 일반적인 Lazy/Eager 방식 대신 Awake에서 Instance를 등록합니다.

```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }

    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
        }
        else
        {
            Destroy(gameObject);
        }
    }
}
```

즉, Unity에서는
```text
생성

↓

GameObject

↓

Awake

↓

Instance 등록
```
이라는 생명주기를 사용합니다.

---

# 면접 답변

## Q. Lazy Initialization과 Eager Initialization의 차이를 설명해 주세요.

> **Lazy Initialization은 객체를 처음 사용할 때 생성하는 방식으로 메모리를 절약할 수 있지만 첫 접근 시 생성 비용이 발생하며, Multi Thread 환경에서는 Thread Safe 처리가 필요합니다. Eager Initialization은 프로그램 시작 시 객체를 미리 생성하는 방식으로 첫 접근이 빠르고 Thread Safe하지만 사용하지 않는 객체도 생성되어 초기 메모리 사용량과 초기화 비용이 증가할 수 있습니다. Unity에서는 MonoBehaviour를 new로 생성할 수 없기 때문에 일반적으로 Awake에서 Singleton Instance를 등록하는 방식을 사용합니다.**

---

# 꼬리 질문

### Q. Lazy Initialization을 사용하는 이유는 무엇인가요?

---

### Q. Eager Initialization이 Thread Safe인 이유는 무엇인가요?

---

### Q. Unity에서는 왜 일반 C# Singleton처럼 구현하지 않나요?

---

### Q. Unity에서 Singleton 초기화 순서 문제는 어떻게 해결하시겠습니까?
