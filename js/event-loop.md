# 🙎🏻 이벤트 루프(Event Loop)에 대해 설명해주세요.
`이벤트 루프(Event Loop)`는 자바스크립트 엔진이 싱글 스레드 환경에서 `비동기 작업`을 처리하고 `콜백 함수`를 관리하는 메커니즘입니다.

이벤트 루프는 콜 스택 내에 현재 실행중인 task 가 있는지, 콜백 큐에 task 가 있는지 반복적으로 확인합니다. 만약 콜 스택이 비어있다면 콜백 큐 내의 task 가 콜 스택으로 이동되며, 실행됩니다.
즉, 이벤트 루프는 콜 스택이 비어있을 때 콜백 큐에서 새로운 콜백 함수를 하나씩 가져와 콜 스택에 할당해줍니다.

## 브라우저의 환경
![https://poiemaweb.com/img/event-loop.png](/image/browser-environment.png)
출처 : [poiemaweb event](https://poiemaweb.com/js-event)

구글 V8 을 비롯한 대부분의 자바스크립트 엔진은 크게 2개의 영역으로 나뉩니다.
- `콜 스택(Call Stack)`
- `힙(heap)`

### Call Stack
콜 스택은 현재 실행중인 함수의 정보를 저장하는 자료 구조로 함수의 호출과 반환에 따른 실행 흐름을 추적합니다. 콜 스택은 `LIFO(Last-In, First-Out)`(후입선출) 원칙을 따르기 때문에 순차적으로 콜백함수들을 처리합니다.

### heap
메모리가 할당이 일어나는 영역

### Callback Queue(=Task Queue, Event Queue)
`콜백 큐(Callback Queue)` 는 비동기 처리가 끝난 후 실행되어야 할 콜백 함수가 보관되는 곳으로, 여러 가지 종류의 Queue 를 묶어 총칭하는 개념입니다.

콜 스택이 비워졌을 때 먼저 대기열에 들어온 순서대로 작업이 수행되는 `FIFO(First-In, First-Out)`(선입선출) 원칙을 따릅니다.

콜백 큐의 종류에 따라 이벤트 루프가 콜 스택으로 옮겨지는 순서가 달라집니다.
- `(macro)Task Queue`
  - 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐
  - ex) setTimeout, setInterval, fetch, addEventListener 등
- `Microtask Queue`
  - 우선적으로 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐
  - Task Queue 보다 우선순위가 높음
  - ex) promise.then, process.nextTick, MutationObserver 등

### Web API
Web API 는 자바스크립트 엔진에서 정의되지 않은 메서드를 브라우저에서 지원하는 다양한 API 를 말합니다. 각 API 마다 스레드가 할당되어있고, 이들이 모여 멀티 스레드로 이루어져 있다.
- HTTP 요청 API (XMLHTTPRequest, Ajax, Fetch API 등)
- DOM Event API (click, mouseover, keydown, submit, load, scroll, change 등)
- Timer Event API (setTimeout)
등 (Web Storage API, Canvas API, WebSockets API, ...)

### 이벤트 루프
이벤트 루프는 콜 스택이 비었을 때 콜백 큐에서 콜백 함수를 가져와 콜 스택에 추가하는 방법을 통해 비동기 작업을 처리합니다.

![http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D](/image/event-loop.gif.gif)
출처 : [이벤트 루프 시각화](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

## 동기적으로 호출되는 콜백함수 VS 비동기적응로 호출되는 콜백함수 차이점
동기적으로 호출되는 콜백함수의 경우 함수 내에서 동기적으로 호출되며, 함수의 실행 흐름을 차단하고 기다립니다.

반면 비동기적으로 호출되는 콜백함수의 경우 비동기적으로 작업을 수행하고 작업이 완료되면 콜백함수가 이벤트 루프(Event Loop)를 통해 호출됩니다.

## 참고
- https://medium.com/@momlin069/event-loop%EA%B0%80-%EB%AD%90%EC%95%BC-aadcc488f820
- https://poiemaweb.com/js-event
- https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC#callback_queue%EC%9D%98_%EC%A2%85%EB%A5%98
- https://beomy.github.io/tech/javascript/javascript-runtime/
- https://www.youtube.com/watch?v=QFHyPInNhbo