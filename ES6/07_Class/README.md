# 클래스(Class)
> 자바스크립트는 `프로토타입 기반(Prototype-based)`객체지향 언어다. <br/>
> 자세한 내용은 따로 정리해두었으니 참고할 것.


## 클래스 정의(Class Definition)

##### ES5
> ES5에서는 **생성자함수**와 **프로토타입**, **클로저**를 사용해서 객체지향프로그래밍을 구현했다.

```jsx
var Person = (function() {
// constructor
  function Person(name){
    this._name = name;
  }
  
  // public method
  Person.prototype.greeting = function () {
    console.log("안녕~ " +this._name);
  };
  
  // return constructor
  return Person;
}());

var me = new Person("Olivia";
me.greeting(); // 안녕~ Olivia
conosole.log(me instanceof Person); // true
```

###### ES6
> ES6 클래스는 `class`키워드를 사용하여 정의한다. 

```jsx
// class 선언문
class Person{

  // constructor(생성자)
class Person{
  constructor(name){
  this._anem = name;
  }
  
  greeting() {
    console.log(`안녕~ ${this._name}`);
  }
}

// 인스턴스 생성 
const me = new Person("Olivia");
me.greeting(); // 안녕~ Olivia

console.log(me instanceof Person); // true
```
- 여기서 클래스를 선언하기 전에 참조하면 안된다.
```jsx
console.log(test); // ReferenceError

class test {}
```
- 클래스 선언문도 `변수 선언`, `함수 정의`처럼 호이스팅이 발생한다. 
- 호이스팅은 `var`, `let`, `const`, `function`, `function*`, `class`키워드를 사용한 모든 선언문에 적용된다.
- 왜냐하면 모든 선언문은 런타임 이전에 먼저 실행되기 때문이다. 

## 인스턴스 생성
> 마치 생성자 함수와 같이 `new 연산자`와 함께 클래스 이름을 호출하면 클래스의 인스턴스가 생성된다.
```jsx
class Test{}

const test = new Test();

console.log(Object.getPrototypeOf(test).constructor === Test); // true
```
- 이렇게 `new 연산자`와 함께 호출한 Test는 클래스의 이름이 아니라, **constructor(생성자)**다.
- 표현식이 아니라 선언식으로 정의된 클래스 이름은 `constructor`와 동일하기 때문이다.


```jsx
class Test{}
const test - Test(); // TypeError
```
- constructor은 `new 연산자` 없이 호출할 수 없기 때문에 위와 같은 상황에서는 **TypeError**가 발생한다.

## constructor
> `constructor`은 **인스턴스를 생성**하고 **클래스 필드를 초기화**하기 위한 특수한 메소드다.

```
[ 💡 NOTE  ]
클래스 필드(class field)
클래스 내부 캡슐화 된 변수며 데이터 멤버 또는 멤버 변수라고도 부른다.
즉, 자바스크립트의 생성자 함수에서 'this'에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 '클래스 필드'라고 부른다.
```

```jsx
// 클래스 선언문 
class Person {
   // constructor -> 이름을 바꿀 수 없다.
   constructor(name){
    // this는 클래스가 생성할 인스턴스를 가르킨다.
    // _name은 클래스 필드다.
    this._name = name;
   }
}

// 인스턴스 생성 
const me = new Person("Olivia");
console.log(me); // Person{_name : "Olivia"}
```
- 인스턴스를 생성할 때 `new 연산자`와 함께 호출한 것이 constructor다.
- 그리고 constructor의 파라미터에 전달된 값은 클래스 필드에 할당한다.
```
[ 💡 NOTE  ]
여기서 중요한 점! 
constructor은 클래스 내에 '한 개'만 존재 
만약, 2개 이상의 constructor하면 'SyntaxError(문법에러)'가 발생한다.
```

##### constructor는 생략할 수 있다.
> constructor를 생략하면, 클래스에 `constructor(){}`가 포함된 것과 동일하게 동작한다.
> 즉 빈 배열이 생성되고, 여기다가 프로퍼티를 추가할 수 있다.

