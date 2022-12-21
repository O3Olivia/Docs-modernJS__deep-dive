# 화살표 함수(Arrow function)
> 화살표 함수는 `function` 키워드 대신 `화살표(=>)`를 사용하여 함수를 선언한다. <br/>
그러나 모든 함수를 화살표 함수로 선언할 수 X.

#### 화살표 함수 기본 문법
```jsx
// 매개변수 지정 방법
() => { } // 매개변수가 없을 경우
x => { } // 매개 변수가 1개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { } // 매개 변수가 1개 이상일 경우, 소괄호를 반드시 사용해야한다.

// 함수 몸체 지정 방법
x => { return x * x} // single line block
x => x * x // 함수 몸체가 한줄의 구문이라면 { } 중괄호를 생락할 수 있고, 암묵적으로 return 된다. 

() => { return { x : 1}; }
() => ({x:1}) // 위의 표현과 동일하다. 만약 객체를 반환한다면 () 소괄호를 사용하여 객체를 감싸준다.

() => {
  const x = 100;
  return x * x;
} // 여러 라인 블록
```
# 화살표 함수의 호출
> 화살표 함수는 익명 함수로만 사용할 수 있기 때문에 화살표 함수를 호출하기 위해서는 `함수 표현식`을 사용해야한다.

```jsx
// ES5
var testFive = function(x){ return x * x };
console.log(testFive(20)); // 400

// ES6
let testSix = x => x * x;
console.log(testSix(20)); // 400
```
- 위의 예제뿐만 아니라 콜백 함수로써 화살표 함수를 사용할 수 있는데, 일반적인 함수 표현식보다 훨씬 간결하다.
```jsx
// ES5
var arr = [1, 2, 3]
var testFive = arr.map(function(x){
  reutrn x * x;
});

console.log(testFive); // [1, 4, 9]
```
```jsx
// ES6
let arr = [1, 2, 3]
let testSix = arr.map(x => x * x );

console.log(testSix); // [1, 4, 9]
```
# this
#### 일반 함수의 this
> 
