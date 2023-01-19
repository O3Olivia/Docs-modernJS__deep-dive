# Iteration & for...of

## 이터레이션 프로토콜

> ES6에서 이터레이션 프로토콜(iteration protocol)은 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)이다.
> `이터레이션 프로토콜`을 준수한 객체는 `for..of`문으로 순회할 수 있고, `spread문법`의 피연산자가 될 수 있다.

## 이터러블 프로토콜(Iterable protocol), 이터레이터 프로토콜(Iterator protocol)

- 이터러블 : 이터레이터를 리턴하는 `[Symbol.iterator]()`메서드를 가진 값.
- 이터레이터 : `{value, done}` 객체를 리턴하는 `next()`메서드를 가진 값.

### 이터러블(Iterable)

> 이터러블 프로토콜을 준수한 객체다.<br/>

```
[ 자바스크립트에 내장된 이터러블 객체]
  Array, Set, Map 문자열
  map.keys(), map.values(), map.entries()
```

- 이터러블은 `Symbol.iterator메소드`를 구현하거나, 프토로타입 체인에 의해 상속한 객체를 말한다.
- `Symbol.iterator`메소드는 이터레이터를 반환한다.
- 이터러블은 `for...of`문에서 순회할 수 있고, `Spread`문법의 대상으로 사용할 수 있다.

- `배열`은 `Symbol.iterator`메소드를 소유하기 때문에 배열은 이터러블 프로토콜을 준수한 이터러블이다.

```jsx
const array = [1, 2, 3];

// 배열은 `Symbol.iterator` 메소드를 소유한다.
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블 프로토콜을 준수한 배열은 `for...of`문에서 순회 가능하다.
for (const item of array) {
  console.log(item); // 1, 2, 3
}
```

- `일반객체`는 `Symbol.iterator` 메소드를 소유하지 않는다.
- 따라서 `일반객체`는 이터러블 프로토콜을 준수한 **이터러블이 아니다**.

```jsx
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator메소드를 소유하지 않는다.
// 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아니라서 일반 객체는 for...of문에서 순회할 수 없다.
// 그렇기 때문에 Spread문법의 대상도 아니다.
for (const p of obj) {
  console.log(p); // TypeError: obj is not iterable
}
```

### 이터레이터(Iterator)

> 이터레이터 프로토콜은 `next`메소드를 소유한다.

- `next` 메소드를 호출하면, 이터러블을 순회하며, `value, done` 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다
console.log("next" in iterator); // true
```

- 이터레이터의 `next`메소드를 호출하면, `value, done` 프로퍼티를 갖는 **이터레이터 리절트(iterator result)** 객체를 반환한다.

```jsx
// 이터레이터의 next 메소드를 호출하면, value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
let iteratorResult = iterator.next();

console.log(iteratorResult); // {value:1, done: false}
```

- 이터레이터의 `next`메소드는 이터러블의 **각 요소를 순회**하기 위한 포이터 역할을 한다.
- `next`메소드를 호출하면, 이터러블을 순차적으로 한 단계씩 순회하며, 이터레이터 리절트 객체를 반환한다.

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다
console.log("next" in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
// next 메소드를 호출할 때마다 이터러블을 순회하며 이터레이터 리절트 객체를 반환한다.
console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```

- 이터레이터의 `next`메소드가 반환하는 이터레이터 리절트 객체의 `value` 프로퍼티는 현재 순회중인 **이터러블의 값을 반환**하고, `done` 프로퍼티는 이터러블의 **순회 완료 여부를 반환**한다.

### 빌트인 이터러블

> ES6에서 제공하는 빌트인 이터러블

```
Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments
```

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다
console.log("next" in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
// next 메소드를 호출할 때마다 이터러블을 순회하며 이터레이터 리절트 객체를 반환한다.
console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}

// 이터러블은 for...of 문으로 순회가 가능하다
for (const item of array) {
  console.log(item);
}

// 문자열은 이터러블이다.
const string = "YO";

// 이터러블은 Symbol.iterator 메소드를 소유한다.
// Symbol.iterator 메소드는 이터레이터를 반환한다.
iter = string[Symbol.iterator]();

// 이터레이터는 next 메소드를 소유한다.
// next 메소드는 이터레이터 리절트 객체를 반환한다.
console.log(iter.next()); // {value: "Y", done: false}
console.log(iter.next()); // {value: "O", done: false}
console.log(iter.next()); // {value: undefined, done: true}

// 이터러블은 for...of문을 순회할 수 있다.
for (const letter of string) {
  console.log(letter);
}

