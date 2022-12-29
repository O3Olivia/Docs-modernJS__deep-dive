# 객체 리터럴 프로퍼티 기능 확장(Enhanced Object property)
> ES6에서는 객체 리터럴 프로퍼티가 기능을 확장하여 더욱 간편하고 동적인 객체 생성을 할 수 있도록 한다.

```
[ 💡 NOTE  ]
  객체 프로퍼티란? 
  자바스크립트는 '객체'기반 프로그래밍 언어이며, 구성하는 거의 모든 것이 '객체'다.
  그리고 이 객체타입은 '다양한 값을 하나의 단위로 구성'하였기 때문에 '변경가능'한 값을 가진다.
  객체는 0개 이상의 '프로퍼티'로 구성된 집합이며, 이 '프로퍼티'는 'key'와 'value'로 구성된다.
  여기서 자바스크립트의 함수는 일급 객체이기 때문에, 값으로 취급할 수 있으므로 함수도 프로퍼티 값으로 사용되며 이를 '메서드'라 부른다.
  
  - 프로퍼티 : 객체의 상태를 나타내는 값
  - 메서드 : 프로퍼티를 참조하고 조작할 수 있는 동작
```

## 프로퍼티 축약 표현
##### ES5
> ES5에서는 객체 리터럴 프로퍼티는 프로퍼티의 이름 & 값으로 구성된다.
> 프로퍼티의 값은 `변수에 할당된 값`일 수 있다.

```jsx
var x = 1, y= 2;

var obj = {
  x: x,
  y: y
};
console.log(obj); // {x: 1, y: 2}
```

##### ES6
> ES6에서는 프로퍼티 값으로 변수를 사용하는 경우, `프로퍼티의 이름`을 **생략**할 수 있고, 
> 프로퍼티 이름은 **자동으로 생성**된다.
```jsx
let x = 1, y = 2;
const obj = { x, y};

console.log(obj); // {x: 1, y: 2}
```

## 프로퍼티 키 동적 생성
> `문자열`이나 `문자열로 변환 가능한 값`을 반환하는 표현식을 사용해 프로퍼티를 동적으로 생성할 수 있다.
> 이때 `[...]` 대괄호로 묶어서 표현해야한다 = `계산된 프로퍼티 이름(Computed property name)`

##### ES5
>ES5에서는 객체 리터럴 **외부**에서 `[...]`를 사용해야한다.
```jsx
var name = `prop`;
var i = 0;

var obj = {};

obj[name + '-' + ++i] = i;
obj[name + '-' + ++i] = i;
obj[name + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

##### ES6
> ES6에서는 객체 리터럴 **내부**에서도 프로퍼티키를 동적으로 생성할 수 있다.
```jsx
const name = 'prop';
let i = 0;

const obj = {
 [`${name}-${++i}`]: i,
 [`${name}-${++i}`]: i,
 [`${name}-${++i}`]: i
};
console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

# 메소드 축약 표현
##### ES5
> ES5에서는 메소드를 선언하기 위해선 프로퍼티 값으로 함수 선언식을 할당해야한다.
```jsx
var obj = {
  name : 'Lee',
  greeting: function() {
    console.log('HI ' + this.name );
  }
};
obj.greeting(); // HI Lee
```

##### ES6
> ES6에서는 메소드를 선언할 때, `function`키워드를 **생략**한 축약표현을 사용한다.
```jsx
const obj={
  name : 'Lee',
  greeing(){
    console.log('HI ' + this.name);
  }
};

obj.greeting(); // HI Lee
```

# __proto__ 프로퍼티에 의한 상속 
##### ES5
> ES5에서 객체 리터럴을 상속하기 위해서 **프로토타입 패턴 상속인** `Object.create()`함수를 사용한다.

```jsx
var parent = {
  name = 'parent',
  greeting: function() {
    console.log('HELLO~ ' + this.name);
  }
};

var child = Object.create(parent);
child.name = 'child';

parent.greeting(); // HELLO~ parent
child.greeing(); // HELLO~ child
```

##### ES6
> ES6에서는 객체 리터럴 내부에서 `__proto__` 프로퍼티를 직접 설정할 수 있다.
> 객체 리터럴에 의해 생성된 객체의 `__proto__` 프로퍼티에 다른 객체를 직접 바인딩하여 상속을 표현할 수 있다.

```jsx
const parent = {
  name: 'parent',
  greeting() {
    console.log('HELLO~~~~ ' + this.name);
  }
};

const child = {
  __proto__: parent,
  name: 'child'
};

parent.greeting(); // HELLO~~~~ parent
child.greeting();// HELLO~~~~ child
```
