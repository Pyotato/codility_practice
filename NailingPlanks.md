# NailingPlanks
* [Report summary Link : Sol](https://app.codility.com/demo/results/trainingGDBM2S-NKR/)

# Task Details
> 나무판을 고정시킬 수 있는 최소한의 못 개수를 구하라.
# Task description
> * 빈 배열이 아닌 배열 A와 B는 N개의 정수로 이루어져있다. 이 배열들은 N개의 나무판을 나타낸다. 즉 A[K]는 K번째 나무판의 앞이고 B[K]는 끝부분에 해당된다.
> * 못 C[I]가 A[K] <= C[I] <= B[K]라면 (A[K], B[K])는 고정되어 있다고 한다.
> * 목표는 모든 나무판이 고정될 때까지 사용된 못의 개수가 최소가 되도록하는 것이다. 즉, J개의 못으로만 모든 판을 고정시킬 수 있도록 하는 J를 구하라. 각 판에는 정확히 못 C[I]가 있어야 하고, 모든 판  (A[K], B[K]) 에 관해  0 ≤ K < N 를 만족해야하며,  I < J 와 A[K] ≤ C[I] ≤ B[K]를 만족해야한다.

예를 들어 배열 A과B가 주어졌을 때,
|Index| A | B  | 
| :--: | :--: |:--: |
| [0] | 1 | 4|
| [1] | 4 | 5 |
| [2] | 5 | 9 |
| [3] | 8 | 10 |

> 다음과 같이 4개의 나무판이 있다 : [1, 4], [4, 5], [5, 9]  [8, 10].

예를 들어 배열 C가 주어졌을 때,
|Index| C |
| :--: | :--: |
| C[0] | 4 | 
| C[1] | 6 | 
| C[2] | 7 | 
| C[3] | 10 |
| C[4] | 2 |

> 다음과 같이 못을 사용한다면 :
> * 0번째 못을 사용하면 나무판 [1, 4] [4, 5]이 고정된다.
> * 0, 1번째 못을 사용하면 나무판 [1, 4], [4, 5] and [5, 9] 이 고정된다.
> * 0, 1, 2번째 못을 사용하면 나무판 [1, 4], [4, 5], [5, 9]이 고정된다.
> * 0, 1, 2, 3번째 못을 사용하면 모든 나무판이 고정된다.

따라서 모든 나무판을 고정하기 위해 연속적으로 사용했을 때, 최소로 사용할 수 있는 못의 개수는 4개이다. 

> * 빈 배열이 아닌 N개의 원소로 이루어진 A와 B와 M개의 원소로 이루어진 비지 않은 배열 C가 주어졌을때, 연속적으로 못을 사용했을 때 모든 나무판을 고정시킬 수 있도록하는 못의 최소 개수를 구하라.
> * 모든 나무판을 고정할 수 있는 경우가 존재하지 않는다면 -1을 리턴해라.

> * 아래의 가정들을 만족하는 **효율적인** 알고리즘을 작성해라.
>     * N과 M는 [1..30,000] 사이의 정수다.
>     * 배열 A,B,C의 각 원소는  [1..2*M]사이의 정수다.
>     * A[K] <= B[K].

# Logic

| A | 1 | 4 | 5 |8|
| :--: | :--: |:--: | :--: |:--: |
| B | 4 | 5 | 9 |10|
| C[2]=7 | O | O | O | X |
| C[3]=10 | O | O | O | O | 

* 못의 개수의 start = 0 이고, end = C의 마지막 인덱스 (4)라고 할 때
* 중간값은 mid = (start + end)/2 = 2 이다.
* C[2] = 7 인 경우 나무판 0,1,2을 고정할 수 있다.
* 아직 모든 판을 고정할 수 않았기 때문에 mid = ((start+1)+ end) )/2 = 3 일 경우 C[3] = 10 이고 모든 나무판을 고정할 수 있으므로 3을 리턴해준다.
* 반대로 만약 C[2]이 모든 나무판을 고정할 수 있었다면 mid= ((end-1)+start) )/2 = 1, C[1] 의 경우로 모든 나무판을 고정할 수 있는 지 확인한다. 만약 가능하다면 최소의 경우를 찾을 때까지 반복한다.

# Solution 
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O((N + M) * log(M))** 
* 풀이
```javascript
function solution(A, B, C) {
    let begin = 0;
    let end = C.length - 1;
    let result = -1;
    while (begin <= end) { //O(log(M))
        let mid = parseInt((begin + end) / 2, 10);
        if (check(A, B, C, mid+1)) { //O(M+N)
            end = mid - 1;
            result = mid+1;
        } else {
            begin = mid + 1;
        }
    }
    return result;
}

function check(a, b, c, num) {
    let nails = new Array(2*c.length + 1).fill(0);
    for (let i = 0; i < num; i++) { //log(M)
        nails[c[i]]++;
    }
    for (let i = 1; i < nails.length; i++) { //log(2M)
        nails[i] += nails[i-1];
    }
    for (let i = 0; i < a.length; i++) { //log(N)
        if (nails[b[i]] <= nails[a[i]-1]) return false;
    }
    return true;
}
```
