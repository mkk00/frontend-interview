# 🤷🏻 마이크로태스크 큐(Microtask queue)와 태스크 큐(macroTask queue)에 대해 설명해주세요.
`이벤트 의 일반적인 비동기 작업을 처리합니다.루프(Evnet Loop)`는 비동기 작업을 처리하고 실행 순서를 관리하는 메커니즘입니다. 이벤트 루프는 여러 종류의 큐를 사용하여 작업을 관리합니다. 이 중에서 가장 중요한 것이 마이크로태스크 큐와 태스크 큐입니다.

## 마이크로태스크 큐(Microtask)
`마이크로태스크 큐(Microtask)`는 `Promise 기반`의 비동기 작업을 처리합니다. Promise 의 콜백함수나 `then`/`catch`/`finally` 메서드를 호출할 때 등록한 콜백함수가 마이크로태스크 큐에 들어가게 됩니다.

마이크로태스크 큐에 있는 작업(콜백함수)은 현재 실행중인 작업이 완료된 직후에 처리됩니다. 즉, 우선순위가 태스크 큐보다 높습니다.

## 태스크 큐(macroTask queue)
`태스크 큐(macroTask queue)`는 setTimeout, setInterval, DOM 이벤트 핸들러 등

이벤트 루프는 마이크로태스크 큐를 먼저 확인한 후 태스크 큐를 확인하여 실행중인 작업이 완료된 후에 실행됩니다.

<br/>

## 우선순위 예시

```javascript
console.log("시작")

// 태스크 큐에 등록된 콜백 함수
setTimeout(() => {
  console.log("태스크 큐")
}, 0)

// 마이크로태스크 큐에 등록된 콜백 함수
Promise.resolve().then(() => {
  console.log("마이크로태스크 큐")
})

console.log("끝")
```

<br/>

실행 결과 :
```
시작
끝
마이크로태스크 큐
태스크 큐
```