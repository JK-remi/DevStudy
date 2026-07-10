# ScriptableObject

## ScriptableObject란?

GameObject 없이 데이터를 저장하는 Unity Asset입니다.

---

## 장점

- 메모리 절약
- 여러 객체가 하나의 데이터를 공유
- Inspector 수정 가능

---

## JSON과 차이

JSON

- 파일 읽기 필요
- 서버 통신

ScriptableObject

- Unity Asset
- 즉시 접근 가능

---

## 언제 사용하는가?

- MasterData
- ItemData
- CharacterData
- SkillData

---

## 면접 답변

OracleWar에서는 서버 기반 MasterData 구조를 사용했기 때문에 ScriptableObject를 사용하지 않았습니다.
