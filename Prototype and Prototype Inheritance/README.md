# 프로토타입

> JavaScript는 클래스라는 개념이 없기 때문에 기존의 `객체를 복사`하여 새로운 객체를 생성하는 `prototype`기반의 언어다.<br/>
> 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는데, 이 객체는 또 다른 객체의 원형이 될 수 있다.

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
