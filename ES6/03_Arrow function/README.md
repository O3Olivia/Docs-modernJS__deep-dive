# 화살표 함수(Arrow function)

> 화살표 함수는 `function` 키워드 대신 `화살표(=>)`를 사용하여 함수를 선언한다. <br/>
> 그러나 모든 함수를 화살표 함수로 선언할 수 X.

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
var testFive = function (x) {
  return x * x;
};
console.log(testFive(20)); // 400

// ES6
let testSix = (x) => x * x;
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
let arr = [1, 2, 3];
let testSix = arr.map((x) => x * x);

console.log(testSix); // [1, 4, 9]
```

# this
> 화살표 함수는 다른 함수의 인수로 전달되어 `콜백 함수`로 사용되는 경우가 많다. 여기서 화살표 함수의 `this`는 일반 함수와 다르게 동작한다.

#### 일반 함수의 this

> 일반 함수의 경우, 함수를 호출 할 때 `해당 함수가 어떻게 호출되었는지`에 따라 this에 바인딩 되는 객체가 동적으로 결정된다.<br/>

```
[ 💡 Note ]
Java에서 this는 인스턴스 자신(self)를 가르키는 참조변수라 this가 객체 자신에 대한 참조값을 가지고 있지만,
JavaScript에서 this는 this에 바인딩 되는 객체가 한가지가 아니라, 해당 함수 호출 방식에 따라 객체가 달라진다.
```

### 일반 함수의 호출 방식

- 함수 호출

  > 일반 모드에서 함수 호출 `this`는 `전역`객체 (모든 객체의 유일한 최상의 객체)다. <br />
  > Browser-side에서는 `window`, Server-side에서는 `global`을 의미한다.

  ```jsx
  // Browser-side
  this === window; // true

  // Server-side
  this === global; // true
  ```

  - 기본적으로 `this`는 전역 객체에 바인딩된다. <br/>
  - 전역 함수 뿐만 아니라 내부 함수도 `this`는 외부 함수가 아니라 `전역 객체`에 바인딩된다.

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

  - 메소드의 내부 함수일 경우에도 `this`는 `전역 객체`에 바인딩된다.
  - 함수가 객체의 프로퍼티 값이면 메소드로서 호출된다. <br/>
    이때 메소드 내부의 `this`는 해당 메소드를 소유한 객체로, 해당 메소드를 호출한 객체에 바인딩된다.

    ```jsx
    var obj1 = {
      name: "Lee",
      getName: function () {
        console.log("this의 이름 : " + this.name);
      },
    };

    var obj2 = {
      name: "Park",
    };
    obj2.getName = obj1.getName;

    obj1.getName();
    obj2.getName();
    // this의 이름 : Lee
    // this의 이름 : Kim
    ```

  - 콜백 함수의 경우에도 `this`는 `전역 객체`에 바인딩된다.

    ```jsx
    var value = 10;

    var obj = {
      value: 20,
      test: function () {
        setTimeout(function () {
          console.log("callback's this : ", this); // window
          console.log("callback's this.value : ", this.value); // 10;
        }, 20);
      },
    };
    obj.test();
    ```

    > 내부 함수는 `일반함수`, `메소드`, `콜백함수` 어디에서 선언되었든 관계없이 `this`는 `전역객체`를 바인딩한다.

- 생성자 함수 호출

  > 생성자 함수는 말 그대로 생성자를 생성하는 역할을 한다. <br/>
  > 함수나 메서드를 호출할 때 앞에 키워드 `new`를 붙이면 생성자로 호출한다. <br/>
  > 단, 생성자 함수가 아닌, 일반 함수에 `new`를 붙이면 생성자 함수처럼 동작할 수 있어서 일반적으로 생성자 함수명은 첫문자를 `대문자`로 적는다.

  ```jsx
  function Person(name) {
    this.name = name;
  }

  var me = new Person("Lee");
  console.log(me); // Person { name: 'Lee' }

  var you = Person("Kim");
  console.log(you); // undefined -> new 붙여야함
  console.log(window.you); // Kim
  ```

  - 생성자 함수를 `new` 없이 호출하면, Person 내부의 `this`는 전역객체를 가르키기 때문에 name은 전역변수(window)에 바인딩된다.

- apply() / call() / bind() 호출

  ```jsx
  var Person = function (name) {
    this.name = name;
  };

  var me = {};

  Person.apply(me, ["oli"]);

  console.log(me); // { name: 'oli' }
  ```

  - apply() 메소드는 Person을 호출한다. <br />
    이때 `this`에 me 객체를 바인딩한다.

  ```jsx
  function Person(name) {
    this.name = name;
  }

  Person.prototype.doSomething = function (callback) {
    if (typeof callback == "function") {
      callback(); // 1
    }
  };

  function getName() {
    console.log(this.name); //2
  }

  var a = new Person("Lee");
  a.doSomething(getName); // undefined
  ```

  - 1️⃣의 this는 Person객체
  - 2️⃣의 this는 전역객체라 window를 가르킨다. 이때 두 함수의 this가 달라서 undefined가 뜨는데, 이를 해결하기위해 `call`메소드 사용.<br/>
    - call()메소드 : 주어진 this 값 및 각각 전달된 인수와 함께 함수를 호출한다.

  ```jsx
  function Person(name) {
    this.name = name;
  }

  Person.prototype.doSomething = function (callback) {
    if (typeof callback == "function") {
      callback.call(this);
    }
  };

  function getName() {
    console.log(this.name);
  }

  var a = new Person("Lee");
  a.doSomething(getName); // 'Lee'
  ```

  **일반 함수** : 함수를 선언할 때 `this`에 바인딩할 객체가 정적으로 결정되지 x<br/>함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 `this`에 바인딩할 객체가 동적으로 결정된다.

### 화살표 함수의 this
> 화살표 함수는 `함수를 선언`할 때 정적으로 결정된다.
> 동적으로 결정되던 일반 함수와 달리 자신을 포함하는 외부 스코프에서 `this`를 받는데, <br/>
> 화살표 함수의 `this`는 상위 스코프의 `this`를 카르키며 이것을 `Lexical this`라고 한다.

```jsx
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map((x) => `${this.prefix}  ${x}`);
};
// 여기서 this는 자신이 아니라 본인을 포함하는 외부 스코프를 말함. Prefixer를 말함

