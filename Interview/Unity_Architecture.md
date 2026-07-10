# Unity Architecture

## 🎯 Goal

- Unity의 Component 기반 구조를 이해한다.
- MonoBehaviour와 일반 클래스의 차이를 설명할 수 있다.
- Singleton, ScriptableObject의 사용 목적을 설명할 수 있다.
- OOP(Interface, Abstract Class)를 Unity 구조와 연결하여 설명할 수 있다.

---

# Basic Questions

## Q1. Component란 무엇인가요?

### 답변

Unity에서 GameObject에 기능을 추가하는 단위입니다.

Transform, Collider, Rigidbody, Script 모두 Component이며 하나의 GameObject는 여러 Component를 조합하여 동작합니다.

### 꼬리 질문

- GameObject와 Component의 관계는?

---

## Q2. Component는 언제 생성되나요?

### 답변

- Prefab 또는 Scene에 배치되어 있을 경우 GameObject 생성과 함께 생성됩니다.
- AddComponent() 호출 시 런타임에 생성됩니다.

---

## Q3. Unity가 Component 구조를 사용하는 이유는 무엇인가요?

### 답변

상속보다 조합(Composition)을 사용하기 위해서입니다.

필요한 기능만 Component로 붙였다 뗄 수 있어 재사용성과 유지보수성이 높습니다.

---

## Q4. MonoBehaviour와 일반 클래스의 차이는 무엇인가요?

### 답변

MonoBehaviour는 Unity Lifecycle(Awake, Update 등)을 사용할 수 있으며 GameObject에 붙어서 동작합니다.

일반 클래스는 Unity 기능이 필요 없는 데이터나 Utility에서 사용합니다.

### 실무 경험

Oracle War에서는 CharacterStat, SkillData 등의 데이터 클래스는 일반 클래스로 관리했고, 게임 로직은 MonoBehaviour에서 처리했습니다.

---

## Q5. 일반 클래스는 언제 사용하시나요?

### 답변

Unity Lifecycle이 필요 없고 데이터를 저장하거나 Utility 성격을 가지는 경우 사용합니다.

대표적으로 DTO, Parser, SkillData, CharacterStat 등을 일반 클래스로 관리했습니다.

### 꼬리 질문

- DTO란?  
[DTO](Advanced/DTO.md)

---

## Q6. GetComponent는 왜 비용이 큰가요?

### 답변

GameObject 내부 Component를 순차적으로 탐색하기 때문입니다.

따라서 Update에서 반복 호출하지 않고 Awake 또는 Start에서 캐싱하는 것이 좋습니다.

### 꼬리 질문

- TryGetComponent와 차이는?

---

## Q7. Singleton은 어떻게 구현했나요?

### 답변

Oracle War에서는 Manager들을 Singleton으로 구현했습니다.

GameManager를 DontDestroyOnLoad로 유지했고 첫 Scene에만 배치했습니다.

필요한 Manager는 Instance를 통해 접근했습니다.

### 꼬리 질문

- Singleton의 장단점은?
- 대체 구조는?  
[Singleton](Advanced/Singleton.md)  
[DI](Advanced/DI.md)  
[Service_Locator](Advanced/Service_Locator.md)  
[Event](#delegate-/-event)  

---

## Q8. ScriptableObject는 언제 사용하나요?

### 답변

ScriptableObject는 데이터를 저장하는 Unity Asset입니다.

Oracle War는 서버 기반 MasterData를 사용했기 때문에 사용하지 않았습니다.

하지만 ItemData나 CharacterData처럼 로컬 데이터를 사용하는 프로젝트에서는 적극적으로 사용할 수 있다고 생각합니다.

### 꼬리 질문

- JSON과 차이는?  
[DTO](Advanced/DTO.md)

---

## Q9. TryGetComponent와 GetComponent의 차이는?

### 답변

GetComponent는 Component가 없으면 null을 반환합니다.

TryGetComponent는 bool을 반환하여 null 체크를 줄일 수 있습니다.

---

## Q10. AddComponent를 여러 번 호출하면 어떻게 되나요?

### 답변

같은 Component가 여러 개 붙을 수 있습니다.

중복 Update 호출이나 의도하지 않은 동작이 발생할 수 있으므로 보통 GetComponent로 존재 여부를 확인한 후 AddComponent를 사용했습니다.

---

## Q11. Destroy(GameObject)와 Destroy(Component)의 차이는?

### 답변

Destroy(GameObject)는 GameObject와 모든 Component를 삭제합니다.

Destroy(Component)는 해당 Component만 제거합니다.

---

## Q12. RequireComponent는 언제 사용하나요?

### 답변

필수 Component가 있어야 하는 Script에서 사용합니다.

Projectile이 반드시 Collider를 가져야 하는 경우처럼 Component를 자동 생성할 수 있습니다.  
  
[Required Component](Advanced/Required_Component.md)

---

## Q13. Interface는 언제 사용하나요?

### 답변

Interface는 기능에 대한 계약입니다.

부모 클래스가 달라도 같은 기능을 반드시 구현해야 할 때 사용합니다.

Oracle War에서는 공통 변수와 기능이 많았기 때문에 Interface보다 Abstract Base Class를 더 많이 사용했습니다.

---

## Q14. Abstract Class와 Interface의 차이는?

### 답변

Abstract Class는 공통 기능과 공통 데이터를 제공합니다.

Interface는 기능만 정의합니다.

Oracle War에서는 AIBase, AISkillBase, PanelBase처럼 공통 기능을 제공해야 했기 때문에 Abstract Base Class를 사용했습니다.

---

# Project Experience

## Q15. Oracle War에서 Unity Architecture를 고려하여 설계했던 경험이 있나요?

### 답변

Oracle War에서는 AI, Skill, UI 등을 각각 Abstract Base Class 기반으로 설계하여 공통 기능은 부모 클래스에서 제공하고 세부 동작은 Override하도록 구현했습니다.

또한 Manager는 Singleton으로 관리하고, 데이터는 일반 클래스로 분리하여 역할을 명확하게 나누었습니다.

이를 통해 유지보수성과 확장성을 높일 수 있었습니다.

---

# 번외 

## static

### 답변

static은 객체가 아닌 클래스 자체에 존재하는 멤버입니다.

Oracle War에서는 Singleton Instance와 Utility 함수에서 사용했습니다.

---

## const / readonly

### 답변

const는 컴파일 타임 상수이고 readonly는 생성 시 한 번 초기화 가능한 런타임 상수입니다.

---

## delegate / event

### 답변

delegate는 함수를 변수처럼 저장하는 타입입니다.

event는 delegate를 기반으로 하며 외부에서는 +=, -=만 가능하고 Invoke는 선언한 클래스만 가능합니다.

UnityEvent는 Inspector 연결이 가능하며 C# Event보다 디자이너 친화적입니다.

---

# Related Concepts

- [Dependency_Injection](Advanced/DI.md)  
- [Service_Locator](Advanced/Service_Locator.md)  
- [Singleton](Advanced/Singleton.md)  
- [ScriptableObject](Advanced/ScriptableObject.md)  
- [DTO](Advanced/DTO.md)  
- [Required Component](Advanced/Required_Component.md)  
