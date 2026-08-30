# 비트마스크 연산

## 1. `1 << n` — 숫자를 비트 위치로 변환

```cpp
1 << 3  →  00001000   (3번째 자리 스위치 ON)
1 << 5  →  00100000   (5번째 자리 스위치 ON)
```

정수 하나의 각 비트 자리를 "숫자 하나의 존재 여부"로 매핑.

---

## 2. `|` (OR) — 여러 숫자를 하나의 집합(마스크)으로 누적

```cpp
mask |= (1 << num);
```

| 연산 | 결과 |
|---|---|
| 0 \| 0 | 0 |
| 0 \| 1 | 1 |
| 1 \| 0 | 1 |
| 1 \| 1 | 1 |

→ 자리가 겹치지 않으면 기존 비트는 유지되면서 새 비트가 추가됨. 5개 숫자를 넣으면 정수 하나가 "5개 숫자 집합"이 됨.

---

## 3. `&` (AND) — 두 집합의 교집합 추출

```cpp
int common = codeMask & qMask[i];
```

| 연산 | 결과 |
|---|---|
| 0 & 0 | 0 |
| 0 & 1 | 0 |
| 1 & 0 | 0 |
| 1 & 1 | 1 |

→ **양쪽 다 1인 비트만 살아남음** = 두 숫자 집합의 교집합.

---

## 4. `__builtin_popcount()` — 교집합 원소 개수 세기
### 정의
`__builtin_popcount`는 **GCC/Clang 컴파일러가 제공하는 내장(built-in) 함수**로, 정수를 이진수로 표현했을 때 **1로 켜져 있는 비트의 개수**를 세어 반환합니다. ("popcount" = population count, 집단의 개수를 센다는 뜻)

```cpp
int __builtin_popcount(unsigned int x);
```

- 표준 C++ 라이브러리 함수가 아니라 **컴파일러 확장 기능**입니다. (C++20부터는 `<bit>` 헤더의 `std::popcount`가 표준으로 존재)
- 내부적으로 CPU의 `POPCNT` 명령어를 사용하거나, 이를 지원하지 않는 환경에서는 소프트웨어 방식으로 비트를 세는 알고리즘으로 대체됩니다. 어느 쪽이든 반복문으로 직접 세는 것보다 훨씬 빠릅니다.

### 동작 예시

```cpp
__builtin_popcount(0b00000101);  // 1과 4번째 비트 → 2 반환
__builtin_popcount(0b00101000);  // 3번, 5번 비트 → 2 반환
__builtin_popcount(7);           // 0b111 → 3 반환
__builtin_popcount(0);           // 0 반환
```

### 변형 함수들

인자의 타입 크기에 따라 다른 버전을 써야 합니다.

| 함수 | 인자 타입 |
|---|---|
| `__builtin_popcount` | `unsigned int` (보통 32비트) |
| `__builtin_popcountl` | `unsigned long` (보통 64비트) |
| `__builtin_popcountll` | `unsigned long long` (64비트) |

→ 값이 32비트를 넘는 `long long` 변수를 셀 때는 `__builtin_popcountll`을 써야 정확합니다.

### 이번 문제에서의 사용

```cpp
int codeMask = (1<<a)|(1<<b)|(1<<c)|(1<<d)|(1<<e);   // 조합 5개 숫자 → 비트마스크
int cnt = __builtin_popcount(codeMask & qMask[i]);    // 교집합 비트 수 = 겹치는 개수
```

- `codeMask & qMask[i]` → 두 숫자 집합의 교집합만 비트로 남김
- `popcount(...)` → 그 교집합에 몇 개의 숫자가 있는지 즉시 계산

`find`로 하나씩 비교하며 카운트를 누적하던 로직을, "비트 켜기 → AND → 개수 세기" 3단계로 압축한 것이 이 함수의 역할입니다.

### 주의사항

- **컴파일러 종속적**입니다. GCC/Clang에서는 잘 동작하지만 MSVC(Visual Studio)에서는 지원하지 않아 `__popcnt`(MSVC 전용) 등 다른 함수를 써야 합니다. 프로그래머스는 GCC 기반이라 문제없이 사용 가능합니다.
- C++20 환경이라면 컴파일러에 상관없이 표준인 `std::popcount` (`<bit>` 헤더)를 쓰는 게 더 이식성이 좋습니다.

---

## 예시 코드 전체

```cpp
#include <string>
#include <vector>
using namespace std;

int solution(int n, vector<vector<int>> q, vector<int> ans) {
    int answer = 0;
    int m = q.size();

    // 각 시도(query)를 비트마스크로 미리 변환
    vector<int> qMask(m, 0);
    for (int i = 0; i < m; i++)
        for (int num : q[i])
            qMask[i] |= (1 << num);

    for (int a = 1; a <= n - 4; a++)
        for (int b = a + 1; b <= n - 3; b++)
            for (int c = b + 1; c <= n - 2; c++)
                for (int d = c + 1; d <= n - 1; d++)
                    for (int e = d + 1; e <= n; e++) {
                        int codeMask = (1 << a) | (1 << b) | (1 << c) | (1 << d) | (1 << e);

                        bool ok = true;
                        for (int i = 0; i < m; i++) {
                            int cnt = __builtin_popcount(codeMask & qMask[i]);
                            if (cnt != ans[i]) { ok = false; break; }
                        }
                        if (ok) answer++;
                    }

    return answer;
}
```