// arguments 객체는 이터러블이다.
(function () {
  // 이터러블은 Symbol.iterator 메소드를 소유한다.
  // Symbol.iterator 메소드는 이터레이터를 반환한다.
  iter = arguments[Symbol.iterator]();

  // 이터레이터는 next 메소드를 소유한다.
  // next 메소드는 이터레이터 리절트 객체를 반환한다.
  console.log(iter.next()); // {value: 1, done: false}
  console.log(iter.next()); // {value: 2, done: false}
  console.log(iter.next()); // {value: undefined, done: true}

  // 이터러블은 for...of 문으로 순회 가능하다.
  for (const arg of arguments) {
    console.log(arg);
  }
})(1, 2);
```

## 이터레이션 프로토콜의 필요성

> 데이터 소비자인 `for...of`문, `spread`문법 등은 아래와 같은 다양한 데이터 소스를 사용한다

```
Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments
```

- 이 데이터 소스는 모두 `이터레이션 프로토콜`을 준수하는 `이터러블`이다. 따라서, 이터러블은 **데이터 공급자의 역할**을 한다.
- 만약, 다양한 데이터 소스가 각자 순회 방식을 갖는다면, 데이터 소비자는 각각의 데이터 소스의 순회방식을 따라야하는 비효율이 발생한다.
- 이 다양한 데이터 소스가 `이터레이션 프로토콜`을 준수하도록 규정하면, 데이터 소비자는 이것만을 지원하도록 구현하면 된다.

```
[ 👩‍💻 정리  ]
  이터레이션 프로토콜은 다양한 데이터 소스가 하나의 순회 방식을 갖도록 규정한다.
  그래서 데이터 소비자가 효율적으로 다양한 데이터 소스를 사용할 수 있도록 한다.
  즉, 이터레이션 프로토콜은 데이터 소비자와 데이터 소스를 연결하는 '인터페이스 역할'을 한다.
```
<img src="https://user-images.githubusercontent.com/87024040/213494146-54f7dc88-6abc-4911-b3f3-ce81fdf0a5b4.png" widht="500">

- 이터러블을 지원하는 `데이터 소비자`는 내부에서 `Symbol.iterator`메소드를 호출해 이터레이터를 생성한다.
- 그리고 `next`메소드를 호출하여 이터러블을 순회한다.
- 그 뒤, `next`메소드가 반환한 이터레이터 리절트 객체의 `value` 프로퍼티 값을 취한다.

### `for...of`문
- `for...of`문은 내부적으로 이터레이터의 `next`메소드를 호출하여 이터러블을 순회한다.
- 그리고 `next` 메소드가 반환한 이터레이터 리절트 객체의 `value` 프로퍼티 값을 `for...of`문의 **변수**에 할당한다.
- 이터레이터 리절트 객체의 `done` 프로퍼티 값이 **false**면, 이터러블의 순회를 계속하고, **true**면 이터러블 순회를 중단한다.

```jsx
// 배열
for(const item of ['a', 'b', 'c']){
  console.log(item);
}

// 문자열
for(const letter of 'abc'){
  console.log(letter);
}

// Map
for(const [key, value] of new Map([['a', '1'], ['b', '2'], ['c', '3']])) {
  console.log(`key : ${key} value: ${value}`); // key : a value : 1...
}

// Set
for (const val of new Set([1, 2, 3])){
  console.log(val);
}
```

#### for...of문이 내부적으로 동작하는 것을 for문으로 표현
```jsx
// 이터러블 
const iterable = [1, 2, 3];

// 이터레이터 
const iterator = iterable[Symbol.iterator]();

for(;;){
  // 이터레이터의 next 메소드를 호출하여 이터러블을 순회한다.
  const res = iterator.next();
  
  // next 메소드가 반환하는 이터레이터 리절트 객체의 done 프로퍼티가 true가 될 때까지 반복
  if(res.done) break;
  
  console.log(res);
}
```

## 커스텀 이터러블
### 커스텀 이터러블 구현
> `일반 객체`는 **이터러블이 아니다**.  <br/>
> `일반 객체`는 `Symbol.iterator` 메소드를 소유하지 않기 때문에 `for...of`문으로 순회할 수 없다.

```jsx
// 일반 객체는 이터러블이 아니다
const obj = {a: 1, b: 2};

// 일반 객체는 Symbol.iterator 메소드를 소유하지 않는다.
// 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of문에서 순회할 수 없다.
for(const p of obj){
  console.log(p); // TypeError: obj is not iterable
}
```

- 하지만, 일반 객체가 이터레이션 프로토콜을 준수하도록 구현하면 이터러블이 된다. 

```jsx
const fibonacci = {
  // Symbol.iterator 메소드를 구현하여 이터러블 프로토콜 준수
  [Symbol.iterator]() {
    let[pre, cur] = [0, 1];
     // 최대값
     const max = 10;
     
     // Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환
     // next 메소드는 이터레이터 리절트 객체 반환
     return {
      // fibonacci 객체 순회할 때마다, next 메소드 호출
      next(){
        [pre, cur] = [cur, pre + cur];
        return {
          value: cur,
          done: cur >= max
        };
      }
     };
  }
};

