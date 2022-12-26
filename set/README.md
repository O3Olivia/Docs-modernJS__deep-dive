# Set(집합)
> `중복`되지 않고 정렬되지 않은 `유일한 값`들의 집합
> `set`을 통해 `교집합`, `합집합`, `차집합`, `여집합`등의 구현이 가능하다.

##### `set`객체 vs `배열`의 차이
###### `set`객체
- `동일한 값`을 `중복`하여 `포함 X`.
- 요소 순서에 의미가 없다.
- 인덱스로 요소 접근이 불가능하다.

##### `set`객체 생성
```jsx
const set = new Set();
console.log(set); // Set(0){}
```
- `set`객체는 `set`생성자를 사용해서 집합을 생성할 수 있다.
- 인수가 없으면 빈 set객체가 생성된다.

##### `set` 객체 생성
```jsx
const set1 = new Set([1, 2, 3]);
console.log(set1); // Set(3){1, 2, 3}

const set2 = new Set('123');
console.log(set2); //Set(3) { '1', '2', '3' }
```
- `set`생성자 함수는 인수로 iterable(배열, 맵, 집합, 문자열)을 받는다. 

##### `삽입`
```jsx
const set2 = new Set([1, 2, 3]);
set2.add(4); // Set(4) { 1, 2, 3, 4 }
```
- `add()` 메소드를 통해 삽입할 수 있다. 

##### `삭제`
```jsx
const set3 = new Set([1, 2, 3, 4]);
set3.delete(4); // Set(3) { 1, 2, 3 }
```
- `delete()` 메소드를 통해 삭제할 수 있다. 

##### `일괄 삭제`
```jsx
const set3 = new Set([1, 2, 3, 4]);
set3.clear();
console.log(set3); // Set(0){}
```

##### `포함여부` 확인 
```jsx
const set4 = new Set([1, 2, 3]);
console.log(set4.has(3)); // true
console.log(set4.has(6)); // false
```
- `has()`메소드를 통해 포함 여부를 확인할 수 있는데 결과 값은  `true`/`false`로 나온다 

##### `중복요소 제거`
```jsx
const set5 = array => [...new Set(array)];
console.log(set5([1, 1, 2, 2, 2, 3, 3, 3, 3])) // [ 1, 2, 3 ]
```
- `set`객체는 중복된 값을 저장하지 않기 때문에 위의 예시처럼 추가해도 중복된 값은 추가되지 않는다. 

##### `요소 순회`
```jsx
const set6 = new Set([1, 2, 3]);
set.forEach((v, v2, set) => console.log(v, v2, set)); 
// 1 1 Set(3) { 1, 2, 3 }
// 2 2 Set(3) { 1, 2, 3 }
// 3 3 Set(3) { 1, 2, 3 }
```
- `set`객체의 요소를 순회하기 위해선 `Set.prototype.forEach`를 사용해야한다. 
- `Array.prototype.forEach`메서드와 유사하게 `콜백함수`와 `forEach`메서드의 콜백함수 내부에서 `this`로 사용할 객체를 전달할 3가지 객체를 인수로 전달한다. 
  - `현재 순회중인 요소값`, `현재 순회중인 요소 값`, `현재 순회중인 set객체 자체`
  - `1`, `2`번째 인수가 같은 이유 : `Array.prototype.forEach`메서드와 인터페이스를 통일하게 하기 위해서다. 

### 집합 연산
##### 교집합
```jsx
Set.prototype.intersection = function(set){
  return new Set([...this].filter(v=>set.has(v)));
}
const set7 = new Set([1, 2, 3, 4, 5]);
const set8 = new Set([2, 5]);

// set7과 set8의 교집합 
console.log(set7.intersection(set8)); // Set(2) {2, 5}
// set8과 set7의 교집합
console.log(set8.intersection(set7)); // Set(2) {2, 5}
```
- 여기서 `this`는 메서드를 호출한 객체다.
- `...` spread 문법으로 새로운 집합을 만든 뒤, `filter`를 통해서 `set.has()`와함께 나머지 집합들이 해당 원소를 가졌는지 체크해준다. 

##### 합집합
```jsx
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const set9 = new Set([1, 2, 3, 4, 5]);
const set10 = new Set([2, 5]);
 
// set9과 set10의 합집합 
console.log(set9.union(set10)); // Set(5) {1, 2, 3, 4, 5}
// set10과 set9의 합집합 
console.log(set10.union(set9)); // Set(5) { 2, 5, 1, 3, 4 }
```
- `...` spread 문법을 통해 두 set를 합쳐서 새로운 set를 리턴한다.
- 여기서 주목해야할 점은, 아래 콘솔을 보면 먼저 나오는 set을 부르고, 그 값을 뺀 나머지를 뒤로 출력하는 것을 볼 수 있다.

##### 차집합 
```jsx
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const set11 = new Set([1, 2, 3, 4, 5]);
const set12 = new Set([2, 5]);

// set11에 대한 set12의 차집합 
console.log(set11.difference(set12)); // Set(3) { 1, 3, 4 } -> set12가 안가지고 있는것만 출력
// set12에 대한 set11의 차집합 
console.log(set12.difference(set11));// Set(0) {} // set11이 안가지고 있는 것만 출력 => 다 가지고 있으니 {}
```
위와 동일
```jsx
function differenceSet(set11, set12){
  let difference = new Set(set11)
  set12.forEach(e => {
    difference.delete(e)
  })
  return difference
}
let set11 = new Set([1, 2, 3]);
let set12 = new Set([3, 4, 5]);

console.log(differenceSet(set11, set12)); // Set(2) {1, 2}
console.log(differenceSet(set12, set11)); // Set(2) {4, 5}
```
- 위의 예시와 동일하다. 
- 역시 합집합의 경우 순서가 바뀌어도 상관없지만, `차집합`의 경우 `순서`에따라 경과가 다르기때문에 **순서에 신경**을 써야한다. 

##### 부분집합 
```jsx
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => superSetArr.includes(v));
};

cosnt set13 = new Set([1, 2, 3, 4, 5]);
const set14 = new Set([2, 5]);

// set13이 set14의 상위 집합인가 
console.log(set13.isSuperset(set14)); // true
// set14가 set13의 상위 집합인가
console.log(set14.isSuperset(set13)); // false
```
