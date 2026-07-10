# ScriptableObject

## ScriptableObject란?

GameObject 없이 데이터를 저장하는 Unity Asset입니다.

---

## 장점

- 메모리 절약
- 여러 객체가 하나의 데이터를 공유
- Inspector 수정 가능

---

## 단점

- 런타임 데이터 저장용으로 사용하기 어려움
- 동적으로 생성·변경되는 데이터 관리에 부적합
- 원본 Asset을 직접 수정하면 데이터 관리에 문제가 발생할 수 있음
- Unity 외 환경에서는 사용할 수 없음

---

## JSON과 차이

JSON

- 파일 읽기 필요
- 서버 통신

ScriptableObject

- Unity Asset
- 즉시 접근 가능

---

## Runtime에서 수정

ScriptableObject는 Asset을 참조하는 객체  
Runtime에서 값을 수정 > 메모리에 올라간 객체의 값이 변경.  

- **빌드된 게임**에서는 메모리에서만 변경되므로 게임 종료 시 원래 Asset 값으로 돌아옵니다.
- **Unity Editor**에서는 Play Mode 중 Asset 값이 변경될 수 있으므로 주의가 필요합니다.

Runtime에서 값을 변경해야 하는 경우에는 원본 Asset을 직접 수정 X  
`Instantiate()`를 통해 복사본을 만들어 사용하는 것이 일반적.  

---

## 언제 사용하는가?

- MasterData
- ItemData
- CharacterData
- SkillData

---

## 면접 답변

OracleWar에서는 서버 기반 MasterData 구조를 사용했기 때문에 ScriptableObject를 사용하지 않았습니다.
