# [GPS](https://school.programmers.co.kr/learn/courses/30/lessons/1837)

## 문제 설명
카카오 택시 개발자 Jay-G는 다음 업데이트를 준비하기 위해 개선사항을 위한 여러 피드백을 받았다. 
그중에서 손님이 자주 탑승하는 위치를 추천해주었으면 한다는 의견이 많았다.

다음 업데이트 준비를 위해 Jay-G는 택시의 승하차 및 이동 경로를 수집하여 분석하기 시작하였다. 
데이터를 분석하던 Jay-G는 몇 가지 특이사항을 발견했다. 
택시의 이동 경로를 GPS를 통해 수집하게 되는데, GPS 신호 불량, 통신 오류 등 다양한 원인으로 위치의 오류가 발생한 것을 알게 되었다. 
다만 승차 위치와 하차 위치는 오류가 없는 것으로 확인이 되었다.

개발자 Jay-G는 수집한 이동 경로의 오류를 최소한으로 수정하여 좀 더 정확한 이동 경로를 구하고 싶어 한다.

택시는 다음과 같은 조건으로만 이동한다. 먼저 택시는 거점을 이동해 다니며, 거점 간의 이동은 해당하는 도로가 있는 경우에만 가능하다. 
또한, 교통 상황에 따라 택시는 한 거점에 머무를 수 있고, 왔던 길을 되돌아갈 수 있다. 모든 도로는 방향이 별도로 없는 왕복 도로이다.

<img src="https://t1.kakaocdn.net/codefestival/gps1.png">

예를 들어, 위 그래프에서 택시가 다음과 같이 시간대별로 이동 경로를 보내왔다.

|t|위치|
|-|--|
|1|1|
|2|2|
|3|3|
|4|3|
|5|6|
|6|7|

하지만 위의 택시가 보내온 경로에는 거점 3에서 거점 6으로 이동할 수 있는 도로가 없으므로 이동 경로에 오류가 있다.

<img src="https://t1.kakaocdn.net/codefestival/gps2.png">

이러한 오류를 최소한으로 수정하여 이동 가능한 경로로 만들고 싶다. 이 경우 1회의 오류를 수정하여 다음과 같이 이동 가능한 경로를 만들 수 있다. 시간 t=4의 위치를 거점 5로 한 번 수정하면 이동 가능한 경로가 된다.

|t|위치|
|-|--|
|1|1|
|2|2|
|3|3|
|4|5|
|5|6|
|6|7|

<img src="https://t1.kakaocdn.net/codefestival/gps3.png">

이와 비슷하게 시간 t=4의 위치를 거점 4로 바꾸거나, 시간 t=5 위치를 거점 5로 바꾸면 이동 가능한 경로로 만들 수 있다. 위의 경우 수정한 오류의 개수는 1개이다.

|t|위치|
|-|--|
|1|1|
|2|2|
|3|3|
|4|4|
|5|6|
|6|7|

|t|위치|
|-|--|
|1|1|
|2|2|
|3|3|
|4|3|
|5|5|
|6|7|

위와 같이 택시가 보내온 경로에서 이동 가능한 경로로 만드는 최소의 오류 수정 횟수를 구하여라.

## 입력 형식
주어지는 입력은 총 다섯 가지로, 거점 개수 n과 도로의 개수 m, 각 거점 간의 연결된 도로 정보 edge_list, 
택시가 시간대별로 보내오는 거점 정보의 총 개수 k, 그리고 머물렀던 거점의 정보 gps_log이다. 제한조건은 아래와 같다.

- 2 <= n <= 200
- 1 <= m <= 10,000
- 2 <= k <= 100
- edge_list는 m × 2 크기의 2차원 배열로, 각 행의 두 값은 도로가 잇는 두 거점의 번호를 의미한다.
- 거점의 번호는 1부터 n까지 숫자이다.
- 모든 도로는 양방향 통행이 가능하다.
- 입력되는 데이터에서 항상 모든 거점 간 경로가 있음이 보장되지 않는다.
- gps_log의 시작 거점과 도착 거점은 바뀔 수 없다.

## 출력 형식
이동 가능한 경로로 만들 수 있는 최소의 오류 수정 횟수를 리턴한다. 올바른 경로로 수정하는 것이 불가능할 경우 -1을 리턴한다.

## 예제 입출력
|변수명|값|
|---|----------------|
|n|7|
|m|10|
|edge_list|\[\[1, 2], \[1, 3], \[2, 3], \[2, 4], \[3, 4], \[3, 5], \[4, 6], \[5, 6], \[5, 7], \[6, 7]]|
|k|6|
|gps_log|\[1, 2, 3, 3, 6, 7]|
|answer|1|


|변수명|값|
|---|----------------|
|n|7|
|m|10|
|edge_list|\[\[1, 2], \[1, 3], \[2, 3], \[2, 4], \[3, 4], \[3, 5], \[4, 6], \[5, 6], \[5, 7], \[6, 7]]|
|k|6|
|gps_log|\[1, 2, 4, 6, 5, 7]|
|answer|0|

