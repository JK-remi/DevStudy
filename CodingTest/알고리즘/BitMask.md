# 비트마스크 연산 정리

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

```cpp
int cnt = __builtin_popcount(common);
```

정수 안에서 1로 켜진 비트의 개수를 반환 → 교집합에 속한 숫자의 개수 = 원래 `find`로 하나씩 세던 값과 동일.

---

## 전체 흐름

```
숫자 → (1 << num) → 비트 위치
숫자들 → OR 누적 → 집합(마스크)
집합 A & 집합 B → 교집합 마스크
popcount(교집합 마스크) → 교집합 원소 개수
```

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
