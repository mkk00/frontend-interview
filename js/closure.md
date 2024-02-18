# 💁🏻 클로저(Closure) 에 대해 설명해주세요.
클로저는 함수가 다른 함수의 외부 변수를 기억하고 그 변수에 접근할 수 있도록 하는 기능입니다.

## MDN 정의
> "A closusre is the combination of a function and the lexical environment within which that function was declared."

클로저란 함수와 그 **함수가 선언된 렉시컬 환경**과의 조합이다.

### 등장 이유
함수형 프로그래밍 언어에서는 함수를 일급 객체로 취급하고 함수를 값으로 다룹니다. 함수가 변수에 `할당`되거나 다른 함수의 `인자`로 전달되며, 이 때 함수와 함수를 감싸는 환경(lexical environment) 사이의 관계가 중요해집니다.

<br/>

## 렉시컬 스코프
렉시컬 스코프란 함수를 어디서 호출했는지가 아닌 **어디에 정의**했는지에 따라 상위 스코프를 정하는 정적 스코프를 말합니다.
상위 스코프에 대한 참조는 함수가 정의된 환경에 의해 결정됩니다.

<br/>

## 클로저를 사용하는 이유
변수를 전역적으로 두지 않고 해당 변수를 함수 외부에서 사용하고자할 때 사용합니다. 즉, `변수를 은닉`하고 외부에서 접근하지 못하도록 하기 위해서 사용합니다.

_변수를 은닉한다는 것의 의미는 해당 변수를 외부에서 직접적으로 접근할 수 없도록 하는 것입니다.
(변수를 은닉하는 방법 : 클로저, 접근 제어자)_

일반적으로 함수 내부에서 선언된 변수는 함수가 실행을 마치면 메모리에서 해제되지만, 클로저를 사용하면 함수가 종료되더라도 해당 변수에 대한 참조가 유지되어 함수 외부에서도 접근할 수 있습니다.


###  전역 변수 선언하는 경우

  전역에서 변수를 선언할 경우 외부에서 직접적으로 접근이 가능하기 때문에 값이 변경될 수 있습니다.
  ```javascript
  let num = 0

  function increase() {
    num++
    console.log(num)
  }

  increase() // 1

  num = 100

  increase() // 101
  ```


### 클로저 사용

클로저를 사용하면 외부에서는 해당 변수에 접근할 수 없습니다.
```javascript
function counter() {
  let num = 0

  function increase() {
    num++
    console.log(num)
  }

  return increase
}

const increaseCounter = counter()
increaseCounter() // 1
increaseCounter() // 2
```

이를 통해 변수가 은닉되고 외부에서 접근할 수 없도록 제어할 수 있는 것입니다.

<br/>

### 클로저를 사용하지 않아도 되는 경우
외부 변수에 의존하지 않는 간단한 함수나 콜백함수를 다룰 때에는 클로저를 사용하지 않아도 됩니다.

ex) 단순한 이벤트 핸들러, 콘솔 로그 출력 함수 등

무분별한 클로저의 사용은 메모리 누수가 발생할 수 있습니다.
클로저는 외부 변수에 대한 참조를 유지하기 때문에 함수가 실행을 마치더라도 해당 변수에 대한 메모리가 해제되지 않습니다.

<br/>

## 클로저 사용 시 주의사항
클로저 함수가 내부 함수를 반환하지 않는다면 외부 변수에 대한 참조가 유지되지 않습니다. 따라서 매번 변수가 초기화되기 때문에 클로저라고 할 수 없습니다.
```javascript
function counter() {
  let num = 0

  function increase() {
    num++
    console.log(num)
  }

  increase()
}

  increase() // 1
  increase() // 1
  increase() // 1
```

반환되는 값이 차례대로 증가되게 하려면 클로저 함수는 내부 함수를 반환해야 합니다.
```javascript
function counter() {
  let num = 0

  function increase() {
    num++
    console.log(num)
  }

  return increase
}

const increaseCounter = counter();
increaseCounter() // 1
increaseCounter() // 2
increaseCounter() // 3
```

<br/>

## 참고
- https://poiemaweb.com/js-closure