# 매개변수(파라미터) 기본값(Default Parameter Value)
> 매개변수 : 함수에 변수값이 전달되어 처리해야하는데, 함수에 전달되는 변수가 `매개변수`다.
> 자바스크립트 함수에서 **인수가 전달되지 않은** 매개변수는 `undefined`다.

- 보통 함수를 호출할 때 매개변수의 개수만큼 인수를 전달해야한다.<br/>
- 수만큼 전달하지 않아도 에러가 발생하지 않고 오히려 인수가 부족하다면 `undefined`가 매개변수의 값이 된다.
- 만약, 매개변수의 갯수보다 인수를 더 많이 전달한다면, `초과된 인수`는 `무시`된다.
  - 그 이유는 자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다.
  - 따라서 매개변수의 `기본값`을 할당할 필요가 있다. `( = 일종의 방어 코드)`
  - 그리고 이 기본값은 `length`프로퍼티나 arguments 객체에 영향을 주지 않는다.
  ```
  [ 💡 NOTE  ]
    그러나 안정성이 더 높은것은 ES6처럼 기본값을 할당하는 것보다 ES5방법이 더 안전하다.
  ```
  

```jsx
function sum(x, y) {
  return x + y; 
}

console.log(sum(5)); // NaN
```
- 위의 상황에서 NaN이 뜨는 이유는 **개수만큼 전달되었지 않기 때문**이다.<br/>
a는 5로 값이 전달되었지만, b는 전달되지 않았기 때문에 `undefined`가 되기 때문에 `NaN`이 출력될 수 밖에 없다.

## 매개변수에 적절한 인수가 전달되었는지 확인 방법
##### ES5의 경우
> 함수 내에서 기본값을 사용해 `인수 체크` 및 `초기화`를 통해 확인할 수 있다.<br/>
```jsx
function sum(x, y){
  x = x || 0;
  y = y || 0;
  
  return x + y;
}

console.log(sum(5)); // 5
console.log(sum(5, 10)); // 15
```
- 이렇게 되면 매개변수 x, y에 인수가 할당되지 않을 경우 0을 할당하기 때문에 `NaN`이 출력되지 않는다.

##### ES6의 경우
> ES5에서 함수내에서 수행하던 것을 ES6에서는 아래와 같이 간소화하여 사용할 수 있다.<br/>
```jsx
function sum(x=0, y=0){
  return x + y;
}
console.log(sum(5)); // 5
console.log(sum(5, 10)); // 15
```
- 위의 예시는 ES5예시와 동일한 의미를 갖는다. <br/>
매개변수 x, y에 인수가 할당되지 않을 경우 0을 할당한다.

```
[ 💡 NOTE  ]
위의 방법처럼 ES6의 경우 간소화할 수 있지만, 안정성이 더 높은 것은 사실 'ES5'다.
ES6 방법처럼 기본값을 할당하면 종종 에러가 발생하기 때문에 안정성이 더 높은 'ES5'를 사용하는 것이 더 좋다.
```


# Rest 파라미터
### Rest 파라미터의 기본 문법
> Rest 파라미터는 매개변수 앞에 Spread syntax `...` 점 3를 붙여서 정의한 매개변수다. <br/>

##### Rest 파라미터는 함수에 전달된 인수들의 `목록을 배열로 전달`받는다.
```jsx
function test(...rest){
  console.log(Arr.isArray(rest)); // true
  console.log(rest); // [1,2,3,4,5]
}

test(1, 2, 3, 4, 5);
````
- ES6에서는 Spread 연산자를 사용하면 `새로운 복사된 배열`을 생성할 수 있기 때문이다.

##### `순차적`으로 파라미터와 Rest파라미터에 할당
```jsx
function test(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [2, 3, 4, 5]
}

test(1, 2, 3, 4, 5);

function test2(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest); // [3, 4, 5]
}

test2(1, 2, 3, 4, 5);
```
- 함수에 전달된 인수들은 순차적으로 파라미터와 Rest 파라미터에 할당된다.
- rest파라미터는 `먼저 선언된 파라미터에 할당된 인수를 제외`하고 나머지 인수들을 모두 배열에 담아서 할당한다.
  따라서, 반드시 `마지막` 파라미터로 써야한다.

##### `rest파라미터`는 length에 영향을 주지 않는다
```jsx
function test1(...rest){ }
console.log(test1.length); // 0

