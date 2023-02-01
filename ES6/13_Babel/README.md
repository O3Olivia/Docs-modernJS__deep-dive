# Babel

## Babel과 Webpack을 이용한 ES6 환경 구축

> 기술에 발전해가면서 모든 언어도 계속해서 발전해왔다.
> JavaScript역시 발전해왔다.
> 대부분의 브라우저는 ES6이상의 버전을 지원하지만, 사양 문제로 인해 ES6버전보다 아래의 버전을 지원하는 구형 브라우저들이 있다.
> 이 경우, `Babel`은 ES6 -> ES6로 **변환**해준다.
> 즉, 브라우저의 하위 호환성을 생각해서 ES6로 코드를 작성하여도 **구형**브라우저가 이를 인식할 수 있도록 ES5로 **변환**시켜준다.

# Babel

## Babel이란?

##### ES6의 화살표 함수와 ES7의 지수 연산자 사용

```jsx
[1, 2, 3].map((n) => n ** n);
```

- 대부분의 모던 브라우저(Chrome 61, FF 60, SF 10.1, Edge 16 이상)은 위 두 기능을 지원한다.
- 그러나 IE나 다른 구형 브라우저는 이 두가지 기능을 지원하지 않는다.

##### ES5이하로 버전 변환

```jsx
// ES5
"use strict";

[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```

- 따라서 `Babel`을 사용하면, 위의 코드처럼 ES5이하의 버전으로 변환할 수 있다.

## Babel CLI 설치
> `Babel CLI`란 터미널에서 커맨드를 입력해서 `babel`을 사용하기 위한 패키지

- `npm`을 사용해서 `Babel CLI`를 설치할 수 있다.
- 다만, 프로젝트마다 설정이 다르니, **로컬**로 설치하는 것이 좋다.

```
👊 프로젝트 폴더 생성
$ mkdir es6-project && cd es6-project
👊 package.json 생성
$ npm init -y
👊 babel-core, babel-cli 설치
$ npm install --save-dev @babel/core @babel/cli
```
설치 이후, `package.json` 파일은 아래와 같으며 불필요한 설정은 삭제.<br/>
```jsx
{
  "name": "es6-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2"
  }
}
```

## babelrc 설정 파일 작성
> Babel을 사용하기 위해선 `@babel/preset-env`를 설치해야한다.
> `@babel/preset-env`는 함께 사용해야하는 **Babel 플러그인**을 모아 둔 것으로 `Babel 프리셋`이다.

```
[ 👩‍💻 BABEL 프리셋 ]
- @babel/preset-env
- @babel/preset-flow
- @babel/preset-react
- @babel/preset-typescript
```
- `@babel/preset-env`는 필요한 플러그인들을 **프로젝트 지원환경에 맞춰 동적으로 결정**해준다.
- 프로젝트 지원 환경은 **Browserslist**형식으로 `.browserslistrc`파일에 상세히 설정 가능하다.

##### 기본설정(기본 설정은 모든 ES6+ 코드로 변환)
```
👊 env preset 설치
$ npm install --save-dev @babel/preset-env
```
##### [package.json]파일
```json
{
  "name": "es6-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/preset-env": "^7.7.1"
  }
}
```
- 설치 후 프로젝트 루트에 `.babelrc` 파일을 생성한 뒤 아래처럼 작성한다.
```jsx
{
  "presets": ["@babel/preset-env"]
}
```
- 설피한 `@babel/preset-env`를 사용하겠다는 의미

## 트랜스파일링
> Babel을 사용해서 `ES6+`코드를 `ES5`이하의 코드로 트랜스파일링하기 위해서 **Babel CLI 명령어**를 사용할 수 있지만, `npm script`를 사용하여 트랜스파일링할 수 있다.

##### [package.json]파일에 scripts를 추가한다.
```json
{
  "name": "es6-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/preset-env": "^7.7.1"
  }
}
```

##### ES6+ 파일을 작성하여 트랜스파일링을 테스트한다.
```jsx
// src/js/lib.js -> 루트에 src/js폴더 생성 후 lib.js와 main.js 추가
// ES6 모듈
export const pi = Math.PI;

export function power(x, y){
  // ES7: 지수 연산자
  return x ** y;
}

// ES6 클래스
export class Test {
  // stage 3: 클래스 필드 정의 제안
  #private = 10;
  
  test() {
    // stage 4: 객체 Reset/Spread 프로퍼티
    const { a, b, ...x } = { ... { a: 1, b: 2}, c: 3, d: 4};
    return {a, b, x};
  }
  
  test2(){
    return this.#private;
  }
}
```
```jsx
// src/js/main.js
// ES5 모듈
import {pi, test, Test} from './lib';

console.log(pi);
console.log(test(pi, pi));

const t = new Test();
console.log(t.test());
console.log(t.test2());
```

