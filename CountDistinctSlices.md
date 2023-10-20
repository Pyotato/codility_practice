# CountDistinctSlices
* [Report summary Link](https://app.codility.com/demo/results/trainingKEDT6K-9H2/)
# Task Details
> 배열에서 연속된 부분이 유일한 수들로 이루어진 슬라이스의 개수를 구하라.
# Task description
> * 정수 M와 빈 배열이 아닌 N개의 양수로 이루어진 배열 A가 주어진다. A배열의 모든 원소는 M보다 작거나 같다.
> * 정수 쌍으로 이루어진 (P,Q)는 0<=P<Q<=N을 만족하는 배열 A의 슬라이스다. 슬라이스는 A[P], A[P + 1], ..., A[Q]로 이루어져 있다. <i>구분되는 슬라이스</i>란 유일한 숫자들로만 이루어진 슬라이스를 가르킨다. 즉, 슬라이스 내의 숫자는 단 한개씩만 존재한다. 

예를 들어 M = 6이고 A 배열이 주어졌을 때,
| A[index] | value |
| :--: | :--: |
| A[0] | 3| 
|  A[1] | 4| 
| A[2]  | 5 | 
| A[3] | 5 | 
| A[4]  | 2| 


> 정확히 9개의 구분되는 슬라이스가 있다: 
> * (0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2), (3, 3), (3, 4), (4, 4).

> 목표는 구분되는 슬라이스의 개수를 구하는 것이다.

> `function solution(M,A)`에 관해, 정수 M와 빈 배열이 아닌 N개의 원소를 가진 배열 A내의 구분되는 슬라이스의 개수를 구하라.

> 만약 구분되는 슬라이스의 개수가 1,000,000,000를 초과한다면 1,000,000,000를 리턴하도록해야한다.

아래의 가정들을 만족하는 **효율적인** 알고리즘을 작성해라.
> * N은 [1..100,000] 사이의 정수다.
> * M은 [1..100,000]  사이의 정수다.
> * A의 각 원소는 [0..M] 사이의 정수다. 

# Logic
| A[index] | A[0] | A[1] | A[2]  | A[3] | A[4]  |
| :--: | :--: | :--: | :--: | :--: | :--: |
| val | 3 | 4 | 5 | 5 | 2 |
| caterpillar | T-H |   |   |   |   |
| caterpillar | T   | H |   |   |   |
| caterpillar |     | T-H |   |   |   |
| caterpillar | T   |   | H  |   |   |
| caterpillar |     | T |  H |   |   |
| caterpillar |     |   | T-H  |   |   |
| caterpillar |     |   |      | T-H |   |
| caterpillar |     |   |      | T  |  H |
| caterpillar |     |   |      |    |  T-H |
* 애벌레의 꼬리(T)와 머리(H)를 A[T]와 A[H]에 둘 때, 머리가 점점 오른쪽 (앞)으로 간다.
* 슬라이스에 이미 값이 있다면 꼬리를 앞으로 하지만 없다면 애벌레의 길이에 추가한다.
  
| M+1 | 0 | 1 | 2 | 3 | 4 | 5 |6 | count|
| :--: | :--: | :--: | :--: | :--: | :--: |:--: |:--: |:--: |
| distinctCount[0] | 0 |0| 0 |1| 0 |0 |0 | 1 | 
| distinctCount[1] | 0 |0| 0 |1 |1 |0 |0 |3 |
| distinctCount[2] |0 |0| 0 |1| 1| 1| 0 |6|
| distinctCount[3] | 0| 0 |0| 0| 0| 1|0 | 7 |
| distinctCount[4] | 0| 0 |1| 0 |0| 1 |0 |9|

# Solution 
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(n)**
* 풀이
```javascript
function solution(M, A) {
    const len = A.length;
    let cnt = 0;
    let distinctCount = new Array(M + 1).fill(0); 
    let left = 0;

    for (let right = 0; right < len; right++) {
        while (distinctCount[A[right]] > 0) {
            distinctCount[A[left]]--;
            left++;
        }
        cnt += (right - left + 1);
        distinctCount[A[right]]++; 
    }

    return cnt > 1000000000 ? 1000000000 : cnt;
}
```
