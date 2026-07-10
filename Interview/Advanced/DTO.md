# DTO (Data Transfer Object)

## DTO란?

DTO(Data Transfer Object)는 계층 간 또는 서버와 클라이언트 간 데이터를 전달하기 위한 객체입니다.

비즈니스 로직은 포함하지 않고, 데이터만 저장하는 것이 목적입니다.

---

## 왜 사용하는가?

DTO를 사용하면

- 데이터 전달 구조를 통일할 수 있습니다.
- 서버와 클라이언트의 내부 구조를 분리할 수 있습니다.
- 필요한 데이터만 전달하여 네트워크 비용을 줄일 수 있습니다.

---

## 예시

```csharp
public class CharacterDTO
{
    public int id;
    public string name;
    public int level;
}
```

DTO는 데이터만 존재하며 별도의 게임 로직은 포함하지 않습니다.

---

## Oracle War에서는?

Oracle War에서는 서버에서 전달받는 Character, Skill, Item 등의 데이터를 DTO 형태의 일반 클래스로 관리했습니다.

DTO로 받은 데이터를 게임에서 사용하는 Character, Skill 객체로 변환하여 사용했습니다.

---

## DTO와 GameObject의 차이

DTO는 데이터를 전달하기 위한 객체입니다.

GameObject는 실제 게임에서 동작하는 객체입니다.

일반적으로

Server

↓

DTO

↓

Game Object

순서로 변환하여 사용합니다.

---

## DTO와 ScriptableObject 차이

DTO

- 서버 통신
- 데이터 전달
- 런타임 생성

ScriptableObject

- Unity Asset
- 로컬 데이터 저장
- Inspector 수정 가능

---

## DTO와 Entity 차이

DTO는 데이터를 전달하기 위한 객체입니다.

Entity는 실제 게임에서 동작하는 객체입니다.

Entity에는 행동과 로직이 존재하지만 DTO에는 데이터만 존재합니다.

---

## 면접 답변

Q. DTO란 무엇인가요?

A.

DTO(Data Transfer Object)는 서버와 클라이언트 간 데이터를 전달하기 위한 객체입니다.

비즈니스 로직은 포함하지 않고 데이터만 저장합니다.

Oracle War에서는 서버에서 전달받은 Character, Skill, Item 정보를 DTO 형태의 일반 클래스로 관리했고, 이를 실제 게임 객체로 변환하여 사용했습니다.
