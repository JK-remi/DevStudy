# Game AI - Path Finding

## 🎯 Goal
- Path Finding의 개념과 목적을 설명할 수 있다.
- BFS, DFS, Dijkstra, A* 알고리즘의 차이를 설명할 수 있다.
- A*에서 사용하는 Heuristic의 개념과 목적을 설명할 수 있다.
- Unity Navigation(NavMesh)의 개념과 주요 컴포넌트를 설명할 수 있다.
- Oracle War 프로젝트에 Path Finding 및 NavMesh를 적용하는 방법을 설명할 수 있다.
- A*와 Unity NavMesh의 차이를 설명할 수 있다.

---

# 1. Path Finding Algorithm

## Q1. Path Finding이란 무엇인가요?

### 답변

시작 지점에서 목표 지점까지의 최적 경로를 탐색하는 알고리즘입니다.  
게임에서는 NPC 이동, Navigation 등에 사용됩니다.

---

## Q2. BFS(Breadth First Search)란 무엇인가요? (언제 사용하는 것이 적합한가요? 포함)

- Queue
- Breadth
- Uniform Cost
- Shortest Path

### 답변

Queue를 이용하여 가까운 노드부터 탐색하는 알고리즘입니다.  
모든 간선의 비용이 동일한 경우 최단 경로를 보장하므로, 맵 이동 횟수가 동일한 게임에서 적합합니다.

---

## Q3. DFS(Depth First Search)란 무엇인가요? (언제 사용하는 것이 적합한가요? 포함)

- Recursion
- Stack
- Backtracking

### 답변

깊이 우선 탐색으로 한 경로를 끝까지 탐색한 뒤 되돌아옵니다.
모든 경로 탐색이나 백트래킹에 적합하지만,
최단 경로 탐색에는 적합하지 않습니다.

---

## Q4. Dijkstra 알고리즘이란 무엇인가요? (어떤 경우 사용하는 것이 적합한가요? 포함)

- Priority Queue
- G Cost
- Shortest Cost

### 답변

시작 지점부터 모든 노드까지의 최소 비용을 구하는 알고리즘입니다.
간선 비용이 서로 다를 때 최단 경로를 보장합니다.
하지만 목표 방향을 고려하지 않기 때문에 게임에서는 탐색 범위가 넓어집니다.

---

## Q5. A* 알고리즘이란 무엇인가요? (왜 게임에서 가장 많이 사용하는지도 포함)

- F = G + H(Heuristic)

### 답변

현재까지의 비용(G Cost)과 목표까지의 예상 비용(H Cost)을 더한
F Cost가 가장 작은 노드를 우선 탐색하는 알고리즘입니다.
목표 방향으로 탐색하기 때문에 탐색 범위가 줄어들어
게임에서 가장 많이 사용됩니다.

---

## Q6. A*에서 Heuristic이란 무엇인가요? (Heuristic을 사용하는 이유 포함)

- Estimated Cost
- Manhattan / Euclidean Distance

### 답변

Heuristic은 현재 위치에서 목표 지점까지의 예상 비용(H Cost)을 의미합니다.  
A*는 현재까지 이동한 비용(G Cost)뿐 아니라 앞으로 남은 비용(H Cost)도 함께 고려하여 탐색하기 때문에 목표 방향으로 효율적으로 탐색할 수 있습니다.  
대표적인 Heuristic으로는 맨해튼 거리 (상하좌우 이동) / 유클리드 거리 (직선 거리) 가 있습니다.

### 꼬리 질문

- Heuristic은 반드시 실제 거리와 같아야 하나요?

---

## Q7. Oracle War 프로젝트에 Path Finding을 적용한다면 어떻게 구현하시겠습니까?

### 답변

일반 NPC는 Unity NavMesh를 사용합니다.  
큰 맵이라면 구역을 나누고, 활성화된 구역에서만 Path Finding을 수행합니다.  
실제 이동은 Unity NavMeshAgent를 사용하여 처리하겠습니다.  

---

## Q8. BFS, DFS, Dijkstra, A*의 차이를 설명해주세요.

### 답변

BFS는 가까운 노드부터 탐색하며, 모든 간선 비용이 동일할 때 최단 경로를 보장합니다.  

DFS는 한 경로를 끝까지 탐색한 뒤 되돌아오는 방식으로, 최단 경로 탐색에는 적합하지 않습니다.  

Dijkstra는 현재까지의 누적 비용(G Cost)이 가장 작은 경로를 선택하여 최단 경로를 찾습니다.  

A*는 Dijkstra에 목표까지의 예상 비용(Heuristic)을 추가한 알고리즘으로, 목표 방향으로 탐색하기 때문에 게임에서 가장 많이 사용됩니다.  

---

# 2. Unity Navigation

## Q9. NavMesh란 무엇인가요?

### 답변

NavMesh는 Unity에서 제공하는 Navigation 시스템으로, 이동 가능한 영역을 Bake하여 생성한 Navigation Mesh입니다.  
NavMeshAgent는 이 Mesh 위를 따라 이동합니다.  

---

## Q10. NavMeshAgent란 무엇인가요? (주요 속성 포함)

- Destination / Speed / Acceleration / StoppingDistance 

### 답변

NavMesh 위를 이동하는 컴포넌트입니다.  
주요 속성으로는 destination / speed / acceleration / stoppingDistance 등이 있습니다.

---

## Q11. NavMeshObstacle이란 무엇인가요? (언제 사용하는 것이 적합한가요?)

- Dynamic Obstacle

### 답변

NavMeshAgent가 회피해야 하는 동적인 장애물입니다.
문, 엘리베이터, 움직이는 박스처럼 움직이는 오브젝트가 있을 때 사용합니다.

---

## Q12. OffMeshLink란 무엇인가요? (언제 사용하는 것이 적합한가요?)

- ex. Jump / Ladder / Gap

### 답변

NavMesh가 연결되지 않은 두 지점을 연결하는 기능입니다.  
점프, 사다리, 단차 이동 등에 사용합니다.  

---

## Q13. Local Avoidance란 무엇인가요?

- Steering

### 답변

NavMeshAgent끼리 서로 충돌하지 않도록 실시간으로 이동 경로를 조금씩 수정하는 기능입니다.

---

## Q14. Oracle War 프로젝트에 NavMesh를 적용한다면 어떻게 구현하시겠습니까?

### 답변

맵을 구성한 뒤 NavMesh를 Bake하고, NPC에는 NavMeshAgent를 적용합니다.  
움직이는 장애물은 NavMeshObstacle을 사용하고, 많은 NPC가 존재하는 경우에는 Local Avoidance와 Update 최적화를 함께 고려하겠습니다.

---

## Q15. A*와 NavMesh의 차이를 설명해주세요.

### 답변

A*는 길찾기 알고리즘입니다.  
  
NavMesh는 Unity에서 해당 알고리즘을 사용할 수 있도록 제공하는 Navigation 시스템입니다.  
  
개발자는 직접 A*를 구현하지 않아도 NavMeshAgent를 통해 쉽게 사용할 수 있습니다.
