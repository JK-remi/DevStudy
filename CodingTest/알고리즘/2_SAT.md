# 2-SAT (2-Satisfiability)

## 1. 개념

- **SAT**: 불리언 변수들로 이루어진 논리식을 참으로 만드는 값 배정이 존재하는가?
- **2-SAT**: 논리식이 **절(clause) 여러 개의 AND**(CNF)이고, **각 절이 리터럴 2개의 OR**인 SAT
- 리터럴: 변수 `x` 또는 그 부정 `¬x`

```
(A ∨ B) ∧ (¬A ∨ C) ∧ (¬B ∨ ¬C)
```

| 문제 | 절의 크기 | 복잡도 |
|---|---|---|
| 2-SAT | 2 | **O(V + E)** (선형) |
| 3-SAT | 3 | NP-완전 |

## 2. 핵심 아이디어: 함의 그래프 (Implication Graph)

절 `(p ∨ q)`는 다음 두 함의와 논리적으로 동치다.

- `¬p → q` (p가 거짓이면 q는 참이어야 함)
- `¬q → p` (q가 거짓이면 p는 참이어야 함)

### 그래프 구성
- 변수 `x`마다 노드 2개: `x`, `¬x` (총 2n개)
- 절 `(p ∨ q)`마다 간선 2개: `¬p → q`, `¬q → p`
- 간선은 항상 **대칭 쌍**으로 생긴다 (`a → b`가 있으면 `¬b → ¬a`도 있음)

### 자주 쓰는 변환

| 조건 | 절 | 간선 |
|---|---|---|
| p 또는 q 중 하나는 참 | `(p ∨ q)` | `¬p → q`, `¬q → p` |
| p와 q는 **동시에 참 불가** | `(¬p ∨ ¬q)` | `p → ¬q`, `q → ¬p` |
| p이면 q | `(¬p ∨ q)` | `p → q`, `¬q → ¬p` |
| p는 반드시 참 (강제) | `(p ∨ p)` | `¬p → p` |
| p와 q는 같아야 함 | `(¬p ∨ q) ∧ (p ∨ ¬q)` | 양방향 |

## 3. 판정과 해 구하기

### 가능 여부
- 어떤 변수 `x`에 대해 `x`와 `¬x`가 **같은 SCC**에 있으면 **불가능**
  - `x → ... → ¬x → ... → x`이므로 x가 참이든 거짓이든 모순
- 그런 변수가 하나도 없으면 **항상 가능**

### 값 배정 (SCC 위상 순서)
- SCC를 압축한 DAG의 위상 순서에서 `x`가 `¬x`보다 **뒤**(더 싱크 쪽)에 있으면 `x = true`
- Tarjan은 SCC 번호를 **역위상 순서**(싱크부터)로 매기므로
  - `comp[x] < comp[¬x]` 이면 `x = true`

### 도달 가능성 기반 배정 (변수가 적을 때)
- 노드가 수십 개면 전이 폐쇄(Floyd–Warshall식)로도 충분하다
- 가능 여부: `x → ¬x`와 `¬x → x`가 **동시에** 성립하면 불가능
- 배정: 변수를 차례로 보며 `x`를 고르고, `x`에서 도달 가능한 리터럴을 모두 함께 고정한다
  - 고르려는 리터럴이 자신의 부정이나 이미 고정된 값과 모순되면 반대쪽을 시도한다

## 4. 알고리즘 순서

1. 변수에 0 ~ n-1 번호를 붙이고, 리터럴을 노드로 매핑한다
2. 모든 절을 간선 2개로 바꿔 그래프를 만든다
3. SCC를 구한다 (Tarjan 또는 Kosaraju)
4. `comp[2i] == comp[2i+1]`인 i가 있으면 불가능
5. 아니면 `comp[2i] < comp[2i+1]` 기준으로 값을 정한다

복잡도는 **O(n + m)** (m = 절 수)이다.

## 5. C++ 템플릿 (Tarjan)

