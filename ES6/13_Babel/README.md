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

> `npm`을 사용해서 `Babel CLI`를 설치할 수 있다.
> 다만, 프로젝트마다 설정이 다르니, **로컬**로 설치하는 것이 좋다.

```sh
# 프로젝트 폴더 생성
mkdir es6-project && cd es6-project
# package.json 생성
npm init -y
# babel-core, babel-cli 설치
npm install --save-dev @babel/core @babel/cli
```
