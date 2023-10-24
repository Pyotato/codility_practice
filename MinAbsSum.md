# MinAbsSum
* [Report summary Link](https://app.codility.com/demo/results/trainingE6KYAY-7NK/)
# Task Details
> 정수 배열이 주어졌을때, 합의 절대값이 최소가 되는 경우를 구하라.
# Task description
> A라는 정수로 이루어진 배열과 N개의 연속된 set {-1,1}를 val(A,S)로 다음과 같이 정의할 수 있다.

> val(A,S) = |sum{A[i]*S[i] for i = 0...N-1}|
(0개의 원소의 합은 0이라고 가정)

> 배열 A가 주어졌을때 val(A,S)가 최소값이 되도록 만족하는 연속된 시퀀스 S를 구하는 함수를 구하라.

> N개의 원소가 있는 배열 A가 주어졌을 때, set {-1,1}에서 val(A,S)값이 최소가 되는 연속된 N개의 시퀀스 S를 만족하는 function solution(A)를 작성하라.

예를 들어, 
> A[0] = 1, A[1] = 5, A[2] = 2 , A[3]  = -2
> 라는 배열이 주어졌을 때, S={-1,1,-1,1}, val(A,S)=0이므로, 0을 리턴해야한다.

> 다음 가정들을 만족하는 효율적인 알고리즘을 작성하라.
> * N은 [0,...,20000] 사이의 정수다.
> * 배열 A의 각 원소는 [-100..100] 사이의 정수다.

# Logic
| index |0|1|2|3|
| :--: | :--: | :--: | :--: | :--: |
| A[index] |1|5|2|-2|
|  max sum | 1*(-1) = **-1**| -1 + (5*1) = **4**| 4 + 2*(-1) = **2** | 2 + (-2*(1))) = **0** |

# Solution 
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : O(N * max(abs(A))**2)
* 풀이
```javascript
function solution(A) {
    let N = A.length;
    let M = 0;
    for (let i = 0; i < N; i++) {
        A[i] = Math.abs(A[i]);
        M = Math.max(A[i], M);
    }
    let S = A.reduce((a, b) => a + b, 0);
    let count = Array(M + 1).fill(0);
    for (let i = 0; i < N; i++) {
        count[A[i]] += 1;
    }
    let dp = Array(S + 1).fill(-1);
    dp[0] = 0;
    for (let a = 1; a <= M; a++) {
        if (count[a] > 0) {
            for (let j = 0; j < S; j++) {
                if (dp[j] >= 0) {
                    dp[j] = count[a];
                } else if (j >= a && dp[j - a] > 0) {
                    dp[j] = dp[j - a] - 1;
                }
            }
        }
    }
    let result = S;
    for (let i = 0; i <= Math.floor(S / 2); i++) {
        if (dp[i] >= 0) {
            result = Math.min(result, S - 2 * i);
        }
    }
    return result;
}

```