```jsx
class Test {}

const test = new Test();
console.log(test); // Test {}

// 프로퍼티 동적 할당 및 초기화
test.num = 10;
console.log(test); // Test {num: 10}
```

##### 만약, 클래스 필드를 초기화해야한다면, constructor는 생략 X
> constructor는 인스턴스를 생성함과 동시에 클래스 필드를 생성하고 초기화하기 때문이다.

```jsx
class Test {
  constructor(num){
    this.num = num;
  }
}

const test = new Test(10);

console.log(test); // Test {test : 10}
```

## 클래스 필드
> 클래스 몸체(class body)에는 `메소드`만 선언할 수 있다. <br/>
> 만약, 변수를 선언하면 문법 에러가 발생한다.

```jsx
class Test{
  name = ''; // SyntaxError
  
  constructor(){}
}
```

- 따라서 클래스 선언과 초기화는 반드시 constructor 내부에서 실시해야한다.
```jsx
class Test {
  constructor(name = '') {
    this.name = name; // 클래스 필드에 선언과 초기화
  }
}
  const test = new Test("Olivia");
  consoel.log(test); // Test {name: "Olivia"}
```
- constructor 내부에 선언한 클래스 필드는 클래스가 생성할 인스턴스를 가르키는 `this`에 바인딩한다.
- 따라서 클래스 필드는 클래스가 생성할 인스턴스의 프로퍼티가 된다.
- 그리고 클래스 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. **언제나 `public`**
```jsx
class Test {
   constructor(name = ""){
     this.name = name; // public 클래식 필드
   }
}

const test = new Test("Olivia");
console.log(test.name); // Olivia -> 클래스 외부에서 참조할 수 있다.
```

## getter, setter

### getter
> getter는 클래스 필드에 **접근**할 때마다 클래스 필드의 값을 조작하는 행위가 필요할 때 사용한다.

- getter는 메소드 이름 앞에 `get`을 사용해야한다.
- 이때 메소드의 이름은 클래스 필드 이름처럼 사용되는데, getter을 호출하는 것이 아니라, **프로퍼티처럼 참조하는 형식으로 사용하는 것**이다.
- getter은 이름 그대로 무엇인가 얻을 때 사용하는 거라, 반드시 무언가를 반환해야한다.

```jsx
class Test{
  constructor(arr = []){
    this._arr = arr;
  }
  
  // getter: get 키워드 뒤에 오는 메소드 이름 firstElem은 클래스 필드 이름처럼 사용된다.
  get firstElem() {
    // getter은 반드시 무언가를 return해야한다.
    return this._arr.length ? this._arr[0] : null;
  }
}

const test = new Test([1, 3]);

// 필드 firstElem에 접근하면, getter이 바로 호출되면서 값을 반환한다.
console.log(test.firstElem); // 1 
// -> 1, 3이 호출 안되고 1만되는 이유: length가 있으면, this._arr[0]첫번째 값만 불러오라 설정
```

### setter
> setter은 클래스 필드 값에 **할당**할 때마다, 클래스 필드 값을 조작하는 행위가 필요할 때 사용한다.

- setter은 메소드 이름 앞에 `set`을 사용한다.
- 메소드 이름은 클래스 필드 이름처럼 사용되는데, setter을 호출하는 것이 아니라, **값을 할당**하는 형식으로 사용하며 메소드가 호출된다.

```jsx
class Test{
    constructor(arr = []) {
      this._arr = arr;
     }
     
     // getter
     get firstElem() {
      return this.arr_lenght ? this._arr[0] : null;
     }
     
     // setter
     set firstElem(elem){
       // ...this._arr는 this._arr를 개별 요소로 분리한다.
       this._arr = [elem, ...this._arr];
     }
  }
  
  const test = new Test([1, 3]);
  
  // 클래스 필드 firstElem에 값을 할당하면 setter가 호출
  test.firstElem = 100;
  console.log(test.firstElem); // 100
```

## 정적 메소드
> 클래스의 정적(static)메소드를 정의할 때 `static` 키워드를 사용한다. <br/>
> 정적 메소드는 클래스의 인스턴스가 아닌 클래스 이름으로 호출해서 인스턴스를 생성하지 않아도 호출 가능하다.

