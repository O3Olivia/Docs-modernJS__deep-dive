# 클래스(Class)
## 클래스 정의(Class Definition)
> ES6 클래스는 클래스 몸체에 메소드만 포함할 수 있다.
> 클래스 몸체에 클래스 프로퍼티를 선언할 수 없고, 반드시 생성자 내부에서 선언하고 초기화해야한다.

```jsx
class Person {
  constructor(name){
    // 클래스 프로퍼티 선언 및 초기화
    this.name = name;
  }
  
  talk(){
    console.log(`${this.name} is talking right now.`);
  }
}
```
- 이 코드는 문제가 없는 코드다.
- 그러나, TypeSCript 파일(tsx,ts)로 바꾸면, 컴파일 에러가 발생한다.

```
person.ts(4,10): error TS2339: Property 'name' does not exist on type 'Person'.
person.ts(8,25): error TS2339: Property 'name' does not exist on type 'Person'.
```
#### TypeScript 클래스는 클래스 `몸체`에 클래스 프로퍼티를 사전에 선언해야한다.

```tsx
class Person{
  name: string;
  
  constructor(name:string){
    this.name = name;
  }
  
  talk(){
    console.log(`${this.name} is talking right now.`);
  }
}

const person = new Person('Olivia');
person.talk(); // Olivia is talking right now
```

## 접근 제한자
> TypeScript 클래스는 클래스 기반 객체 지향 언어가 지원하는 `접근 제한자(Access modifier)` : `public`, `private`, `protected`를 지원한다.
> 단, 접근 제한자를 명시하지 않는다면, 다른 클래스 기반의 언어는 암묵적으로 `protected`로 지정된다.
> 그러나, TypeScript는 `public`으로 선언된다.

```tsx
class Test {
  public x : string;
  protected y: string;
  private z: string;
  
  constructor(x: string, y: string, z: string){
    this.x = x;
    this.y = y;
    this.z = z;
  }
}

const test = new Test('x', 'y', 'z');

console.log(test.x); // public 접근 제한자는 클래스 인스턴스를 통해 클래스 외부에서 참조 가능.
console.log(test.y); // protected 접근 제한자는 클래스 인스턴스를 통해 클래스 외부에서 참조 불가.
console.log(test.z); // private 접근 제한자는 클래스 인스턴스를 통해 클래스 외부에서 참조 불가.

class Hi extends Test {
  constructor(x: string, y:string, z: string){
    super(x, y, z);
    
    console.log(this.x); // public 접근 제한자는 자식 클래스 내부에서 참조 가능.
    console.log(this.y); // protected 접근 제한자는 자식 클래스 내부에서 참조 가능.
    console.log(this.z); // private 접근 제한자는 자식 클래스 내부에서 참조 불가.
    console.log(this.x); //  
  }
}
```

##### 정리
- public : 클래스 내부(O), 자식 클래스 내부(O), 클래스 인스턴스(O)
- protected : 클래스 내부(O), 자식 클래스 내부(O), 클래스 인스턴스(X)
- private : 클래스 내부(O), 자식 클래스 내부(X), 클래스 인스턴스(X)


## 생성자 파라미터에 접근 제한자 선언
> 접근 제한자는 생성자 파라미터에 선언할 수 있다.
> 접근 제한자가 사용된 생성자 파라미터는 **암묵적으로 클래스 프로퍼티로 선언**된다.
> 그리고 **생성자 내부에서 별도의 초기화 없이 `암묵적`으로 초기화**된다.

- `private` 접근 제한자를 사용하면, 클래스 **내부** 에서만 참조 가능하다.
- `public` 접근 제한자를 사용하면, 클래스 **외부**에서도 참조 가능하다.

