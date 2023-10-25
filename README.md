# Euclidean algorithm
## Reading Material
* [[reference]](https://codility.com/media/train/10-Gcd.pdf)
### Euclidean algorithm
> 유클리드 알고리즘은 현재까지 자주 사용되는 알고리즘이다. 두 양수의 최대공약수(gcd)를 구하여 문제를 풀이하는 방식이다.
### 뺄셈으로 유클리드 알고리즘 적용
* 오리지널 유클리드 알고리즘은 뺄셈을 활용한다 : 재귀적으로 큰 수에서 작은 수를 빼는 방식
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function gcd(a,b){
  if(a === b) return a;
  if(a > b) gcd(a-b,b);
  else gcd(a,b-a);
}
```
* 위의 시간복잡도를 계산해보자 (n a+b)라고 할 때, 매번 뺄셈을 할 때는 예를 들어 gcd(x, 1)이고, 시간복잡도는 <i>O</i>(n),이다. 최악의 경우 x+y만큼 값이 감소하기 때문이다.

### 곱셈으로 유클리드 알고리즘 적용
* a>=b인 숫자 a와 b에 관해
    * b | a라면 gcd(a, b) = b 인 반
    * gcd(a, b) = gcd(b, a mod b).
![image](https://github.com/Pyotato/codility_practice/assets/102423086/34603809-83c7-4c2e-9e38-3c1d905fd33a)
* r = a mod b 이고 a = b * t + r 일 경우 gcd(a, b) = gcd(b, r)를 증명해보자면:
    * 먼저 d = gcd(a, b) 일 경우 d | (b · t + r) 와 d | b이므로 d | r이다.
    * 따라서 gcd(a, b) | gcd(b, r)이다.
    * c = gcd(b, r)이면 c | b 와 c | r 이므로 c | a이고
    * 따라서 gcd(b, r) | gcd(a, b)이다.
* 따라서 gcd(a, b) = gcd(b, r)를 얻는다. a가 b로 나누어 떨어지지 않는 한 재귀적으로 함수를 호출할 수 있다. 
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function gcd(a,b){
  if(a % b === 0) return b;
  else gcd(b,a % b);
}
```
* (a<sub>i</sub>, b<sub>i</sub>)의 짝은 값 a와b에 관해 알고리즘이 i번 실행한 경우다. 따라서 b<sub>i</sub> >= Fib<sub>i−1</sub> 임을 귀납적으로 증명 가능하다.
    * 첫번째 단계에서 b<sub>1</sub> = 0 이다.
    * 2번째 단계에서 b >= 1이다.
    * 그 이후의 단계로는 (a<sub>k+1</sub>, b<sub>k+1</sub>) → (a<sub>k</sub>, b<sub>k</sub>) → (a<sub>k−1</sub>, b<sub>k−1</sub>), 그리고 a<sub>k</sub> = b<sub>k+1</sub>, a<sub>k−1</sub> = b<sub>k</sub>,
b<sub>k−1</sub> = a<sub>k</sub> mod b<sub>k</sub>, 따라서 a<sub>k</sub> = q · b<sub>k</sub> + b<sub>k−1</sub> for some q >= 1, so b<sub>k+1</sub> >= b<sub>k</sub> + b<sub>k−1</sub>.

* 피보나치 수는 대략적으로 다음과 같다
* ![image](https://github.com/Pyotato/codility_practice/assets/102423086/915caf32-419a-4a1d-a129-b9ed048c5b68)
* 따라서 시간복잡도는 a+b의 로그 시간인 `O(log(a + b))`이다.

### 이진 유클리드 알고리즘
* 이 알고리즘은 뺄셈, 이진법, shift와 교차확인 통해 최대공약수를 찾는다. 우리는 분할정복을 적용해 풀이 가능하다.
* 다음 공식은 gcd(a, b, res) = gcd(a, b, 1) * res을 계산한다. gcd(a, b)를 계산하면 gcd(a, b, 1) = gcd(a, b)를 호출할 수 있다.

#### 구현
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function gcd(a,b,res){
  if(a === b) return res*a;
  else if(a%2 === 0 && b%2 === 0) return gcd(Math.floor(a / 2), Math.floor(b / 2), 2 * res);
  else if(a%2 === 0) return gcd((Math.floor(a / 2), b, res);
  else if(b%2 === 0) return gcd(a, (Math.floor(b / 2), res);
  else if(a > b) return gcd(a-b , b, res);
  else return gcd(a , b-a, res);
}
```
> * 위의 알고리즘은 모든 산술계산식들이 상수시간으로 처리되지 않는 매우 큰 수를 연산해야할 때 유리하다. 이진법으로 표현되기 때문에 모든 연산은 이진법으로 표현된 수의 길이인 <i>O</i>(n)로 이루진다. 반면 알고리즘 [10.2]()를 적용할 경우 n = a + b일 때, O(log n · log log n)로 시간복잡도가 악화된다.
> * 숫자 a와b에 관해 i번 위의 알고리즘을 돌렸을 경우를 
 (a<sub>i</sub>, b<sub>i</sub>)로 표현할 때 a<sub>i+1</sub> >= a<sub>i</sub>, b<sub>i+1</sub> >= b<sub>i</sub>, b<sub>1</sub> = a<sub>1</sub> > 0 이다. 3번 알고리즘을 돌리면
a<sub>i+1</sub> * b<sub>i+1</sub> >= 2 * a<sub>i</sub> * b<sub>i</sub>*b<sub>1</sub> 이다. 4번째로 알고리즘을 돌렸을 경우 a<sub>i+1</sub> * b<sub>i+1</sub> >= 2 * a<sub>i-1</sub> * b<sub>i-1</sub>이다. 왜냐하면 두 홀수의 차는 짝수이기 때문이다. 귀납법을 통해 다음과 같은 결과를 얻을 수 있다: 
> ![image](https://github.com/Pyotato/codility_practice/assets/102423086/2faf5772-2997-4a16-a3b8-31234f8a7284)
* 따라서 시간복잡도는 O(log(a · b)) = O(log a + b) = O(log n)이고 매우 큰 수에 관해서는 각 연산을 O(log n)시간 동안 해야하기 때문에 총 시간복잡도 O((log n)<sup>2</sup>)이다.

### 최소공배수 구하기
* 최소공배수(lcm)는 두 정수 a와 b에 관해 a와 b 모두로 나눌 수 있는 가장 작은 양의 정수다. 따라서 다음과 같이 나타낼 수 있다 : 
![image](https://github.com/Pyotato/codility_practice/assets/102423086/a0c3a29a-59a3-4fbc-ab74-cf0297f20fbb)
* gcd(a, b) 를 O(log(a+b))로 계산할 수 있다면 같은 시간복잡도로 lcm(a, b)를 계산할 수 있다. 

#### 예시 1 : 
* 마이클, 마크, 매튜는 각각 a,b,c원의 동전들을 모으고 있다. (아이당 하나의 동전 종류를 갖고 있다) 자기자신만의 동전을 사용해서 지불할 수 있는 금액의 최소를 구하라.
* 최소공배수lcm(a, b, c)는 다음과 같이 정형화 될 수 있다. n개의 정수에 관해 : 
![image](https://github.com/Pyotato/codility_practice/assets/102423086/d0512a63-b482-4410-b334-a04d34a11985)
이고 따라서 O(log n)만에 문제를 해결할 수 있다. 
# Sol

1. [ [Lesson 12 Fibonacci numbers](https://github.com/Pyotato/codility_practice/tree/Fibonacci-numbers) ]: [ChocolatesByNumbers](https://github.com/Pyotato/codility_practice/blob/Fibonacci-numbers/ChocolatesByNumbers.md) [👉to report](https://app.codility.com/demo/results/training7WBCFJ-YK2/)
