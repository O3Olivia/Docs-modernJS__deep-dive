# Promise(프로미스)
> 비동기 처리를 위한 `콜백함수`의 단점을 보안하고 **비동기 처리 시점을 명확하게 표현**하는 패턴

자바스크립트는 비동기 처리를 위해 `콜백 함수`를 사용한다. <br/>
그러나 이 콜백함수는 `콜백 헬`로 인해 가독성이 나쁘고, 비동기 처리 중 발생한 에러 처리하는데 곤란하는 등 여러가지 한계가 있다. <br/>
이를 해결하고자 ES6에서는 **비동기처리**를 위한 또 다른 패턴인 `프로미스`를 도입했다.

## 콜백 패턴의 단점
#### 동기식 처리모델
> 동기식 처리모델은 **직렬적**으로 명령을 수행한다.
<img src="https://user-images.githubusercontent.com/87024040/212609985-16aefa1f-6178-48a7-94a4-ae5af6a45ad3.png"><br/>
- 즉, 작업이 순차적으로 실행되고, 어떤 작업이 **수행**중이면, 다음 작업은 **대기**한다.
- 서버에서 데이터를 가져와서 화면에 표시하는 작업을 예로 들면, 서버에서 데이터를 요청하고<br/>
  데이터가 **응답**될 때까지 그 이후의 작업들은 **중단**된다.
  
#### 비동기식 처리모델
> 비동시기 처리모델은 **병렬적**으로 명령을 수행한다.
<img src="https://user-images.githubusercontent.com/87024040/212610488-8c554e5d-4786-4e54-a238-1f7b3caa8d19.png"><br/>
- 즉, 작업 명령이 **완료되지 않아도** 그 다음 작업들이 **대기하지 않고**, **즉시 다음 명령을 수행**한다.
- 서버에서 데이터를 가져와서 화면에 표시하는 작업을 예로 들면, 서버에서 요청한 데이터가 응답할 때까지 기다리지 않고, 계속 다음 작업을 수행한다.
- 만약, 서버로부터 데이터가 응답되면, 이벤트가 발생하고, 이벤트 핸들러 데이터를 가지고 수행할 작업을 계속 수행한다.
- 따라서, 자바스크립트에서 사용하는 `비동기식 처리 모델`은 작업 요청을 `병렬`로 처리해서 다른 요청이 블로킹(중단)되지 않는 장점이 있다.

## 콜백 헬
```
[ 💡 NOTE  ]
콜백 함수: 콜백 함수는 다른 함수에 매개변수로 넘겨준 함수.
         특정 이벤트가 발생했을 경우, 시스템에 의해 호출된다.
         따라서, 이 콜백 함수는 필요에 따라 즉시 실행할 수도 있고, 나중에 실행될 수도 있다.
```
> 비동기 처리를 위해 콜백 패턴을 사용하려면, 처리 순서를 보장하기 위해서 여러개의 콜백 함수가 네스팅(중첩)되어 복잡도가 넢아지는 콜백 헬이 발생한다. 
> 즉, 함수의 매개 변수로 넘겨지는 콜백 함수가 반복되어, 코드의 들여쓰기 수준이 감당하기 힘들어질 정도로 깊어지는 현상이다.
> 이 콜백 헬은 가독성을 나쁘게 하며, 코드 수정이 어려우면서 실수를 유발하는 원인이 된다.

```jsx
step1(function(value1) {
  step2(value1, function(value2) {
     step3(value2, function(value3) {
        step4(value3, function(value4) {
           step5(value4, function(value5) {
                // value5를 사용하는 처리
            });
         });
      });
   });
});
```
#### 콜백 헬이 발생하는 이유
> 비동기 처리 모델은 실행 완료를 기다리지 않고 **즉시** 다음 작업을 실행한다.
> 따라서 비동기함수(비동기를 처리하는 함수)내에서 처리 결과를 반환하면 기대한 대로 동작하지 않는다.

- 만약, 비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야하는 경우, 함수의 호출이 중첩되어 복잡도가 높아지면서 **콜백 헬**이 발생한다.
- 이 콜백 헬은 가독성을 나쁘게하고, 코드의 복잡도를 증가시키면서, 에러를 처리하기가 어려워지는 문제점을 가지고 있다.
## 에러 처리의 한계
> 콜백 방식의 비동기처리가 갖는 문제점 중 가장 심각한 것은 에러 처리가 힘들다는 것이다.

