# Greedy algorithms

## Reading Material
* [[reference]](https://codility.com/media/train/14-GreedyAlgorithms.pdf)
### 그리디 알고리즘
> 최적의 해를 구하기 위해서는 연속된 단계나 선택의 결과를 고려해야한다. 그리디 프로그래밍은 선택의 순간에 가장 최적의 결과를 선택해서 결과를 도출하는 결과론적인 문제 해결방식이다. 다시말해 현재의 단계에서 가장 최적의 결정을 선택해서 해를 내는 방식이다.

> 문제에 따라서 그리디 알고리즘을 활용한 풀이는 최적의 접근일 수도 있고, 아닐 수도 있다. 만약 최적의 접근방식이 아닐 경우, 정답과 근사한 결과는 도출하지만 차선의 결과를 도출한다. 이 경우 dynamic programming이나 brute-force를 사용하는 경우가 최선책이다. 반면 풀이가 최적의 접근법인 경우, dp나 brute-force 보다 실행시간이 더욱 빠르다. 
#### 예시 1 : 동전 교환 문제
> 주어진 동전 단위 set이 있다고 할 때, 지불해야하는 금액을 최소한의 동전 개수로 할 경우의 수를 구하라. 이 문제는 지불금액을 초과하지 않는 가장 큰 동전 단위를 선택하고 나머지값을 지불하는 방식으로 그리디 알고리즘을 통해 접근 가능하다. 지불해야하는 금액이 0보다 클 경우 이과정을 계속 반복한다.

> 정답인 알고리즘은 항상 최소 동전개수를 리턴해야한다. 하지만 그리디 알고리즘은 어떤 동전 단위를 선택할 경우는 정답을 도출하지만 항상 그렇지는 않다는 걸 알 수 있다. 예를 들어 동전의 종류가 1,2,5인 경우 알고리즘은 지불해야하는 금액에 관한 최적의 동전 수를 항상 리턴하지만 동전 종류 1,3,4 일 경우, 차선의 결과를 리턴한다. 그리디 알고리즘을 활용해 풀이를 하면 6을 지불해야하는 경우 4,1,1 동전 3개를 사용한다는 답이 나오지만, 실제로 최적의 동전 개수는 3,3으로 2개다.

##### 구현
* n개의 도언 종류와, 지불해야하는 금액k에 관해 0<=m<sub>0</sub><=m<sub>1</sub><=...<=m<sub>n-1</sub>라고 가정할 때

```javascript
function greedyCoinChanging(M,k){
  const n = M.length;
  const result = [];
  for(let i= n-1; i>-1;i--){
    result.push([M[i], Math.floor(k/M[i])]);
    k %= M[i];
  }
  return result;
}
```
* 위의 함수는 동전의 종류와 동전의 개수를 짝지어 리스트로 리턴한다. 이 알고리즘의 시간 복잡도는 동전하나가 증가할수록 증가하므로 *O*(n)이다.

#### 정확도 증명
> 연속적인 선택을 텅해 최선의 해결법을 구축하면 귀납법을 통해 증명가능하다 : 지금까지 선택한 결과들이 일정하게 최선의 답을 포함했다면, 앞으로 할 선택 또한 (첫 선택포함) 최선의 답을 포함하고 있어야한다.

#### 예시 2
> 문제 : 각각 1<=w<sub>0</sub><=w<sub>1</sub><=...<=w<sub>n-1</sub><=10<sup>9</sup> 사이의 무게를 지닌 n(n>0)명의 카누 선수가 있다. 목표는 최대 무게 k를 만족하도록 최소한의 더블 카누로 모든 인원을 배치하는 것이다. w<sub>i</sub><=k라고 가정하라.

> * 해결방안A <i>O</i>(n): 그리디 알고리즘을 활용해서 문제를 해결할 수 있다. 가장 무거운 카누 선수를 <i>heavy</i>라고 하자. <i>heavy</i>와 같이 않을 수 있는 카누 선수들을 <i>light</i>라고 하자. 나머지 카누 선수들도 <i>heavy</i>라고 부르자.
* 아이디어는 가장 무거운 <i>heavy</i>와 같이 앉을 수 있는 가장 무거운 <i>light</i>를 찾는 것이다. 따라서 가장 무거운 <i>heavy</i>와 가장 무거운 <i>light</i>를 같이 배치하면 된다. 가장 무거운 <i>heavy</i>가 가벼울 수록 <i>light</i>은 더 무거울 수 있다는 점을 주목하자. 따라서 <i>heavy</i>와 <i>light</i>는 <i>light</i>와 무게가 비슷해질때 쯤, <i>heavy</i>와 <i>light</i>의 경계가 바뀔 것이다.

`※원문 pdf는 파이썬으로 구현되어있다`
##### 구현 방법 A: O(n)인 경우
```javascript
function greedyCanoeistA(W,k){
    const N = W.length;
    const lightDq = [];
    const heavyDq = [];
    for(let i=0; i<N-1; i++){
      if(W[i]+W[N-1]<=k) lightDq.push(W[i]);
      else heavyDq.push(W[i]);
    }
    heavyDq.push(W[N-1]);
    let canoes = 0;
    while (lightDq.length!==0 || heavyDq.length!==0){
      if(lightDq.length > 0) lightDq.pop();
      heavyDq.pop();
      canoes++;
      if(heavyDq.length && lightDq.length) heavyDq.push(lightDq.pop())
      while (heavyDq.length > 1 && heavyDq[heavyDq.length-1]+heavyDq[0]<=k) lightDq.push(heavyDq.shift())
    }
    return canoes;
  }
```
##### 정확도 증명
>* 가장 무거운 <i>heavy</i> h와 가장 무거운 <i>light</i> i가 같이 않자 있는 최적의 해가 존재한다. h가 혼자 앉아 있는 더 좋은 해법이 있더라도 l은 무조건 h랑 앉게 된다. h가 <i>light</i> 보다 가벼운 x (x<=l)과 앉게 되더라도 x와 l을 바꿔주면 된다. l이 동반자 y가 있더라도 x와 y가 y<=h 이므로 x와 y를 같이 앉게 하면 된다.
>* 첫 카누에 관해서 이는 최적의 답이므로, 나머지 카누의 자리를 배분하는 문제도 남은 카누 선수들의 최소 카누 개수로 문제를 축소해서 풀이 가능하다.
>* 전체 시간 복잡도는 <i>O</i>(n)이다. 각 단계에서 한명 또는 두명의 카누선수들이 착석하기 때문에 외부의 while문은 O(n) 번 실행한다. 내부의 while문은 <i>heavy</i>가 <i> light</i>로 바뀔 때마다 실행한다. 시작에 <i>O</i>(n)의 <i>heavy</i>가 있고 각 단계에서 외부 while문이 있고 하나의 <i>light</i>만 <i>heavy</i>가 되므로 전체 단계에서 내부 while문은 <i>O</i>(n)이다.

##### 구현 방법 B: O(n) 
> 가장 무거운 카누 선수가 가장 가벼운 카누선수와 앉는다. 단 둘의 무게는 k보다 작거나 같아야한다. 아니면 가장 무거운 카누 선수는 혼자 앉아야한다.

`※원문 pdf는 파이썬으로 구현되어있다`
```javascript
function greedyCanoeistB(W, k){
  let canoes = 0;
  let j =0;
  let i = W.length-1;
  while(i>=j){
    if(W[i] + W[j] <= k){
        j++;
    }
    canoes++;
    i--;
  }
  return canoes;
}
```
* 각 루프에서 적어도 하나의 카누 선수가 착석하기 때문에 시간복잡도는 <i>O</i>(n)이다.

##### 정확도 증명
> * 구현 A와 비교하자면, 만약 <i>light</i> l가 <i>heavy</i>인 사람과 착석했을 경우 x<h일 경우 x와 h를 바꿔 앉히면 된다.
> * 만약 가장 무거운 카누 선수가 혼자 앉았다면 그 선수와 앉을 수 있는 선수가 없다. 가장 무거운 카누선수 h 다른 카누 선수 x와 함께 앉아있다면,가장 가벼운 카누 선수 l와 x를 바꿔 앉힐 수 있다. 또한 l이 동반자 y가 있다면 y<=h 일 경우 x는 l의 자리에 앉을 수 있다. 

----

# Sol

1. [ [Lesson 16  Greedy algorithms](https://github.com/Pyotato/codility_practice/tree/Greedy-algorithms) ]: [MaxNonoverlappingSegments](https://github.com/Pyotato/codility_practice/blob/Greedy-algorithms/MaxNonoverlappingSegments.md) [👉to report](https://app.codility.com/demo/results/trainingA7CPB4-DCS/)
2. [ [Lesson 16  Greedy algorithms](https://github.com/Pyotato/codility_practice/tree/Greedy-algorithms) ]: [TieRopes](https://github.com/Pyotato/codility_practice/blob/Greedy-algorithms/TieRopes.md) [👉to report](https://app.codility.com/demo/results/trainingE6KYAY-7NK/)
