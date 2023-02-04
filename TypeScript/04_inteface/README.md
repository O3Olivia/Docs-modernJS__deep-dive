# 인터페이스(interface)
> 인터페이스는 정의한 규칙이다.

  ```
  [ 💡 인터페이스 규칙 종류  ]
    객체의 스펙(객체 속성 & 속성 타입)
    함수 파라미터
    함수 스펙(파라미터, return 타입 등)
    클래스
    배열과 객체 접근 방식
  ```
- 인터페이스는 일반적으로 `타입 체크`를 위해 사용되며, `변수`, `함수`, `클래스`에 사용된다.
- 여러가지 타입을 갖는 프로퍼티로 이루어진 새로운 타입을 정의하는 것과 유사하다.
- 인터페이스는 프로퍼티와 메소드를 가질 수 없다.
- 따라서 클래스와 비슷하지만, **직접 인스턴스를 생성할 수 없고 모든 메소드는 추상 메소드다**
- 하지만, `abstract`키워드는 사용하지 않는다.

## 변수와 인터페이스
> 인터페이스는 변수의 타입으로 사용할 수 있다.
> 이때 변수는 해당 인터페이스를 준수해야하며, 이것은 새로운 타입을 정의하는 것과 유사하다.

```tsx
// 인터페이스 정의(대문자)
inteface Todo{
  id: number;
  content: string;
  completed: boolean;
}

// 변수의 todo 타입으로 Todo 인터페이스 선언
let todo: Todo;

// 변수 todo는 Todo 인터페이스를 따라야한다.
todo = {id: 1, content 'Learning TypeScript', completed: true};
```
- 이렇게 인터페이스를 준수하는 인수를 전달하면, 복잡한 매개변수 체크를 할 필요가 없어 유용하며, 디버깅에 편리하다.

```tsx
// 인터페이스 정의
interface Test{
  id: number;
  content: string;
  completed: boolean;
}

let todos: Todo[] = [];

// 파라미터 todo의 타입으로 Todo 인터페이스 선언
function addTodo(todo: Todo){
 todos = [...todos, todo];
}

// 파라미터 todo는 Todo 인터페이스를 준수
const newTodo: Todo = {id: 1, content: 'Learning TypeScript', completed: true};
addTodo(newTodo);
console.log(todos); // [{id: 1, content: 'Learning TypeScript', completed: true}]
```

## 함수와 인터페이스
> 인터페이스는 함수의 타입으로도 사용할 수 있다.
> 여기서 함수의 인터페이스에는 타입이 선언된 파라미터 리스트와 리턴 타입을 정의해야하며, 이 함수는 인터페이스르 준수해야한다.

```tsx
// 함수 인터페이스 정의
interface SquareFunc {
  (num: number): number;
}

// 함수 인터페이스를 구현하는 함수는 인터페이스를 준수
const squareFunc: SquareFunc = function(num: number){
  return num * num;
}

console.log(squareFunc(100)); // 10000
```

## 클래스와 인터페이스
> 클래스 선언문의 implements 뒤에 인터페이스를 선언하면, 해당 클래스는 반드시 그 인터페이스를 구현해야한다.
> 단, 직접 인스턴스를 생성할 순 없다.

```tsx
// 인터페이스 정의
interface ITodo{
  id: number;
  content: string;
  completed: boolean;
}

// ITodo 인터페이스 구현
class Todo implements ITodo {
  constructor (
    public id: number,
    public content: string,
    public completed: boolean
  ) { }
}

const todo = new Todo(1, 'Learning TypeScript', true);

console.log(todo);
```

### 인터페이스는 메소드도 포함할 수 있다.
> 단, 메소드는 추상 메소드여야하고, 인터페이스에서 정의한 프로퍼티와 추상 메소드를 반드시 구현해야한다.

```tsx
// 인터페이스 정의
interface IPerson {
  name: string;
  greeting(): void;
}

// 인터페이스를 구현하는 클래스는 인터페이스에서 정의한 프로퍼티와 추상 메서드를 반드시 구현해야한다.
class Person implements IPerson{
  // 인터페이스에서 정의한 프로퍼티 구현
  constructor(public name: string){}
  
  // 인터페이스에서 정의한 추상 메서드 구현
  greeting(){
    console.log(`Hey, ${this.name}`);
  }
}

function greeter(person: IPerson): void{
  person.greeting();
}

const me = new Person('Olivia');
greeter(me); // Hey, Olivia
```