function test2(param1, ...rest){ }
console.log(test2.length); // 1
```
- 위의 예시 처럼 `rest 파라미터`는 매개변수의 개수를 나타내는 `length 프로퍼티에 영향을 주지 않는다`.

### arguments와 rest 파라미터
> **ES5** : 인자의 개수를 사전에 알 수 없는 `가변인자함수(매개변수의 최대 갯수가 지정되지 않은 함수)`일 경우 `arguments`객체를 통해 인수를 확인한다.
**ES6** : `rest파라미터`를 통해 쉽게 arguments를 대처할 수 있다.

```jsx
function arr() {
  console.log(arguments);
} 
arr(1, 2, 3); // [Arguments] { '0': 1, '1': 2, '2' : 3 }
arr(); // [Arguments] {}
```
- arguments : argument(인수)들의 정보를 담고 있는 순회 가능한 유사 배열 객체

```
[ 💡 NOTE  ]
parameter(매개변수) : 함수의 작업 실행을 위해 추가적인 정보가 필요한 경우, 매개변수를 지정하며, 함수 내에서
                    변수처럼 동작한다.
argument(인수) : 매개변수안에 들어가는 값
arguments객체 : 함수 호출 시 전달된 argument(인수)들의 정보를 담고 있는 순회 가능한 유사 배열 객체다.
               함수 내에서 지역변수처럼 사용되며, 함수 외부에서는 사용할 수 없다.

function test(x) { }
test(10);
- test함수의 x = parameter
- test(10)으로 함수 test를 호출할 때 10 = argument다.
```
#### `가변 인자 함수`는 `arguments객체`를 활용해서 전달받는다
> `가변 인자 함수`는 `parameter`를 통해 `argument`를 전달 받는 것이 불가능하기 때문이다. <br/>
그러나, arguments는 유사배열객체기 때문에 배열 메소드를 이용하는데, 이 배열 메소드를 이용하려면 `prototype`의 `call`을 이용해야만 한다.

##### ES5
```jsx
function sum() {
  var arr = Array.prototype.slice.call(arguments);
  return arr.reduce(function (pre, cur) {
    return pre + cur;
  });
}

console.log(sum(1, 2, 3)); // 6
```
- 여기서 `reduce`함수는 배열의 각 요소를 순회하며 callback함수의 실행 값을 누적하여 하나의 결과값을 반환한다.

##### ES6
```jsx
function sum(...args) {
  console.log(arguments); // [Arguments] { '0': 1, '1': 2, '2': 3 }
  console.log(Array.isArray(args)); // true
  return args.reduce((pre, cur) => pre + cur);
}
console.log(sum(1, 2, 3)); // 6
```
- 위의 예시처럼 `ES6`에서는 `rest파라미터`를 사용해서 `가변 인자`를 `배열`로 전달받을 수 있다.
- ES5에서는 객체를 배열로 변환해야만 했지만, ES6는 `rest파라미터`로 이런 번거로움을 피할 수 있다.
- 그러나 `화살표 함수`는 함수 객체의 arguments property가 없어서, `화살표 함수`를 사용하려면 반드시 `rest파라미터`를 이용해야한다.

# Spread 문법
> Spread의 의미는 `펼치다`, `퍼뜨리다`처럼 spread는 대상을 `개별 요소`로 **분리**한다.
단, spread 문법의 대상은 `이터러블(Iterable)`이여야만 한다.

```
[ 💡 NOTE  ]
이터러블(Iterable) : '반복하다'라는 의미처럼, 주로 '반복 가능한 객체'를 말한다.
                   특징 :  'for..of문 조회', 'spread문법', '배열 destructuring문법'
```

```jsx
//1️⃣
console.log(...[1, 2, 3]); // 1, 2, 3
//2️⃣
console.log(... 'HELLO'); // H E L L O
//3️⃣
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // [ 'a', '1' ] [ 'b', '2' ]
//4️⃣
//5️⃣
```
### 함수의 인수로 사용하는 경우
>배열의 각 요소를 분해해서 파라미터로 전달하고 싶은 경우, `Function.prototype.apply`를 사용한다.

##### ES5
```jsx
  function test(x, y, z) {
    console.log(x); // 1
    console.log(y); // 2
    console.log(z); // 3
    
    const arr = [1, 2, 3]
    
    test.apply(null, arr); 
  }
```
- arr의 배열을 분해해서 각각의 요소를 파라미터에 전달하기 위해서 `apply`를 사용했다.

##### ES6
```jsx
  function test(x, y, z){
    console.log(x); // 1
    console.log(y); // 2
    console.log(z); // 3
  }
  
  const arr = [1, 2 ,3];
  
  test(...arr);