```jsx
try {
  setTimeout(()=>{throw new Error('error!!!!'); }, 1000};
 } catch(error){
  console.log('에러 캐치 불가');
  cosnole.log(error);
}
```
- try 블록 내에서 `setTimeout`함수가 실행되면, 1초 후 콜백함수가 실행되고, 이 콜백함수는 예외를 발생시킨다.
- 하지만, 이 예외는 `catch`블록에서 캐치되지 않는다.
  - 비동기 처리 함수의 콜백 함수는 해당 이벤트가 발생하면 `이벤트 큐(event queue)`로 이동한다.
  - 호출 스택이 비어져있을 때, 호출 스택으로 이동되어 실행되어진다.
  - 여기서 `setTimeout` 함수는 **비동기 함수**라서 콜백함수의 실행 완료를 기다리지 않고, 즉시 종료되어 `호출스택에서 제거`된다.
  - 이후 에러 처리하는 이벤트가 발생하면, `setTimeout` 함수는 이미 호출 스택에서 제거되었기 때문에 이를 통해 콜백 함수를 호출한 것이 `setTimeout`함수가 아니라는 것을 의미한다.
  - 콜백 함수의 호출자가 `setTimeout`함수라면, 호출 스택에 `setTimeout`함수가 존재해야하는데, 그렇지 않기 때문이다.
  - 예외는 호출자 방향으로 전파되지만, 콜백함수를 호출한 것은 `setTimeout`함수가 아니기 때문에, **`catch` 블록에서 캐치되지 않아 해당 프로세스는 종료**된다.
- 이러한 문제를 극복하기 위해 `Promise`가 제안되었다.

##### 콜백 헬 예 (사용자 데이터 가져오는 클래스)
```jsx
class UserStor {
  loginUser(id, pwd, onSuccess, onError){
    // 로그인 실패 메세지 출력 콜백함수
    setTimeout(() => {
      if( 
        (id === 'Oli' && pwd === 'via') ||
        (id === 'Lee' && pwd === 'Gi')
      ) {
        onSuccess(id)
      } else {
        onError(new Error ('사용자를 찾을 수 없습니다'))
      }
    }, 2000)
  } 
 getRoles(user, onSuccess, onError) {
  // 사용자 역할을 요청해서 받아오는 함수
  setTimeout(()=> {
    if(user === 'Oli'){
      onSuccess({name: "Oli", role:'FE'})
    } else {
      onError(new Error('권한이 없습니다))
    }
  }, 1000
 }
};
```
- 이 코드를 가지고, 아이디와 비밀번호를 입력해서 로그인한다.
- 로그인 성공하면 사용자의 아이디를 받아서 그걸 가지고 사용자의 역할을 가져온다.
- 가져오지 못할 경우, 에러 
```jsx
const userStorage = new UserStorage()
const id = prompt('id를 입력하세요');
const pwd = prompt('비밀번호를 입력하세요');

userStorage.loginUser(
  id,
  pwd,
  user => {
    userStorage.getRoles(
      user,
      userWithRole => {
        alert(`hey, ${userWithRole.name}`, 오늘부터 당신은 ${userWithRole.role}입니다`)
      }
    )
  }, (error)=> {
    console.log(error);
  }
);
```
[ 문제점 ] 
- 콜백 함수에서 다른 함수를 호출하고 그 안에서 또 다른 함수를 호출했다.
- 이렇게 되면 결국 `콜백 헬`이 발생하는 것이다.
  - 가독성이 떨어진다.
  - 에러 발생 시 디버깅이 어렵다.
  - 로직을 이해하기 어렵다.
  - 유지 보수가 어렵다.
  
[해결 방법]<br/>
이를 해결하기 위해서 `promise`를 사용한다.

## Promise(프로미스) 생성
> 프로미스는 `Promise` 생성자 함수를 통해 인스턴스화한다.
> `Promise` 생성자 함수는 비동기 작업을 수행 할 콜백 함수를 **인자**로 전달 받는다.
> 이 콜백 함수는 `resolve`와 `reject`함수를 인자로 받는다.

```jsx
// Promise 객체 생성
const promise = new Promise((resolve, reject)=> {
  // 비동기 작업 수행
  if( 비동기 작업 성공) {
    resolve('result');
  } else {
    // 비동기 작업 실패
    reject('실패!!!!');
  }
});
```

`Promise`는 비동기 처리가 `성공(fulfilled)`, `실패(rejected)` 등 상태(state)를 갖는다.
<img src="https://user-images.githubusercontent.com/87024040/212621568-ca931b56-5044-4885-88b2-094c50322193.png">
- `Promise`생성자 함수가 인자로 전달받은 `콜백 함수`는 **내부에서 비동기처리 작업을 수행**한다.
- 비동기 처리가 **성공**하면, 콜백 함수는 인자로 전달받은 `resolve`함수를 호출한다.
- 이때 `Promise`는 `fulfilled`상태가 된다.
- 비동기 처리가 **실패**하면, 콜백 함수는 인자로 전달받은 `reject`함수를 호출한다.
- 이때 `Promise`는 `rejected` 상태가 된다.

## 프로미스의 후속 처리 메소드 (then, catch)
```jsx
function getData(state){
  return new Promise(function (resolve, reject) {
    if(state === "success"){
      resolve("성공입니동~~!");
    } else {
      reject(new Error("실패입니동~~!!!"));
    }
  });
}

getData("success")
  .then(console.log)
  .catch(function (error){
    console.log(error);
  });
  
getData("fail")
  .then(console.log)
  .catch(function(error){
    console.log(error); // Error: Request is Failed
  });