```tsx
class Test {
  constructor(public x: string){ }
}

const test = new Test('Hey');
console.log(test); // Test{x: 'Hey'}
console.log(test.x); // Hey

class Hej {
  constructor(private x: string){ }
}

const hej = new Hej('hej då');

console.log(hej); // Hej{x: 'hey då}

console.log(hej.x); // private으로 선언되어서 오직 클래스 안에서만 참조 가능.
```
- 만약 생성자 파라미터에 **접근 제한자를 선언하지 않는다**면, 생성자 파라미터는 **생성자 내부에서만 유효한 지역 변수**가 된다.
- 따라서 **외부에서 참조가 불가능**하다.
```tsx
class Test {
  constructor(x: string){
    console.log(x);
  }
}

const test = new Test('Hey');
console.log(test); // Test {}
```


##  `readonly` 키워드
> `readonly`키워드가 선언된 클래스 프로퍼티는 선언 / 생성자 내부에서만 값을 할당할 수 있다.
> 그 이외에는 **오직 읽기만 가능**

```tsx
class Test {
  private readonly MAX_LEN: number = 5;
  private readonly MSG: string;
  
  constructor(){
    this.MSG = "HEYYY";
  }
  
  log(){
    this.MAX_LEN = 10; // Cannot assign to 'MAX_LEN' because it is a constant or a read-only property. 
    // readonly가 선언되어서 재할당할 수 없다.
    this.MSG = "BYE"; // Cannot assign to 'MSG' because it is a constant or a read-only property.
  
    console.log(`MAX_LEN: ${this.MAX_LEN}`); // MAX_LEN: 5
    consoel.log(`MSG: ${this.MSG}`); // MSG: HEYYY
  }
}

new Test().log();
```

## `static`키워드
> `static`키워드는 클래스의 `정적`메소드를 정의한다.
> 정적 메소드는 클래스의 인스턴스가 아닌 클래스 이름으로 호출하기 때문에, 인스턴스를 생성하지 않아도 호출할 수 있다.

```tsx
class Test {
  constructor(prop) {
    this.prop = prop;
  }
  
  static staticMethod(){
    return 'staticMethod';
  }
  
  prototypeMethod(){
    return this.prop;
  }

  // 정적 메소드는 클래스 이름으로 호출한다.
  console.log(Test.staticMethod());
  
  const test = new Test(123);
  // 정적 메소드는 인스턴스로 호출할 수 없다.
  console.log(test.staticMethod()); // Uncaught TypeError: foo.staticMethod is not a function
```

### Typescript에서는 `static` 키워드를 클래스 프로퍼티에도 사용할 수 있다. 
> 정적 메소드처럼 클래스 이름으로 호출하며, 클래스 인스턴스를 생성하지 않아도 호출할 수 있다.

```tsx
class Test{
  static instanceCounter = 0;
  constructor(){
    Test.instanceCounter++;
  }
}

var test1 = new Test();
var test2 = new Test();

console.log(Test.instanceCounter); // 2
consoel.log(test2.instanceCounter) // error TS2339: Property 'instanceCounter' does not exist on type 'Test'.
```

## 추상 클래스
> 하나 이상의 추상 메소드를 포함해 일반 메소드도 포함할 수 있다.
> 추상 메소드는 **내용 없이** `메소드 이름`과 `타입`만이 선언된 메소드를 말한다.
> 이를 선언할 때 `abstract`키워드를 사용한다.
> 직접 인스턴스를 생성할 수 없으며, **상속만을 위해 사용**된다.
> 추상 클래스를 상속한 클래스는 추상 클래스의 추상 메소드를 반드시 구현

```tsx
abstract class Fruit {
  // 추상메소드
  abstract colour(): void;
  // 일반 메소드
  eat(): void {
    console.log('얼마나 맛있을까...');
  }
}
  // 직접 인스턴스를 생성할 수 없다.
  // new Fruit();
  // rror TS2511: Cannot create an instance of the abstract class 'Fruit'.
  
  class Orange extends Fruit {
    colour(){
      console.log("주황색");
    }
  }
  
  const myOrange = new Orange();
  myOrange.colour();
  myOrange.eat();

```













