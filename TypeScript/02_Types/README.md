# TypeScript의 타입
## 타입 선언(Type Declaration)
```tsx
let hi: string = 'HELLO';
```
- 변수 hi는 `string` 타입이다.
- 만약, 선언과 맞지 않는 타입을 할당하면, **컴파일**시점에서 에러가 발생한다.

```tsx
let age: number = "29" // Error: Type '"29' is not assignable to type 'number'.
```

## TypeScript의 기본 12가지 타입

- Boolean
- Number
- String
- Object
- Array
- Tuple
- Enum
- Any
- Void
- Null
- Undefined
- Never

## 타입별 선언 방법
## Boolean
```tsx
let areYouHappy: boolean = true;
```
## Number
```tsx
let age: number = 29;
```
## String
```tsx
let name: string = 'Olivia';
```
## Array
```tsx
let nums: Array<number> = [1, 2, 3, 4, 5];
// or
let nums2: number[] = [1, 2, 3, 4, 5];
```
## Tuple
> Tuple이란, 고정된 요소수만큼의 타입을 미리 선언 후 배열을 표현한 것이다.<br/>
```tsx
let tuple: [string, number];
tuple = ['hey', 10]; // OK
tuple = [10, 'hey']; // ERROR
tuple = ['hey', 10, 'hi', 20]; // ERROR
tuple.push(true); // ERROR
```

## Enum
> Enum이란, 열거형으로 특정 값(상수)들의 집합에 이름을 정한 것이다.<br/>
```tsx
enum Color1 {Red, Yellow, Pink};
let c1: Color1 = Color1.Pink;

console.log(c1); // 2

enum Color2 {Red = 1, Yellow, Pink};
let c2: Color2 = Color2.Pink;

console.log(c2); // 3

enum Color3 {Red = 1, Yellow = 2, Pink = 5};
let c3: Color3 = Color3.Pink;

console.log(c3); // 5
```

## Any
> 모든 타입에서 허용된다.
> 타입 추론(type, inference)할 수 없거나, 타입 체크가 필요 없는 변수에 사용한다.

```tsx
let unSure: any = 4;
unSure = 'hmm.. not sure';
unSure = false; // OK
```

## Void
> Void는 일반적으로 함수에서 **반환값이 없을 때** 사용한다.<br.>
```tsx
function warnMsg(): void{
  conosle.log("THIS IS A WARNING MESSAGE");
}
```

## Never
> 결코 발생하지 않는 값 
> 함수의 끝에 절대 도달하지 않는 타입<br/>
```tsx
function error(message: string): never{
  throw new Error(message);
}

function infiniteLoop(): never {
  while(true) {}
}
```
## 주의점
> 타입은 **소문자, 대문자**를 구별해야한다.
> TypeScript가 제공하는 타입은 모두 `소문자`다.

```tsx
let greeting: string;
greeting = 'hello' // OK

greeting = new String('hello') // ERROR
```
- 대문자는 래퍼 객체 타입이므로, 대문자로 타입을 할당하면 안된다.
- 단, 객체의 유형도 타입이 될 수 있다.

## 객체 유형의 타입
## Date
```tsx
const today: Date = new Date();
```
## HTMLElement 
```tsx
const elem: HTMLElement = document.getElementById('userId');
```

## 타입 예
```tsx
class Person {}

const person: Person = new Person();
```

## 타입 추론 (Type Inference)
> 타입 선언을 생략하면, 값이 할당하는 과정에서 동적으로 타입이 결정된다.

```tsx
let num = 123; // num은 number 타입 
let name = "olivia"; // name은 string 타입 
```

## 타입 캐스팅
> 기존의 타입에서 다른 타입으로 캐스팅하려면 `as` 키워드를 사용하거나, `< >` 연산자를 사용한다.

```tsx
const $input = document.querySelector('input[type="text"]');
// $input: Element | null
const val = $input.value;
// Error: 'value' does not exist on type 'Element'
```
#### `as` 사용
```tsx
const $input = document.querySelector('input[type="text"]')as HTMLInputElement;
const val = $input.value;
```
#### `< >` 사용
```tsx
const $input = <HTMLInputElement>document.querySelector(`input[type="text"]');
const val = $input.value;
```