```jsx
class Test {
  constructor(prop){
    this.prop = prop;
  }
  
  static staticMethod(){
    return 'staticMethod';
  }
  
  prototypeMethod() {
    return this.prop;
  }
}

// 정적 메소드는 클래스 이름으로 호출한다.
console.log(Test.staticMethod());

const test = new Test("hi");
// 정적 메소드는 인스턴스로 호출할 수 없다.
console.log(test.staticMethod()); // Uncaught TypeError
```
- 정적 메소드는 `this`를 사용할 수 없다.
- 정적 메소드 내부에서 `this`는 클래스의 인스턴스가 아닌 클래스 자기 자신이다.
```
[💡 NOTE  ]
정적 메소드는 Math 객체의 메소드처럼 애플리케이션 전역에서 사용할 유틸리티(utility)함수를 생성할 때로
주로 사용한다.
```

##### 사실 class도 **함수**이고, 기존 prototype 기반 패턴의 Syntactic sugar일 뿐이다.
```jsx
class Test{}

cosnole.log(typeof Test); // function
```
##### 위의 예시를 ES5로 변경

```jsx
var Test = (funciton () {
    function Test(prop) {
    this.prop = prop;
  }
  
  Test.staticMethod = function () {
    return 'staticMethod';
  };

  Test.prototype.prototypeMethod = function () {
    return this.prop;
  };
  
  return Test;
}());

var test = new Test("hi");
console.log(test.prototypeMethod()); // hi
console.log(Test.staticMethod()); // staticMethod
console.log(test.staticMethod()); // Uncaught TypeError
```

- **함수 객체는 prototype 프로퍼티를 갖**는데 일반 객체와는 다르며, **일반 객체는 prototype 프로퍼티를 갖지 않는다**.
- 함수 객체만이 가지고 있는 `prototype 프로퍼티`는 함수 객체가 **생성자**로 사용될 때, <br/>
  이 함수를 통해 생성된 객체의 `부모`역할을 하는 프로토타입 객체를 의미한다.
- 위의 예시는 Test는 생성자 함수. <br/>
  생성자 함수 Test의 prototype 프로퍼티가 가르키는 프로토타입 객체는 생성자 함수 Test를 통해 생성되는 <br/>
  인스턴스 test의 부모역할을 한다.
  ```jsx
  console.log(Test.prototype === test.__proto__); // true
  ```
  - 생성자 함수 Test의 prototype 프로퍼티가 가르키는 프로토타입 객체가 가지고 있는 constructor는 <br/>
  생성자 함수 Test를 가르킨다.
  ```jsx
  console.log(Test.prototype.constructor === Test);// true
  ```
  <img src="https://user-images.githubusercontent.com/87024040/210776674-0e738092-6723-4c12-938b-98c0f31a8234.png" widht="550" height="300">
 ```
 [ 👩‍ NOTE  ]
 정적 메소드인 staticMethod는 생성자 함수 Test의 메소드고, 
 일반 메소드인 prototypeMethod는 프로토타입 객체 Test.prototype 메소드다.
 따라서 staticMethod는 test에서 호출될 수 없다.
 ```

## 클래스 상속
> 클래스 상속(Class Inheritance)는 코드를 재사용할 때 유용하다. <br/>
> 만약 새롭게 정의해야 될 클래스가 기존의 클래스와 매우 유사할 경우, `상속`을 통해 그대로 사용할 수 있다.<br/>
> 그리고 다른 점만 따로 구현하면 되기 때문에 개발 비용을 현저히 줄일 수 있다.

### `extends` 키워드
> **extends**키워드는 `부모 클래스`를 상속받는 `자식 클래스`를 정의할 때 사용.