const pre = new Prefixer("Hello!");
console.log(pre.prefixArray(["Lee", "Kim"]));
```

- 화살표 함수는 `call()`, `apply()`, `bind()`메소드를 사용하여 `this`를 변경할 수 X

  ```jsx
  window.a = 1;
  const normal = function () {
    return this.a;
  };
  const arrow = () => this.a;

  console.log(normal.call({ x: 100 })); // 100
  console.log(arrow.call({ x: 100 })); // 1
  ```
##### 일반 함수와 화살표 함수의 `this`가 다른 이유
- `콜백 함수` 내부의 `this`문제를 해결하기 위해 다르게 설계되었다.
  - 일반함수 : `this`는 함수가 `누구`로부터 호출되었는지 => `동적`으로 결정
  - 화살표 함수 : 함수 자체의 `this` 바인딩을 갖지 않는다. 
               함수 내부에서 `this`를 참조하면, `상위 스코프 this`를 참조한다. => `Lexical this`
               마치, `Lexical` 스코프처럼 **함수가 정의된 위치에 의해 결정**되는 것처럼 보이기 때문. 
               화살표 함수는 함수 자체의 `this`의 바인딩을 갖지 않아 `Function,prototype`에서 
               `apply`, `bind` 메서드를 사용해도 **화살표 함수 내부의 this를 교체할 수 X **
               
# 화살표 함수를 사용해선 안되는 경우

> 화살표 함수는 `Lexical this`를 지원하므로 콜백 함수로 사용하기 좋지만,<br/>
> 오히려 혼란을 불러일으키는 경우가 있어서 아래와 같은 상황에서는 화살표 함수를 사용하는 것을 주의하여야한다.

### 메소드

> 화살표 함수로 메소드를 정의 X
> 화살표 함수로 메소드를 정의하면 `this`때문에 원하지 않은 결과가 나올 수 있다.

```jsx
const person = {
  name: "Lee",
  greeting: () => console.log(`Hello, ${this.name}`),
};

person.greeting(); // Hello undefined
```

- 메소드로 정의한 화살표 함수 내부에서 `this`는 이 메소드를 호출한 객체를 가르키는 것이 아니라 전역 객체인 `window`를 가르키기 때문에 undefined가 뜬다.<br/>

##### ES6 축약 메소드 표현 (권장)

```jsx
const person = {
  name: "Lee",
  greeting() {
    console.log(`Hello, ${this.name}`);
  },
};

person.greeting(); // Hello, Lee
```

### Prototype

```
[ 💡 Note ]
prototype 따로 정리. 프로토타입과 프로토타입 상속 확인.
```

- 화살표 함수로 정의된 메소드를 prototype에 할당

```jsx
const person = {
  name: "Lee",
};

Object.prototype.greeting = () => console.log(`Hello, ${this.name}`);

person.greeting(); // Hello, undefined
```

- 메소드를 prototype에 할당하면, 메소드를 정의한 것과 동일한 문제가 발생한다. 따라서 이 경우, `일반 함수`를 할당해야한다.

```jsx
const person = {
  name: "Lee",
};
Object.prototype.greeting = function () {
  console.log("Hello, ${this.name}");
};

person.greeting(); // Hello, Lee
```

### 생성자 함수

> 화살표 함수는 생성자 함수로 사용할 수 없다. <br/>
> 생성자 함수는 `prototype 프로퍼티`를 가지며, `prototype 프로퍼티`가 가르키는 프로토타입 객체의 `constructor`를 사용한다.
> 그러나 화살표 함수는 `prototype 프로퍼티`를 가지고 있지 않기 때문에 사용할 수 없다.

```jsx
const Test = () => {};

console.log(Test.hasOwnProperty("prototype")); // false

const test = new Test(); // TypeError: Test is not a constructor
```

- `hasOwnProperty()`메소드는 (key) key 이름이 있으면 `true`, 없으면 `false`를 반환한다.
- 화살표 함수는 prototype 프로퍼티가 없어서 false를 반환한다.

### `addEventListener`함수의 콜백 함수

> `addEventListener` 함수의 콜백 함수를 화살표함수로 정의할 경우, `this`는 전역객체인 `window`를 가르킨다.

```jsx
var button = document.getElementById("btn");

button.addEventListener("click", () => {
  console.log(this === window); // true
  this.innerHTML = "CLiCKED BUTTON";
});
```

- 따라서 `addEventListener`를 콜백 함수 내에서 `this`를 사용하려면 **화살표 함수로 정의하지 말고 일반 함수로 정의**하는 것이 좋다.

```jsx
var button = document.getElementById("btn");

button.addEventListener("click", function () {
  console.log(this === button); // true
  this.innerHTML = "CLiCKED BUTTON";
});
```
