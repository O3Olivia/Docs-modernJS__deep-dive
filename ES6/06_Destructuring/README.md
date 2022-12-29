# 디스트럭처링(Destructuring)
> 디스트럭처링은 구조화된 `배열`이나 `객체`를 **비구조화, 파괴**하여 `개별적인 변수에 할당`하는 것이다.
>  `배열`이나 `객체`에서 필요한 값만 추출하여 변수에 할당할 때 사용

## 배열 디스트럭처링(Array destructuring)
##### ES5
> ES5에서 배열의 각 요소를 배열로부터 디스트럭처링하여 변수에 할당한다.
```jsx
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1, 2, 3
```

##### ES6
> ES6의 배열 디스트럭처링은 배열의 각 요소를 배열로부터 추출하여 변수 리스트에 할당한다.
> 여기서 추출/할당의 기준은 `배열의 인덱스`다.

```jsx
const arr = [1, 2, 3];
const [one, two, three] = arr;

console.log(one, two, three); // 1, 2, 3
```
- 여기서 중요한 것은 `디스트럭처링`을 할 때는 반드시 `initializer(초기화)`해야한다.

```jsx
const [one, two, three];
```
- 위와 같은 경우 `SyntaxError: Missing initializer in destructuring declaration` 에러 발생.

##### 배열 디스트럭처링시 할당 연산자 왼쪽에 `배열`형태의 변수 리스트가 필요하다.
```jsx
let x, y, z;

[x, y, z] = [1, 2, 3];

[x, y] = [1, 2];
console.log(x, y); // 1, 2

[x, y] = [1];
console.log(x, y); // 1, undefined

[x, y] = [1, 2, 3];
console.log(x, y); // 1, 2

[x, , z]= [1, 2, 3];
console.log(x, z); // 1, 3

[x, ...y] = [1, 2, 3];
console.log(x, y); // 1, [2, 3]
```

##### ES6의 배열 디스트럭처링 사용 예시
> ES6의 배열 디스트럭처링은 `년도, 월, 일`을 추출할때 사용하면 좋다.
```jsx
const today = new Date();// 2022-12-29T05:55:01.094Z
const formattedDate = today.toISOString().substring(0, 10); // 2022-12-29
const [year, month, day] = formattedDate.split('-');

console.log(year, month, day); // ["2022", "12", "29"]
```

## 객체 디스트럭처링(Object destructuring)
#####ES5
>ES5에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 사용하기 위해 이름(키) 사용한다.
```jsx
var obj = { firstName : "Seulgi", lastName: "Lee"};

var firstName : obj.firstName;
var lastName : obj.lastName;

console.log(firstName, lastName); // Seulgi Lee
```

##### ES6
> ES6의 경우, 객체로 부터 추출하여 변수 리스트에 할당하는데 할당 기준은 `프로퍼티 이름(키)`
```jsx
const obj = {firstName: 'Seulgi', lastName: 'Lee'};

const {firstName, lastName} = obj;

console.log(firstName, lastName); // Seulgi Lee
```
- 여기서 객체 리스트럭처링을 위해서는 할당 연산자 왼쪽에 객체 형태의 변수 리스트가 필요하다.
```jsx
const {props1 : p1, props2: p2} = {props1 : 'a', props2: 'b'};
console.log(p1, p2); // 'a', 'b'
console.log({props1: p1, props2: p2}); // {props1: 'a', props2: 'b'}

// 위와 동일 [축약형]
const {props1, props2} = {props1: 'a', props2: 'b'};
console.log({props1, props2}); // {props1: 'a', props2: 'b'}

// default value
const { prop1, prop2, prop3 = 'c' } = { prop1: 'a', prop2: 'b' };
console.log({ prop1, prop2, prop3 }); // { prop1: 'a', prop2: 'b', prop3: 'c' }
 ```
 
 ##### `이름(키)`로 필요한 프로퍼티 값만 추출 가능
 ```jsx
 const todos = [
   { id: 1, content: 'HTML', completed: true },
   { id: 2, content: 'CSS', completed: true },
   { id: 3, content: 'JS', completed: false }
 ];
 
 const completedTodos = todos.filter(({completed}) => completed);
 console.log(completedTodos); 
 // [{ id: 1, content: 'HTML', completed: true },
   { id: 2, content: 'CSS', completed: true }]
 ```
 ##### 또 다른 예시
 ```jsx
 const person = {
 name: 'Lee',
 address: {
  zipCode: '12345',
  city: 'Seoul'
 }
};

const {address: {city}} = person;
console.log(city); // Seoul
 ```
 