##### 터미널에서 명령어로 트랜스파일링 실행
```
$ npm run build

> es6-project@1.0.0 build /Users/leeungmo/Desktop/es6-project
> babel src/js -w -d dist/js

SyntaxError: /Users/leeungmo/Desktop/es6-project/src/js/lib.js: Unexpected character '#' (12:2)

  10 | export class Test {
  11 |   // stage 3: 클래스 필드 정의 제안
> 12 |   #private = 10;
     |   ^
  13 |
  14 |   test() {
  15 |     // stage 4: 객체 Rest/Spread 프로퍼티
...
```

다만, `@babel/preset-env`는 현재 제안 단계 사양에 대한 플러그인을 지원하지 않기 때문에 에러가 발생한다. <br/>
별도의 플러그인을 설치해야 한다.

## Babel 플러그인
> 설치가 필요한 플러그인은 Babel 홈페이지에서 찾아볼 수 있다.
> 클래스 필드 정의 제안 플러그인을 검색해야하므로 `Class field`를 검색한다.

##### `@babel/plugin-proposal-class-properties` 설치. (검색한 플러그인임)
```
$ npm install --save-dev @babel/plugin-proposal-class-properties
```
##### package.json
```json
{
  "name": "es6-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/plugin-proposal-class-properties": "^7.7.0",
    "@babel/preset-env": "^7.7.1"
  }
}
```
##### 설치한 플러그인은 `.babelrc`파일에 추가
```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```
##### 터미널에서 트랜스파일링하기
```
npm run build

> es6-project@1.0.0 build /Users/leeungmo/Desktop/es6-project
> babel src/js -w -d dist/js

Successfully compiled 2 files with Babel.
```
- 트랜스파일링에 성공하면, 프로젝트 루트에 `dist/js`폴더가 자동으로 생성된다.
- 트랜스파일링된 main.js와 lib.js가 저장된다.

##### 트랜스파일링된 main.js실행
```
$ node dist/js/main
3.141592653589793
36.4621596072079
{ a: 1, b: 2, x: { c: 3, d: 4 } }
10
```

## 브라우저에서 모듈 로딩 테스트
위에서 ES5로 변환이 문제없이 실행되는 것을 확인했다. <br/>
또한, ES6 모듈의 `import export`키워드도 트랜스파일링되어 모듈 기능도 동작하였다. <br/>
그러나, 모듈 기능은 `node.js`환경에서 동작한 것이고, Babel이 모듈을 트랜스파일링한 것도 `node.js`가 기본적으로 지원하는 `CommonJS`방식의 `module loadiing system`에 따른 것이다.

##### src/js/main.js가 Babel에 의해 트랜스파일링된 결과
```jsx
// dist/js/main.js
"use strict";

var _lib = require("./lib");

cosole.log(_lib.pi);
console.log((0, _lib.test)(_lib.pi, _lib.pi));
var t = new _lib.Test();
console.log(t.test());
console.log(t.test2());
```
- 브라우저는 `CommonJS`방식의 `module loading system(require함수)`를 지원하지 않는다.
- 따라서, 트랜스파일링 된 결과를 그대로 브라우저에 실행하면 에러가 발생한다.

##### index.html을 작성하여 트랜스파일링된 JavaScript파일을 브라우저에서 실행
```html
<!DOCTYPE html>
<html>
<body>
  <script src="dist/js/lib.js"></script>
  <script src="dist/js/main.js"></script>
</body>
</html>
```
- 다음과 같은 에러 발생
```
Uncaught ReferenceError: exports is not defined
    at lib.js:3
main.js:3 Uncaught ReferenceError: require is not defined
    at main.js:3
```

- 브라우저의 ES6 모듈 기능을 사용하도록 Babel을 설정할 수도 있다.
- 그러나 브라우저의 ES6 모듈 기능을 사용하는 것은 문제가 있다.
- 이는 `Webpack`을 통해 해결할 수 있다.
