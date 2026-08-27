# C++ 람다(Lambda)와 캡처(Capture) 정리

## 1. 기본 문법

```cpp
[캡처](매개변수) -> 반환타입 { 함수 본문 }
```

- `[]` : 캡처 리스트 (외부 변수를 람다 내부로 가져오는 방법)
- `()` : 매개변수 리스트 (없으면 생략 가능)
- `-> 반환타입` : 대부분 생략 가능 (컴파일러가 추론)
- `{}` : 함수 본문

```cpp
auto add = [](int a, int b) { return a + b; };
cout << add(3, 4); // 7
```

---

## 2. 캡처 방식

외부 변수를 람다 안에서 쓰려면 반드시 캡처해야 합니다. 캡처하지 않은 외부 변수는 람다 내부에서 사용할 수 없습니다.

| 캡처 표기 | 의미 |
|---|---|
| `[]` | 아무것도 캡처하지 않음 |
| `[x]` | `x`를 값으로 캡처 (복사) |
| `[&x]` | `x`를 참조로 캡처 |
| `[=]` | 사용하는 모든 외부 변수를 값으로 캡처 |
| `[&]` | 사용하는 모든 외부 변수를 참조로 캡처 |
| `[x, &y]` | `x`는 값으로, `y`는 참조로 캡처 (혼합) |
| `[=, &y]` | 기본은 값 캡처, `y`만 참조 캡처 |
| `[this]` | 멤버 함수 내부에서 클래스 멤버 접근 |

### 값 캡처 vs 참조 캡처 예제

```cpp
int si = 3;

// 값 캡처: 람다 생성 시점의 si 값이 복사됨. 이후 si가 바뀌어도 람다 내부 값은 그대로.
auto byValue = [si]() { return si; };

// 참조 캡처: si 변수 자체를 참조. si가 바뀌면 람다 내부 값도 바뀜.
auto byRef = [&si]() { return si; };

si = 10;
cout << byValue(); // 3  (캡처 당시 값)
cout << byRef();   // 10 (현재 값)
```

**주의:** 참조 캡처는 원본 변수의 생명주기가 끝나면 댕글링 참조가 될 수 있습니다. 특히 함수를 반환하거나 비동기로 실행되는 람다에서는 값 캡처가 더 안전합니다.

---

## 3. mutable

값으로 캡처한 변수는 기본적으로 람다 내부에서 수정할 수 없습니다(const 취급). 수정하려면 `mutable`을 붙입니다.

```cpp
int cnt = 0;
auto counter = [cnt]() mutable {
    cnt++;
    return cnt;
};
cout << counter(); // 1
cout << counter(); // 2 (람다 내부에서만 증가, 외부 cnt는 그대로 0)
cout << cnt;        // 0
```

### 부록: const 취급의 실제 원리

값 캡처한 변수는 람다의 `operator()`가 기본적으로 `const` 멤버 함수로 생성되기 때문에 수정이 막힙니다. `mutable`은 이 `operator()`를 non-const로 바꿔주는 키워드일 뿐입니다.

```cpp
// [cnt]() { ... }  →  operator()가 const라서 cnt++ 컴파일 에러
// [cnt]() mutable { ... }  →  operator()가 non-const라서 cnt++ 가능
```

- `mutable`이 없으면 캡처된 복사본을 못 건드림, 있으면 건드릴 수 있음
- 어느 쪽이든 **원본 외부 변수는 절대 바뀌지 않음** (값 캡처이므로 복사본만 존재)
- 참조 캡처(`[&cnt]`)는 원본을 직접 참조하므로 호출할 때마다 원본 자체가 바뀜

### 부록: 람다는 객체다 — counter() 재호출 시 값이 누적되는 이유

람다는 실행되는 순간 **객체(인스턴스) 하나**가 만들어져 변수에 저장됩니다. 캡처된 `cnt`는 그 객체의 멤버 변수입니다. `counter()`를 여러 번 호출해도 매번 새 람다가 만들어지는 게 아니라 **같은 객체의 `operator()`를 반복 호출**하는 것이므로, 멤버 `cnt`가 호출 사이에 값을 유지하며 누적됩니다. 이는 아래처럼 일반 함수 객체(functor)와 완전히 동일한 원리입니다.

```cpp
class Counter {
    int cnt = 0;
public:
    int operator()() { cnt++; return cnt; }
};

Counter counter; // 객체 생성, 해제 안 됨
counter(); // 1
counter(); // 2 (같은 객체의 멤버가 유지되니까)
```

값 캡처 + `mutable`은 결국 컴파일러가 이런 함수 객체 클래스를 자동으로 만들어주는 것과 같습니다.

---

## 4. 일반화 캡처 (Init Capture, C++14~)

캡처 리스트에서 새 변수를 선언하거나, `std::move`로 이동 캡처할 때 사용합니다.

```cpp
int si = 3;
auto lam = [newSi = si * 2]() { return newSi; }; // si*2 값을 newSi라는 새 변수로 캡처
cout << lam(); // 6

// unique_ptr처럼 복사 불가능한 객체는 이동 캡처
auto ptr = make_unique<int>(42);
auto lam2 = [p = move(ptr)]() { return *p; };
```

