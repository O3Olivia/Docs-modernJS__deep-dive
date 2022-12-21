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
> 일반 함수의 경우, 함수를 호출 할 때 `해당 함수가 어떻게 호출되었는지`에 따라 this에 바인딩 되는 객체가 동적으로 결정된다.<br/>
```
[ 💡 Note ]
Java에서 this는 인스턴스 자신(self)를 가르키는 참조변수라 this가 객체 자신에 대한 참조값을 가지고 있지만, 
JavaScript에서 this는 this에 바인딩 되는 객체가 한가지가 아니라, 해당 함수 호출 방식에 따라 객체가 달라진다.
```
##### 일반 함수의 호출 방식
- 함수 호출
   > 전역(global)객체는 모든 객체의 유일한 최상의 객체를 의미하는데, Browser-side에서는 `window`, Server-side에서는 `global`을 의미한다.
   ```jsx
   // Broswer-side
   this === window // true
   
   // Server-side
   this === global // true
   ```
   - 기본적으로 `this`는 전역 객체에 바인딩된다. <br/>
   - 전역 함수 뿐만 아니라 내부 함수도 `this`는 외부 함수가 아니라 `전역 객체`에 바인딩된다.
   - 또한, 메소드의 내부 함수일 경우에도 `this`는 `전역 객체`에 바인딩된다.
   ```jsx
   function test1() {
    console.log("test1's this : ", this); // window
    
    function test2() {
      console.log("test2's this : ", this); // window
     }
     test2();
   }
   test1();
   ```

- 메소드 호출
- 생성자 함수 호출
- apply / call / bind 호출

