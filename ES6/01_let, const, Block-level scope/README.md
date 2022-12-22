# 변수 선언(Variable Declaration)

> `ES6` 전 JavaScript에서는 `var`키워드만으로 사용해서 변수를 선언했다. 그러나 `var` 키워드로 선언된 변수는 아래와 같은 특징으로 인해 심각한 문제를 일으킨다.

**1. 함수 레벨 스코프 (Function-level scope)**<br/>

- `var`는 블록 스코프를 가지지 않고, **함수 바디를 스코프**로 가진다. <br/>
  함수 밖에서 `var`를 사용하면 모두 전역 변수로 선언되어 전역변수를 남발할 수 있다. <br/>

  ````jsx
  var example = 111;
  console.log(example); // 111

      {
        var example = 999;
      }
      conosle.log(example); // 999
      ```
      `var`은 함수 레벨 스코프를 따르기 때문에, { } 내의 변수는 지역 변수가 아니라 전역 변수.<br/>
      이미 전역변수가 선언되어 있지만, `var`특성 상 중복 선언이 가능하기 때문에 999로 재할당하여 값을 덮어쓴다.

  ````

- for 루프에서 인덱스 변수로 자주 사용된다. <br/>
  여기서 `var`변수의 스코프는 루프 바디가 아니기 때문에 for 루프 외부 또는 전역에서 다시 선언하거나 초기화하는 등 참조할 수 있다.

**2. var 키워드 생략 허용**<br/>

- 의도하지 않게 전역 변수가 선언될 수 있다.

**3.변수 중복 선언 허용** <br/>

- `let`과 달리 `var`은 몇 번이고 같은 변수를 선언할 수 있어 의도치 않게 변수를 남발하거나 변수 값이 변경될 수 있다.<br>

```jsx
[ 💡 Note ]
let num = 111;
let num = 222; // ERROR
```

**4.변수 호이스팅(끌어올림)** <br/>

- `var`로 변수를 선언하면 이 선언문은 함수의 맨 위로 끌어올려져서 선언 이전에 참조할 수 있다.<br>

```
[ 💡 Note ]
스코프 : 프로그램 소스 코드에서 변수가 정의된 영역으로 변수에 접근할 수 있는 범위.
전역(global) 스코프 : 어디에서든 해당 변수에 접근 가능
지역(local) 스코프 : 한정적인 범위에서 해당 변수에 접근 가능
  - 블록 스코프 : 블록{ }안에서 선언된 변수만 유효하고 블록 외부에서는 참조할 수 없다.
  - 함수 스코프 : 함수에서 선언한 변수는 해당 함수내에서만 접근 가능하고 외부에서는 참조할 수 없다. JavaScript는 함수 레벨 스코프를 따른다.
```

`전역변수`는 사용하기 편리하지만, 스코프 범위가 넓어 어디에서 사용할지 파악하기 어려우며, 의도치않게 변경될 수 있어서 복잡하다. <br />이러한 `var`의 단점을 보안하기 위해서 `let` 과 `const`이 도입되었다.

# let과 호이스팅(Hoisting)

> JavaScript는 모든 선언을 호이스팅한다. <br />
> 호이스팅 : `var`, function 선언문 등을 해당 스코프의 선두로 끌어 올려진 것처럼 동작한다.

```jsx
console.log(hello); // undefined
var hello;