```
- `Promise`로 구현된 비동기 함수는 `Promise`객체를 반환해야한다.
- 반환되는 `Promise`객체는 후속처리 메서드 `then, catch`를 통해 비동기 처리의 **결과, 에러 메세지**를 전달받아 처리한다.
- `Promise` 객체의 상태 정보에 따라 후속처리 메서드를 체이닝 방식으로 호출한다.

```
[ 💡 NOTE  ]
then : then 메소드는 2개의 콜백 함수를 인자로 전달받는다.
        1. 첫번째 콜백 함수 : 성공(fulfilled, resolve함수가 호출된 상태)시 호출
        2. 두번째 콜백 함수 : 실패(rejected, reject 함수가 호출된 상태)시 호출된다.
catch: catch는 예외(비동기 처리에서 발생한 에러와 then메소드에서 발생한 에러)가 발생하면 호출
```
## 프로미스의 에러 처리
- 비동기 함수 `setTime`은 `Promise`객체를 반환한다.
- `Promise` 객체의 후속처리 메서드를 사용하여, 비동기 처리 결과에 대한 후속 처리를 수행한다.
- 비동기 처리시 발생한 **에러** 메서지는 `then`메서드의 **2번째 콜백**함수로 전달된다.
- `promise` 객체의 후속 처리 메소드 `catch`를 사용해도 에러를 처리할 수 있다.

```jsx
function setTime(time){
  return new Promise((resolve, reject) => {
    setTimeout(()=> {
      if(bool){
          resolve("성공입니동~~~!!!!");
      } else {
          reject("에러발생!!!!!");
          throw catchMessage;
      }
    }, time);
  })
}

var bool = false;
var catchMessage = "에 러 발 생";
setTime(1000)
.then(
  result => console.log(result);
  error => console.log(error);
).catch(
  error=> console.log(error);
);
```
- 여기서는 `then`메서드의 두번째 콜백 함수를 실행하지 않고 바로 `catch`메서드를 실행해서 에러를 캐치했다.
- `then` 메서드의 두번째 콜백 함수에서는 **비동기 처리**에서 발생한 **에러(reject 함수가 호출된 상태)만 캐치**한다.
- `catch` 메서드는 **비동기 처리에서 발생한 에러** & **`then`메서드 내부에서 발생한 에러도 캐치**한다.
- 따라서 에러처리는 `catch` 메서드를 사용하는 것이 더 효율적.

#### `Promise`를 활용하여 사용자 데이터 가져오는 클래스에서의 콜백 헬 해결
```jsx
class UserStorage{
  loginUser(id, pwd){
    return new Promise((resolve, reject)=> {
      setTimeout(()=>{
        if((id === "Oli" && pwd === "1004") ||
           (id === "Seulgi" && pwd === "1004")
          ){
            resolve(id);
          } else {
            reject(new Error("에러 에러!!!"));
          }
      }, 2000);
    })
  }
  
  getRole(user){
    return new Promise((resolve, reject) => {
      setTimeout(()=> {
        if(user === "Oli") {
          resolve({name: "Oli", role: "FE" }); 
        } else {
          reject(new Error("에러 발생"));
        }
      }, 1000);
    })
  }
 };
 
 const userStorage = new UserStorage();
 const id = prompt('ID를 입력하세요~");
 const pwd = pormpt("비밀번호를 입력하세요~");
 
 userStorage.loginUSer(id, pwd)
  .then(userStorage.getRoles)
  .then(user=> alert(`hey ${user.name}, 당신은 이제부터 ${user.role} 입니다`))
  .catch(console.log);
```
- 이렇게 `Promise`를 사용하면, 복잡도가 크게 줄어들기 때문에 가독성이 향상된다.
- 그리고 콜백 함수의 한계인 `후속처리`와 `에러처리`를 해결할 수 있다.


## 프로미스 체이닝
> 비동기 함수의 처리 결과를 가지고 다른 비동기 처리를 호출해야할 경우, 
> 함수의 호출이 **중첩**되어, **복잡도가 높아**지는 `콜백 헬`일 발생한다.
> 프로메스는 `후속처리 메서드`를 체이닝하여 여러개의 프로미스를 연결하여 콜백헬을 해결한다.

- `Promise`객체를 반환한 비동기 함수는 프로미스 후속 처리 메서드인 `then`이나 `catch`메서드를 사용할 수 있다.
- `then`메서드가 `Promise`객체를 반환하면, 여러개의 프로미스를 연결해서 사용할 수 있다.

```jsx
var time = 1000;
var bool = true;

function setTime1(time){
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      if(bool) {
        console.log("성 공 ! ! ! ");
        resolve(time -= 200);
      } else {
        console.log("실 패 ! ! ! " );
        reject(time -= 200);
      }
    }, time);
  })
}

function setTime2(time){
  return new Promise((resolve, reject)=> {
    setTimeout(()=>{
      if(bool){
        resolve("성 공 2 ! ! ! ");
      } else {
        reject("실 패 2 ! 1 ! ");
        time -= 200;
      }
    }, time);
  })
}
setTime1(time)
  .then(result1 => setTime2(result1))
  .then(result2 => console.log(result2))
  .catch(error=> setTime2(error))
  .catch(error=> console.log(error));
```
