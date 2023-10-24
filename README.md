# Fibonacci numbers
## Reading Material
* [[reference]](https://codility.com/media/train/11-Fibonacci.pdf)
### Fibonacci numbers
> 피보나치수는 다음과 같이 연속된 정수들이 재귀적으로 정의된다. 피보나치수의 첫 두 숫자는 0과 1이고, 다음 수는 앞의 두 수의 합으로 이루어져 있다.
> ![image](https://github.com/Pyotato/codility_practice/assets/102423086/2a824b0b-bb66-4940-8031-8538ef2c8524)
* 첫 12개의 피보나치는 아래와 같다.
![image](https://github.com/Pyotato/codility_practice/assets/102423086/8f0d99cd-1676-443b-92b0-28c0ab4931d8)

### Logic
* 정의에 의해서 숫자를 재귀적으로 나타낼 때 매우 느리다는 점을 알아차릴 수 있다. <i>F</i><sub>n</sub>의 정의에 의하면 피보나치 수열의 이전 숫자를 반복적으로 가르킨다.

#### 예시 1 : 재귀적으로 피보나치수 구하기
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function fibonacci(n){
  if(n <= 1) return n;
  return fibonacci(n-1)+fibonacci(n-2);
}
```
> * 위의 알고리즘은 <i>F</i><sub>n</sub>을 1씩 증가시키면서 계산하므로 2<sup>n</sup>으로 증가하는 비효율적인 알고리즘이 된다.

#### 예시 2 : 동적프로그래밍을 활용해 피보나치수 구하기 : <i>O</i>(n)
`※원본의 코드는 파이썬으로 작성되어있다.`
```javascript
function fibonacciDynamic(n){
  let fib = new Array(n+2).fill(0);
  fib[1] = 1;
  for(let i =2 ; i<n+1; i++){
    fib[i] = fib[i-1] + fib[i-2]
  }
  return fib[n];
}
```

#### 예시 3 : 피보나치 수를 구하기 위한 더 빠른 알고리즘들
> * 피보나치수는 <i>O</i>(log n)시간으로 찾을 수도 있다. 하지만 이를 위해서는 다음과 같은 공식으로 행렬곱셈을 사용해야한다.

![image](https://github.com/Pyotato/codility_practice/assets/102423086/7c4f39a5-6ada-4b67-8105-a3366d055035)

다음과 같은 공식을 활용하면 더 빨리 해를 구할 수 있다. 

![image](https://github.com/Pyotato/codility_practice/assets/102423086/caa50a35-24cc-45c7-8bdc-f5cbd4600905)


#### 예시 4 : 두 수의 합이 피보나치 수인지 여부
> x<sub>0</sub>, x<sub>1</sub>, . . . , x<sub>n−1</sub>이고 1 <= x<sub>i</sub>, <= m <= 1,000,000 일 때, 두 수의 합이 피보나치 수인지 구하라

* **Sol** <i>O</i>(n+m)인 경우
* 피보나치의 수가 최대인 m보다 작은 경우는 31개 뿐이다. 모든 짝을 고려할 때 k <= m이면 k번째 배열의 인덱스를 두수의 합이 피보나치 수라고 표시를하면된다. 즉, 각 숫자 x<sub>i</sub>에 관해 그 수가 두 피보나치 수의 합인지 여부를 상수시간인 <i>O</i>(n+m)로 구할 수 있다. 


# Sol

1. [ [Lesson 12 Fibonacci numbers](https://github.com/Pyotato/codility_practice/tree/Fibonacci-numbers) ]: [FibFrog](https://github.com/Pyotato/codility_practice/blob/Fibonacci-numbers/FibFrog.md) [👉to report](https://app.codility.com/demo/results/training4NST5Q-BBG/)
2. [ [Lesson 12 Fibonacci numbers](https://github.com/Pyotato/codility_practice/tree/Fibonacci-numbers) ]: [Ladder](https://github.com/Pyotato/codility_practice/blob/Fibonacci-numbers/Ladder.md) [👉to report](https://app.codility.com/demo/results/trainingKJUZG3-6XK/)
