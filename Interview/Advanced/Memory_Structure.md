# Memory Layout

프로그램이 실행되면 메모리는 크게 다음과 같이 구성됩니다.

```text
┌────────────────────────────┐
│ Code Area                  │
│ - 실행 코드                │
│ - Method                   │
└────────────────────────────┘

┌────────────────────────────┐
│ Static Area                │
│ - static 변수              │
│ - static field             │
└────────────────────────────┘

┌────────────────────────────┐
│ Heap                       │
│ - new로 생성한 객체(Object) │
│ - class                    │
│ - Array                    │
│ - List                     │
│ - StringBuilder            │
└────────────────────────────┘

┌────────────────────────────┐
│ Stack                      │
│ - 지역 변수(Local Variable) │
│ - 매개변수(Parameter)       │
│ - 함수 호출 정보(Call Stack)│
│ - Value Type               │
└────────────────────────────┘
```

---

## Code Area

프로그램의 실행 코드(Method)가 저장되는 영역입니다.

프로그램이 종료될 때까지 유지됩니다.

예)

- Method
- 실행 코드

---

## Static Area

static 변수와 static field가 저장되는 영역입니다.

프로그램이 종료될 때까지 유지됩니다.

예)

```csharp
public static int Count;
```

---

## Heap

new를 통해 생성한 객체(Object)가 저장되는 영역입니다.

대표적으로

- class
- Array
- List
- Dictionary
- StringBuilder

등이 저장됩니다.

Heap 객체는 더 이상 참조되지 않을 때 Garbage Collector가 회수합니다.

---

## Stack

메서드 호출 시 생성되는 지역 변수와 함수 호출 정보가 저장되는 영역입니다.

대표적으로

- 지역 변수(Local Variable)
- Parameter
- Value Type
- 함수 호출 정보(Call Stack)

등이 저장됩니다.

메서드가 종료되면 자동으로 제거됩니다.

---

## 📖 Stack이 Heap보다 빠른 이유

Stack = LIFO구조 사용.  
메모리를 순차적으로 사용
- 메모리 탐색 X
- 단순 Pointer 이동
만 수행하면 된다.

---

Heap = 빈 공간 탐색 > 메모리 할당 > GC 관리

---

## 📖 StringBuilder

string은 수정이 불가능한 객체이다. (수정 시 기존 객체 삭제 후 새 객체 할당)  

StringBuilder는 **같은 버퍼**를 계속 수정한다.  
새로운 객체를 계속 생성하지 않으므로 GC를 크게 줄일 수 있다.

---

## 💡 실무 포인트

게임에서는 **GC를 완전히 없애는 것**이 아니라 **게임 플레이 중 GC가 발생하지 않도록 만드는 것**이 중요하다.
그래서  
- Loading
- Scene 전환
- Lobby  
등 플레이에 영향이 적은 시점에서 GC가 발생하도록 설계하는 경우가 많다.