```
- `spread`문법을 사용했다.
  - ...arr 는 `...[1, 2, 3]`이며, 여기 배열 안의 [1, 2, 3]을 개별 요소로 분리한다.
  - 그리고 개별 요소는 개별적인 인자로 각각의 `매개변수`에 전달된다.

##### `Rest` 파라미터역시 `spread`문법을 사용하여 파라미터를 정의했기때문에 혼동에 주의할 것.
###### `Rest`로 분리될 경우
> ...rest는 분리된 요소들을 함수 내부에 배열로 전달한다.

```jsx
function test(param, ...rest){
  console.log(param); // 1
  console.log(rest); // [2, 3, 4]
}
test(1, 2, 3);
```

##### `spread`로 분리될 경우
> 배열 인수는 분리되어서 `순차적`으로 매개변수에 할당된다

```jsx
function test2(x, y, z) {
  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3
}

test2(...[1, 2, 3]);
```

##### `spread`문법을 사용한 인수는 더 자유롭다
> `Rest`파라미터는 반드시 마지막 파라미터여야하지만, `spread`문법을 사용한 인수는 `자유롭게` 사용할 수 있다.

```jsx
function test3(a, b, c, d, e, f, g) {
  console.log(a); // 1
  console.log(b); // 2
  console.log(c); // 3
  console.log(d); // 4
  console.log(e); // 5
  console.log(f); // 6
  console.log(g); // 7
}

test3(1, ..[2, 3], 4, ...[5, 6, 7]);
```
- ...[2, 3]과 ...[5, 6, 7]은 개별 요소로 분리한다 = `2,3 5, 6, 7`

## 배열에서 사용하는 경우
> `spread`문법을 배열에서 사용하면, 보다 `간결`하고 `가독성`있게 표현할 수 있다.

### concat
##### ES5
> `ES5`에서 기존 배열의 요소를 새로운 배열 요소의 일부로 만들고 싶을 경우 `concat`메소드를 사용했다.

```jsx
var arr = [1, 2, 3];
console.log(arr.concat([4, 5, 6]); // [1, 2, 3, 4, 5, 6]
```
##### ES6
> `ES6`에서는 `spread`문법을 사용하여 기존 배열을 새로운 배열 요소의 일부로 만들 수 있다.
```jsx
const arr = [1, 2, 3];
console.log([...arr, 4, 5, 6]); // [1, 2, 3, 4, 5, 6];
```

### push
##### ES5
> `ES5`에서는 기존 배열에 다른 배열의 개별 요소를 각각 넣으려면 아래와 같이 사용해야했다.

```jsx
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

Array.prototype.push.apply(arr1, arr2);
console.log(arr1); // [1, 2, 3, 4, 5, 6]
```
- `2번째`인자를 개별 인자로 `push` 메소드에 전달된다.

##### ES6
```jsx
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

arr1.push(...arr2); 
console.log(arr1); // [1, 2, 3, 4, 5, 6]
```

### splice
> 기존의 배열에 다른 배열의 개별요소를 삽입 
```jsx
var arr1 = [1, 2, 5, 6];
var arr2 = [3, 4];

Array.prototype.splice.apply(arr1, [2, 0].concat(arr2));
console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]
```
`Array.prototype.splice.apply(arr1, [2, 0].concat(arr2));` => arr1[2]부터 0개의 요소를 제거한 뒤 그 자리에 arr2를 삽인한다.

##### ES6
```jsx
const arr1 = [1, 2, 3, 7, 8];
const arr2 = [4, 5, 6];

arr1.splice(3, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4, 5, 6, 7, 8]
```

### copy
> 기존의 배열을 복사
##### ES5
```jsx
var arr = [1, 2, 3];
var test = arr.slice();

console.log(test); // [1, 2, 3]

test.push(4); 
console.log(test); // [1, 2, 3, 4]
console.log(arr); // [1, 2, 3]
```
- arr의 값은 변경되지 않는다.
- arr의 값을 복사해서 test값으로 넘겨줬기 때문이다.

##### ES6
```jsx
const arr = [1, 2, 3];

const test = [ ...arr ];
console.log(test); // [1, 2, 3]

test.push(4); 
console.log(test); // [1, 2, 3, 4]
console.log(arr); // [1, 2, 3]
```
- arr의 값은 변경되지 않는다.
- arr의 값을 복사해서 test값으로 넘겨줬기 때문이다. (ES5와 동일)

##### 여기서 복사를 한다고해서 같다고 볼 수 있을까?
> NOPE 다만, 배열의 요소는 같다.
```jsx
const skills = [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false }
];

const mySkills = [...skills];
console.log(mySkills === skills); // false
console.log(myskills[0] === mySkills[0]); // true
```
- 이는 원본 배열의 각 요소를 `얕은 복사(shallow copy)`하여 복사본은 만들었기 때문이다.

##### 유사배열객체(Array-like Object)를 배열로 변환
```jsx
const htmlCollection = document.getElementsByTagName('li');
const newArr = [...htmlCollection]; 

// Array.from을 사용해도 된다.
const newArr = Array.from(htmlCollection);
```