console.log(bye); // Error: Uncaught ReferenceError
let bye;
```

`var`키워드로 선언된 변수는 호이스팅되어 변수를 선언하기 전에 참조해도 에러가 발생하지 않지만, <br/>
`let`키워드로 선언된 변수는 선언하기 전에 참조하면 변수의 일시적 사각지대(Temporal DeadZone: TDZ)에 빠져 참조에러(ReferenceError)가 발생한다.<br/>

## 변수 선언의 3단계

> 변수는 `선언`, `초기화`, `할당` 3단계를 거쳐 생성된다.

<img src="https://user-images.githubusercontent.com/87024040/208873161-86b72c00-0ce6-4746-be9c-3a91b07464d0.png" width="500px" height="200px">

**1️⃣선언 단계(Declaration phase)**<br/>

- 변수 객체를 생성하고 변수를 등록한다. <br/>
- 스코프는 이 변수 객체를 참고한다.

**2️⃣초기화 단계(Initialization phase)** <br/>

- 변수 객체에 등록된 변수를 위한 공간을 메모리에 할당한다. <br/>
- 그리고 변수는 undefined로 초기화된다.

**3️⃣할당 단계(Assignment phase)** <br/>

- undefined로 초기화 된 변수에 값을 할당한다.

## 변수의 일시적 사각지대(Temporal DeadZone: TDZ)

`var` 키워드로 선언된 변수 생명 주기<br/>
<img src="https://user-images.githubusercontent.com/87024040/208879212-11fec12d-38b7-4cca-a756-6f8d8d8f9142.png" width="600px" height="200px"><br/>

- `var`는 변수 선언의 3단계 중 `선언단계`와`초기화단계`가 스코프의 선두에서 한 번에 이루어진다(호이스팅).

#### `let` 키워드로 선언된 변수 생명 주기<br/>

<img src="https://user-images.githubusercontent.com/87024040/208882504-24148eec-4ad8-46d9-9b2d-ed345750fd9b.png"><br/>

#### `let`으로 선언된 변수 생명 주기

<img src="https://user-images.githubusercontent.com/87024040/208882504-24148eec-4ad8-46d9-9b2d-ed345750fd9b.png"><br/>

- `let`으로 선언된 변수는 `선언단계`와 `초기화단계`가 분리되어있기 때문에, 스코프의 시작 지점부터 초기화 지점까지는 변수를 참조할 수 없다. <br/>
- `스코프의 시작`부터 `초기화단계`까지의 구간을 `변수의 일시적 사각지대`라고 부른다.

```jsx
let num = 1;
{
  console.log(num); // Error: ReferenceError
  let num = 2;
}
```

- `let`은 블록 레벨 스코프를 가지기 때문에 블록 내에서도 호이스팅이 발생하기 때문에 참조 에러가 발생한다.

# 클로저(Closure)

> for 루프처럼 `var`을 사용하면 초기화식에 사용되었던 변수는 계속해서 전역 변수로 남아있다. <br/>따라서 해당 변수를 불러올 때 최종적인 변수의 값으로 대입이 되는데 클로저는 이를 해결할 수 있다. <br/>

- 클로저 : `함수 내에서 선언된 지역변수`를 `외부`에서도 사용할 수 있는 기능이다. <br/>
  보는 관점에 따라서 모든 자바스크립트의 함수가 클로저라고 볼 수 있다. <br/>
  클로저가 유용할 때는 함수가 정의된 곳과 다른 스코프에서 호출될 때이며, 가장 흔한 경우는 `함수가 함수를 정의해 반환 (중첩된 함수)`하는 경우다. <br/>
  **`closure` 이해를 위한 예문**

```jsx
function test(a) {
  return function (b) {
    return a + b;
  };
}

var testNum = test(10);
console.log(testNum(20)); // 30
```

- 1️⃣ testNum로 test함수를 호출한 순간 `var a = 10`이 생성된다.
- 2️⃣ function(b){return a + b}가 testNum에 리턴되면서 testNum()을 사용하게 한다.
- 3️⃣ console.log(testNum(20))을 호출하면, testNum은 기억하고 있던 function(b){return a + b} 에서 a변수를 부르고, b에다가 값 20이 대입된다.
- 4️⃣ 따라서 a + b 값인 30이 출력된다.

**`var`의 경우** <div/>

```jsx
var funcs = [];
for (var i = 0; i < 3; i++) {
  funcs.push(function () {
    console.log(i);
  });
}
for (var j = 0; j < 3; j++) {
  funcs[j]();
}
// 3 3 3
```

- 첫 번째 코드는 for 문안에 i가 `var`로 선언되어있다. <br/>
  따라서 0 1 2로 나오는 것이 아니라 최종 변수 값 3이 계속 출력되는 것이다.<div/>

**`let`의 경우**<div/>

```jsx
var funcs = [];

