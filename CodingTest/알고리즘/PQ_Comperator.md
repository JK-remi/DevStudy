# `priority_queue` Comparator 등록 방식과 `decltype`

## 1. 배경 — `priority_queue`의 템플릿 구조

```cpp
template<
    class T,
    class Container = vector<T>,
    class Compare = less<typename Container::value_type>
>
class priority_queue;
```

`Compare`는 `class Compare`로 선언된 **타입 전용 템플릿 파라미터**다. 이 자리에는 반드시 "타입 이름"만 올 수 있고, 함수 이름이나 표현식 같은 "값(value)"은 올 수 없다.

이 제약 때문에 comparator를 어떤 형태로 만드느냐에 따라 등록 방식이 달라진다.

---

## 2. 등록 방식 4가지 비교

| 방식 | 템플릿 인자 자리 | 생성자 인자 필요? | 비고 |
|---|---|---|---|
| 기본 (`less<T>`) | `Compare` 생략 | 불필요 | operator< 오름차순 정렬(최대 힙) |
| 람다 | `decltype(cmp)` | **필요** (`pq(cmp)`) | 익명 클로저 타입 |
| 함수 포인터 | `decltype(&cmp)` | **필요** (`pq(cmp)`) | 함수 타입 자체는 멤버로 못 가짐 |
| functor (struct) | `Cmp` (이름 그대로) | 불필요 (상태 없으면) | 가장 직관적, 재사용 용이 |

### 2-1. 람다 + `decltype`

```cpp
auto cmp = [](const pair<float,int> &a, const pair<float,int> &b) {
    if (a.first == b.first) return a.second > b.second;
    return a.first < b.first;
};

priority_queue<pair<float,int>, vector<pair<float,int>>, decltype(cmp)> pq(cmp);
```

- 람다 리터럴 `[](...){...}`은 **표현식**이며, 평가되면 컴파일러가 생성한 **이름 없는(anonymous) 클로저 타입**의 객체가 만들어진다.
- 템플릿의 `Compare` 자리는 "타입"을 요구하는데, 람다 표현식 자체는 타입이 아니라 "그 타입의 인스턴스를 만드는 식"이라서 직접 넣을 수 없다.
- `decltype(cmp)`로 그 이름 없는 타입을 컴파일 타임에 추출해서 타입 자리를 채운다.
- 타입만 채웠다고 인스턴스가 생기는 게 아니므로, 생성자 인자로 `cmp`를 **반드시 같이 넘겨야** 한다. 빼먹으면(`pq;`만 쓰면) 클로저의 기본 생성을 시도하게 되는데, 캡처 없는 람다라도 C++20 이전에는 기본 생성자가 없어 컴파일 에러가 나거나(또는 컴파일러/표준에 따라) 초기화되지 않은 채로 동작해 위험하다.

### 2-2. 함수 + `decltype`

```cpp
bool cmp(const pair<float,int> &a, const pair<float,int> &b) {
    if (a.first == b.first) return a.second > b.second;
    return a.first < b.first;
}
```

**`decltype(cmp)`를 그대로 쓰면 안 된다.**

- `decltype`의 규칙상, 피연산자가 괄호 없는 id-expression(함수 이름 그 자체)이면 **선언된 그대로의 타입**을 반환한다.
- 함수 `cmp`가 선언된 타입은 `bool(const pair<float,int>&, const pair<float,int>&)`라는 **함수 타입**이지, 포인터 타입이 아니다.
- 평소 함수 이름을 값으로 쓸 때 자동으로 일어나는 "함수→포인터 변환(decay)"이 `decltype`에는 적용되지 않는다.
- 그런데 `priority_queue`는 `Compare comp;`처럼 비교자를 **멤버 변수**로 저장하며, C++에서 **함수 타입은 멤버 변수로 가질 수 없다** (객체가 아니므로 크기·저장 방식이 정의되지 않음).
- 따라서 `&`를 붙여 명시적으로 포인터 타입으로 만들어야 한다.

```cpp
priority_queue<pair<float,int>, vector<pair<float,int>>, decltype(&cmp)> pq(cmp);
```

- `decltype(&cmp)` → `bool(*)(const pair<float,int>&, const pair<float,int>&)` (함수 포인터 타입 → 객체 타입 → 멤버 저장 가능)
- 생성자에 `cmp`를 넘기면 함수 이름이 자동으로 포인터로 decay되어 들어간다.
- **주의**: 생성자 인자를 빼먹고 `priority_queue<..., decltype(&cmp)> pq;`만 쓰면 `Compare` 멤버가 값 초기화되어 함수 포인터가 `nullptr`이 되고, 내부에서 `comp(a,b)`를 호출하는 순간 **null 함수 포인터 호출 UB**가 발생한다.

### 2-3. `cmp`(함수 이름)를 템플릿 인자에 직접 못 쓰는 이유

