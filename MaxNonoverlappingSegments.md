# MaxNonoverlappingSegments
* [Report summary Link](https://app.codility.com/demo/results/trainingA7CPB4-DCS/)
# Task Details
> 겹치지 않는 최대 set segment를 구하라.
# Task description
> * 하나의 선에 각각 위치가 배열 A,B로 정해진 0에서 N-1까지 숫자가 매겨진 N개의 부분이 있다. 각 I(0<=I<N)에 관해 segment I는 A[i]에서 B[l]을 포함한 위치이다. 각각의 segment들은 끝에 따라 정렬이 되므로 0<=K<=N-1 K에 관해서 B[K]<=B[K+1]이다.
> * 서로 다른 두 segment I와 J에 관해 적어도 하나의 공통 부분이 있을 경우 <i>'겹친다'</i>고 한다. 다시 말해 A[i] <=A[J]<B[I] 또는 A[J]<A[I]<=B[J]이다.
> * 하나의 셋의 segment이 겹치는 부분이 없다면 <i>'겹치지 않는다'</i>고 표현한다.

예를 들어 A,B 배열이 주어졌을 때,
| A | B |
| :--: | :--: |
| A[0]= 1 |B[0] = 5 | 
|  A[1] = 3 | B[1] = 6| 
| A[2] = 7 | B[2] = 8 | 
| A[3] = 9 | B[3] = 9 | 
| A[4] = 9 | B[4] = 10 | 

Segment의 모양은 아래와 같다.

![image](https://github.com/Pyotato/codility_practice/assets/102423086/fa473615-396f-4c4c-9d90-ff530d14d595)

> * 겹치지 않는 셋을 포함하는 가장 큰 segment의 수는 3이다. 예를 들어 가능한 set은 {0,2,3}, {0,2,4}, {1,2,3}, {1,2,4}이다. 이 4개의 segment에는 겹치는 set이 없다.

> N개의 원소로 이루어진 배열 A와 B가 주워졌을때, 겹치지 않는 set을 포함하고 있는 최대segment 개수를 리턴해라. 위와 같은 경우는 3을 리턴해야한다.

아래의 가정들을 만족하는 **효율적인** 알고리즘을 작성해라.
> * N은 [0....30,000] 사이의 정수다.
> * 배열 A와 B의 각 원소는 [0,... 1,000,000,000] 사이의 정수다.
> * 각 0<=l<N인 l에 관해 A[l] <=B[l]다.
> * 각 0<=K<N-1인 l에 관해 B[K] <= B[K+1]다.

# Logic
| Index |  1 | 2 | 3 | 4  | 5 | 6 | 7 | 8 | 9 | 10 |
| :--: | :--: |:--: | :--: |:--: | :--: |:--: | :--: |:--: | :--: |:--: |
| 0 | O | O | O |O | O | |  | |  | |  | |
| 1 |   |  | O | O| O | O |  | |  | |  | |
| 2 |   |  | |  |  |  |  O | O |  | |  | |
| 3 |   |  |  | |  |  |  | | O | |  | |
| 4 |   |  |  | |  |  |  | | O | O  |  | |

* A는 시작점, B는 끝점이라고 볼 때
  * 현재 시작점이 다음 끝점보다 먼저면 겹치지 않는다.
  * 또는 현재 시작점이 이전 끝점보다 먼저면 겹치지 않는다.
* 모든 선들이 끝점을 기준으로 정렬되어있으므로 겹치면 고려를 안해도 된다.
* index 0 일 경우 1과는 겹치므로 고려하지 않고 넘어간다.
* index 2와 index 3일 경우나 index2와 index4일 경우, 겹치지 않으므로 셋의 최대는 3이 된다.
* index 3 일 경우 4와 겹치므로 고려하지 않는다.
* 따라서 겹치지 않는 set은 최대 크기가 3이다. 

# Solution 
* 사용언어 : javascript
* 점수 : 100
* 시간 복잡도 : **O(n)**
* 풀이
```javascript
function solution(A, B) {
    if(A.length <= 1) return A.length;
    let cnt = 1;
    let end = B[0];
    for(let i = 1; i < A.length; i++){
        if(A[i] > B[i+1] || A[i] > end){
            cnt++;
            end = B[i];
        }
    }
    return cnt;
}
```