for (let i = 0; i < 3; i++) {
  funcs.push(function () {
    console.log(i);
  });
}

for (var j = 0; j < 3; j++) {
  console.dir(funcs[j]);
  funcs[j]();
}
// 0 1 2
```

- `let`키워드를 for루프 초기화 식에 사용하면, 블록 스코프이기 때문에 i는 for 루프에서만 유효한 변수다. <br />
  따라서 for 문을 반복할 때마다 새로운 블록으로 간주해서 0 1 2로 출력된다.

# 전역 객체와 let

> 전역 객체(Global obj)는 모든 객체의 **유일한** 최상위 객체를 말한다. <br/>
> Browser-side에서는 widow객체, Server-side(Node.js)에서는 Global 객체를 말한다. <br/>

- `var`키워드로 선언된 변수를 전역 변수로 사용하면, 전역 객체의 프로퍼티가 된다.<br/>

```jsx
var num = 111; // 전역변수
console.log(window.num); // 111
```

- `let`키워드로 선언된 변수를 전역 변수로 사용하면, let 전역 변수는 전역 객체의 프로퍼티가 **X 안된다**. <br/>

```jsx
let num = 222; // 전역변수
console.log(window.num); // undefined : let 전역 변수는 블록 내에만 존재.
```

# `const` 재할당 금지

> `const`는 상수(변하지 않는 값)를 위해 사용하지만, 반드시 상수만을 위해 사용하진 않는다. <br/> > `const`는 `let`과 비슷하지만(블록 레벨 스코프) 아래와 같이 다른 점도 존재한다.

- `let`은 재할당이 자유롭지만, **`const`는 재할당이 금지**된다. <br/>

```jsx
const num = 111;
num = 222; // TypeError
```

```
[ 💡 Note ]
const는 반드시 변수 선언과 함께 할당이 이루어져야한다.
만약, 할당이 이루어지지 않으면 SyntaxError(문법 에러)가 발생한다.
```

## const 상수

> 상수는 `가독성`과 `유지보수`의 편의를 위해 적극적으로 사용하는게 좋다.

```jsx
if (width > 100) {
} // 100의 의미를 알 수 없어서 가독성이 안좋다.

const MinWidth = 100;
if (width > MinWidth) {
}
// MinWidth를 통해 의미를 정확하게 기술해서 가독성이 좋다.
```

따라서 적절한 네이밍을 통한 상수 선언은 위의 예시처럼`가독성`과 `유지보수성`이 좋아진다.<br/>

## const 객체

> const는 객체에도 사용할 수 있지만, 재할당을 하게 되면, TypeError가 발생한다.

```jsx
const obj = { num: 111 };
obj = { age: 111 }; // TypeError
```

위의 예시처럼 const객체가 재할당이 금지되는 이유는, 객체에 대한 참조를 변경하지 못하기 때문이다. <br/>
하지만, 이때 객체의 프로퍼티는 보호되지 않기 때문에, 할당된 객체의 내용(프로퍼티 `추가`, `삭제`, `값 변경`)은 가능하다.

```jsx
const user = { name: "Lee" };

user = {}; // TypeError : 재할당 금지

user.name = "Seulgi"; // 객체의 내용은 변경할 수 있다.
user.height = 164;
console.log(user); // {name:Seulgi, height: 164}
```

- 객체의 내용이 변경되더라도 변수에 할당된 `주소값`은 **변경되지 않는다**. <br/>
  따라서 객체 타입의 변수 선언일 경우 `const`를 사용하는 것이 좋다.<br/>
  만약, `주소값`을 **변경(재할당)**할 경우: `let`사용하는 것이 좋다.

# `var` / `let` / `const`

> 변수 선언의 기본 : `const` <br/>
> 재할당할 경우 : `let`

- ES6 사용의 경우 `var`키워드 사용 X <br/>
- 재할당이 필요한 경우 `let`사용, but 변수 스코프는 좁게 만들 것. <br/>
- 재할당이 필요 없는 원시 값과 객체는 `const` 사용. **재할당을 금지해서 `let`과 `var`보다 안전**
