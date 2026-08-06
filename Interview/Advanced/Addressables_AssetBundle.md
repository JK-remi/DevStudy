# Addressables & AssetBundle

## AssetBundle

### 정의

AssetBundle은 Unity 에셋(Prefab, Texture, Material, Audio 등)을 하나의 파일로 묶어 런타임에서 동적으로 로드할 수 있도록 만든 패키지 파일이다.

주로 DLC, 패치, 리소스 다운로드 등에 사용된다.

---

## 생성 과정

```text
Prefab

↓

Inspector에서
AssetBundle Name 지정

↓

BuildPipeline.BuildAssetBundles()

↓

Monster.bundle 생성
```

생성된 `.bundle` 파일이 실제 패치 파일이 된다.

---

## 사용 과정

```text
AssetBundle.LoadFromFile()

↓

LoadAsset()

↓

Instantiate()
```

즉, 개발자가 직접 Bundle을 로드하고, 원하는 Asset을 찾아 생성해야 한다.

---

## 장점

- 필요한 리소스만 다운로드 가능
- 패치 용량 감소
- 런타임 메모리 관리 가능

---

## 단점

- 의존성 직접 관리
- Bundle 관리 코드 작성 필요
- 버전 관리가 복잡

---

# Addressables

## 정의

Addressables는 AssetBundle을 Unity가 자동으로 관리해주는 시스템이다.

개발자는 Asset의 "주소(Address)"만 지정하면 AssetBundle 생성, 로드, 의존성 관리 등을 Unity가 처리한다.

---

## 생성 과정

```text
Prefab

↓

Addressable 체크

↓

Address 지정

(ex. Monster)

↓

Build

↓

Addressables가 내부적으로
AssetBundle 자동 생성
```

즉, AssetBundle을 직접 만들 필요가 없다.

---

## 사용 과정

```csharp
Addressables.LoadAssetAsync<GameObject>("Monster");
```

Address 문자열만으로 Asset을 로드할 수 있다.

---

## 내부 동작

```text
Address

↓

Group

↓

AssetBundle

↓

Asset
```

Address는 AssetBundle 이름이 아니라 Asset을 찾기 위한 Key 역할을 한다.

---

## 장점

- AssetBundle 자동 생성
- 의존성 자동 관리
- 메모리 해제 자동 지원
- 패치 관리 쉬움

---

## 단점

- 초기 설정 필요
- 내부 구조를 이해하지 않으면 디버깅이 어려움

---

# AssetBundle vs Addressables

| AssetBundle | Addressables |
|--------------|--------------|
| 개발자가 직접 관리 | Unity가 자동 관리 |
| 직접 Build | 자동 Build |
| 의존성 직접 처리 | 의존성 자동 처리 |
| 직접 Load | Address로 Load |
| 관리 어려움 | 관리 쉬움 |

---

## Oracle War에 적용한다면

Oracle War는 모바일 게임이므로 대용량 리소스를 업데이트해야 하는 상황이라면 Addressables를 사용하는 것이 적합하다.

초기 설치에는 최소한의 리소드만 포함하고, 이후 이벤트 리소스나 신규 캐릭터를 Addressables를 통해 다운로드하면 패치 용량을 줄일 수 있다.
