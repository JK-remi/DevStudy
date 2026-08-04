# 오일러 경로(Eulerian Path) 정리

## 1. 정의

| 용어 | 정의 |
|---|---|
| 오일러 경로 (Eulerian Path) | 그래프의 **모든 간선을 정확히 한 번씩** 지나는 경로 |
| 오일러 회로 (Eulerian Circuit) | 오일러 경로 중 시작점과 끝점이 같은 경우 |

**유래**: 1736년 오일러가 "쾨니히스베르크 다리 문제(네 지역, 7개 다리를 한 번씩만 건너 모두 지날 수 있는가?)"를 그래프 이론으로 해결하며 정립된 개념. 그래프 이론의 시초로 평가됨.

---

## 2. 존재 조건

### 무방향 그래프
| 조건 | 의미 |
|---|---|
| 홀수 차수(degree) 정점 0개 | 오일러 회로 존재 |
| 홀수 차수 정점 정확히 2개 | 오일러 경로 존재 (그 두 정점이 시작·끝점) |
| 홀수 차수 정점 3개 이상 | 오일러 경로/회로 존재하지 않음 |

### 방향 그래프
| 조건 | 의미 |
|---|---|
| 모든 정점에서 in-degree = out-degree | 오일러 회로 존재 |
| 정확히 한 정점만 out - in = 1 (시작점),<br>정확히 한 정점만 in - out = 1 (끝점),<br>나머지는 in = out | 오일러 경로 존재 |

> 이 문제(여행 경로 찾기)는 "모든 티켓을 사용하는 경로가 항상 존재한다"는 전제가 주어지므로, 위 조건이 이미 만족된 상태로 취급하고 풀면 된다.

---

## 3. 대표 알고리즘 비교

| | Fleury's Algorithm | **Hierholzer's Algorithm** |
|---|---|---|
| 방식 | 간선을 지우며 "다리(bridge)"가 아닌 간선을 우선 선택 | 아무 간선이나 따라가다 막히면 기록 후 되돌아옴 |
| 매 단계 다리 판정 필요 여부 | 필요 (비용 큼) | 불필요 |
| 시간복잡도 | O(E²) 수준 | O(E) ~ O(E log E) (정렬 자료구조 사용 시) |
| 실전 사용 | 개념 설명용 | **실전에서 거의 항상 사용** |

---

## 4. Fleury's Algorithm 상세

### 핵심 규칙
> 현재 정점에서 나갈 수 있는 간선이 여러 개라면, **다리(bridge)가 아닌 간선을 우선 선택**한다. 다리란 그 간선을 지웠을 때 그래프가 두 개 이상으로 끊어지는 간선을 말한다. 다리를 먼저 써버리면 그래프의 나머지 부분이 단절되어 남은 간선들을 더 이상 순회할 수 없게 되기 때문이다.

### 진행 절차
1. 시작 정점을 정한다 (오일러 경로면 조건에 맞는 정점, 회로면 아무 정점).
2. 현재 정점에서 나갈 수 있는 간선 후보들을 확인한다.
   - 후보가 1개뿐이면 그 간선을 사용한다.
   - 후보가 여러 개면, 다리가 아닌 간선을 우선 사용한다. (다리만 남았다면 어쩔 수 없이 다리를 사용)
3. 사용한 간선은 그래프에서 제거하고, 이동한 정점을 경로에 기록한다.
4. 더 이상 나갈 간선이 없을 때까지 2~3을 반복한다.

### 예시로 보는 동작

무방향 그래프, 간선: `A-B`, `B-C`, `C-A`, `C-D` 가 있다고 하자.

```
    A
   / \
  B---C---D
```

- `A`, `B`는 차수 2(짝수), `C`는 차수 3(홀수), `D`는 차수 1(홀수)
- 홀수 차수 정점이 2개(`C`, `D`) → 오일러 경로 존재, `C`나 `D`에서 시작해야 함
- `D`에서 시작한다고 하자.

| 단계 | 현재 정점 | 후보 간선 | 다리 판정 | 선택 | 경로 |
|---|---|---|---|---|---|
| 1 | D | `D-C` | `D-C`는 다리 (지우면 D 고립) | 어쩔 수 없이 `D-C` 사용 | D → C |
| 2 | C | `C-A`, `C-B` | 둘 다 삼각형(A-B-C) 안에 있어 다리 아님 | `C-A` 선택 (사전순) | D → C → A |
| 3 | A | `A-B` | `A-B` 제거해도 B-C 경로 남음 → 다리 아님 | `A-B` 사용 | D → C → A → B |
| 4 | B | `B-C` | 남은 마지막 간선 (다리 여부 무의미, 선택지 1개) | `B-C` 사용 | D → C → A → B → C |

