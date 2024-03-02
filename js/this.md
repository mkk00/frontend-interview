# this 에 대해 설명해주세요.

### MDN 정의
> 대부분의 경우 this 의 값은 함수를 호출한 방법에 의해 결정됩니다.
- ES5 bind() : 함수를 어떻게 호출했는지 상관하지 않고 this 값 설정 가능
- ES2015 arrow function : this 바인딩을 제공하지 않고 렉시컬 컨텍스트 안의 this 값 유지

```javascript
console.log(typeof this) // 'object'
```

## this 의 결정
- [함수 호출](#1-함수-호출)
- [메소드 호출](#2-메소드-호출)
- [프로토타입](#3-프로토타입)

### 1. 함수 호출

기본적으로 this 는 전역 객체(`Global object`)에 바인딩됩니다.

```javascript
function outer() {
  console.log("outer's this", this) // window

  function inner() {
    console.log("inner's this", this) // window
  }
  inner() // ✅
}
outer() // ✅
```

- 전역 함수의 this → window
- 전역 함수의 내부 함수의 this → window
<br/>

메소드의 내부 함수 또는 콜백함수의 내부 함수인 경우도 마찬가지로 전역 객체에 바인딩됩니다.

```javascript
let outerValue = "value";

const myObj = {
  outerValue: "outerValue",
  func: function() {
    console.log(this) // myObj

    setTimeout(function () { // ✅
      console.log(this) // window
    }, 1500)

    function innerFunc () {
      console.log(this) // window
    }
    innerFunc() // ✅
  }
}
myObj.func() // ✅
```

<br/>

즉, 내부 함수는 어디에서 선언되었든 this 는 전역객체를 바인딩합니다.

전역객체를 바인딩하는 것을 회피하기 위해서는 화살표 함수 사용 또는 this 를 명시적으로 바인딩하는 메소드를 이용하면 됩니다.
- Arrow function
- `apply`, `call`, `bind`

<br/>

### 2. 메소드 호출
메소드 내부의 this 는 해당 메소드를 호출한 객체에 바인딩됩니다.

```javascript
let myObj1 = {
  name: "kim",
  sayHi: function() {
    console.log(this.name)
  }
}

let myObj2 = {
  name: "choi"
}

myObj2.sayHi = myObj1.sayHi

myObj1.sayHi() // ✅ kim
myObj2.sayHi() // ✅ choi
```

### 3. 프로토타입
프로토타입 객체 메소드 내부에서 사용된 this도 해당 메소드를 호출한 객체에 바인딩됩니다.
```javascript
function UserInfo(user) {
  this.user = user;
}

UserInfo.prototype.getUser = function() {
  return this.user;
}

let newUser = new UserInfo('user2024');
console.log(newUser.getUser()); // ✅ user2024

UserInfo.prototype.user = 'user_Kim';
console.log(UserInfo.prototype.getUser());  // ✅ user_Kim
```

<br/>

## 명시적 바인딩
### apply 와 call
apply 와 call 은 함수를 `호출`하는 동시에 this를 특정 객체에 바인딩합니다.

- `apply()` : 함수의 매개변수들을 배열 담아 전달하는 경우
- `call()` : 함수의 매개변수들을 각각 하나의 인자로 전달하는 경우

```javascript
  func.apply(thisArg, [argsArray])
  func.call(thisArg[, arg1[, arg2[, ...]]])
```

```javascript
  const obj = {
    name: "kim"
  }

  function callName(myName) {
    return myName + this.name
  }

  callName.apply(obj, ["My name is "]) // My name is kim
  callName.call(obj, "My name is ") // My name is kim
```

### bind
apply, call 메소드는 함수를 호출할 때만 this 를 바인딩합니다.

반면 `bind` 는 함수를 호출하지 않으며, this 특정 객체에 <u>영구적</u>으로 바인딩합니다.영구적으로 바인딩한다는 것은 원본 함수의 호출과는 상관없이 바인딩된 this 를 사용한다는 것을 의미합니다.

bind 는 새로운 함수를 생성하며 변수에 할당하여 사용합니다.

```javascript
  const obj = {
    name: "Lee"
  };

  function callName() {
    return this.name
  }

  const callNameAgain = callName.bind(obj);

  callNameAgain() // ✅ Lee
```

<br/>

## 화살표 함수
화살표 함수는 this 를 자신이 생성될 때의 컨텍스트에 this 를 바인딩합니다.
```javascript
  const obj = {
    name: "choi",
    getName: function() {
      const innerFunc = () => {
        return this.name;
      };
      return innerFunc();
    }
  };

  console.log(obj.getValue()); // 출력: 42
```

<br/>

## 참고
- https://poiemaweb.com/js-this