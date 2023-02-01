# TypeScript

## TypeSCript란?

> JavaScript 타입을 부여한 언어다.
> 즉, JavaScript의 확장된 언어라고 볼 수 있다.
> 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 파일을 한번 변환(Compile)해주어야 한다.

## TypeScript 사용 이유

- 에러를 사전에 방지한다.
- 코드 가이드 및 자동 완성으로 개발 생산성이 향상된다.

  ### 에러 사전 방지
  > TypeScript를 사용하는 가장 큰 이유는 `정적 타입`을 지원한다. 
  > 타입을 정의하면, 개발자의 의도를 명확하게 알 수 있으며, 컴파일 단계에서 오류를 쉽게 발견할 수 있다.
  ##### JavaSCript
  ```jsx
  function sum(x, y){
    return x + y;
  }
  
  sum('0', '1'); // '01'
  ```
- 이 코드는 문법상 문제가 없다.
- 이 처럼 타입을 정하지 않는다면, 어떤 타입으로 인수를 전달해야하는지, 어떤 타입의 반환값을 리턴해야하는지 명확하지 않다.

##### TypeScript
  ```tsx
  function sum(x: number, y: number){
    return x + y;
  }

  sum('0', '1'); // Error: Argument of type '"0"' is not assignable to parameter of type 'number'.
  ```
  - TypeScript는 컴파일단계에서 오류를 파악할 수 있다.
  - 개발자가 어떤 의도인지 파악할 수 있으며, 어떤 타입으로 인수를 전달하고, 반환해야할지 명확하게 알 수 있어 `디버깅`이 쉽다.
  
  ### 코드 가이드 및 자동 완성
  > 어떤 타입으로 작성되야하는지, 어떤 타입인지 확인할 수 있다. 
  > `VSCode`에서 해당 타입을 미리보기로 띄워보내주며, `tab`으로 자동 완성할 수 있다.
  
## 개발환경 구축
> TypeScript 파일은 브라우저에서 동작하지 않기때문에, 컴파일러를 이용해 JavaScript 파일로 변환해야한다.

1️⃣ Node.js 설치 <br/>
 Node.js를 설치하고, 설치과 완료되면 terminal에서 <br/>
 ```
 $ node -v
 ```
 를 입력햇을 때 버전이 나오면 설치가 완료된 것이다.<br/>
 <br/>
2️⃣ TypeScript 컴파일러 설치
  Node.js를 설치하면, `npm`도 설치된다. <br/>
  터미널에서 `npm`을 이용해서 TypeSript를 설치한다.<br/>
  ```
  $ npm install -g typescript
  ```
  ```
  [ 👩‍💻 TypeScript 설치 확인]
    $ tsc -v
  ```
  버전이 출력되면 설치가 완료된 것이다.<br/>
 