최종 경로: **D → C → A → B → C**, 모든 간선(4개)을 정확히 한 번씩 사용.

**2단계가 핵심 포인트**: `C`에서 `C-D`는 이미 사용했으므로 후보는 `C-A`, `C-B`뿐이다. 만약 여기서 다리 판정 없이 아무거나 골랐다면 문제가 안 됐겠지만, 만약 그래프에 `C-D` 같은 다리가 아직 남아있는 상태였다면 그 다리를 먼저 써버리는 순간 반대편 정점들이 단절되어 나머지 간선을 순회할 수 없게 된다. 그래서 매 단계마다 "지금 이 간선이 다리인가?"를 확인하는 것이 Fleury 알고리즘의 핵심 비용이다.

### 예시 코드 (C++, 무방향 그래프 기준)

앞서 예시로 든 `A-B`, `B-C`, `C-A`, `C-D` 그래프를 그대로 구현하면 다음과 같다. 매 단계마다 "다리인지 아닌지"를 DFS로 직접 계산하는 부분이 Hierholzer와의 결정적 차이다.

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <set>
using namespace std;

map<string, multiset<string>> graph;

// 간선을 제거했을 때 dest에서 도달 가능한 정점 수를 세어 다리 여부 판단
int countReachable(const string& start) {
    set<string> visited;
    vector<string> stack = {start};
    visited.insert(start);
    while (!stack.empty()) {
        string cur = stack.back();
        stack.pop_back();
        for (auto& next : graph[cur]) {
            if (!visited.count(next)) {
                visited.insert(next);
                stack.push_back(next);
            }
        }
    }
    return visited.size();
}

// u-v 간선이 다리인지 확인: 제거 전후로 도달 가능한 정점 수가 줄어들면 다리
bool isBridge(const string& u, const string& v) {
    int before = countReachable(u);

    graph[u].erase(graph[u].find(v));
    graph[v].erase(graph[v].find(u));

    int after = countReachable(u);

    graph[u].insert(v);   // 판정 후 원상 복구
    graph[v].insert(u);

    return after < before;
}

vector<string> fleury(string start) {
    vector<string> route;
    string cur = start;
    route.push_back(cur);

    while (!graph[cur].empty()) {
        string chosen;
        // 나갈 수 있는 간선 중 다리가 아닌 것을 우선 탐색
        for (auto& next : graph[cur]) {
            if (graph[cur].size() == 1 || !isBridge(cur, next)) {
                chosen = next;
                break;
            }
        }
        // 전부 다리뿐이면 어쩔 수 없이 첫 후보 사용
        if (chosen.empty()) chosen = *graph[cur].begin();

        graph[cur].erase(graph[cur].find(chosen));
        graph[chosen].erase(graph[chosen].find(cur));

        route.push_back(chosen);
        cur = chosen;
    }
    return route;
}

