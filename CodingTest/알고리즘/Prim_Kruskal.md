# MST (최소 신장 트리) - Kruskal & Prim 알고리즘

## MST란?

**MST (Minimum Spanning Tree, 최소 신장 트리)**는 그래프의 모든 정점을 사이클 없이 연결하면서, 간선 비용의 총합이 최소가 되는 트리입니다.

- 정점이 n개면 간선은 정확히 **n-1개**
- 그리디 알고리즘(Kruskal, Prim)으로 전역 최적해를 구할 수 있음 (cut property 기반)

---

## Kruskal 알고리즘

### 핵심 아이디어
전체 간선 중 **비용이 작은 것부터** 순서대로 보면서, 사이클만 안 생기면 채택한다. (간선 중심)

### 동작 과정
1. 모든 간선을 **비용 오름차순 정렬**
2. 비용이 작은 간선부터 확인
3. 두 노드가 이미 같은 집합(트리)에 속해 있는지 **Union-Find**로 확인
   - 다른 집합 → 채택하고 두 집합을 합침 (union)
   - 같은 집합 → 사이클 생기므로 버림
4. 간선을 **n-1개** 채택하면 종료

### Union-Find (서로소 집합)
```cpp
vector<int> parent;

int find(int x) {
    if (parent[x] == x) return x;
    return parent[x] = find(parent[x]); // 경로 압축
}

bool unite(int a, int b) {
    a = find(a), b = find(b);
    if (a == b) return false; // 이미 같은 집합 -> 사이클
    parent[a] = b;
    return true;
}
```

### 전체 구현
```cpp
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

vector<int> parent;

int find(int x) {
    if (parent[x] == x) return x;
    return parent[x] = find(parent[x]);
}

bool unite(int a, int b) {
    a = find(a), b = find(b);
    if (a == b) return false;
    parent[a] = b;
    return true;
}

int solution(int n, vector<vector<int>> costs) {
    parent.resize(n);
    iota(parent.begin(), parent.end(), 0);

    sort(costs.begin(), costs.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[2] < b[2];
    });

    int answer = 0, edgeCount = 0;
    for (auto& c : costs) {
        if (unite(c[0], c[1])) {
            answer += c[2];
            edgeCount++;
            if (edgeCount == n - 1) break;
        }
    }
    return answer;
}
```

### 시간복잡도
- 정렬: O(E log E)
- Union-Find: O(E α(n)) ≈ 거의 상수
- **전체: O(E log E)** (정렬이 병목)

---

## Prim 알고리즘

### 핵심 아이디어
하나의 트리를 시작점에서부터 **점점 넓혀나가며**, 매 순간 트리와 바깥을 잇는 간선 중 **가장 싼 것**을 선택한다. (정점 중심)

### 동작 과정
1. 임의의 시작 노드에서 출발, `pq`에 `(0, 시작노드)` 삽입
2. `pq`에서 최소 비용 노드를 꺼냄
   - 이미 방문(vis)한 노드면 스킵 (더 싼 경로로 이미 편입됨)
   - 방문 안 했으면 트리에 편입, `answer += cost`, `count++`
3. 방금 편입한 노드에서 연결된, 아직 미방문인 노드들을 `pq`에 삽입
4. `count == n`이 될 때까지 반복

### 구현
```cpp
#include <vector>
#include <queue>
using namespace std;

int solution(int n, vector<vector<int>> costs) {
    vector<vector<pair<int,int>>> adj(n); // adj[u] = {(cost, v), ...}
    for (auto& c : costs) {
        adj[c[0]].push_back({c[2], c[1]});
        adj[c[1]].push_back({c[2], c[0]});
    }

    vector<bool> vis(n, false);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, 0}); // 0번 노드에서 시작

    int answer = 0, count = 0;
    while (!pq.empty() && count < n) {
        auto [cost, u] = pq.top(); pq.pop();
        if (vis[u]) continue;

        vis[u] = true;
        answer += cost;
        count++;

        for (auto& [w, v] : adj[u]) {
            if (!vis[v]) pq.push({w, v});
        }
    }
    return answer;
}
```

### 주의점
- `pq`에는 같은 노드가 여러 번 들어갈 수 있음 (더 싼 간선이 나중에 발견되면 또 push)
- 꺼낼 때마다 `vis` 체크로 중복/사이클 방지 필수
- 트리의 정확한 모양(경로)은 알 수 없지만, 총 비용은 정확히 계산됨

### 시간복잡도
- 우선순위 큐 사용 시: **O(E log V)**
- 인접행렬 + 배열 사용(작은 그래프): O(V²)

---

## Kruskal vs Prim 비교

| | Kruskal | Prim |
|---|---|---|
| 접근 방식 | 간선 중심 | 정점 중심 |
| 전략 | 간선 비용순 정렬 후 사이클 안 만들면 채택 | 트리를 시작점에서부터 점점 확장 |
| 자료구조 | Union-Find | 우선순위 큐 + 방문배열 |
| 진행 중 상태 | 여러 조각(forest)으로 존재하다 마지막에 하나로 합쳐짐 | 항상 하나의 연결된 트리로 성장 |
| 시간복잡도 | O(E log E) | O(E log V) |
| 유리한 상황 | 간선이 적은 희소 그래프 | 간선이 많은 촘촘한 그래프 |

**공통점**
- 둘 다 그리디 알고리즘 (매 순간 지역 최선을 선택)
- cut property에 의해 그리디 선택이 전역 최적해로 이어짐이 증명됨
- 최종 MST의 총 비용은 항상 동일

---

## 그 외 MST 알고리즘 (참고)

### Borůvka 알고리즘 (1926년, 가장 오래된 MST 알고리즘)
- 매 라운드마다 **모든** 컴포넌트가 동시에 자신과 연결된 최소 비용 간선을 선택해 병합
- 한 라운드마다 컴포넌트 수가 최소 절반으로 감소 → O(log V) 라운드
- 시간복잡도: O(E log V)
- **병렬화에 유리**해서 분산/병렬 컴퓨팅 환경에서 사용됨

### Reverse-Delete 알고리즘
- Kruskal의 반대 발상: 비용이 **큰** 간선부터 보면서, 지워도 그래프가 연결되어 있으면 삭제
- 개념은 직관적이나 매번 연결성 확인이 필요해 비효율적 → 실무보다는 이론적 의미

### 실무 정리
코딩테스트/실무에서는 **Kruskal, Prim** 두 가지면 충분. Borůvka는 병렬 특수 상황, Reverse-Delete는 이론 위주.