```cpp
priority_queue<pair<float,int>, vector<pair<float,int>>, cmp> pq;  // ❌ 컴파일 불가
```

- `Compare`는 **타입 파라미터**(`class Compare`)로 선언되어 있어 그 자리엔 오직 타입만 올 수 있다.
- `cmp`는 함수라는 **엔티티(값)**이지 타입이 아니므로, 타입 자리에 값이 들어와 타입/값 불일치 에러가 난다.
- 참고로 `std::sort(first, last, cmp)`처럼 함수 템플릿에서는 `cmp`를 직접 넘겨도 되는데, 이는 `sort`가 **클래스 템플릿이 아니라 함수 템플릿**이라 `Compare` 타입을 인자로부터 **자동 추론(template argument deduction)**하기 때문이다. `priority_queue`는 클래스 템플릿이라 인스턴스화 이전에 `< >` 안 타입이 먼저 확정돼야 해서, 타입 명시(`decltype`)와 값 전달(생성자 인자)이 분리된다.

### 2-4. functor (struct + `operator()`)

```cpp
struct Cmp {
    bool operator()(const pair<float,int> &a, const pair<float,int> &b) const {
        if (a.first == b.first) return a.second > b.second;
        return a.first < b.first;
    }
};

priority_queue<pair<float,int>, vector<pair<float,int>>, Cmp> pq;
```

- `struct Cmp`를 정의하는 순간 `Cmp`라는 **이름 있는 타입**이 이미 존재하므로, `decltype` 같은 우회가 필요 없다.
- 캡처(상태)가 없으면 기본 생성자가 자동으로 존재해서, 생성자 인자 없이 `priority_queue`가 알아서 `Cmp{}`를 기본 생성한다 → 함수 포인터처럼 null이 되어 UB가 나는 문제 자체가 없다.
- 상태를 가진 비교자(비교 기준을 생성자로 받는 경우), 여러 컨테이너(`priority_queue`, `sort`, `set` 등)에서 같은 비교 로직을 재사용하는 경우에 유리하다.
- 컴파일러가 `operator()` 호출을 정적으로 인라인하기 쉬워 함수 포인터 방식보다 유리한 경우가 많다(람다도 동일한 이점을 가짐 — 람다 자체가 컴파일러가 만든 익명 functor이기 때문).

---

## 3. `decltype`의 정확한 정의

`decltype(entity)` 또는 `decltype(expression)` — **피연산자를 평가(실행)하지 않고**, 그 선언된 타입 또는 표현식의 타입을 컴파일 타임에 얻어내는 연산자.

### (1) 괄호 없는 id-expression / 클래스 멤버 접근인 경우
→ 그 **엔티티가 선언된 그대로의 타입**을 반환.

```cpp
int x = 0;
decltype(x)     // int

struct S { double d; };
S s;
decltype(s.d)   // double

bool cmp(int, int);
decltype(cmp)   // bool(int, int)  ← 함수 타입 그 자체 (포인터 아님, decay 없음)
```

### (2) 그 외 일반 표현식인 경우
→ 표현식의 **값 범주(value category)**에 따라 결정.

| 값 범주 | 결과 |
|---|---|
| prvalue | `T` |
| xvalue | `T&&` |
| lvalue | `T&` |

```cpp
int x = 0;
decltype((x))            // int&   — 괄호를 씌우면 id-expression이 아닌 일반 표현식(lvalue)으로 취급되어 참조가 붙음
decltype(x + 1)          // int    — prvalue
decltype(std::move(x))   // int&&  — xvalue
```

### `auto`와의 차이

- `auto`: 값을 복사하듯 추론 → 참조/최상위 const는 기본적으로 벗겨짐.
- `decltype`: 참조성과 cv-한정자를 **그대로 보존**.

```cpp
const int& r = x;
auto a = r;          // int          (참조·const 모두 벗겨짐)
decltype(r) d = x;   // const int&   (그대로 보존)
```

---

## 4. 한눈에 정리

- **템플릿 인자 자리(`< >`)**: 반드시 타입만 가능. 표현식(람다 리터럴), 값(함수 이름)은 불가.
- **람다**: 이름 없는 타입 → `decltype`으로 이름을 뽑아냄. 인스턴스는 생성자 인자로 별도 전달 필수.
- **함수**: 이름이 있지만 함수 타입 자체는 객체가 아니라 멤버로 못 가짐 → `decltype(&fn)`으로 포인터 타입화 필요. 인스턴스(포인터) 전달 필수.
- **functor(struct)**: 처음부터 이름 있는 진짜 타입 → 그대로 사용. 상태 없으면 인스턴스 전달도 생략 가능.
- **`decltype`**: 평가하지 않고 타입만 추출. id-expression은 선언된 타입 그대로, 그 외 표현식은 값 범주(lvalue/xvalue/prvalue)에 따라 참조 여부가 결정됨. `auto`와 달리 참조·const를 보존.
