# Caterpillar-method

## Reading Material
* [[reference]](https://codility.com/media/train/13-CaterpillarMethod.pdf)
### Caterpillar-method
> 애벌레 방식은 인기 있는 알고리즘 풀이 방식 중 하나의 애칭이다. 아이디어는 각각의 요소를 애벌레가 기어가는 모습처럼 확인하는 방식이다. 애벌레는 배열을 기어서 통과한다고 하자. 애벌레의 앞과 뒤 위치를 기억하고, 매 단계마다 둘의 위치가 앞으로 땡껴지는 모습을 생각하면 된다.
![giphy](https://github.com/Pyotato/codility_practice/assets/102423086/fae1bf3e-9fb3-4d21-bf4d-cbc09e66a0bb)

#### 예시 1 : 연속된 수의 합이 특정수를 만족하는 지 존재 여
> * a<sub>0</sub>, a<sub>1</sub>, . . . , a<sub>n-1</sub>(1 <= a<sub>i</sub> <= 109)에서 연속된 수들의 합이 s를 만족하는 지 여부를 확인하자. 예를 들어 s=12일 경우
![image](https://github.com/Pyotato/codility_practice/assets/102423086/26e11a8f-c643-4eae-8a1b-dc06ca19eb46)
* 애벌레의 각 위치는 다른 연속된 수를 포함한 부분이며 s보다 작다. 애벌레의 첫위치를 첫 원소에 배치하자. 다음으로는 아래와 같은 단계를 실행하면 된다.
   * 가능하다면 오른쪽 끝(머리)를 앞으로 놓고 애벌레의 크기를 키운다.
   * 그러지 않을 경우, 왼쪽 끝 (꼬리)를 앞으로 놓고 애벌레의 크기를 줄인다.
* 이 방식을 통해, 왼쪽과 오른쪽의 각 위치에서 s를 초과하지 않는 가장 긴 애벌레의 길이를 알 수 있다. s와 연속된 수의 총합이 같은 경우가 있다면 애벌레의 길이가 모든 원소들을 통과한 경우가 존재한다.


##### 구현
* 시간복잡도와 공간복잡도 *O*(n)인 풀이 :
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function caterpillarMethod(A,s){
  const n = A.length;
  let front=0, total = 0;
  for(let back= 0; back<n; back++){
    while(front<n && total+A[front] <= s){
      total+=A[front];
      front++;
    }
    if(total === s) return true;
    total -= A[back];
  }
  return false;
}

```

* 위의 풀이의 시간복잡도를 예상해보자. 첫눈에 볼 떄는 for문 안에 또 while문이 있어서 n<sup>2</sup>라고 예상할 수 있다. 하지만 각 단계에서 애벌레가 앞 또는 뒤로 이동하고 각각의 위치가 n을 초과할 수 없으므로 *O*(n) 인 풀이 방법이 된다.

#### 예시 2 : 막대기로 삼각형 만들기
> 길이가 1 <= a<sub>0</sub> <= a<sub>1</sub> <= . . . <= a<sub>n−1</sub> <= 10<sup>9</sup>인 n개의 막대가 주어진다. 목표는 이 막대들을 활용해 만들 수 있는 삼각형의 개수를 구하는 것이다. 정확히는 각 인덱스 x<y<z에 관해 a<sub>x</sub> + a<sub>y</sub> > a<sub>z</sub>을 만족해야한다.

* **Sol** *O*(n<sup>2</sup>)인 경우
* 각 짝 x,y에 관해 가장 큰 막대기 z를 갖고 삼각형을 만들 수 있다. 각 막대기 k는 y<k<=z 또한 a<sub>x</sub> + a<sub>y</sub> > a<sub>k</sub>를 만족하므로 이 또한 경우에 해당된다. 한번에 이 모든 삼각형들을 더해주면 된다.
* z가 매번 처음에 발견된다면 시간복잡도는 *O*(n<sup>3</sup>)이 된다. 하지만 대신, 애벌레 방식을 사용하면 y를 증가시킬 때 z도 가능한 증가 시킬 수 있다. 

##### 구현 : 시간복잡도가 O(n<sup>2</sup>):
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function triangles(A) {
  const n = A.length;
  let result = 0;
  for (let x = 0; x < n; x++) {
    let z = x + 2;
    for (let y = x + 1; y < n; y++) {
      while (z < n && A[x] + A[y] > A[z]) z++;
      result += z - y - 1;
    }
  }
  return result;
}
```
* 해당 알고리즘의 시간복잡도는 O(n<sup>2</sup>)이다. 왜냐하면 각 막대기 x에 관해 y와 z값은 O(n<sup>2</sup> 번 증가하기 때문이다.

----

# Sol

1. [ [Lesson 15 Caterpillar method](https://github.com/Pyotato/codility_practice/tree/Caterpillar-method) ]: [AbsDistinct](https://github.com/Pyotato/codility_practice/blob/Caterpillar-method/AbsDistinct.md) [👉to report](https://app.codility.com/demo/results/trainingGRMB69-7DY/)
2. [ [Lesson 15 Caterpillar method](https://github.com/Pyotato/codility_practice/tree/Caterpillar-method) ]: [CountDistinctSlices](https://github.com/Pyotato/codility_practice/blob/Caterpillar-method/CountDistinctSlices.md) [👉to report](https://app.codility.com/demo/results/trainingKEDT6K-9H2/)