int main() {
    graph["A"].insert("B"); graph["B"].insert("A");
    graph["B"].insert("C"); graph["C"].insert("B");
    graph["C"].insert("A"); graph["A"].insert("C");
    graph["C"].insert("D"); graph["D"].insert("C");

    vector<string> result = fleury("D");
    for (auto& s : result) cout << s << " ";
    // 출력: D C A B C
}
```

**코드에서 다리 판정 부분이 비용의 핵심**: `isBridge()`가 호출될 때마다 `countReachable()`로 그래프 전체를 다시 순회(O(V+E))한다. 이 판정이 매 간선 선택마다 반복되므로 전체 시간복잡도가 O(E²) 수준으로 커진다. Hierholzer 코드에는 이런 사전 확인 로직 자체가 없다는 점과 대조된다.


- **Fleury**: 매 단계마다 다리 여부를 계산해야 하므로 단계당 O(E) 비용 → 전체 O(E²)
- **Hierholzer**: 다리 여부를 신경 쓰지 않고 일단 아무 간선이나 따라가다가, 막히면 기록 후 물러나면서 나중에 이어 붙인다 → 전체 O(E log E)

즉 Fleury는 "매 순간 안전한 선택을 하려고 확인하는" 방식이고, Hierholzer는 "일단 저지르고, 문제가 생기면(막히면) 나중에 이어 붙여서 해결하는" 방식이라 훨씬 효율적이다.

---

## 5. Hierholzer 알고리즘 원리

### 핵심 직관
> 오일러 회로가 존재하는 그래프에서, 한 정점에서 출발해 간선을 따라 아무렇게나 계속 걸으면 **반드시 출발점으로 되돌아온다.**

**이유**: 매 정점에 들어올 때(in) 간선 하나를 소비하고 나갈 때(out) 간선 하나를 소비한다. 조건상 모든 정점이 in-degree = out-degree이므로, 들어온 만큼 나갈 간선이 항상 남아있다. 유일하게 이 균형이 깨진 곳이 시작점(처음엔 나가기만 하므로 +1)이라, 결국 막히는 시점은 시작점으로 되돌아왔을 때뿐이다.

### 진행 절차
1. 시작 정점에서 간선을 따라 계속 이동한다.
2. 이 과정에서 첫 번째 부분 회로(subcircuit)가 만들어진다.
3. 아직 간선이 남아있는 정점이 이 회로 위에 있다면, 그 정점에서 다시 같은 방식으로 새로운 부분 회로를 만들어 **기존 회로에 끼워 넣는다(splice).**
4. 모든 간선이 소비될 때까지 반복하면 하나의 완전한 오일러 회로가 완성된다.

### 스택 기반 구현이 이 과정을 자연스럽게 처리하는 이유
- 더 나갈 간선이 없는 막다른 정점 = "부분 회로의 끝"
- 그 지점을 결과 리스트에 기록하고 스택을 한 칸 pop하는 것 = 회로를 한 단계 물러나 다른 갈림길에서 새 부분 경로를 이어 붙이는 것(splice)과 동일한 효과
- 결과 리스트는 "막다른 지점 → ... → 시작점" 순으로 쌓이므로, 마지막에 뒤집어야 실제 이동 순서가 된다.

---

## 6. 참고 구현 (C++, 사전순 최소 경로)

```cpp
#include <string>
#include <vector>
#include <map>
#include <set>
#include <algorithm>

using namespace std;

vector<string> solution(vector<vector<string>> tickets) {
    map<string, multiset<string>> graph;
    for (auto& t : tickets)
        graph[t[0]].insert(t[1]);

    vector<string> route;
    vector<string> stack = {"ICN"};

    while (!stack.empty()) {
        string cur = stack.back();
        auto& dests = graph[cur];
        if (dests.empty()) {
            route.push_back(cur);          // 막다른 길 -> 결과에 기록
            stack.pop_back();
        } else {
            string next = *dests.begin();  // 사전순 최소 목적지
            dests.erase(dests.begin());
            stack.push_back(next);
        }
    }

    reverse(route.begin(), route.end());
    return route;
}
```

### 코드 요소별 대응 관계
| 코드 요소 | 알고리즘 개념 |
|---|---|
| `stack` | 재귀 호출 스택 역할 (push = 함수 호출, pop = 함수 리턴) |
| `graph[cur]` (multiset) | 간선(티켓) 사용 여부 판단. 노드 방문 여부가 아니라 **간선 소비 여부**가 핵심 |
| `dests.empty()` | 현재 정점에서 더 나갈 간선이 없음 = 막다른 길 |
| `route.push_back(cur)` | 부분 회로의 끝 지점을 기록 |
| `reverse(route...)` | 끝 지점부터 쌓였으므로 뒤집어 시작점부터의 순서로 복원 |
| `multiset` 자동 정렬 | 여러 오일러 경로 중 사전순으로 가장 빠른 경로 보장 |

---

## 7. 시간복잡도 비교 (재귀 백트래킹 방식 vs Hierholzer)

| 항목 | 재귀 DFS + 백트래킹 | Hierholzer |
|---|---|---|
| 아이디어 | 사전순 완전탐색, 실패 시 이전 상태로 되돌리고 재시도 | 각 간선을 정확히 1번씩만 소비, 재시도 없음 |
| 시간복잡도 | 최악의 경우 지수적으로 증가 (O(E!)에 근접) | O(E log E) |
| 원인 | 막다른 길에 도달하면 경로 전체를 취소하고 다른 분기 재탐색 → 같은 부분 경로 재방문 가능 | 오일러 경로 존재가 보장되므로 "틀린 선택 → 되돌리기" 자체가 불필요 |

**요약**: 오일러 경로가 존재하는 그래프에서는 백트래킹 없이 한 번의 순회만으로 정답이 보장되기 때문에, Hierholzer 알고리즘이 이론적으로도 실전적으로도 우월하다.
