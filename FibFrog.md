# FibFrog
* [Report summary Link](https://app.codility.com/demo/results/trainingE6KYAY-7NK/)
# Task Details
> 강을 건너 반대편으로 가기 위해 개구리가 점프해야하는 회수의 최소값을 구하라.
# Task description
> 피보나치 수열은 다음과 같은 공식으로 재귀적으로 정의된다:
> * F(0) = 0
> * F(1) = 1
> * F(M) = F(M - 1) + F(M - 2) if M >= 2

> 작은 개구리가 강변을 건너 반대편으로 도착하고자 한다. 개구리의 처음 위치 -1에서 반대편 강변 위치 N으로 가고자 한다. 개구리는 K번째 피보나치 수인 위치 F(K)로 이동 가능하다. 운이 좋게도 강변에는 나뭇잎들이 많고, 개구리는 이 나뭇잎들 위로 뛰어서 반대 강변 방향(N)으로만 건널 수 있다.

> 강에 있는 나뭇잎은 N개의 정수로 이루어진 배열 A로 나타낼 수 있다. 0부터 N-1번째 위치에 나뭇잎이 있다면 1, 없다면 0이다. -1에서 N까지 나뭇잎이 있는 위치로 점프해서 반대편 강변으로 갈 수 있는 최소 점프 수를 구하라. 

예를 들어 배열 A가, 
>   A[0] = 0 
    A[1] = 0
    A[2] = 0
    A[3] = 1
    A[4] = 1
    A[5] = 0
    A[6] = 1
    A[7] = 0
    A[8] = 0
    A[9] = 0
    A[10] = 0
> 라는 배열이 주어졌을 때,  F(5) = 5, F(3) = 2 and F(5) = 5, 3번의 점프로 반대편으로 갈 수 있다.

> 다음 가정들을 만족하는 효율적인 알고리즘을 작성하라. 만약 반대편 강변으로 갈 수 없다면 -1을 리턴하도록 하라.
> * N은 [0..100,000] 사이의 정수다.
> * 배열 A의 각 원소는 0 또는 1로 이루어진 정수다.
<!--
# Logic
* [학습자료](https://github.com/Pyotato/codility_practice/tree/Fibonacci-numbers#%EC%98%88%EC%8B%9C-4--%EB%91%90-%EC%88%98%EC%9D%98-%ED%95%A9%EC%9D%B4-%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98-%EC%88%98%EC%9D%B8%EC%A7%80-%EC%97%AC%EB%B6%80)에 의하면 30개의 숫자로 1,000,000 이하의 피보나치 수를 log(n)시간으로 알 수 있다 (동적프로그래밍 활용).
* 
| Index | 0 | 1 | 2 | `3` | 4  | `5` | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| :--: | :--: |:--: | :--: |:--: | :--: |:--: |:--: |:--: |:--: |:--: |:--: |:--: |:--: |
| A[Index] | 0 |0 | 0 |`1` | `1` |0 |`1 |0 |0 |0 |0 | | |
-->
# Solution 
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : O(N * log(N))
* 풀이
```javascript
function solution(A) {
    let fib = new Array(30);
    fib[0] = 1; 
    fib[1] = 2;
    for (let i = 2; i < fib.length; i++) fib[i] = fib[i-1] + fib[i-2];
    let mins = new Array(A.length+1);

    for (let i = 0; i < mins.length; i++) {
        if (i < A.length && A[i] === 0) {
            mins[i] = -1;
            continue;
        }
        let min = Infinity;

        for (let j = fib.length-1; j >= 0; j--) {
            let k = i - fib[j];
            if (k < -1) continue;
            if (k === -1) {
                min = 1;
                break;
            }
            if (mins[k] < 0) continue;
            let newMin = mins[k] + 1;
            if (newMin < min) min = newMin;
        }
        if (min !== Infinity) mins[i] = min;
        else mins[i] = -1;
    }    
    
    return mins[A.length];
}
```
