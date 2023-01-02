# 프로토타입

> JavaScript는 클래스라는 개념이 없기 때문에 기존의 `객체를 복사`하여 새로운 객체를 생성하는 `prototype`기반의 언어다.<br/>
> 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는데, 이 객체는 또 다른 객체의 원형이 될 수 있다.

```jsx
[ 💡 NOTE  ]

프로토타입 기반 프로그래밍이란?
  클래스가 필요없는(class-free) 객체지향 프로그래밍 스타일로 프로토타입 체인과 클로저 등으로 객체지향 언어의 '상속', '캡슐화(정보 은닉)'등의 개념을 구현할 수 있다
```

### 함수와 객체 내부 구조

JavaScript에서 함수를 정의하면, 함수 멤버로 `prototype`이 있다. <br/>
이 `prototype`은 다른 곳에 생성된 함수의 프로토타입 객체를 참조하며, `prototype`객체 멤버인 constructor 속성이 함수를 참조하는 내부 구조를 가진다.
<img src="https://user-images.githubusercontent.com/87024040/209089085-60dc329b-3be1-49a1-b3d7-b66a3c052492.png" width="600" height="300">

#### Person이라는 함수로 예시

- 처음 속성이 없는 `Person`이라는 함수를 정의하면, Person함수의 prototype 속성은 `prototype 객체를 참조`한다. <br/>
- `prototype 객체`의 `constructor`은 `Person`함수를 참조한다.

```jsx
function Person() {}
```

- Person함수의 `prototype 객체`는 `new`라는 연산자와 Person함수를 통해 생성된 모든 객체의 원형이 되며, `생성된 모든 객체가 참조`한다.

```jsx
function Person() {}

var me = new Person();
```

##### 프로토타입 객체

> 함수를 정의하면 다른 곳에 생성되는 `prototype 객체`는 다른 객체의 원형이 되는 객체다. <br/>
> 모든 객체가 접근할 수 있고, `prototype 객체`도 동적으로 런타임에 멤버들을 추가할 수 있다.

<img src="https://user-images.githubusercontent.com/87024040/209093503-1ff80999-8ed4-4ffd-afce-e9b3d172fd9e.png" width="600" height="300">

```jsx
function Person() {}

var me = new Person();

Person.prototype.getType = function () {
  return "human";
};

console.log(me.getType()); // human
```

- 여기서 props와 value를 설정할 수 있다.

  <img src="https://user-images.githubusercontent.com/87024040/209090397-3fedfc54-d8f8-47e5-8493-8840eaa1f860.png" width="600" height="300">

```jsx
me.name = "Lee";

console.log(me.name); // Lee
```

### 프로토타입 패턴 상속(Prototypal Inheritance)
> 프로토타입 패턴 상속은 `Object.create 함수`를 사용하여 객체에서 다른 객체로 직접 상속을 구현하는 방식.
> `new` 연산자가 필요없으며, 생성자의 링크도 파괴되지 않고, 객체 리터럴에서도 사용할 수 있다.

```jsx
// 부모 생성자 함수
var Parent = (function () {
  // Constructor
  function Parent(name) {
    this.name = name;
  }
  
  // method
  Parent.prototpye.greeting = function() {
    console.log("HI ' + this. name);
  };
  // trturn constructor
  return Parent;
}());

var child = Object.create(Parent.prototype);
child.name = 'child';
child.greeting(); // HI child
console.log(child instanceof parent); // true
```

##### 객체 리터럴 패턴으로 생성한 객체에도 프로토타입 패턴을 상속할 수 있다.
```jsx
var parent= {
  name: 'parent',
  greeting: function() {
    console.log('HI~ ' + this.name);
  }
};

var child = Object.create(parent);
child.name = 'child';

parent.greeting(); // HI~ parent
child.greeting(); // Hi~ child

console.log(parent.isPrototypeOf(child)); true
```
<img src="https://user-images.githubusercontent.com/87024040/209906129-73824bde-ed3c-4f1b-b5dc-4760ccefe8f5.png" width="600" height="300"/>

- `Oject.create`함수는 매개변수에 프로토타입으로 설정할 `객체` 또는 `인스턴스`를 전달하고, 이를 상속하는 
**새로운 객체**를 생성한다.