**쓰는 이유:**
- **이동 전용(move-only) 객체 캡처** — `unique_ptr`, `unique_lock` 등은 복사 생성자가 삭제되어 있어 일반 값 캡처 `[ptr]`이 컴파일 에러를 냅니다. 실무에서 가장 흔한 사용 이유이며, C++14 이전에는 이런 타입을 캡처할 방법 자체가 없었습니다.
- 캡처 시점에 값을 가공해서 저장하고 싶을 때 (`[half = si / 2]`) — 바깥에 임시 변수를 따로 안 만들어도 됨
- 원본과 이름이 겹치지 않는 독립된 복사본임을 명확히 하고 싶을 때 (`[localCnt = cnt]`)

이 중 코딩테스트 수준에서는 세 번째까지는 몰라도, 이동 전용 객체 캡처는 거의 쓸 일이 없습니다 (`unique_ptr`을 정렬 비교 함수 등에서 캡처할 일은 드묾).

### 부록: `lam2`의 `p`는 `ptr`과 별개의 존재

`[p = move(ptr)]`은 클로저(람다 객체) 내부에 `p`라는 새 멤버 변수를 선언하고, `unique_ptr`의 이동 생성자로 초기화합니다. 즉 `p`는 `ptr`과 이름도 실체도 다른 완전히 독립적인 객체이며, 다만 힙 메모리의 소유권만 이동됩니다. 이동 후 `ptr`은 빈 상태(`nullptr`)가 됩니다.

```cpp
auto ptr = make_unique<int>(42);
auto lam2 = [p = move(ptr)]() { return *p; };

cout << (ptr == nullptr); // 1 (true) — ptr은 이제 아무것도 안 가리킴
cout << lam2();           // 42 — 실제 데이터는 p가 소유
```

`ptr`이 이동 후 null이 되는 이유: `unique_ptr`은 "하나의 객체를 오직 하나의 `unique_ptr`만 소유"하도록 복사 생성자/대입 연산자가 삭제되어 있습니다. 그래서 소유권 이전은 복사가 아닌 이동으로만 가능하고, `move(ptr)`은 rvalue로 캐스팅만 할 뿐 실제 이동은 `p`의 이동 생성자가 수행합니다 — 내부 raw 포인터를 `p`로 넘기고 `ptr`의 내부 포인터는 `nullptr`로 비웁니다. 이렇게 원본을 비워야 소멸자 이중 호출(double free)을 막을 수 있습니다. 일반 포인터(`int*`)였다면 `move` 후에도 단순 값 복사라서 `ptr`과 `p`가 같은 주소를 동시에 가리키는 상태가 됐을 것이라는 점이 다릅니다.

---

## 5. 코딩테스트에서의 주요 활용: 정렬 비교 함수

전역 변수나 별도 함수 대신, 정렬 기준을 람다 캡처로 넘기면 상태를 캡슐화할 수 있습니다.

### Before: 전역 변수 방식
```cpp
int si;
bool cmp(vector<int> &a, vector<int> &b) {
    return a[si] < b[si];
}
// ...
si = m[sort_by];
sort(answer.begin(), answer.end(), cmp);
```

### After: 람다 캡처 방식
```cpp
int si = m[sort_by];
sort(answer.begin(), answer.end(), [si](vector<int> &a, vector<int> &b) {
    return a[si] < b[si];
});
```

전역 변수가 없어져서 재귀 호출이나 여러 정렬 기준을 동시에 다뤄야 할 때도 안전합니다.

### 여러 캡처를 활용한 예: "Best Album" 스타일 정렬

장르별 재생 횟수 합이 큰 순서로, 같은 장르 내에서는 재생 횟수가 큰 순서로 정렬하는 경우:

```cpp
unordered_map<string, int> genreTotal; // 장르별 총 재생 횟수
vector<int> plays;                     // 각 곡의 재생 횟수
vector<string> genres;                 // 각 곡의 장르

sort(indices.begin(), indices.end(), [&](int a, int b) {
    if (genreTotal[genres[a]] != genreTotal[genres[b]])
        return genreTotal[genres[a]] > genreTotal[genres[b]];
    return plays[a] > plays[b];
});
```

`[&]`로 외부의 `genreTotal`, `genres`, `plays`를 모두 참조 캡처해서 별도의 매개변수 전달 없이 비교 로직에서 바로 사용합니다.

---

## 6. std::function과 함께 쓰기

람다를 변수에 저장해서 재사용하거나, 재귀 람다를 만들 때는 `std::function`이 필요합니다 (일반 `auto`로는 재귀 람다를 선언할 수 없음).

```cpp
#include <functional>

function<int(int)> factorial = [&](int n) -> int {
    return n <= 1 ? 1 : n * factorial(n - 1);
};
cout << factorial(5); // 120
```

---

## 7. 요약: 언제 무엇을 쓸까

| 상황 | 추천 |
|---|---|
| 정렬 비교 함수에 외부 값 하나만 필요 | `[si]` (값 캡처) |
| 여러 외부 컨테이너를 참조해야 함 | `[&]` (참조 캡처, 단 람다 실행 시점까지 원본이 살아있어야 함) |
| 람다 내부에서 캡처한 값을 변경해야 함 | `mutable` 추가 |
| 이동만 가능한 객체(unique_ptr 등)를 캡처 | 일반화 캡처 `[p = move(ptr)]` |
| 재귀 람다 | `function<...>` + `[&]` |
| 전역 변수로 상태 공유하던 코드 리팩터링 | 캡처로 대체해 상태를 지역화 |
