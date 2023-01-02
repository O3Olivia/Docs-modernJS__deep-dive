# 함수의 메소드 (call, apply, bind)
> 자바스크립트에서 함수를 호출하는 방법에는 일반적으로 함수 뒤에 `( )`를 붙여 호출하는 방법과
> `call`, `apply`를 붙여 호출하는 방법이 있다.

## call()
> `call`메서드는 메서드의 호출 주체인 함수를 **즉시 실행**하는 메서드
```jsx
function.call(thisArg[ , arg1, arg2[, ...]])
```
- **thisArg** : 함수 호출에 제공되는 `this`의 값
- **arg1, arg2, ...** : 함수가 호출되어야하는 인수
  ##### 예
  ```jsx
  let person1 = {
    name: 'Olivia'
  };

  let person2 = {
    name: 'Seulgi',
    greeting: function() {
      console.log(this.name + '야, 안녕~?');
     }
  };
  person2.greeting(); // Seulgi야, 안녕~?
  person2.greeting.call(person1); // Olivia야, 안녕~?
  ```
  - `person2` 함수를 호출하고 있지만, `call`메소드를 통해 **첫 번째** 매개변수(thisArg)에 `person1`을 넣어줘서 
    `this`의 값은 `person1`이 되어 **Olivia가 출력**된다.
    
## apply()
> `apply`메서드는 `call`메서드와 같이 메서드의 호출 주체인 함수를 **즉시 실행**하는 메서드
```jsx
function.apply(thisArg, [argsArr])
```
- **thisArg** : 함수 호출에 제공되는 `this`의 값
- **argsArr** : 함수가 호출되어야하는 인수를 지정하는 유사 배열 객체

##### call()과 apply()의 차이
- `call` : 인자를 넣음
- `apply` : 인자를 묶어서 **배열**로 넣음

##### 예
```jsx
function sum(x, y){
  return x + y;
}
console.log(sum(3, 6)); // 9
console.log(sum.call(this, 3, 6)); // 9
console.log(sum.apply(this, [3, 6])); // 9
```
```jsx
let person = {
  name : "Olivia"
}
function introduce(age, location) {
  console.log(`${this.name}은 ${age}살이고, ${location}에 살고 있습니다`);
}
introduce.apply(person, [29, "Seoul"]); // Olivia은 29살이고, seoul에 살고 있습니다
```

##### apply() 메소드를 사용하기 좋은 상황
> 배열 중 가장 큰 값이나 작은 값 구하기 같은 것

###### apply()메소드를 사용하지 않고 가장 큰 수를 구할 경우
```jsx
let arr = [1, 3, 5, 7, 9, 11, 13, 15];
let maxNum = Math.max(...arr);
conosle.log(maxNum); // 15
```
###### apply()메소드를 하여 가장 큰 수를 구할 경우
```jsx
let arr = [1 ,3, 5, 7, 9, 11, 13, 15];
let maxNum = Math.max.apply(null, arr);

console.log(maxNum); // 15
```


## bind()
> `bind`메서드는 **함수를 즉시 실행하지 않고**, 넘겨 받은 인수를 바탕으로 **새로운 함수를 반환**하는 메서드
> 함수가 가르키는 `this`만 바꾸고 호출하지는 않는다. 
> 변수를 할당하여 호출하는 형태로 사용할 수 있고 커스텀 `this`를 가르키는 함수를 만들 수 있다
```jsx
function.bind(thisArg[ , arg1[, arg2[, ...]]])
```
- **thisArg** : 바인딩 함수가 타겟 함수의 `this`에 전달하는 값.
  전달하는게 없으면, `null`
- **arg1, arg2, ...** : 함수가 호출되어야하는 인수

##### 예
```jsx
let person1 = {
  name: "Olivia",
  age: 25,
  location: "Dublin"
}

let person2 = {
  name: "Seulgi",
  age: 29,
  location: "Seoul",
  introduce: function(age, name) {
    console.log(`${this.name}은 ${this.age}살이고, ${this.location}에 살고 있습니다`);
  }
};
person2.introduce(); // Seulgi은 29살이고, Seoul에 살고 있습니다

let oldMe = person2.introduce.bind(person1);

oldMe(); // Olivia은 25살이고, Dublin에 살고 있습니다
```
- `this`를 영구하게 가질 수 있다.

```
[ 💡 NOTE  ]

- call(), apply(), bind() 메소드는 함수시 'this'를 바인딩 할 때 사용한다.
- call()과 apply() 메소드는 메소드의 호출 주체인 함수를 '즉시 실행'한다.
- call()과 apply() 메소드의 차이는 apply()메소드는 '배열'로 인자를 받는다.
- bind()메소드는 call()과 apply()메소드와 다르게 함수를 즉시 실행하지 않고, '새로운 함수'를 반환한다.

```






