# CountTriangles
* [Report summary Link : Sol1](https://app.codility.com/demo/results/trainingUVNVMX-M3Z/)
* [Report summary Link : Sol2](https://app.codility.com/demo/results/training4QSHGZ-DYM/)

# Task Details
> 주어진 set의 간선들로 만들 수 있는 삼각형의 개수를 구하라.
# Task description
> * 빈 배열이 아닌 A는 N개의 숫자를 원소로 한다. 세 쌍 (P,Q,R)로 A[P], A[Q], A[R]를 각 변으로 삼각형을 만들 수 있다면 (P,Q,R)은 <i>삼각형을 띤다</i>고 할 수 있다. 즉 0 <= P < Q < R < N 이고 아래의 조건을 만족한다면 세 쌍 (P,Q,R)은 삼각형을 띤다.
>     * A[P] + A[Q] > A[R]
>     * A[Q] + A[R] > A[P]
>     * A[R] + A[P] > A[Q]

예를 들어 A 배열이 주어졌을 때,
| A |  |
| :--: | :--: |
| A[0] | 10 | 
| A[1] | 2 | 
| A[2] | 5 | 
| A[3] | 1 | 
| A[4] | 8 | 
| A[5] | 12 | 

> 위의 구별되는 절대값의 개수는 4개이다 :  (0, 2, 4), (0, 2, 5), (0, 4, 5), (2, 4, 5). 

> * 아래의 가정들을 만족하는 **효율적인** 알고리즘을 작성해라.
>     * N은  [1..1000]사이의 정수다.
>     * 배열 A의 각 원소는 [1..1,000,000,000] 사이의 정수다.

# Logic
| A[index] | 1 | 2 | 5 | 8 | 10  | 12 | isTriangle |
| :--: | :--: |:--: | :--: |:--: | :--: |:--: |:--: |
| caterpillar | T | W | H | |  |  |1+2<5 -> False |
| caterpillar | T | W |  | H |  |  |1+2<8 -> False |
| caterpillar | T | W |  |  | H |  |1+2<10 -> False |
| caterpillar | T | W |  |  |  | H |1+2<12 -> False |
| caterpillar | T | | W  | H |  |  |1+5<8 -> False |
| caterpillar | T | | W  | | H  |  |1+5<10 -> False |
| caterpillar | T | | W  | | H  |  |1+5<12 -> False |
| caterpillar | T | | W  | | H  |  |1+8<10 -> False |
| caterpillar | T | | W  | | H  |  |1+8<12 -> False |
| caterpillar |  |T | W  |H |   |  |2+5<8 -> False |
| caterpillar |  |T | W  | | H  |  |2+5<10 -> False |
| caterpillar |  |T | W  | | H  |  |2+5<12 -> False |
| caterpillar |  | |  T | W| H  |  |5+8>10 -> True |
| caterpillar |  | |  T | W|   | H |5+8>12 -> True |
| caterpillar |  | |  T | | W  | H |5+10>12 -> True |
| caterpillar |  | |   |T | W  | H |8+10>12 -> True |
* 애벌레 방식을 활용하기 위해서 오름차순으로 정렬한다. (조건에 따라 머리나 꼬리를 이동시켜줘서 끝에 도달해야하기 때문.)
* 각 수에 놓인 애벌레의 몸을 꼬리(T), 허리(W), 머리(H)라고 할 때, 처음 위치로 꼬리는 정렬해준 A[0]에 놓이고 W은 A[0+1], H은 [0+2]에 놓인다.
* 위와 같이 A의 길이를 반복문으로 3번 돌면 시간 복잡도가 O(N<sup>3</sup>)가 되므로 시간 초과가 날 수 있다. 

| A[index] | 1 | 2 | 5 | 8 | 10  | 12 | isTriangle |
| :--: | :--: |:--: | :--: |:--: | :--: |:--: |:--: |
| caterpillar | T | W | H | |  |  |1+2<5 -> False (H이동X, T이동) |
| caterpillar | T | | W  | H |  |  |1+5<8 -> False (H이동X, T이동) |
| caterpillar |  |T | W  |H |   |  |2+5<8 -> False (H이동X, T이동) |
| caterpillar |  | |  T | W| H  |  |5+8>10 -> True(H이동, W도 이동) |
| caterpillar |  | |  T | W|   | H |5+8>12 -> True(H이동, W도 이동) |
| caterpillar |  | |  T | | W  | H |5+10>12 -> True(H이동, W도 이동) |
| caterpillar |  | |   |T | W  | H |8+10>12 -> True(H이동, W도 이동) |
* 대신  위와 같이 H를 이동해서 T+W>H인 경우가 더 존재하는 지 더 탐색해야할 경우는 T+W>H인 경우에만 돌도록 한다.

# Solution 1 : for문2번으로 돌고 count를 머리와 꼬리의 위치 이동만 있을 경우만 세주는 풀이
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(N**2)** 
* 풀이
```javascript
function solution(A) {
    if(A.length<3) return 0;
    let count = 0;
    A.sort((a, b) => a - b);

    for(let i=0; i<A.length-2; i++){
        let k = i + 2;
        for(let j=i+1; j<A.length-1; j++){
            while(k < A.length && A[i] + A[j] > A[k]){
                k++;
            }
            count += k - j - 1;
        }
    }

    return count;
}
```
# Solution 2 : while문 2번과 count를 하나씩 증가시키는 풀이
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(N**2)** 
* 풀이
```javascript
function solution(A) {
  if (A.length < 3) return 0;
  let count = 0;
  A.sort((a, b) => a - b);

  for (let t = 0; t < A.length - 2; t++) {
    let w = t + 1;
    while (w < A.length - 1) {
      let h = w + 1;
      while (h < A.length && A[t] + A[w] > A[h]) {
        count++;
        h++;
      }
      w++;
    }
  }

  return count;
}
```
