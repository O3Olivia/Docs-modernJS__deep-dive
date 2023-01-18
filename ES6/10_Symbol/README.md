# Symbol( 타입 심볼 )

## Symbol이란?

> 타입스크립트는 6개의 타입을 가지고 있다.

```
[ 💡 NOTE  ]
변수 : 값을 저장하기 위한 메모리 공간 또는 그 메모리 공간을 식별하기 위해 붙인 이름
값 : 표현식이 평가되어 생성된 변수에 저장된 데이터
원시타입 : - 변경 불가능한 값. 한 번 생성된 값은 읽기 전용으로 변경할 수 없다.
        - 원시 타입은 '값' -> 값에 의한 전달
        - 값에 의한 전달의 의미는 값이 전달되는 것이 아니라, 메모리 주소가 전달 된다는 말이다.

객체타입 : - 변경 가능한 값
         -
```

- 원시 타입(Primitive data type)
  - Boolean
  - null
  - undefined
  - Number
  - String
- 객체 타입(Object type)
  - Object

> 이 6개의 타입에서 Symbol은 ES6가 새롭게 추가한 타입으로, 변경이 불가능한 `원시 타입`이다.
> Symbol 타입은 `이름 충돌이 없는 유일한 객체의 프로퍼티 키(property key)`를 만들기 위해 사용한다.

## Symbol 생성

> Symbol은 `Symbol()`로 생성하며, 이 함수는 호출될 때마다 값을 생성하며, 이 값은 변경 불가능한 `원시타입 값`이다.

```jsx
let mySymbol = Symbol();

console.log(mySymbol); // Symbol()
console.log(typeof mySymbol); // symbol
```

- Symbol()함수는 `string`, `Number`, `boolean`과 깉이 래퍼객체를 생성하는 생성자 함수와 달리 `new 생성자`를 사용하지 **않는다.**

```jsx
new Symbol(); // TypeError
```

```
[ 💡 NOTE  ]
Number, String, Boolean 타입은 new 연산자를 이용해서 래퍼 객체 생성이 가능하다.
이렇게 생성된 래퍼 객체는 해당 타입의 원시 값을 저장하고 있으며, 메소드들을 가지고 있다.
만약, new 연산자를 사용하지 않고, 위 타입의 함수를 호출하면, 해당 타입의 원시 값만 생성되고 래퍼 객체는 생성되지 않는다.
```

```
[ 💡 NOTE  ]
래퍼객체: 원시 타입의 값을 감싸는 형태의 객체.
number, string, boolean, symbol 데이터 타입에 대응하는
Number, String, Boolean, Symbol 이다.
만약, 문자열 프로퍼티에 접근할 때, 자바스크립트는 new String()을 호출한 것 처럼 문자열 값을 객체로 변환하는데 이 객체가 래퍼객체다.
이 래퍼 객체는 프로퍼티를 참조할 때 생성되고, 프로퍼티 참조가 끝나면 사라진다.

래퍼 타입들 덕에 기본형 값에 메서드를 사용할 수 있고, 정적 메서드(String.fromCharCode와 같은)도 사용할 수 있다.
```

- Symbol()함수는 `문자열`을 인자로 전달할 수 있다.
- 이 `문자열`은 Symbol생성에 영향을 주지 않고, Symbol에 대한 **설명**으로 `디버깅`용도로 사용된다.
- `console.log`를 이용한 디버깅 시, 각 Symbol를 구분하기 위한 용도다.

```jsx
let mySymbolDesc = Symbol("심볼입니동");
let symbol1 = Symbol("1");
let symbol2 = Symbol("1");

console.log(mySymbolDesc); // Symbol(심볼입니동)
console.log(mySymbolDesc === Symbol("심볼입니동")); // false
console.log(mySymbolDesc === mySymbolDesc); // true
console.log(symbol1); // Symbol(1)
console.log(symbol2); // Symbol(1)
console.log(symbol1 === symbol2); // false
```

## Symbol의 사용

> Symbol은 `객체의 프로퍼티 키`로 사용된다.

```
[ 💡NOTE ]
프로퍼티 키 : 해당 프로퍼티 값에 접근할 때 사용하는 이름.
           대부분 문자열 값이며, 숫자를 써도 문자열로 생각하면 된다.
```

#### 객체의 프로퍼티키는 빈 문자열을 포함해서 모든 `문자열`로 만들 수 있다.

```jsx
const obj = {};

obj.prop = "hello";
obj[123] = 123; // 123은 문자열로 변환된다.
// obj.123 = 123; // SyntaxError: Unexpected number
obj["prop" + 123] = false;
console.log(obj); // {'123' : 123, prop: 'hello', prop123: false}
```

#### 객체의 프로퍼티 키를 Symbol 값으로 사용할 수 있다.

> 보통 프로퍼티 키는 문자열 값으로 되어있으나 문자열 대신 Symbol을 사용할 수 있다.
> 이렇게 되면, Symbol은 고유하기 때문에 Symbol을 키로 갖는 프로퍼티는 다른 프로퍼티와 충돌이 일어나지 않는다.

