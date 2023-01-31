# Generator (제너레이터와 async/await)

## Generator란? 

> 이터러블을 생성하는 함수<br/>
> Genertor을 사용하면, `이터레이션 프로토콜`을 준수해 이터러블을 생성하는 것보다 간편하게 구현할 수 있고, <br/>
> **비동기 처리**에 유용하다.

```jsx
// 이터레이션 프로토콜을 구현하여 무한 이터러블을 생성하는 함수
const createInfinityByIteration = function() {
  let i = 0; // 자유 변수
  return {
    [Symbol.iterator]() {return this;},
    next(){
      return {value: ++i};
    }
  };
};

for(const n of createInfinityByIteration()){
  in(n > 5) break,
  consolel.log(n); // 1 2 3 4 5
}

// 무한 이터러블을 생성하는 generator 함수
function* createInfinityByGenerator(){
  let i = 0;
  while(true){
    yield ++i;
  }
}

for(const n of createInfinityByGenerator()){
  if(n > 5) break;
  console.log(n); // 1 2 3 4 5
}
```
- Generator함수는 일반 함수와 달리 코드 블록을 **한 번**에 실행하지 않고, 코드 블록을 **일시 중지**했다가 필요한 시점에 **재시작**한다.
```jsx
function* counter(){
  console.log('1번째'); 
  yield 1; // 첫 번째 호출 시, 이 지점까지 실행
  console.log('2번째');
  yield 2; // 두 번째 호출 시, 이 지점까지 실행
  console.log('3번째'); // 세 번째 호출 시, 이 지점까지 실행
}
const generatorObj = counter();

console.log(generatorObj.next()); // 첫 번째 호출 {value: 1, done: false}
console.log(generatorObj.next()); // 두 번째 호출 {value: 2, done: false}
console.log(generatorObj.next()); // 세 번째 호출 {value: undefined, done: true} // 3은 설정하지 않아서 undefined
```
- 일반 함수를 호출하면 `return`문으로 반환값을 리턴한다.
- 그러나, Generator 함수를 호출하면, `generator`을 반환한다.
- 이 Generator는 **이터러블(iterable)이면서 동시에 이터레이터(iterator)인 객체다**
- 제너레이터 함수가 생성한 제너레이터는 `Symbol.iterator`메소드를 소유한 이터러블이다.
- 그리고 제너레이터는 `next`메소드를 소유하고, `next`메소드를 호출하면, `value, done`프로퍼티를 갖는 **이터레이터 리절트 객체 반환**하는 이터레이터다.

```jsx
// Generator 함수 정의
function* counter(){
  for(const v of [1, 2, 3]) yield v;
  // -> yield* [1, 2, 3];
}

// Generator 함수를 호출하면 generator을 반환한다.
let generatorObj = counter();

// Generator은 이터러블이다
console.log(Symbol.iterator in generatorObj); // ture

for(const i of generatorObj){
  console.log(i); // 1 2 3
}

generatorObj = counter();

// Generator은 이터레이터다.
console.log('next' in generatorObj); // true

console.log(generatorObj.next()); // {value: 1, done: false}
console.log(generatorObj.next()); // {value: 2, done: false}
console.log(generatorObj.next()); // {value: 3, done: false}
console.log(generatorObj.next()); // {value: undefined, done: true}
```

## Generator함수의 정의
> generator함수는 `function*`키워드로 선언한다. 그리고 하나 이상의 `yield`문을 포함한다.

```jsx
// 제너레이터 함수 선언문
function* genDecFunc(){
  yield 1;
}

let generatorObj = genDecFunc();

// generator함수 표현식 
const genExpFun = function*(){
  yield 1;
};
generatorObj = genExpFunc();

// generator 메소드
const obj = {
  *generatorObjMethod(){
    yield 1;
  }
};

generatorObj = obj.generatorObjMethod();

// generator 클래스 메소드 
class MyCalss{
  * generatorClsMethod(){
    yield 1;
  }
}

const myClass = new MyClass();
generatorObj = myClass.generatorClsMethod();
```

## Generator 함수의 호출과 generator 객체
> generator 함수를 호출하면 generator 함수의 코드 블록이 실행되는 것이 아니라, 그 객체를 반환한다.

- generator객체는 **이터러블**이면서 동시에 **이터레이터**다.
- 따라서 `next`메소드를 호출하기 위해 `Symbol.iterator`메소드로 이터레이터를 별도 생성할 필요가 없다.

