# C# Generic / Collection

## 🎯 Goal

- Generic의 개념과 사용 목적을 이해한다.
- Generic Method와 Generic Class의 차이를 이해한다.
- where 제약조건을 이해한다.
- Collection 종류와 특징을 이해한다.
- 상황에 맞는 Collection을 선택할 수 있다.
- Unity 실무에서 Generic과 Collection을 활용하는 방법을 이해한다.

---

# 1. Generic

## Q1. Generic이란 무엇인가요?

- Type Parameter

### 답변

자료형을 미리 정하지 않고 사용할 수 있도록 하는 문법입니다.  

컴파일 시 실제 사용할 타입이 결정되므로 하나의 코드로 여러 타입을 안전하게 처리할 수 있습니다.

---

## Q2. Generic을 사용하는 이유는 무엇인가요?

- 코드 재사용
- Type Safety
- Boxing / Unboxing 제거

### 답변

같은 로직을 여러 자료형에서 재사용하여 코드 중복을 줄일 수 있습니다.  

또한 object를 사용하지 않으므로 Boxing / Unboxing이 발생하지 않아 성능과 타입 안정성을 확보할 수 있습니다.

---

## Q3. Generic Method란 무엇인가요?

### 답변

Generic Type을 매개변수나 반환형으로 사용하는 메소드입니다.  

메소드를 호출할 때 실제 타입이 결정됩니다.

---

## Q4. Generic Class란 무엇인가요?

### 답변

클래스 자체가 Generic Type을 사용하는 클래스입니다.  

멤버 변수와 메소드에서 동일한 Generic Type을 사용할 수 있습니다.

---

## Q5. where 제약조건은 무엇인가요?

- Constraint

### 답변

where는 Generic Type에 제약조건을 부여하는 문법입니다.  

특정 타입만 Generic으로 사용할 수 있도록 제한합니다.  

### 꼬리 질문

- where T : MonoBehaviour / where T : new()는 각각 어떤 의미인가요?

---

## Q6. 왜 Generic은 Boxing / Unboxing이 발생하지 않나요?

- object

### 답변

Generic은 컴파일 시 실제 타입이 결정되므로 object로 변환할 필요가 없습니다.  

