# 모듈(Module)
> 모듈이란 애플리케이션을 구성하는 개별적 요소로 `재사용`이 가능한 `코드 조각`이다.
> **여러 기능들에 대한 코드가 모여있는 하나의 파일**
> 모듈은 세부 사항을 `캡슐화`하고 공개가 필요한 API만을 외부에 노출시킨다.

모듈은 **파일 단위**로 분리되어있어 애플리케이션의 필요에 따라 **명시적으로 모듈을 로드하여 재사용**한다.<br/>
이렇게 기능별로 분리되어 있기 때문에 **코드를 재사용할 수 있다는 점**에서 `효율성`과 `유지보수성`을 높인다.<br/>
그러나 JavaScript에서는 **웹페이지의 보조적인 기능을 수행하기 위해 한정적인 용도로 만들어졌기 때문**에 다른 언어에 비해 부족한 점이 많다.<br/> 대표적인 것이 바로 `모듈 기능 부재`다.

**C언어**는 `#include`, **Java**는 `import`등 대부분 언어는 모듈 기능을 가지고 있다.<br/>
그러나 **클라이언트 사이드 자바스크립트**는 `script`태그를 사용하여 외부의 스크립트 파일을 가져올 순 있지만, 
파일마다 독립적인 파일 스코프를 갖지 않고, **하나**의 `전역객체`에 바인딩되기 때문에, <br/>
전역 변수가 **중복**되는 등의 문제가 발생할 수 있고, 이것으로는 **모듈화를 구현할 수 없다**.

자바스크립트를 클라이언트 사이드에 국한되지 않고 범용적으로 사용하기 위해선 `모듈`기능을 반드시 해결해야하는 과제가 되었다.<br/>
여기서 제안된 것이 `CommonJS`와 `AMD(Asynchronous Module Definition)`이다<br/>

```
[ 💡NOTE ]
AMD(Asynchronous Module Definition)
- 비동기적 모듈 선언
- 모듈 로딩이 다 될때까지 동기로 기다릴 수 없다.
  따라서 미동기 모듈방식이 브라우저에서 더 큰 효과를 발휘한다.
- define(), require()를 사용한다.
- 모듈로더는 RequireJs를 추천한다.

CommonJS
- 자바스크립트의 공식 스펙이 브라우저에서만 지원했기 때문에 서버사이드 및 데스크탑 어플리케이션에서 지원하기 위해 만든 방식
- 노드에서 채택한 방식으로 유명하다.
- 서버사이드에서 CommonJs를 더 자주 사용한다.
- require, module.export를 사용한다.
  - 모듈을 사용할 때는 require을 사용한다.
  - 모듈을 해당 스코프 밖을 보낼때 : module.export사용한다.
    module은 예약어로 '현재 모듈에 대한 정보를 가지고 있는 객체'다.
```

자바스크립트의 모듈화는 크게 `CommonJs`와 `AMD`로 나뉘게 되고, 브라우저에서 모듈을 사용하기 위해선 이 두개를 구현한 `모듈 로더 라이브러리`를 사용해야했다.<br/>
- 서버사이드 자바스크립트인 `Node.js`는 모듈 시스템의 사실상 표준(de facto standard)인 `CommonJs`를 채택
- 현재는 100% 동일하진 않지만, 기본적으로 `CommonJS` 방식을 따른다.

#### ES6
> 이러한 상황에서 ES6는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가하였다.
> script 태그에 `type="module"` 어트리뷰트를 추가하면, 로드된 자바스크립트 파일은 모듈로서 동작한다.

단, 아래와 같은 이유로 ES6모듈 기능보단, `Webpack`등의 모듈 번들러를 사용하는 것이 좋다.
  - IE를 포함한 구형 브라우저는 ES6를 지원하지 않는다.
  - 브라우저의 ES6 모듈 기능을 사용하더라도, 트랜스파일링이나 번들링이 필요하다.
  - 아직 지원하지 않는 기능( Bare import ) 등이 있다.
  - 아직 이슈가 있다.
  
