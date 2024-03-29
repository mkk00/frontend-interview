# 💁🏻‍♀️ 동기 방식(Synchronous)과 비동기 방식(Asynchronous)의 차이점을 설명해주세요.
`동기(Synchronous)`, `비동기(Asynchronous)`는 작업에 대한 순차적 흐름의 유무의 차이를 나타냅니다.

서버의 요청에 대한 응답이 오래 걸리는 작업을 동기적으로 처리할 경우 대기 시간이 길어지게 됩니다. 이때 비동기적으로 처리하면 응답을 기다리지 않고 다음 작업을 수행하 작업이 끝날 때까지 기다리지 않아도 됩니다.

하지만 비동기 로직 이후에 실행되는 콜백함수가 많아지면 콜백 지옥이 발생하여 가독성과 유지보수에 좋지 않을 수 있습니다. 이를 해결하기 위해 비동기 함수를 사용합니다.

[이벤트 루프에 대해](/js/event-loop.md)

### 어원
Syn 은 그리스어로 '함께' 라는 뜻이고 chrono 는 '시간' 이라는 뜻입니다. 즉, `Synchronous` 는 작업 시간을 함께 맞춰 실행한다는 뜻입니다.

`Asynchrounous` 는 Synchronous 와 반대로 요청한 작업에 대해 완료 여부를 따지지 않아 다음 작업을 그대로 수행하게 됩니다.

### 전통적인 비동기 처리 방식: `콜백 함수`
콜백함수는 호출하는 비동기 함수의 깊이가 깊어질 수록 `에러 핸들링`이 까다롭습니다.
콜 스택 기준으로 안쪽 스택에서 발생한 에러는 바깥쪽 호출자 방향으로 전파됩니다. 그러나 함수의 깊이가 깊어지면 안쪽 콜 스택이 먼저 제거되어 에러가 바깥쪽까지 전파되지 않을 수 있습니다.

또한 콜백 함수를 중첩해서 호출하다보면 `콜백 지옥(Callback Hell)`이 발생할 수 있습니다.

- ES6 : Promise 객체 도입
- ES8 : async/await 구문 도입

### 모던 브라우저의 비동기 로직 처리 방식: `이벤트 루프`
모던 브라우저는 비동기 작업을 [`이벤트 루프`](/js/event-loop.md)와 `Web API`(Promise, async/await, fetch 등)를 통하여 자바스크립트의 싱글 스레드 특성을 유지하면서도 비동기 작업을 효율적으로 처리합니다.

<br/>

## 동기(Synchronous)
- 서버에서 요청을 보낸 후 응답을 받아야만 다음 동작이 이루어집니다.
  - 즉, 한 작업이 `순치적`으로 완료되기를 기다린 후에 다음 작업을 실행되는 방식입니다.
- 동기적 방식은 작업들 간의 순서가 중요한 경우에 유용합니다.

```javascript
function synchronousFunc() {
  console.log("안녕하세요?")
  console.log("반가워요.")
  console.log("다음에 또 봐요.")
}
```

<br/>

## 비동기(Asynchronous)
- 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다른 코드를 먼저 실행하는 방식입니다.
  - 작업이 백그라운드에서 비동기적으로 실행됩니다.
  - 즉, 작업의 완료를 기다리지 않고 다음 코드가 실행됩니다.
- I/O 작업(입력 및 출력 작업)을 처리할 때 유용합니다.
  - 네트워크 요청, 콜백함수, Promise, async/await 등

```javascript
function asynchronous() {
  console.log("안녕하세요?")

  setTimeout(() => {
    console.log("반가워요.")
    callback()
  }, 1000)

  function callback() {
    console.log("청주 날씨는 어때요?")
  }
}
asynchronous()
console.log("다음에 또 봐요.")
```

<br/>

## 동기/비동기, 블로킹/논블로킹 차이
- 동기/비동기 : 작업에 대한 순차적 흐름의 유무(수행 시점)
- 블로킹/논블로킹 : 작업의 흐름 자체를 Block 하는가, 안 하는가의 유무(제어권)

## 참고
- https://poiemaweb.com/js-async
- https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC#blocking_/_non-blocking
- https://yozm.wishket.com/magazine/detail/1982/