따라서 Boxing / Unboxing이 발생하지 않습니다.
- [Boxing](Interview/Csharp_Memory.md#Q5.-Boxing과-Unboxing이란?)  

---

## Q7. List<T>도 Generic인가요?

### 답변

네.

List<T>는 대표적인 Generic Collection이며 원하는 타입을 지정하여 사용할 수 있습니다.

---

## Q8. Generic Interface란 무엇인가요?

### 답변

Interface도 Generic Type을 받을 수 있습니다.

이를 통해 구현 클래스가 사용할 타입을 나중에 결정할 수 있어 코드 재사용성이 높아집니다.

### 꼬리 질문

- IComparable<T>를 본 적이 있나요?

---

## Q9. Unity에서 Generic Class를 사용할 만한 예를 설명해 주세요.

- Object Pool
- Singleton
- Factory

### 답변

Unity에서는 동일한 로직을 여러 타입에서 재사용해야 하는 시스템에 많이 사용합니다.  

대표적으로 ObjectPool<T>를 구현하면 Bullet, Effect, UI 등을 하나의 코드로 관리할 수 있습니다.  

또한 Singleton<T>, Factory<T>처럼 공통 기능을 제공하는 시스템에서도 Generic을 활용하여 코드 중복을 줄일 수 있습니다.  

---

## Q10. 언제 Generic을 사용하지 않는 것이 좋을까요?

- 로직 차이
- 상속 / Interface

### 답변

타입마다 동작 방식이 다른 경우에는 Generic보다 상속이나 Interface를 사용하는 것이 적절합니다.  

Generic은 같은 로직을 여러 타입에서 사용할 때 가장 효과적입니다.

---

# 2. Collection

## Q11. Collection이란 무엇인가요?

- 자료 구조
- 데이터 관리

### 답변

Collection은 여러 개의 데이터를 저장하고 관리하기 위한 자료구조입니다.

---

## Q12. Array와 Collection의 차이는 무엇인가요?

- 삽입 / 삭제 / 검색 등

### 답변

Array는 크기가 고정된 기본 배열입니다.

Collection은 삽입, 삭제, 검색 등 데이터를 효율적으로 관리하기 위한 자료구조입니다.

---

# Array

## Q13. Array의 특징을 설명해 주세요.

- 연속 메모리
- 고정 크기

### 답변

연속된 메모리에 저장되며 Index 접근 속도가 빠릅니다.  

생성 후 크기를 변경할 수 없습니다.

---

## Q14. Array의 장단점을 설명해 주세요.

- 접근 속도
- 고정 크기

### 답변

Index 접근이 매우 빠르다는 장점이 있습니다.  

반면 크기를 변경할 수 없어 새로운 배열을 생성하고 복사해야 하는 단점이 있습니다.

---

## Q15. 언제 Array를 사용하는 것이 좋을까요?

### 답변

데이터 개수가 처음부터 정해져 있고 실행 중 변경되지 않는 경우 사용하는 것이 좋습니다.

---

# List

## Q16. List의 특징을 설명해 주세요.

- 동적 배열
- Capacity / Count

### 답변

동적으로 크기를 변경할 수 있으며 Index 접근이 빠른 Collection입니다.

---

## Q17. Array와 List의 차이를 설명해 주세요?

- 고정 / 동적 크기

### 답변

Array는 크기가 고정되어 있습니다.  

List는 실행 중에도 크기를 변경할 수 있으며 Add, Remove 등의 기능을 제공합니다.

---

## Q18. List의 Capacity란 무엇인가요?

### 답변

Capacity는 현재 확보한 메모리 크기이며,

Count는 실제 저장된 데이터 개수입니다.  

Count가 Capacity를 초과하면 Resize가 발생합니다.

---

## Q19. List에서 Add()의 시간복잡도는 어떻게 되나요?

- O(1)
- Resize

### 답변

평균적으로 O(1)입니다.  

하지만 Resize가 발생하면 데이터를 복사해야 하므로 O(N)이 됩니다.

### 꼬리 질문

- Capacity를 미리 지정하는 이유는 무엇인가요?

---

## Q20. List에서 중간 삽입 / 삭제가 느린 이유는 무엇인가요?

- Shift

### 답변

뒤에 있는 모든 데이터를 한 칸씩 이동해야 하기 때문입니다.

---

# Dictionary

## Q21. Dictionary란 무엇인가요?

- Key / Value
- Hash Table

### 답변

Key와 Value를 한 쌍으로 저장하며 Key를 이용해 빠르게 검색하기 위한 Collection입니다.

---

## Q22. Dictionary의 Key는 왜 중복될 수 없나요?

### 답변

하나의 Key가 하나의 데이터를 식별해야 하기 때문입니다.  

동일한 Key는 하나만 존재할 수 있습니다.

---

## Q23. Dictionary는 내부적으로 어떻게 데이터를 저장하나요?

- Hash Table
- Bucket

### 답변

Key를 Hash 함수로 변환하여 Bucket 위치를 계산한 뒤 데이터를 저장합니다.

---

## Q24. Dictionary의 검색 시간복잡도는 얼마인가요?

- O(1)
- Hash Collision

### 답변

평균적으로 O(1)입니다.  

다만 Hash Collision이 많아지면 최악의 경우 O(N)이 될 수 있습니다.

### 꼬리 질문

- Hash Collision이란 무엇인가요?
- Hash Collision은 어떻게 해결하나요?

---

## Q25. TryGetValue()를 사용하는 이유는 무엇인가요?

- Exception

### 답변

Key 존재 여부를 확인하면서 동시에 Value를 가져올 수 있습니다.  

예외가 발생하지 않고 한 번의 검색으로 처리할 수 있습니다.

### 꼬리 질문

- TryGetValue()와 dict[key]의 차이는 무엇인가요?

---

## Q26. ContainsKey()와 TryGetValue()의 차이를 설명해 주세요.

### 답변

ContainsKey()는 존재 여부만 확인합니다.  

TryGetValue()는 존재 여부를 확인하면서 Value도 함께 반환하여 한 번의 검색으로 처리할 수 있습니다.

---

# Queue

## Q27. Queue의 특징을 설명해 주세요.

### 답변

먼저 들어온 데이터가 먼저 나가는 FIFO 구조입니다.

---

## Q28. Queue를 사용하는 대표적인 예를 설명해 주세요.

- Object Pool
- 작업 대기열

### 답변

Object Pool, 작업 대기열, 네트워크 패킷 처리 등 순차적으로 처리해야 하는 경우 사용합니다.

---

# Stack

## Q29. Stack의 특징과 사용 예를 설명해 주세요.

- Popup / Undo

### 답변

Stack은 가장 마지막에 들어온 데이터가 가장 먼저 나가는 LIFO 구조입니다.  

최근에 열린 UI부터 닫는 Popup 관리나 Undo 기능에서 자주 사용됩니다.

---

# HashSet

## Q30. HashSet의 특징을 설명해 주세요.

- 중복 제거
- Hash

### 답변

HashSet은 중복을 허용하지 않는 Collection입니다.  

Hash를 이용하기 때문에 평균 O(1)의 빠른 검색이 가능합니다.

### 꼬리 질문

- HashSet은 왜 검색이 빠른가요?
- HashSet은 왜 순서를 보장하지 않나요?

---

# Oracle War

## Oracle War 프로젝트에서 Collection을 사용한 사례를 설명해 주세요.

### 답변

Object Pool에서는 Queue를 사용하여 비활성 객체를 관리했습니다.  

UI Popup은 최근 열린 창부터 닫는 구조이므로 Stack이 적합합니다.  

Animation Sequence에서는 Event를 순차적으로 관리하기 위해 List를 사용했습니다.  