```cpp
#include <vector>
#include <stack>
#include <algorithm>
using namespace std;

struct TwoSAT {
    int n, timer = 0, sccCnt = 0;
    vector<vector<int>> g;
    vector<int> num, low, comp, st;
    vector<bool> onStack;

    TwoSAT(int n) : n(n), g(2 * n), num(2 * n, -1), low(2 * n),
                    comp(2 * n, -1), onStack(2 * n, false) {}

    // 리터럴: 변수 i가 val이면 노드 2*i, 아니면 2*i+1 (부정은 ^1)
    static int lit(int i, bool val) { return 2 * i + (val ? 0 : 1); }

    // (p ∨ q)
    void addClause(int p, int q) {
        g[p ^ 1].push_back(q);
        g[q ^ 1].push_back(p);
    }

    void dfs(int u) {
        num[u] = low[u] = timer++;
        st.push_back(u); onStack[u] = true;
        for (int v : g[u]) {
            if (num[v] == -1) { dfs(v); low[u] = min(low[u], low[v]); }
            else if (onStack[v]) low[u] = min(low[u], num[v]);
        }
        if (low[u] == num[u]) {
            while (true) {
                int v = st.back(); st.pop_back();
                onStack[v] = false; comp[v] = sccCnt;
                if (v == u) break;
            }
            sccCnt++;
        }
    }

    // 가능하면 true, assign[i]에 변수 i의 값
    bool solve(vector<bool> &assign) {
        for (int i = 0; i < 2 * n; i++) if (num[i] == -1) dfs(i);
        assign.assign(n, false);
        for (int i = 0; i < n; i++) {
            if (comp[2 * i] == comp[2 * i + 1]) return false;
            assign[i] = comp[2 * i] < comp[2 * i + 1];
        }
        return true;
    }
};
```

## 6. 예시

```
(A ∨ B) ∧ (¬A ∨ C) ∧ (¬B ∨ ¬C)
```

- 간선: `¬A→B`, `¬B→A`, `A→C`, `¬C→¬A`, `B→¬C`, `C→¬B`
- `A`와 `¬A`가 같은 SCC가 아니므로 가능
- 해 중 하나: `A=T, B=F, C=T` → `(T∨F) ∧ (F∨T) ∧ (T∨F)` = 참

## 7. 이 문제(프로그래머스 1830)에서의 적용

| 2-SAT 요소 | 문제에서의 의미 |
|---|---|
| 변수 | 모호한 기호 하나 (`AxBxC` 형태) |
| 참 / 거짓 | 규칙 2 / 규칙 1 |
| 절 | 두 기호의 구간이 충돌하는 선택 조합 금지 |
| 함의 간선 | 충돌하는 선택 X, Y에 대해 `X → ¬Y`, `Y → ¬X` |
| 강제 | 확정 구간과 충돌하는 선택지는 `(¬선택)` 절로 반대쪽 강제 |
| 풀이 | 노드 최대 52개 → 전이 폐쇄로 판정, 규칙 1부터 시도해 고정 |

- mask 전수 탐색은 O(2^m)이었다
- 제약이 전부 **기호 2개 사이의 관계**라서 2-SAT으로 다항 시간에 풀린다

## 8. 2-SAT을 의심하는 신호

- 각 대상이 **둘 중 하나**를 고르는 이진 선택이다
- 제약이 전부 **두 대상 사이의 관계**다 ("A가 이걸 고르면 B는 저걸 못 고름")
- 전수 탐색은 2^n이라 시간초과가 난다

### 대표 유형
- 팀 나누기, 스위치/전구, 회의실·시간대 배정
- 커플/연인 배치 (BOJ 11280 2-SAT 3, 11281 2-SAT 4)
- 선택 가능한 두 후보 중 충돌 없는 조합 찾기

## 9. 주의점

- **절이 3개 이상의 리터럴이면 2-SAT이 아니다** (3-SAT은 NP-완전)
- 간선을 **반드시 대칭 쌍**으로 넣어야 한다 (한쪽만 넣으면 오답)
- 변수당 노드 2개이므로 배열 크기는 `2 * n`
- 해가 여러 개일 수 있다. 특정 해(사전순 최소 등)가 필요하면 별도 처리(변수 순서대로 고정 + 도달 가능성 전파)가 필요하다
- Tarjan의 `comp` 번호는 **역위상 순서**다. 대소 비교 방향을 헷갈리면 해가 반대로 나온다
