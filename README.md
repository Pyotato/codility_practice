# Dynamic Programming

## Reading Material
* [[reference]](https://codility.com/media/train/15-DynamicProgramming.pdf)
### 동적 프로그래밍
> 연속된 더 작은 단위의 문제를 함으로써 결과를 도출해내는 문제해결 방식.
#### 예시 1 : 동전 바꾸기 문제
> 단위가 다른 동전들이 주워졌을 때 (ex. 500원, 100원, 50원, 10원), 최소 동전 수로 금액을 지불할 수 있는 경우의 수를 찾아라. 동전의 개수는 원하는 만큼 사용가능하다. 
* `그리디 알고리즘`적인 접근은 지불해야하는 금액을 넘지 않는 선에서 항상 가장 큰 동전 단위를 선택하는 방법이다. 지불해야하는 금액이 0보다 크다면 이 선택과정을 반복하면 된다. 하지만 이 알고리즘은 최선의 결과가 아닐 수 있다. 예를 들면 6개의 동전이 있고, 각각의 단위가 1,3,4일 경우 `6=4+1+1`라는 결과보다는 `6=3+3`이라는 최선의 결과다.
* 동적 알고리즘은 지불해야하는 금액을 초과하지 않으면서 각 동전의 단위를 증가시키면서 해결책을 찾는데 쓰일 수 있다. 예를 들면, `0부터 까지 모든 금액`과, `∅, {1}, {1, 3}, {1, 3, 4}`라는 동전 단위 세트를 고려할 수 있다.
* *dp[i, j*]에서 지불해야하는 금액을 위한 동전의 최소 개수 j와 동전 단위의 최소 종류  세트를 i라고 할 때, 동전개수는 다음의 규칙들을 반드시 지켜야한다.
1. 지불해야하는 금액이 0원이라면 동전들이 필요하지 않다: *dp[i, 0]* = 0 (모든 i에 관해)
2. 동전 단위가 없고 지불해야하는 금액이 양수라면 지불할 수 있는 방법이 없으므로 편의상 결과를 무한으로 할 수 있다 : *dp[0,j]* (모든 j>0에 관해)
3. 지불해야하는 금액이 가장 큰 동전의 단위 c<sub>i</sub>보다 작을 경우, 이 동전 단위는 버릴 수 있다 : dp[i, j] = dp[i − 1, j] (c<sub>i</sub> > j일 경우의 모든 i > 0 와 모든 j)
4. 위의 경우 이외에는 두가지 조건을 고려해서 더 적은 개수의 동전을 요구하는 경우를 고려하면 된다 : 가장 단위가 큰 동전단위를 사용하면서 적은 금액을 지불해야할 경우 남은 동전 단위로 매꾸거나, 가장 큰 동전 단위를 사용하지 않는 경우 (그 단위의 동전은 버리기) : 
: dp[i, j] = min(dp[i, j − c<sub>i</sub>] + 1, dp[i − 1, j]) (c<sub>i</sub> >= j일 경우의 모든 i > 0 와 모든 j)
* 위의 경우를 표로 나타내면 아래와 같은 작은 단위의 문제들로 쪼갤 수 있다. 

![image](https://github.com/Pyotato/codility_practice/assets/102423086/c7e3da8b-c7a1-405f-85d1-15bf374d18f1)

##### 구현
n개의 동전 단위  0 < c<sub>0</sub> <= c<sub>1</sub> <= . . . <= c<sub>n-1</sub> 가 있다고 하자. 알고리즘은 각각의 동전 단위와 0에서 k까지의 금액을 지불해야하는데 필요한 동전의 최소 개수를 계산한다 각 연속된 동전단위를 고려하면, 이전 연산결과를 통해 더 적은 동전 개수를 구한다. 

* 시간복잡도와 공간복잡도 *O*(n*k)인 풀이 :

```javascript
function dynamic_coin_changing (C,k){
  const n = C.length;
  // 0으로 채워진 2차원 배열 만들기
  const dp = new Array((n+1)).fill((new Array(k+1).fill(0)));
  dp[0].fill(Infinity);
  dp[0][0] = 0;
  for(let i=1; i<n+1; i++){
    for(let j in C[i-1])dp[i][j] = dp[i-1][j];
    for(let j= C[i-1] ; j<k+1; j++)dp[i][j] = Math.min(dp[i][j - C[i - 1]] + 1,  dp[i - 1][j]);
  }
  return dp[n];
}

```

* 위의 풀이에서 모든 행의 결과를 기억할 필요없이 이전 행의 결과만 필요하다는 점에서 메모리 최적화를 할 수 있다. 따라서 시간복잡도 *O*(n*k), 공간 복잡도 *O*(k)인 풀이로 최적화를 할 수 있다.

```javascript
 function dynamic_coin_changing (C,k){
    const n = C.length;
    const dp = new Array(k+1).fill(Infinity);
    dp[0] = 0;
    for(let i =1 ; i< n+1; i++){
      for(let j=C[i-1] ; j<k+1; j++){
        dp[j] = Math.min(dp[j-C[i-1]]+1,dp[j]);
      }
    }
    return dp;
  }

```

#### 예시 2 : 동전 바꾸기 문제
> 작은 개구리가 0에서 k(1<=k<=10000)로 이동하고 싶어한다. 개구리는 정해진 거리 n s<sub>0</sub>,s<sub>1</sub>,...,s<sub>n-1</sub> 중 어느 곳으로 점프를 할 수 있다. 목표는 위치 k로 점프를 할 수 있는 다른 경우의 수다. overflow를 피하기 위해서 q값이 주어졌을 때 결과%q를 만족하는 값을 구하라. k로 이동하는 두 경우가 다르다는 기준은 다른 경우에서 개구리가 가지 않은 위치로 점프했을 경우 다른 경우라고 한다.

* **Sol** *O(n*k)*인 경우
* 이 문제는 동적 프로그래밍을 활용해 풀이가 가능하다. k개의 원소를 가진 배열 dp를 만들고, j로 이동할 수 있는 경우의 수를 *dp[j]*로 나타내자.
* dp의 각 연속된 원소를 업데이트한다. 개구리가 위치0으로 점프를 할 수있는 경우는 한가징므로 *dp*[0] = 1 이다. 다음으로 j>0일 경우를 생각해보자. 
* 위치 j로 이동할 수 있는 최소 점프회수 s<sub>i</sub>는 *dp[j-s<sub>i</sub>*이다. 따라서 개구리가 위치 j로 이동할 수 잇는 경우의 수는 각 점프 s<sub>i</sub> 마다 위치 *j-s<sub>i</sub>*만큼 증가한다. 즉, *dp[j]*는 모든 s<sub>i</sub><= j 에 대해 *dp[j-s<sub>i</sub>]* 만큼 증가한다. 

##### 구현 : 시간복잡도가 O(n*k), 공간복잡도가 O(k)
```javascript
  function frog(S,k,q){
    const n = S.length;
    const dp = new Array(k+1).fill(0);   // k개의 원소를 가져야겠지만 index는 0부터 시작하므로 k+1이 아닌 k 위치로 인덱스 접근을 하기 위해 k+1 만큼 원소를 가지는 배열을 만들자.
    dp[0] = [1];  // 0으로 이동할 수 있는 경우의 수는 한가지 뿐
    for(let j = 1; j < k+1; j++){
      for(let i= 0; i<n; i++){
        if(S[i] <= j ) dp[j] = (dp[j]+dp[j-S[i]]) % q;
      }
    }
    return dp[k];
  }

```


----

# Sol

1. [ [Lesson 17 Dynamic programming](https://github.com/Pyotato/codility_practice/tree/Dynamic-programming) ]: [NumberSolitaire](https://app.codility.com/demo/results/trainingA7CPB4-DCS/)
2. [ [Lesson 17 Dynamic programming](https://github.com/Pyotato/codility_practice/tree/Dynamic-programming) ]: [MinAbsSum](https://app.codility.com/demo/results/trainingE6KYAY-7NK/)
