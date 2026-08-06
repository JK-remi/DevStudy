# [N으로 표현](https://school.programmers.co.kr/learn/courses/30/lessons/42895#)

## 문제 설명
아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)  
12 = 55 / 5 + 5 / 5  
12 = (55 + 5) / 5  

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.  
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.  

## 제한사항
- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

## 입출력 예
|N|number|return|
|--|---|--|
|5|12|4|
|2|11|3|

### 입출력 예 설명
#### 예제 #1
문제에 나온 예와 같습니다.

#### 예제 #2
11 = 22 / 2와 같이 2를 3번만 사용하여 표현할 수 있습니다.

---

## ver1. BFS
```
const char blank = ' ';
const char op[] = {'+', '-', '*', '/', blank};
int func(const string &s)
{
    int r=0;
    string tmp;
    char pre = blank;
    for(int i=0, idx=s.length(); i<s.length(); i=idx+1)
    {
        idx = s.find(blank, i);
        if(idx==string::npos) break;
        
        tmp = s.substr(i, idx-i);
        try
        {
            switch(pre)
            {
                case '+': r+=stoi(tmp); break;
                case '-': r-=stoi(tmp); break;
                case '*': r*=stoi(tmp); break;
                case '/': r/=stoi(tmp); break;
                default: r=stoi(tmp);
            }
        }
        catch(...) { pre = tmp[0]; }
    }
    
    return r;
}

string str_form(const string &s, char c, const string &n)
{
    return s + blank + c + blank + n;
}

int MAX_CNT = 9;
int solution(int N, int number) {
    int answer = -1;
    
    queue<pair<string, int>> q;
    const string sN = to_string(N);
    q.push({sN, 1});
    q.push({'-'+sN, 1});
    for(pair<string,int> p; !q.empty(); q.pop())
    {
        p = q.front();
        if(p.second == MAX_CNT) break;
        if(func(p.first+blank)==number) 
        {
            answer = p.second;            
            break;
        }
                
        for(int i=0; i<5; i++)
        {
            if(op[i]!=blank) q.push({str_form(p.first, op[i], sN), p.second+1});
            else q.push({p.first+sN, p.second+1});
        }
    }
    
    return answer;
}
```
### 시간복잡도  
- 루트 2개(N, -N), 각 단계 5갈래(+,-,*,/,concat(연속N)) 분기 → depth i에서 노드 수 = O(5^i)
- 1~8단계 전체 노드 수 합 = O(Σ 5^i) ≈ O(5^8) ≈ 39만 개 * 2(루트 2개) ≈ 78만 개
- 노드 하나마다
  - 문자열 연결(substr, +) = O(i)
  - func()에서 토큰마다 stoi 시도 = 예외 발생/캐치 = 문자열 길이당 최대 i/2번 예외
- 예외(try/catch) = c++에서 스택 언와인딩 비용 큼 = 일반 분기보다 수백~수천 배 느림
- 종합: O(5^8 * i) 문자열/파싱 연산 + 예외 처리로 인한 큰 상수 배율

BFS로 문자열 수식을 한 글자씩 늘려가면서 func()로 수식 계산해 number와 같아지는 최소 개수 찾음.

### 문제점
- 우선 순위(괄호) 표현 못함: 사칙연산 + 괄호 모두 허용하는데, 왼쪽부터 차례로 계산만 함(= 왼쪽 결합)
- 중복 상태 재탐색: 동일한 값을 여러 수식이 반복 생성하여 탐색 공간이 기하급수적으로 증가
- 예외 기반 파싱: stoi 실패를 catch로 잡아 연산자 구분 = 예외처리 비용 큼, 의도 파악 어려움
- 불필요한 확장: MAX_CNT에서 멈추긴 하지만 count=8의 자식을 이미 다 만듦 = 불필요한 문자열 생성
- 매 5단계 분기로 최악엔 5^8 = 약 39만개 문자열 + 문자열 연산 비용까지 하면 비효율적

---

## ver+. DP
```
int solution(int N, int number) {
    vector<unordered_set<long long>> dp(9); // dp[i]: N을 i번 써서 만들 수 있는 값들

    for (int i = 1; i <= 8; i++) {
        // 1) N을 i번 이어붙인 수 (5, 55, 555, ...)
        long long concat = 0;
        for (int k = 0; k < i; k++) concat = concat * 10 + N;
        dp[i].insert(concat);

        // 2) i개를 (j개, i-j개)로 나눠서 모든 조합을 사칙연산으로 결합
        //    → j를 1..i-1 전부 시도하는 것이 곧 모든 괄호 위치를 시도하는 것과 같음
        for (int j = 1; j < i; j++) {
            for (long long a : dp[j]) {
                for (long long b : dp[i - j]) {
                    dp[i].insert(a + b);
                    dp[i].insert(a - b);
                    dp[i].insert(a * b);
                    if (b != 0) dp[i].insert(a / b);
                }
            }
        }

        if (dp[i].count(number)) return i;
    }

    return -1;
}
```
### 시간복잡도
- dp[i]의 실제 원소 수는 N<=9, i<=8 범위에서 경험적으로 수백~수천 개 수준으로 유지
  - 사칙연산 조합 수는 크지만 실제 값 다양성은 적음
- i를 구할때: Σ_{j=1}^{i-1} |dp[j]| × |dp[i-j]|
- 전체: O(Σ_{i=1}^{8} Σ_{j} |dp[j]|·|dp[i-j]|) = 가장 큰 dp 크기를 M이라 하면 O(8 * M^2)  

### 동작 방식
- N을 i번 써서 만들 수 있는 모든 값의 집합을 DP에 누적  
- dp\[i]를 만들 때 dp\[j]와 dp\[i-j]의 모든 조합을 사칙연산으로 묶으면 괄호 배치(트리 모양) 자동 커버
  - dp\[j], dp\[j-i]가 이미 그 안에서 가능한 모든 괄호 배치 담고 있으므로 재귀적으로 모든트리 모양 커버

---

## Tags

### Algorithm

- Dynamic Programming
- BFS

### STL

- vector
- unordered_set

### Concept

- Dynamic Programming
- State Compression
- Expression Generation
- Divide and Combine

### Lesson

- DP 상태 정의
- 점화식 설계
- 괄호 표현
- 상태 중복 제거
