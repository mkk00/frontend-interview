# 💁🏻‍♂️ Javascript 는 어떤 언어입니까?
`Javascript` 는 기본적으로 `싱글 스레드` 기반의 언어로 `이벤트 루프(Event Loop)` 를 통해 비동기 작업을 지원합니다. 비동기 작업은 `논 블로킹(Non-Blocking)` 방식으로 처리됩니다.

- 싱글 스레드(Single thread) : 한번에 하나의 작업만 할 수 있습니다.
- 이벤트 루프(Event Loop) : 이벤트 큐에 있는 작업들을 순서에 맞게 처리하고, 호출 스택이 비워졌을 때 이벤트 큐에서 작업을 가져와 실행하는 방식으로 동작합니다.
- 논 블로킹(Non-Blocking) : 비동기 작업을 처리할 때 호출 스택을 차단하지 않고(다른 코드의 실행을 막지 않고도) 진행될 수 있다는 것을 의미합니다.

[이벤트 루프에 대해](/js/event-loop.md)
[동기 VS 비동기에 대해](/js/synchronous-asynchronous.md)
[블로킹 VS 논 블로킹에 대해](/js/blocking-non-blocking.md)

## Javascript 의 특징
- `싱글 스레드`
- `비동기`
- `동시성`
- `논 블로킹 I/O`

### 싱글 스레드이면서 동시성을 가질 수 있는 이유
비동기, 논 블로킹 작업들은 Javascript 의 런타임 환경에서 담당합니다. 하지만 런타임 자체에서 비동기 API 를 실행하는 것은 틀린 말입니다.

Javascript 는 비동기 작업들을 Web API 에 넘겨줌으로써 해당 작업이 완료될 때까지 다르코드를 실행할 수 있습니다.(=논 블로킹)

### 비동기 코드가 실행되는 과정
1. 함수가 `콜 스택`에 쌓인 후 실행
2. Javascript 엔진이 해당 비동기 작업을 `Web API` 에 위임
3. Web API 가 `비동기 작업` 수행한 후
4. 콜백 함수를 `이벤트 루프`를 통해 `태스크 큐`에 넘겨줌
5. 이벤트 루프는 콜 스택에 쌓여 있는 함수가 없을 경우 태스크 큐에서 대기하고 있던 콜백함수를 콜 스택으로 넘겨줌
6. 콜 스택에 쌓인 콜백 함수가 실행되며, 이후 콜스택에서 제거됨

<br/>

## 참고
- https://medium.com/@vdongbin/javascript-%EC%9E%91%EB%8F%99%EC%9B%90%EB%A6%AC-single-thread-event-loop-asynchronous-e47e07b24d1c