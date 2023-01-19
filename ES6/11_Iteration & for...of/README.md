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
