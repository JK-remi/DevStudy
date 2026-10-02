# [N-Queen](https://school.programmers.co.kr/learn/courses/30/lessons/12952)

## 문제 설명
가로, 세로 길이가 n인 정사각형으로된 체스판이 있습니다. 체스판 위의 n개의 퀸이 서로를 공격할 수 없도록 배치하고 싶습니다.

예를 들어서 n이 4인경우 다음과 같이 퀸을 배치하면 n개의 퀸은 서로를 한번에 공격 할 수 없습니다.

<img src="https://i.imgur.com/lt2zdK6.png">
<img src="https://i.imgur.com/5c5EUrq.png">

체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.

## 제한사항
- 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
- n은 12이하의 자연수 입니다.

## 입출력 예
|n|result|
|---|---|
|4|2|

### 입출력 예 설명
#### 입출력 예 #1
문제의 예시와 같습니다.

--- 

## ver1. 백트래킹 (vis: 열 번호, 호출마다 can[] 재계산)
```c++
void dfs(const int n, vector<int> &vis, int &ans)
{
    if(vis.size()==n)
    {
        ans++;
        return;
    }
    
    vector<bool> can(n, true);
    for(int i=0, tmp; i<vis.size(); i++)
    {
        can[vis[i]]=false;
        tmp = vis[i]-(vis.size()-i);
        if(tmp>=0) can[tmp]=false;
        tmp = vis[i]+(vis.size()-i);
        if(tmp<n) can[tmp]=false;
    }
    
    for(int i=0; i<n; i++)
        if(can[i]) 
        {
            vis.push_back(i);
            dfs(n, vis, ans);
            vis.pop_back();
        }
}

int solution(int n) {
    vector<int> vis;
    int answer = 0;
    dfs(n, vis, answer);
    return answer;
}
```
### 시간복잡도
- 탐색 노드 수를 N(n)이라 하면, 노드당 can 계산 O(r) + 후보 순회 O(n) = O(n)이고, 총 **O(N(n)·n)**
- N(n)은 정확한 닫힌 식이 없지만 가지치기 덕분에 n!보다 작음
  - 상한 = O(n!)
- 공간은재귀 깊이 n에 vis와 호출당 can(n)이 쌓여 O(n^2)
  - 비트 압축된 vector<bool이라 실제로는 작음

### 동작 원리
- vis\[i]는 i행에놓은 퀸의 열 번호이고, vis.size()가 현재 행
- 호출마다 can\[n]을 새로 만들고, 이미 놓인 퀸마다 같은 열과 두 대각선 **(vis\[i] ± (현재 행-i))** 을 false로 막음
- can\[i]가 true인 열만 push_back → 재귀 → pop_back
- vis.size()==n이 되면 유효한 배치이므로 ans++

### 문제점
- 호출마다 vector<bool 힙 할당 일어나므로, 상수 비용 큼
- 막힌 칸을 매번 처음부터 다시 계산
- 비트마스크 같은 최적화로 확장하기 어려움 

---

## ver+. 백트래킹 (열, /대각선, \대각선 점유 배열, O(1) 충돌 검사)
```c++
void dfs(const int n, vector<vector<bool>> &vis, int r, int &ans)
{
    if(r==n)
    {
        ans++;
        return;
    }
    
    for(int i=0; i<n; i++)
    {
        if(vis[0][i]||vis[1][i+r]||vis[2][i-r+n-1]) continue;
        
        vis[0][i]=vis[1][i+r]=vis[2][i-r+n-1]=true;
        dfs(n,vis,r+1,ans);
        vis[0][i]=vis[1][i+r]=vis[2][i-r+n-1]=false;
    }
}

int solution(int n) {
    vector<vector<bool>> vis;
    vis.push_back(vector<bool>(n,false));
    vis.push_back(vector<bool>(2*n-1, false));
    vis.push_back(vector<bool>(2*n-1, false));
    int answer = 0;
    dfs(n, vis, 0, answer);
    return answer;
}
```
### 시간복잡도
- 노드당 후보 n개를 O(1)씩 검사하므로 총 **O(N(n) · n)**
  - ver1과 점근적으로 같으나 can 재계산과 힙 할당이 없으므로, 상수 줄어듦
- 공간은 배열 O(n) + 재귀 스택 O(n) = **O(n)** 

### 동작 원리
- dfs(r)은 0~r-1행까지 놓은 상태에서 r행을 채움
- vis\[0]\[c]는 열, vis\[1]\[r+c]는 /대각선, vis\[2]\[r-c+n-1]은 \대각선 점유 여부
- 세 곳 중 하나라도 true면 continue, 아니면 셋 다 true 표시 → 재귀 → false 복구
- r==n 이면 ans++

---

## Tags

Algorithm
- Backtracking
- DFS
- Brute Force

STL
- vector
- vector<bool>

Concept
- N-Queen
- Pruning
- Diagonal Index
- Visited
- Undo

Lesson
- 백트래킹
- 완전탐색
- DFS 재귀
- 가지치기
- 대각선 인덱스
- 방문 처리
- 상태 복구
