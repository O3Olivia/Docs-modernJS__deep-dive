# 타입 앨리어스(Type Alias)

> 타입 앨리어스는 새로운 타입을 정의한다. <br/>
> 타입을 정의할 수 있어서 `interface`와 유사하다.

#### interface

```tsx
interface Person {
  name: string,
  age?: number,
}

// 빈 객체를 Person 타입으로 지정
const person = {} as Person;
person.name: 'Olivia';
person.age: 29;
person.address = 'Seoul, South Korea' // ERROR
```

#### 타입 앨리어스

```tsx
// 타입 앨리어스
type Person = {
  name: string;
  age?: number;
};

// 빈 객체를 Person 타입으로 지정
const person = {} as Person;
person.name = "Olivia";
person.age = 29;
person.address = "Seoul, South Korea"; // ERROR
```

- 별도로 타입 앨리어스는 `원시값`, `유니온`, `튜플` 등으로 타입을 지정할 수 있다.

```tsx
// 문자열 리터럴로 타입 지정
type Str = "Olivia";

// 유니온 타입으로 타입 지정
type Union = string | null;

// 문자열 유니온 타입으로 타입 지정
type Name = "Olivia" | "Seulgi";

// 숫자 리터럴 유니온 타입으로 타입 지정
type Num = 1 | 2 | 3 | 4 | 5;

// 객체 리터럴 유니온 타입으로 타입 지정
type Obj = { a: 1 } | { a: 2 };

// 함수 유니온 타입으로 타입 지정
type Func = (() => string) | (() => void);

// 인터페이스 유니온 타입으로 타입 지정
type Shape = Square | Rectangle | Circle;

// 튜플로 타입 지정
type Tuple = [string, boolean];
const t: Tuple = ["", ""]; // ERROR
```

인터페이스는 `extends`나 `implements`가 될 수 있다. <br/>
그러나 타입 앨리어스는 `extends`나 `implements`가 될 수 없다. <br/>
만약, **상속**을 통해 확장이 필요하다면 **인터페이스 사용** <br/>
그러나, 인터페이스로 표현할 수 없거나, `유니온`, `튜플`을 사용해야한다면, **타입 앨리어스** 사용하는 것이 좋다.