```jsx
// generator 함수 정의
function* counter(){
  console.log('1');
  yield 1; // 첫번째 next 메소드 호출 시 여기까지 실행된다.
  
  console.log('2');
  yield 2; // 두번째 next 메소드 호출 시 여기까지 실행된다.
  
  console.log('3');
  yield 3; // 세번째 next 메소드 호출 시 여기까지 실행된다.
  
  console.log('4'); // 네번째 next 메소드 호출 시 여기까지 실행된다.
}

// generator 함수를 호출하면 generator객체를 반환
// generator 객체는 이터러블이며 동시에 이터레이터 
// Symbol.iterator 메서드로 이터레이터를 별도 생성할 필요가 없다
const generatorObj = couter();

// 첫 번째 next 메소드 호출 : 첫번째 yield 문까지 실행되고 일시 중단
console.log(generatorObj.next()); // Point 1 // {vluae: 1, done: false}

// 두번째 next 메소드 호출: 두번째 yield 문까지 실행되고 일시 중단된다.
console.log(generatorObj.next()); // Point 2 // {value: 2, done: false}

// 세번째 next 메소드 호출: 세번째 yield 문까지 실행되고 일시 중단된다.
console.log(generatorObj.next()); // Point 3 // {value:3, done: false}

// 네번째 next 메소드 호출: 제너레이터 함수 내의 모든 yield 문이 실행되면 done 프로퍼티 값은 true가 된다.
console.log(generatorObj.next()); // Point 4 // {value: undefined, done: true}
```
- generator함수가 생성한 generator 객체의 `next`메소드를 호출하면, 처음 만나는 `yield`문까지 실행되고, **일시중단(suspend)**된다
- 또 다시 `next`메소드를 호출하면, 중단된 위치에서 다시 실행(resume)되고, 다음 만나는 `yield`문까지 실행된 뒤 다시 **일시중단(suspend)**된다.
- `next` 메소드는 이터레이터 리저트 객체와 같이 `value, done`프로퍼티를 갖는 객체를 반환
- `value`프로퍼티는 `yield`문이 반환
- `done` 프로퍼티는 generator 함수 내의 모든 yield문이 실행되었는지 나타내는 `boolean`타입의 값.
- 마지막 `yield`문까지 실행된 상태에서 `next`메소드를 호출하면 `done`프로퍼티 값은 **true**

## 제너레이터의 활용
### 이터러블의 구현
> generator함수를 사용하면 `이터레이션 프로토콜`을 준수해 이터러블을 생성하는 방식보다 간편하게 이터러블을 구현한다.
```
이터러블 : 자료를 반복할 수 있는 객체
```

```jsx
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function(){
  let [pre, cur] = [0, 1];
  
  return {
    [Symbol.iterator]() {
      return this;
    },
    next(){
      [pre, cur] = [cur, pre + cur];
      
      // done 프로퍼티 생략
      return {
        value: cur
      };
    }
  };
}());

// infiniteFibonacci는 무한 이터러블
for(const num of infiniteFibonacci){
  if(num > 10000) break;
  console.log(num); // 1 2 3 5 8 .....
}
```

##### generator활용한 무한 피보나치 수열
```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

// infiniteFibonacci는 무한 이터러블.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num);
}
```
##### generator함수에 최대값을 인수를 전달
```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const createInfiniteFibByGen = function* (max) {
  let [prev, curr] = [0, 1];
  
  while(true){
    [prev, curr] = [curr, prev + curr];
    if(curr >= max) return; // generator 함수 종료
    yield curr;
  }
};

for(const num of createInfiniteFibByGen(10000)){
  console.log(num);
}
```
- 이터레이터의 `next`메소드와 다르게, generator객체의 `next` 메소드는 인수를 전달할 수 있으며, 이를 통해 generator 객체에 **데이터 전달**할 수 있다.

```jsx
function* gen(n) {
  let res;
  res = yield n;    // n: 0 ⟸ gen 함수에 전달한 인수

  console.log(res); // res: 1 ⟸ 두번째 next 호출 시 전달한 데이터
  res = yield res;

  console.log(res); // res: 2 ⟸ 세번째 next 호출 시 전달한 데이터
  res = yield res;

  console.log(res); // res: 3 ⟸ 네번째 next 호출 시 전달한 데이터
  return res;
}
const generatorObj = gen(0);

console.log(generatorObj.next());  // 제너레이터 함수 시작
console.log(generatorObj.next(1)); // 제너레이터 객체에 1 전달
console.log(generatorObj.next(2)); // 제너레이터 객체에 2 전달
console.log(generatorObj.next(3)); // 제너레이터 객체에 3 전달
/*
{ value: 0, done: false }
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: true }
*/
```

### 비동기 처리
> generator을 사용해 `비동기 처리`를 `동기`처럼 할 수 있다.

```jsx
const fetch = require('node-fetch');

function getUser(genObj, username){
  fetch(`https://api.github.com/user/${username}`)
  .then(res=>res.json())
   // 1. 제너레이터 객체에 비동기 처리 결과를 전달한다.
  .then(user=>genObj.next(user.name));
}

// 제너레이터 객체 생성
const g = (function* () {
   let user;
  // 2. 비동기 처리 함수가 결과를 반환한다.
  // 비동기 처리의 순서가 보장된다.
  user = yield getUser(g, 'jeresig');
  console.log(user); // John Resig

  user = yield getUser(g, 'ahejlsberg');
  console.log(user); // Anders Hejlsberg

  user = yield getUser(g, 'ungmo2');
  console.log(user); // Ungmo Lee
}());

// 제너레이터 함수 시작
g.next();
```
1️⃣ 비동기 처리가 완료되면 next 메소드를 통해 제너레이터 객체에 비동기 처리 결과를 전달<br/> 
2️⃣ 제너레이터 객체에 전달된 비동기 처리 결과는 user 변수에 할당 <br/> 

- 제너레이터을 통해 비동기 처리를 동기 처리처럼 구현할 수 있으나 더 간편하게 비동기 처리를 구현할 수 있는 `async/await`가 ES7에서 도입되었다.

#### 위 예제를 async/await 구현
```jsx
const fetch = require('node-fetch`);

// Promise를 반환하는 함수
function getUser(username){
  return fetch(`https://api.github.com/users/${username}`)
  .then(res => res.json())
  .then(user => user.name);
}

async function getUserAll(){
  let user;
  user = await getUser('jeresig');
  console.log(user);
  user = await getUser('ahejlsberg');
  console.log(user);
  user = await getUser('ungmo2');
  console.log(user);
}

getUserAll();
}
```
