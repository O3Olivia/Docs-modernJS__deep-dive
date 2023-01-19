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