// 이터러블 최대값을 외부에서 전달할 수 없다.
for(const num of fibonacci){
  // for...of 내부에서 break는 가능.
  // if(num >= 10) break;
  console.log(num); // 1 2 3 5 8
}
```
- `Symbol.iterator`메소드는 `next` 메소드를 갖는 이터레이터를 반환해야한다.
- `next` 메소드는 `done, value` 프로퍼티를 가지는 **이터레이터 리절트 객체** 반환해야한다.
- `for...of`문은 `done` 프로퍼티가 **true**가 될 때까지 반복하고, `done`프로퍼티가 **true**가 되면 반복을 **중지**
- `for...of`문 이외에 `Spread문법`, `디스트럭쳐링 할당`, `Map, Set 생성자`에도 사용된다.

##### Spread 문법 & 디스트럭처링 사용하면 이터러블을 배열로 쉽게 변환할 수 있다.
```jsx
// Spread 문법
const arr = [...fibonacci];
console.log(arr); // [1, 2, 3, 5, 8]

// 디스트럭처링
const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [3, 5, 8]
```

### 이터러블을 생성하는 함수
> 외부에서 값을 전달하는 방법

```jsx
// 이터러블을 반환하는 함수
const fibonacciFn = function(max){
  let[pre, cur] = [0, 1];
  
  return {
    // Symbol.iterator 메소드를 구현해 이터러블 프로토콜 준수
    [Symbol.iterator](){
      // Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환
      // next 메소드는 이터레이터 리절트 객체 반환
      return {
        // fibonacci 객체 순회할 때마다 next 메소드 호출
        next(){
          [pre, cur] = [cur, pre + cur];
          return {
            value: cur,
            done: cur >= max
          };
        }
      };
    }
  };
};

// 이터러블을 반환하는 함수에 이터러블의 최대값 구하기
for (const num of fibonacciFn(10)){
  console.log(num); // 1 2 3 5 8
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수
> 이터레이터를 생성하려면 이터러블의 `Symbol.iterator`메소드를 호출해야하는데,
> 이터러블이면서 이터레이터 객체를 생성하면 더이상 `Symbol.iterator`메소드를 호출하지 않아도 된다.

```jsx
// 이터러블이면서 이터레이터인 객체를 반환
const fibonacciFn = function(max){
  let [pre, cur] = [0, 1]; 
  
  // Symbol.iterable 메소드와 next 메소드 소유한
  // 이터러블이면서 이터레이터 객체 반환 
  reutrn {
     // Symbol.iterator 메소드
     [Symbol.iterator]() {
      return this;
     }, 
     // next 메소드는 이터레이터 리절트 객체 반환 
     next() {
      [pre, cur] = [cur, pre + cur];
      return {
        value: cur,
        done: cur >= max
      };
     }
    };
  };
  
  // iter은 이터러블이면서 이터레이터
  let iter = fibonacciFn(10);
  
  // iter은 이터레이터
  console.log(iter.next()); // {value: 1, done: false}
  console.log(iter.next()); // {value: 2, done: false}
  console.log(iter.next()); // {value: 3, done: false}
  console.log(iter.next()); // {value: 5, done: false}
  console.log(iter.next()); // {value: 8, done: false}
  console.log(iter.next()); // {value: 13, done: true}

  iter = fibonacciFn(10);
  
  // iter = 이터러블
  for(const num of iter){
    console.log(num); // 1 2 3 5 8
  }
```

##### Symbol.iterator 메소드와 next 메소드를 소유한 이터러블이면서 이터레이터.
> Symbol.iterator 메소드는 this를 반환하기 때문에 next 메소드를 갖는 이터레이터 반환
```jsx
{
  [Symbol.iterator](){
    return this;
  }, 
  next() {/***/}
}
```

## 무한 이터러블(infinite sequence)과 Lazy evaluation(지연 평가)
> 무한 이터러블을 생성하는 함수는 `무한 수열`을 간단히 표현 가능하다.

```jsx
// 무한 이터러블 생성 함수
const fibonacciFn = function() {
  let [pre, cur] = [0, 1];
  
  return {
    [Symbol.iterator](){
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
};

// fibonacciFn 함수는 무한 이터러블을 생성
for (const num of fibonacciFn()){
  if(num > 10000)break;
    console.log(num); // 1 2 3 5 8 ......
}

// 무한 이터러블에서 3개만을 취득
const [f1, f2, f3] = fibonacciFn();
console.log(f1, f2, f3); // 1 2 3
```

## 정리
```
[ 👩‍💻 정리  ]
  이터러블은 데이터 공급자의 역할을 한다.
  배열, 문자열, Map, Set등 빌트인 이터러블은 데이터를 모두 메모리에 확보하고나서 동작한다.
  그러나 이터러블은 Lazy evaluation(지연평가)를 통해 값을 생성한다.
  지연 평가는 평가 결과가 나올 때까지 평가를 늦춘다.
  
  fibonacciFn 함수는 무한 이터러블을 생성한다.
  여기서 데이터 소비자인 for...of문이나 디스트럭처링이 할당되기 전까지 데이터를 생성하지 않는다.
  for...of문에서는 이터러블을 순회할 때 내부에서 이터레이터 next할때 데이터가 생성된다.
  next 메소드가 호출되기 전까지는 데이터를 생성하지 않는다.
  
  * 데이터가 필요할 때까지 데이터 생성을 지연하다 데이터가 필요할때 데이터를 생성한다 *
```