```jsx
// 부모 클래스
class Circle {
  constructor(radius) {
    this.radius = radius; // 반지름
  }
  
  // 원의 지름 
  getDiameter() {
    return 2 * this.radius;
  }
  
  // 원의 둘레
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  }
  
  // 원의 넓이
  getArea(){
    return Math.PI.Math.pow(this.radius, 2);
  }
}

// 자식 클래스
class Cylinder extends Circle {
  constructor(radius, height){
     super(radius);
     this.height = height;
  }
  
  // 원통의 넓이 : 부모 클래스의 getArea 메소드를 오버라이딩
  getArea(){
    // (원통의 높이 * 원의 둘레) + (2 * 원의 넓이)
    return (this.height * super.getPerimeter()) + (2*super.getAre());
  }
  
  // 원통의 부피
  getVolume() {
    return super.getArea() * this.height;
  }
}

// 반지름이 2, 높이가 10인 원통
const cylinder = new Cylinder(2, 10)l

// 원의 지름
console.log(cylinder.getDiameter());  // 4
// 원의 둘레
console.log(cylinder.getPerimeter()); // 12.566370614359172
// 원통의 넓이
console.log(cylinder.getArea());      // 150.79644737231007
// 원통의 부피
console.log(cylinder.getVolume());    // 125.66370614359172

// cylinder은 Cylinder 클래스의 인스턴스다.
console.log(cylinder instanceof Cylinder); // true

// cylinder는 Circle 클래스의 인스턴스다.
conosle.log(cylinder instance of Circle); // true
```

- **오버라이딩(Overriding)** : `상위클래스`가 가지고 있는 메소드를 `하위클래스`가 **재정의**하여 사용
- **오버로딩(Overloading)** : 같은 이름의 메서드 여러개를 가지면서 매개변수의 유형과 개수가 다르도록 하는 기술
<img src="https://user-images.githubusercontent.com/87024040/210942193-d95bf25b-72a5-404f-a9e1-7d7663998cb9.png" widht="600" height="300">

```jsx
console.log(cylinder.__proto__ === Cylinder.prototype); // true
console.log(Cylinder.prototype.__proto__ === Circle.prototype); // true
console.log(Circle.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__ === null); / true
```

### `super`의 키워드
> `super`키워드는 `부모 클래스`를 참조(reference)할 때 또는 `부모 클래스`의 constructor을 호출할 때 사용

##### `자식 클래스`의 `constructor`에서 `super()`를 호출하지 않으면, `this`에 대한 참조 에러가 발생
```jsx
class Parent {}

class Child extends Parent{
  constructor() {}
} // ReferenceError

const child = new Child();
```
##### `super`메소드를 호출하기 전에는 `this`를 참조할 수 없다.
- `super`키워드는 부모 클래스에 대한 참조다. 부모 클래스의 필드 또는 메소드를 참조하기 위해 사용.

### `static`메소드 & `prototype`메소드의 상속
> 자식 클래스의 [[Prototype]] 내부 슬롯이 가르키는 프로토타입 객체는 부모 클래스다.

```jsx
class Parent {}

class Child extends Parent {}

console.log(Child.__proto__ === Parent); // true
console.log(Child.prototype.__proto__ === Parent.prototype); // true
```
- 이는 `prototype chain`에 의해 부모 클래스의 `정적 메소드`도 **상속**된다.
```jsx
class Parent {
  static staticMethod() {
    return 'staticMethod';
  }
}

class Child extends Parent {}

console.log(Parent.staticMethod()); // 'staticMethod'
console.log(Child.staticMethod()); // 'staticMethod'
```

##### 자식 클래스 정적 메소드 내부에서 `super`키워드를 사용하여 부모 클래스의 정적 메소드를 호출할 수 있지만, 자식 클래스의 일반메소드(프로토타입 메소드)내부에서는 `super`키워드를 사용하여 부모 클래스의 정적 메소드를 호출할 수 없다.
> 자식 클래스는 `프로토타입 체인`에 의해 부모 클래스의 `정적 메소드를 참조`할 수 있지만,
> 자식 클래스의 인스턴스는 부모 클래스의 정적 메소드는 참조할 수 없다.

```jsx
class Parent {
  static staticMethod(){
    return 'Hello';
  }
}

class Child extends Parent {
  static staticMethod(){
    return `${super.staticMethod()} word`;
  }
  
  prototypeMethod() {
    return `${super.staticMethod()} world`;
  }
}

console.log(Parent.staticMethod()); // Hello
console.log(Child.staticMethod()); // Hello World
console.log(new Child().prototypeMethod()); // TypeError

```