## 모듈 스코프
> ES6모듈 기능을 사용하지 않으면, 분리된 자바스크립트 파일에 독자적인 스코프를 갖지 않고 하나의 기억을 공유한다.
> ES6 모듈은 **독자적인 `모듈 스코프`**를 갖는다.
> 따라서 `var키워드`로 선언한 변수는 더 이상 **전역 변수가 아니며**, **window 객체의 프로퍼티도 아니다**.

```jsx
[test1.mjs]

var x = "test1";
console.log(x); // test1

console.log(window.x); // undefined
```

```jsx
[test2.mjs]
var x = "test2";
console.log(x);// test2
console.log(window.x); // undefined
```
```jsx
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="test1.mjs"></script>
  <script type="module" src="test2.mjs"></script>
</body>
</html>
```
- 모듈 내 선언한 변수는 **모듈 외부**에서 참조할 수 **없다**
- 스코프가 다르기 때문.

```jsx
[test3.mjs]
const x = "test3";

console.log(x); // test3
```

```jsx
[test4.mjs]
console.log(x); // ReferenceError: x is not defined
```

```jsx
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="test3.js"></script>
  <script type="module" src="test4.js"></script>
</body>
</html>
```

  
## `export` 키워드
> 모듈은 독자적인 모듈 스코프를 갖기 때문에 모듈 안에 선언한 모든 식별자는 기본적으로 **해당 모듈 내부**에서만 참조할 수 있다.
> 모듈 안에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 참조할 수 있게 하려면 `export`키워드를 사용하면 된다.
> 선언한 `변수`, `함수`, `클래스` 모두 export 할 수 있다.

```jsx
[lib.mjs]
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function squre(x) {
  return x * x;
}

// 클래스의 공개
export class Person{
  constructor(name){
    this.name = name;
  }
}

- 모듈을 공개하려면 위의 예시처럼 선언문 앞에 `export`키워드를 사용한다.
- 여러개를 export할 수 있는데 각각의 **이름**으로 구별할 수 있다.

```jsx
[lib.mjs]
const pi = Math.PI;

function squre(x){
  return x * x;
}

class Person{
  constructor(name){
    this.name = name;
  }
}

// 변수, 함수, 클래스를 하나의 객체로 구성하여 공개
export {pi, suqre, Person};
```
- export 대상을 모아 하나의 객체로 구성하여 한 번에 export할 수 있다.

## `import` 키워드
> 모듈에서 `export`한 대상을 로드하려면 `import`키워드를 사용한다.
```jsx
[app.mjs]
// 같은 폴더 내의 lib.mjs 모듈을 로드.
// lib.mjs 모듈이 export한 식별자로 import한다.
// ES6 모듈의 파일 확장자를 생략할 수 없다.

import {pi, square, Person} from './lib.mjs';

console.log(pi);// 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Seulgi')); // Person {name : 'Seulgi'}
```
##### 여기서 만약 `export`한 식별자를 각각 지정하지 않고, 하나의 이름으로 한꺼번에 `import`하고 싶다면 <br/>
import되는 식별자 `as`뒤에 지정한 이름을 프로퍼티로 할당된다.
```jsx
[app.mjs]
import * as lib from './lib.mjs';

console.log(lib.pi); // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person('Seulgi')); // Person {name : 'Seulgi'}
```

##### 이름을 변경해서 import 할 수 있다.
```jsx
[app.mjs]
import {pi as PI, square as SQ, Person as P} from './lib.mjs';

console.log(PI); // 3.141592653589793
console.log(SQ(2)); // 4
console.log(new P('Seulgi')); // Person {name : 'Seulgi'}
```

##### 모듈 하나만 export할때는 `default`키워드를 사용할 수 있다.
> 그러나 var, let, const는 사용할 수 없다.

```jsx
[lib.mjs]
export default function(x){
  return x * x;
}

[test1.mjs]
export default () => {}; // OK

export default const test = () => {}; // SyntaxError: unexpected token 'const'
```
