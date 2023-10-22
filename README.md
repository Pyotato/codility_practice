# Binary search algorithm
## Reading Material
* [[reference]](https://codility.com/media/train/12-BinarySearch.pdf)
### Binary search algorithm
> 이진탐색 알고리즘은 선형 탐색알고리즘의 시간 복잡도를 log로 최적화할 수 있는 간단하고 매우 유용한 알고리즘이다.
### Logic
* 다음과 같은 게임이 있다고 상상해보자. 컴퓨터는 1과 16사이의 정수를 선택한다. 최소한의 횟수로 해당 숫자를 맞추면 된다. 컴퓨터는 숫자를 외칠 경우 해당 숫자가 정답보다 크거나 작은 지 알려준다.
![image](https://github.com/Pyotato/codility_practice/assets/102423086/fbecba54-d2c3-4651-b2e3-6487214adef1)
* 연속된 순서를 값을 찾는 경우(1,2,...16)의 시간 복잡도는 O(N)이다. 왜냐하면 정답을 외칠때마다 외칠 수 있는 숫자의 개수가 하나씩 감소하기 때문이다.
 * 목표는 외칠 수 있는 숫자의 개수를 한번 정답을 외칠 때 최대로 제거하는 것이다. 최선의 선택은은 가운데의 숫자를 외치는 것이다. 왜냐하면 매번 정답을 외칠 경우 선택지의 반을 버릴 수 있기 때문이다. 이 접근방식을 통해 시간복잡도를 최대 log(N)만큼 축소할 수 있다. 

#### 예시 1 : 숫자 맞추기
> * 이진 탐색의 경우 원소들이 정렬이 돼 있어야한다. 다음과 같이 정렬이 된 배열 a에 관해  a<sub>0</sub> <= a<sub>1</sub> <= . . . <= a<sub>n−1</sub>이고 x를 a의 원소 중 하나의 값이라고 가정하자. x=31일 경우 선택지가 감소되는 양상을 살펴보자.
![image](https://github.com/Pyotato/codility_practice/assets/102423086/6ccd32b2-c802-4958-a241-3d6bf24abde3)
알고리즘의 각 단계에서 우리는 배열의 처음과 끝을 각각 기억하고 있어야한다 (각각 변수 beg와 end). 중간의 원소는 간단하게 mid = (beg+end)/2값으로 계산가능하다. 

##### 구현
* 시간복잡도와 공간복잡도 *O*(log n)인 풀이 :
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function binarySearch(A,x){
  const n = A.length;
  let beg = 0, let end = n-1;
  let result = -1;
  while (beg <= end){
    let mid = (beg+end)/2;
    if(A[mid] <= x){
      beg = mid+1;
      result = mid;
    }
    else end = mid-1;
  }
  return result;
}

```

* 위의 풀이는 가장 큰 원소를 x보다 작거나 같은 횟수로 찾을 수 있다. 반복문을 통해 순회를 해야하는 원소의 개수(선택지)는 반으로 줄어드므로, 시간복잡도는 <i>O</i>(log n)이 된다. 위의 구현 방식은 이진탐색에 있어서 모든 경우에 통용된다. 다만 while문 안의 조건만 변경해주면 된다.

### 결과의 이진탐색
* 많은 문제들은 종종 최적의 값과 특정 조건을 만족하는 어떤 정수를 구하라고 한다. 우리는 이러한 숫자를 이진 탐색을 통해 쉽게 찾을 수 있다. 특정값을 갖고 이 숫자보다 크거나 작은 지를 결과값과 비교하면 된다. 처음에는 결과를 찾을 수 있는 특정 범위가 주어진다. 매 번 정답을 찾기 위해 시도를 할 경우 이 범위는 반으로 줄어드므로 시간복잡도는 <i>O</i>(log n)로 예상할 수 있다.
* 따라서 최적의 해를 찾으려는 문제는 값이 타당(정답보다 크거나 작은값인 지)하고 최적의 값인 지 체크하는 문제로 축소된다. 후자와 같은 문제는 종종 더 간단하고, 시간복잡도 또한 전체 문제에 log<i>n</i>만큼의 시간복잡도만 더한다.

#### 예시 2 : 최소 보드 개수로 지붕 구멍 가리기
> x<sub>0</sub>, x<sub>1</sub>, . . . , x<sub>n−1</sub>인 n개의 이진값들이 주워진다 (즉, x<sub>i</sub> ∈ {0, 1}이다.). 이 배열은 지붕에 있는 구멍을 나타낸다 (1은 구멍이다). 또한 k개의 동일한 크기의 보드가 주워진다. 목표는 보드의 최소개수로 모든 구멍을 가릴 수 있는 보드의 크기를 구하는 것이다. 

* **Sol** <i>O</i>(log n)인 경우
* 보드의 크기는 이진 탐색을 통해 찾을 수 있다. 보드의 크기 x가 모든 구멍을 가릴 만큼 크다면 우리는 x+1,x+2,...,n 크기의 보드 또한 모든 구멍을 막을 수 있다는 걸 알 수 있다. 반면에 x가 모든 구멍을 가릴만큼 크지 않다면 x-1,x-2,...,1 크기의 보드 또한 구멍을 막을 수 없다는 걸 알 수 있다.  

##### 구현 : 시간복잡도가 <i>O</i>(logn):
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function boards(A) {
  const n = A.length;
  let beg = 1, end = n;
  let result = -1;
  while (beg<=end) {
    let mid = (beg+end)/2;
    if (check(A, mid) <= k) {
      end = mid - 1;
      result mid;
    }
    else beg = mid + 1
  }
  return result;
}
```
* x크기의 보드가 충분한 지 어떻게 아냐는 질문에 관해서는 보드 배열의 왼쪽에서 오른쪽으로 greedy 방식으로 하나씩 탐색할 수 있다. 만약 이전 보드로 가려질 수 없는 구멍이 존재할 경우에만 새로운 보드를 추가한다. 
##### greedy 방식으로 check구현 : 시간복잡도가 <i>O</i>(n):
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function check(A,k){
  const n = A.length;
  let boards = 0;
  let last = -1;
  for(let i = 0; i<n; i++){
    if(A[i] === 1 && last<i){
      boards ++;
      last = i+k-1;
    }
  }
  return boards;
}
```
----
* 이진탐색을 하기 떄문에 위의 총 시간복잡도는 O(n log n) 이다.

# Sol

1. [ [Lesson 15 Binary search algorithm](https://github.com/Pyotato/codility_practice/tree/Binary-search-algorithm) ]: [MinMaxDivision](https://github.com/Pyotato/codility_practice/blob/Binary-search-algorithmMinMaxDivision.md) [👉to report](https://app.codility.com/demo/results/trainingWDXGG6-4SJ/)