### 예제에 대한 설명
두 예제 모두 edge_list의 데이터는 본문의 그림과 같은 예이다.
첫 번째 테스트 케이스에서 gps_log로 주어진 경로 중 거점 3에서 거점 6으로 가는 도로가 없다. 여기서 시간 t=4의 위치를 거점 5로 한 번 수정하면 이동 가능한 경로가 된다.
두 번째 테스트 케이스는 gps_log로 주어진 경로가 모두 도로로 연결된 경우이므로 수정이 필요 없다.

---

## ver1. BFS 전 쌍 최단 거리 테이블 + 구간 단위 그리디 보정
```c++
void func(const int n, vector<vector<int>> &grid, const int s, const int e)
{
    if(s==e || grid[s][e]>0) return;
    
    vector<bool> vis(n+1, false);
    queue<pair<int,int>> q;
    q.push({s,grid[s][s]});
    vis[s]=true;
    for(int cur,cost; !q.empty(); q.pop())
    {
        cur=q.front().first;
        cost=q.front().second;
        
        for(int i=1; i<=n; i++)
        {
            if(vis[i] || grid[cur][i]!=1) continue;
            if(i==e)
            {
                grid[e][s]=grid[s][e]=cost+1;
                return;
            }
            
            q.push({i,cost+1});
        }
    }
}

int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    vector<vector<int>> grid(n+1, vector<int>(n+1, 0));
    for(int i=1; i<=n; i++)
        grid[i][i]=1;
    for(int i=0; i<m; i++)
        grid[edge_list[i][0]][edge_list[i][1]]=grid[edge_list[i][1]][edge_list[i][0]]=1;
    
    for(int i=1; i<=n; i++)
        for(int j=i+1; j<=n; j++)
            func(n, grid, i, j);
    
    int answer = 0;
    for(int i=1, l,r,e; i<gps_log.size(); i++)
    {
        if(grid[gps_log[i-1]][gps_log[i]]==1) continue;
        
        for(l=i-2; l>0; l--)
            if(grid[gps_log[l]][gps_log[i]]==i-l) break;
        for(r=i+1; r<n; r++)
            if(grid[gps_log[i-1]][gps_log[r]]=r-(i-1)) break;
        e=min(i-l,r-i+1);
        
        if(e==1) continue;
        
        answer+=e-1;
        if(e>i) i=e;
    }
    
    return answer;
}
```
### 시간복잡도
- BFS = O(n^2) (인접 행렬 스캔)
- BFS n^2/2 반복 = O(n^4)

### 동작 원리
- 인접 행렬 grid를 만들고, 모든 정점 쌍 (i,j)에 대해 BFS를 돌려 최단 거리를 grid에 채움
- GPS로그를 앞에서부터 훑으며 이전 로그와 현재 로그가 연결되지 않은 지점(오류 구간)을 찾음
- 그 지점과 앞쪽과 뒤쪽에서 최단 거리와 시간 간격이 맞는 로그를 찾아 둘 중 짧은 구간만큼 수정 횟수를 더함
- 처리한 구간은 건너뛰도록 i를 점프

### 문제점
- 구간을 국소적으로 고치는 그리디라 전체 최적 보장 X
- 시작 비용을 grid\[s]\[s]=1로 잡아 거리가 실제보다 1 크게 저장
- grid의 0이 **도달 불가** & **미계산**구분 X
- grid의 1이 **간선** & **자기 자신** & **거리 1**의 의미가 섞임

---
## ver+. (시점, 정점) 상태 DP
```c++
int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    const int INF=k+1;
    
    vector<vector<bool>> grid(n+1, vector<bool>(n+1, false));
    for(int i=1; i<=n; i++)
        grid[i][i]=1;
    for(int i=0; i<m; i++)
        grid[edge_list[i][0]][edge_list[i][1]]=grid[edge_list[i][1]][edge_list[i][0]]=1;
    
    vector<vector<int>> dp(k, vector<int>(n+1, INF));
    dp[0][gps_log[0]]=0;
    for(int t=1; t<k; t++)
    {
        for(int v=1, tmp; v<=n; v++)
        {
            tmp=INF;
            for(int u=1; u<=n; u++)    
            {
                if(!grid[u][v]) continue;
                tmp=min(tmp, dp[t-1][u]);
            }    
            
            dp[t][v]=tmp;
            if(gps_log[t]!=v) dp[t][v]++;
        }
    }   
    
    int answer = dp[k-1][gps_log.back()];
    if(answer>=INF) answer=-1;
    
    return answer;
}
```
### 시간복잡도
- O(S * n^2)
- 공간 = O(S*n + n^2)

### 동작 원리
- grid에 자기 자신과 간선을 표시 (자기 자신 true로 두어 **제자리 머무름** 허용)
- dp\[t]\[v]는 t번째 시점에 v에 있을 때의 최소 수정 횟수
  - 시작은 dp\[0]\[grid_log\[0]]=0, 나머지는 INF
- 전이는 v와 연걸된 모든 u의 dp\[t-1]\[u]최솟값에 gps_log\[t]!=v이면 +1
- 도착지는 dp\[S-1]\[gps_log.back()]로 고정, 불가능하면 -1 반환

---

## Tags

Algorithm
- DP
- Graph

STL
- vector

Concept
- State DP
- Adjacency Matrix
- Transition

Lesson
- DP 상태 정의
- 인접 행렬
- 제자리 머무름
- 롤링 배열
