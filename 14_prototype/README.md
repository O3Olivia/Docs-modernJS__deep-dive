# 프로토타입

> JavaScript는 클래스라는 개념이 없기 때문에 기존의 `객체를 복사`하여 새로운 객체를 생성하는 `prototype`기반의 언어다.<br/>
> 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는데, 이 객체는 또 다른 객체의 원형이 될 수 있다.

```
[ 💡 NOTE  ]

프로토타입 기반 프로그래밍이란?
  클래스가 필요없는(class-free) 객체지향 프로그래밍 스타일로 프로토타입 체인과 클로저 등으로 객체지향 언어의 '상속', '캡슐화(정보 은닉)'등의 개념을 구현할 수 있다
```

# 객체 생성 방법 
> 객체 리터럴, Object() 생성자 함수, 생성자 함수(Constructor)

## 생성자 함수(Constructor)와 인스턴스 생성
> 자바스크립트는 생성자 함수와 `new 연산자`를 통해 인스턴스를 생성할 수 있다.
> 생성자 함수 : 클래스이자 생성자 역할을 한다.

```jsx
// 생성자 함수(Constructor)
function Person(name){
  // 프로퍼티 
  this.name = name;
  
  // 메소드
  this.setName = function (name){
     this.name = name;
  };
  
  // 메소드
  this.getName = function () {
    return this.name;
  };
}

// 인스턴스 생성
var me = new Person("Seulgi");
console.log(me.getName()); // Seulgi

// 메소드 호출
me.setName("Olivia");
console.log(me.getName()); // Olivia
```
- 이렇게 작성한 코드는 작동은 잘 되지만, Person 생성자로 여러개의 인스턴스를 생성할 경우, `setName()`과 `getName()`이 중복되어 생성된다.
- 각 인스턴스가 동일한 메서드를 갖게되는데 이는 `메모리 낭비`로 이어지는 문제를 발생한다.
- 이를 해결하기 위해 `프로토타입` 접근 방식이 필요하다.

## 프로토타입 체인과 메소드 정의
> 모든 객체는 **프로토타입**이라는 다른 객체를 가르키는 `내부 링크`를 갖고 있다.
> 이렇게 프로토타입을 통해 직접 객체를 연결하는 것을 `프로토타입 체인`이라 한다.

- 프로토타입을 이용하여 생성자 함수 내부의 `메소드`를 생성자 함수의 prototype 프로퍼티가 가르키는 `프로토타입 객체`로 이동시키면, 생성자 함수에 의해 **생성된 모든 인스턴스**는 `프로토타입 체인`을 통해 **프로토타입 객체의 메소드를 참조할 수 있다**
```jsx
// 생성자 함수(Constructor)
function Person(name){
  this.name = name;
}

// 프로토타입 객체에 메소드를 정의함
Person.prototype.setName = function(name){
  this.name = name;
};

// 프로토타입 객체에 메소드를 정의함
Person.prototype.getName = function(){
 return this.name;
};

const me = new Person('Seulgi');
const nickName = new Person('Olivia');
const familyName = new Person('Lee');

console.log(Person.prototype);
// Person {setName: [function], getName: [function] }

console.log(me); // Person {name : 'Seulgi'}
console.log(nickName); // Person { name: 'Olivia' }
console.log(familyName); //Person { name: 'Lee' }
```
<img src="https://user-images.githubusercontent.com/87024040/210226639-f884b25e-7649-4e38-a88e-b99bde8b2b62.png" widht="550" height="300">

- Person 생성자 함수의 prototype 프로퍼티가 가르키는 프로토타입 객체로 이동시킨 setName()
- getName() 메소드는 **프로토타입 체인**에 의해 `모든 인스턴스`가 **참조**할 수 있다.
- `프로토타입 객체`는 **상속할 것들이 저장**되어있다.

### 더글라스 크락포드가 제안한 프로토타입 객체에 메소드를 추가하는 방식
> 더글라스 크락포드 : 자바스크립트 언어의 개발에 참여한 프로그래머

```jsx
/**
* 모든 생성자 함수의 프로토타입 : Function.prototype
* 따라서, 모든 생성자 함수는 Function.prototype.method()에 접근 가능.
* @method Function.prototype.method
* @param ({string}) (name) - (메소드 이름)
* @param ({function}) (func) - (추가할 메소드 본체)
*/

Function.prototype.method = function (name, func) {
// 생성자 함수의 프로토타입에 동일한 이름의 메소드가 없을 경우, 생성자함수의 프로토타입에 메소드 추가

  // this: 생성자 함수 
  if(!this.prototype[name]){
    this.prototype[name] = func;
  }
};

/**
* 생성자 함수
*/
function Person(name){
  this.name = name;
}

/**
* 생성자 함수 Person의 프로토타입에 메소드 setName을 추가
*/
Person.method('setName', function(name){
  this.name = name;
});

/**
* 생성자함수 Person의 프로토타입에 메소드 getName을 추가
*/
Person.method('getName', function() {
  return this.name;
});

const me = new Person("Seulgi");
const nickName = new Person("Olivia");
const familyName = new Person("Lee");

console.log(me); // Person {name : 'Seulgi'}
console.log(nickName); // Person { name: 'Olivia' }
console.log(familyName); // Person { name: 'Lee' }
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