```jsx
const obj = {};

const mySymbol = Symbol("hello");
obj[mySymbol] = 123;

console.log(obj); // {[Symbol(mySymbol)]: 123}
console.log(obj[mySymbol]); // 123;
```

## Symbol 객체

> Symbol()함수로 Symbol 값을 생성할 수 있는데, 이는 Symbol이 `함수 객체`라는 의미다.<br/>

- Symbol객체는 프로퍼티와 메소드를 가지고 있는데, `length`와 `prototype`을 제외하고는 모두 `well-known symbol`이라 부른다.
- 이 `well-known symbol`은 자바스크립트 엔진에 상수로 존재하며, 자바스크립트는 이를 참조하여 일정한 처리를 한다.
- 이렇게 미리 생성되어 상수로 존재하는 symbol을 `내장 심볼(Built-in Symbol)`이라 한다.

### Symbol.iterator

> `well-known symbol`의 `내장 심볼(Built-in Symbol)` 중 가장 대표적인 예시 중 하나다.
> 자바스크립트는 이 엔진으로 Symbol을 키로 갖는 메소드가 정의된 객체를 `iterable` 객체로 인식한다.
> 그리고 이렇게 `iterable` 객체로 인식되는 객체들만 `for ...of`문법 등으로 반복 할 수 있다.

- Symbol.iterator를 프로퍼티 key로 사용하여 메소드를 구현하고 있는 빌트인 객체는 iterator 프로토콜을 준수하며, `iterator`를 반환한다.

```jsx
// Array
Array.prototype[Symbol.iterator];
// String
String.prototype[Symbol.iterator];
// Map
Map.prototype[Symbol.iterator];
// Set
Map.prototype[Symbol.iterator];
// DOM data structures
NodeList.prototype[Symbol.iterator];
HTMLCollection.prototype[Symbol.iterator];
// arguments
arguments[Symbol.iterator];
```

##### 이터러블

> Symbol.iterator를 프로퍼티 key로 사용한 메소드를 구현하여야 한다.
> 배열에는 Array.prototype[Symbol.iterator] 메소드가 구현되어 있다.

```jsx
const iterable = ["a", "b", "c"];
```

##### 이터레이터

> 이터러블의 Symbol.iterator를 프로퍼티 key로 사용한 메소드는 이터레이터를 반환한다.

```jsx
const iterator = iterable[Symbol.iterator]();
```

- 이터레이터는 순회 가능한 자료 구조인 **이터러블의 요소를 탐색**하기 위한 포인터다.
- `value`, `done` 프로퍼티를 갖는 객체를 반환하는 `next() 함수`를 메소드로 갖는 객체다.
- 이터레이터의 `next() 메소드`를 통해 이터러블 객체를 순회할 수 있다.

```jsx
console.log(iterator.next()); // { value: 'a', done: false }
console.log(iterator.next()); // { value: 'b', done: false }
console.log(iterator.next()); // { value: 'c', done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

### Symbol.for

> `Symbol.for` 메소드는 인자로 전달받은 문자열을 **키**로 사용하여 Symbol 값들이 저장되어 있는 전역 Symbol 레지스트리에서 해당 키와 일치하는 Symbol 값을 **검색**한다.

- 검색에 성공하면, Symbol 값을 **반환**
- 검색에 실패하면, 새로운 Symbol값을 **생성**하여, 해당 키로 전역 Symbol 레지스트리에 저장한 뒤, 생성한 Symbol 값을 반환한다.

```jsx
const sym1 = Symbol.for("test1");
const sym2 = Symbol.for("test1");

console.log(sym1 === sym2); // true
```

- sym1은 전역 Symbol 레지스트리에 test1이라는 키로 저장된 Symbol이 없어서 새로운 Symbol을 생성한다.
- sym2는 레지스트리에 test1이라는 키로 생성된 Symbol이 있어서 해당 Symbol을 반환하기 때문에 sym1과 sym는 동일하다.

#### Symbol vs Symbol.for

> Key 값의 유무

```
Symbol : Symbol함수는 매번 다른 Symbol값을 생성한다.
         Symbol함수로 생성된 Symbol 값은 key X
Symbol.for : Symbol.for메소드는 하나의 Symbol을 생성해 여러 모듈이 key를 통해 같은 Symbol를 공유할 수 있다.
             Symbol.for 메소드를 통해 생성된 Symbol은 반드시 Key O
```

```jsx
const shareSym = Symbol.for("MYKEY");
const key1 = Symbol.keyFor(shareSym);
console.log(key1); // MYKEY

const unsharedSym = Symbol("MYKEY");
const key2 = Symbol.keyFor(unsharedSym);
console.log(key2); // undefined
```

##### Symbol.keyFor() 메소드

> 인자로 전달받은 Symbol을 전역 심볼 레지스트리에서 찾은 뒤, Symbol의 `키`를 반환하고, 탐색에 실패하면 `undefined`를 반환
