# 템플릿 리터럴(Template literal)
> ES6부터 `템플릿 리터럴`이라는 새로운 문자열 표기법을 도입하였다. <br />
일반 문자열과 비슷하지만, `''` 혹은 `""`과 같은 따옴표 대신 백틱(`)을 사용한다.

- 일반 문자열에서는 줄바꿈이 허용되지 않고, 공백은 \로 시작하는 **이스케이프 시퀀스(Escape Sequence)** 를 사용해야한다.
- 템플릿 리터럴을 사용하면 일반 문자열과 달리 `여러 줄이 있는 줄바꿈`이 가능하고 그대로 적용된다.
```jsx
// ES5
var normIntroduce = "Hello!\nJavaScript!";
console.log(normIntroduce); 
// Hello
// JavaScript!

// ES6
const temIntroduce = `Hello
JavaScript!`;
console.log(temIntroduce);
// Hello
// JavaScript!
```

### 문자열 삽입
> 일반 문자열에서는 `+ 연산자`를 사용하여 삽입하였다면, <br/>
템플릿 리터럴은 `${ ... }` 표현식인 `문자열 인터폴레이션(String Interpolation)`으로 삽입할 수 있다.
```jsx
const firstName = "Seulgi";
const lastName = "Lee";

// ES5
console.log("My name is " + lastName + firstName + "!!!"); // My name is Lee Seulgi!!!

// ES6
console.log(`My name is ${lastName} ${firstname} !!!`); // My name is Lee Seulgi!!!
```
[ 💡 Note ] <br/>
문자열 인터폴레이션으로 감싼 표현식은 **강제로 타입 변환**된다. <br/>
자바스크립트 엔진은 문자열 타입이 아닌 값을 문자열 타입으로 암묵적 타입변환을 수행할 때 다음과 같이 동작한다.
```jsx
const a = 2;
const b = 6; 

console.log(`2 더하기 6은 ${a+b} 입니다.`); // 2 더하기 6은 8 입니다.
```
