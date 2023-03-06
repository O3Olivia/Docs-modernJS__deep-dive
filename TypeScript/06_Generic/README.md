# 제네릭 (Generic)
> 함수 혹은 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언해야한다.<br/>
> 그러나, 함수나 클래스를 정의할 때 매개변수나 반환값 타입을 선언하기 힘든 경우가 있다.<br/>
> 이때, 제네릭은 한 번의 선언으로 다양한 타입을 재사용할 수 있다.

```tsx
class Queue {
  protected data: any[] = [];
  
  push(item: any){
    this.data.push(item);
  }
  
  pop() {
    return this.data.shift();
  }
}

const queue = new Queue();

queue.push(0);
queue.push('1');  // 실수

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // RUNTIME ERROR
```
- Queue클래스의 data 프로퍼티는 `any[ ]` 타입이다.
- `any[ ]`타입은 어떤 타입 요소도 가질 수 **있는** 배열이다.
- `any[ ]` 타입은 배열 요소의 타입이 모두 같지 않다.
- 위의 코드의 경우, data 프로퍼티가 `number`타입만을 포함하는 배열이라는 기대하에, `Number.prototype.toFixed`를 사용했다.
- 따라서 `number` 타입이 아닌 요소의 경우, 런타임 에러가 발생한다.

##### Queue클래스를 상속하여, number 타입 전용 NumberQueue 클래스를 정의
```tsx
class Queue {
  protected data: any[] = [];
  
  push(item: any){
    this.data.push(item);
  }
  
  pop(){
    return this.data.shift();
  }
}

// Queue 클래스를 상속하여 number 타입 전용 NumberQueue 클래스를 정의
class NumberQueue extends Queue{
  // number 타입의 요소만을 push한다
  push(item: number){
    super.push(item);
  }
  
  pop(): number{
    return super.pop();
  }
}

const queue = new NumberQueue();

queue.push(0);
// 의도하지 않은 실수를 사전 검출 가능
// error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
// queue.push('1');

queue.push(+'1'); // 실수를 사전에 인지하고 수정 가능. +를 붙이면 num로 인식
console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // 1
```

- number타입 전용 `NumberQueue` 클래스를 정의하면, number 타입 이외의 요소가 push 되었을 때, **런타임 이전에 `에러`를 사전에 감지**할 수 있다.

#### 다양한 타입을 지원할 경우
```tsx
class Queue<T>{
  protected data: Array<T> = [];
  push(item: T) {
    this.data.push(item);
  }
  pop(): T | undefined {
    return this.data.shift();
  }
}

// number 전용 Queue
const numberQueue = new Queue<number>();

numberQueue.push(0);
// numberQueue.push('1'); // 의도하지 않은 실수를 사전 검출 가능
numberQueue.push(+'1'); // 실수를 사전 인지하고 수정할 수 있다.

// ?. => optional chaining
// https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining
console.log(numberQueue.pop()?.toFixed()); // 0
console.log(numberQueue.pop()?.toFixed()); // 1
console.log(numberQueue.pop()?.toFixed()); // undefined

// string 전용 Queue
const stringQueue = new Queue<string>();

stringQueue.push('hello');
stringQueue.push('world');

console.log(stringQueue.pop()?.tpUpperCase()); // HELLO
console.log(stringQueue.pop()?.toUpperCase()); // WORLD
console.log(stringQueue.pop()?.toUpperCase()); // undefined

// 커스텀 객체 전용 Queue
const myQueue = new Queue<{name: string, age: number}> ();
myQueue.push({name: 'Olivia', age: 29});
myQueue.push({name: 'Lee', age: 29});

console.log(myQueue.pop()); // {name : 'Olivia', age: 29}
console.log(myQueue.pop()); // {name : 'Lee', age : 29}
console.log(myQueue.pop()); // undefined
```

- 제네릭은 선언 시점이 아니라 **생성 시점**에 타입을 명시하여, 하나의 타입만이 아닌 **다양한 타입**을 사용할 수 있다.
- 한 번의 선언으로 다양한 타입을 **재사용**할 수 있다.
- 위의 코드를 보면, `T`는 제네릭을 선언할 때 관용적으로 사용되는 식별자로, `Type Parameter`라고 한다.
- `T`는 `Type`의 약자로 반드시 `T`를 사용하지 않아도 된다.
- 함수에도 제네릭을 사용할 수 있는데, 이렇게 제네릭을 사용하게 되면, 다양한 타입의 매개변수와 리턴값을 사용할 수 있다.

```tsx
function reverse<T>(itmes: T[]): T[]{
  return items.reverse();
}
```
- reverse 함수는 인수의 타입에 의해 매개변수가 정해진다.
- reverse 함수는 다양한 타입의 요소로 구성된 배열을 인자로 전달 받는다.
- number 타입의 요소를 갖는 배열을 전달받으면, 타입 매개변수는 number가 된다.

```tsx
function reverse<T>(items: T[]): T[] {
  return items.reverse();
}

const arg = [1, 2, 3, 4, 5];

// 인수에 의해 타입 매개변수가 결정된다.
const reversed = reverse(arg);
console.log(reversed); // [5, 4, 3, 2, 1]
```

- 만약 {name: string} 타입의 요소를 갖는 배열을 전달받으면 타입 매개변수는 {name: string}가 된다.

```tsx
function reverse<T>(items: T[]): T[] {
  return items.reverse();
}

const arg = [{ name: 'Olivia' }, { name: 'Lee' }];
// 인수에 의해 타입 매개변수가 결정된다.
const reversed = reverse(arg);
console.log(reversed); // [ { name: 'Lee' }, { name: 'Olivia' } ]
```
