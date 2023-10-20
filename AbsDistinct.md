# AbsDistinct
* [Report summary Link : Sol1](https://app.codility.com/demo/results/trainingGRMB69-7DY/)
* [Report summary Link : Sol2](https://app.codility.com/demo/results/trainingQGCQK8-ZDS/) 
# Task Details
> 정렬이 된 배열의 원소들의 구별되는 절대값의 개수를 구하라.
# Task description
> * 빈 배열이 아닌 A는 N개의 숫자를 원소로 한다. 배열은 오름차순으로 정렬되어 있다. 이 배열에서 <i>구별되는 절대값</i>의 개수는 배열의 원소들을 절대값으로 바꿨을 때 구별되는 수의 개수다.

예를 들어 A 배열이 주어졌을 때,
| A |  |
| :--: | :--: |
| A[0] | -5 | 
| A[1] | -3 | 
| A[2] | -1 | 
| A[3] | 0 | 
| A[4] | 3 | 
| A[5] | 6 | 

> 위의 구별되는 절대값의 개수는 5개이다. 각 배열의 원소들을 절대값으로 치환했을 경우, 서로 다른 숫자들은 0,1,3,5,6 뿐이기 때문이다.

> * 아래의 가정들을 만족하는 **효율적인** 알고리즘을 작성해라.
>     * N은  [1..100,000]사이의 정수다.
>     * 배열 A의 각 원소는 [−2,147,483,648..2,147,483,647] 사이의 정수다.
>     * 배열 A는 오름차순으로 정렬되어 있다. 

# Logic
| Index | 0 | 1 | 2 | 3 | 4  | 5 | 
| :--: | :--: |:--: | :--: |:--: | :--: |:--: |
| A[Index] | -5 | -3 | -1 | 0 | 3 | 6 |
| Math.abs(A[Index]) | 5 | 3 | 1 | 0 | 3 | 6 |
| Math.abs(A[Index]) sorted | 0 | 1 | 3 | 3 | 5 | 6 |
| count  |1 | 2 | 3 |3 | 4 | 5 |
| caterpillar | T | H |  | |  |  | 
| caterpillar |  | T | H  | |  |  | 
| caterpillar |  |  | T | H |  |  | 
| caterpillar |  |  | T |  | H  |  |
| caterpillar |  |  |  |  | T  | H  | 
* A의 모든 값에 절대값을 취하고 오른차순으로 정렬하면 0,1,3,3,5,6이 된다.
* 애벌레방식으로 머리를 H, 꼬리를 T라고 할 경우 T와 H값이 다르다면 앞으로 H 와 T 모두 이동시켜준다. (머리의 위치+1하고 이전 머리의 위치에 꼬리가 간)
* 꼬리가 이동했을 경우만 count+1을 해준다.
* 값이 같을 경우 H만 앞으로 이동해준다.
* 배열의 끝에 도달하면 종료.

# Solution 1 : map을 활용한 풀
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(n)** 또는 **O(N*log(N))**
* 풀이
```javascript
function solution(A) {
    const absA = A.map((v)=>Math.abs(v));
    const setA = new Set(absA);
    return setA.size;
}
```
# Solution 2 : caterpillar method 활용한 풀이
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(n)** 또는 **O(N*log(N))**
* 풀이
```javascript
function solution(A) {
    if(A.length === 1) return 1;
    const abs = A.map((v) => v= Math.abs(v));
    abs.sort((a,b)=>a-b);
    let count = 1;
    let head = 1, tail = 0;
    while(head<A.length){
        if(abs[head]===abs[tail]){
            head++;
            continue;
        }
        head++;
        tail=head-1;
        count++;   
    }
    return count;

}
```
