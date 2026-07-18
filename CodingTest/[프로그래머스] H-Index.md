# [H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

## 문제 설명
H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.  

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.  

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.  

## 제한사항
과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.  
논문별 인용 횟수는 0회 이상 10,000회 이하입니다.  

## 입출력 예  
|citations|return|
|---|---|
|\[3, 0, 6, 1, 5]|3|

### 입출력 예 설명
이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

## ver1. 맵 카운팅
```
int solution(vector<int> citations) {
    const int cnt = citations.size();
    map<int,int> m;
    for(int i=0; i<cnt; i++)
        m[citations[i]]++;
    
    int answer = 0;
    int up=cnt;
    for(auto it=m.begin(); it!=m.end(); answer++)
    {
        if(it->first<=answer)
        {
            up-=it->second;
            ++it;
        }
        
        if(answer>=up)
            break;
    }
    
    return answer;
}
```
시간복잡도: map삽입(O(N logN)) + 탐색(O(N)) = O(N logN)  
map<인용 횟수, 개수> 로 압축하여 후보값을 0부터 늘려가며 판별  

## ver+. 배열 카운팅
```
int solution(vector<int> citations) {
    int n = citations.size();
    vector<int> cnt(n + 1, 0);
    for (int c : citations)
        cnt[min(c, n)]++;   // n보다 큰 인용수는 n으로 클램핑

    int total = 0;
    for (int h = n; h >= 0; h--) {
        total += cnt[h];
        if (total >= h) return h;
    }
    return 0;
}
```
시간복잡도: 배열 카운팅(O(N)) + 누적, 조건 검사(O(N)) = O(N)  
min(c,n): 논문이 n편밖에 없으므로, h-index는 n을 넘길 수 없음  
total: 인용 수가 h이상인 논문의 누적 개수  
역순 탐: h-index는 조건을 만족하는 가장 큰 h찾는 문제 

---

## Tags

Algorithm
- Counting
- Sorting

STL
- vector
- map

Concept
- Frequency Counting
- Prefix Sum
- H-Index

Lesson
- 카운팅
- 누적합
- 시간 복잡도
- 문제 분석
