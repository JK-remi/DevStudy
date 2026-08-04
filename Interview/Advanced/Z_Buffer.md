# Z Buffer (Depth Buffer)

## 정의

Z Buffer는 **Pixel의 깊이(거리) 정보를 저장하는 버퍼**이다.

즉 카메라에서 누가 더 앞에 있는지를 판단한다.

---

## 왜 필요한가?

예)

```text
Wall

Depth = 10

------------

Character

Depth = 15
```

벽이 더 가까우므로 캐릭터는 그리지 않는다.

---

반대로

```text
Character

Depth = 5
```

이면 캐릭터를 그린다.

---

## 동작

```text
Pixel 생성

↓

Depth 비교

↓

더 가까운 Pixel만 출력
```

---

## Opaque

```text
Z Buffer

사용
```

---

## Transparent

```text
뒤가 보여야 함

↓

Z Buffer만으로 해결 불가

↓

뒤 → 앞 순으로 Rendering
```

그래서 Transparent는 Rendering 순서가 중요하다.

---

## 면접 포인트

Z Buffer는

> Pixel 깊이를 저장하여

> 가까운 오브젝트만 출력하는 버퍼

이다.

Transparent는 Z Buffer만으로는 해결되지 않기 때문에 Rendering 순서가 중요하다.