## 덕 타이핑(Duck Typing)
> 타입 체크에서 중요한 것은 실제로 그 값을 가지고 있느냐는 것.

```tsx
interface IDuck { // 1
  quack(): void;
}

class MallardDuck implements IDuck { // 3
  quack() {
    console.log('QUACK!');
  }
}

class RedheadDuck { // 4
  quack() {
    console.log("QUACK~~~~!!!!!!"); 
  }
}

function makeSounds(duck: IDuck): void{ // 2
  duck.quack();
}

makeSounds(new MallardDuck()); // QUACK!
makeSounds(new RedheadDuck()); // QUACK~~~~!!!!!! // 5
```
1️⃣ 인터페이스 `IDuck`은 quack 메소드를 정의한다.<br/>
2️⃣ `makeSounds`함수는 인터페이스 IDuck을 구현했다.<br/>
3️⃣ 클래스 `MallardDuck`은 인터페이스 IDuck을 구현했다.<br/>
4️⃣ 클래스 `RedheadDuck`은 인터페이스 IDuck을 구현하지 않았지만, quack 메소드를 갖는다.<br/>
5️⃣ `makeSounds` 함수에 인터페이스 IDuck을 구현하지 않은 `RedheadDuck`의 인스턴스를 인자로 전달해도 에러 없다.<br/>

- 만약, 해당 인터페이스에서 정의한 `프로퍼티나`, `메소드`를 가지고 있다면, 그 인터페이스를 구현한 것이나 마찬가지다.
- 이를 `Duck typing(덕 타이핑)` 또는 `구조적 타이핑`이라 한다.

```tsx
interface IPerson{
  name: string;
}

function greeting(person: IPerson): void{
  console.log(`Hey, ${person.name}`);
}

const me = {name: 'Olivia', age: 29};
greeting(me); // Hey, Olivia
```
- 변수 `me`는 인터페이스 `IPerson`와 일치하지 않는다.
- 그러나, `IPerson`의 `name프로퍼티`를 갖고 있기 때문에 인터페이스에 부합된다.

```tsx
function greeting(person){
  console.log("Hey, " + person.name);
}

var me = {name : "Olivia", age: 29};
greeting(me); // Hey, Olivia
```

## 선택적 프로퍼티
> 인터페이스 프로퍼티는 반드시 구현되어야하지만, 선택적으로 사용할 경우 프로퍼티명 뒤에 `?`를 붙인다.
> `?`를 붙이면, 생략해도 에러가 발생하지 않는다.

```tsx
interface UserInfo {
  userName: string;
  password: string;
  age? : number;
  address? : string;
}

const userInfo: UserInfo = {
  userName: 'Olivia',
  password: '123456'
}
console.log(userInfo);
```

## 인터페이스 상속
> 인터페이스는 `extends`키워드를 사용해서 **인터페이스 & 클래스**를 상속받을 수 있다.

```tsx
interface Person {
  name: string;
  age?: number;
}

interface Student extends Person {
  grade: number;
}

const student: Student = {
  name: "Olivia",
  age: 29,
  grade: 3
}
```

#### 복수 인터페이스 상속
```tsx
interface Person {
  name: string;
  age?: number;
}
interface Developer {
  skills: string[];
}

interface WebDeveloper extedns Person, Developer {}

const webDeveloper: WebDeveloper =  {
  name: 'Olivia',
  age: 29,
  skills: ['HTML', 'CSS', 'JavaScript"]
}
```
- 인터페이스는 인터페이스 외에 클래스도 상속받을 수 있다.
- 다만, 모든 멤버(public, protected, private)가 상속되지만, 구현까지 상속되진 않는다.

```tsx
class Person{
  constructor(public name: string, public age: number){}
}

interface Developer extends Person{
  skills: string[];
}

const developer: Developer = {
  name: "Olivia",
  age: 29,
  skills: ["HTML", "CSS", "JavaScript"]
}
```




























