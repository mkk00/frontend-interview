# 🙋🏻 이벤트 전파(버블링, 캡처링)에 대해 설명해주세요.
이벤트 전파는 HTML 요소에 이벤트가 발생할 경우 DOM 트리를 따라 연쇄적으로 이벤트가 전파되는 과정을 말합니다.
- `이벤트 버블링(Event Bubbling)`이란 이벤트가 발생한 요소부터 시작하여 상위 요소로 이벤트가 전파되는 현상을 말합니다.
- `이벤트 캡처링(Event Capturing)` 이란 최상위 부모 요소부터 시작하여 이벤트가 발생한 요소로 이벤트가 전파되는 현상을 말합니다.

## 이벤트 전파 흐름
버블링과 캡처링은 둘 중 하나만 발생하는 것이 아니라 캡처링부터 시작하여 버블링으로 종료한다는 것을 주의해야합니다. 즉, 다음 단계가 순차적으로 발생합니다.
- 캡처링 단계
- 타깃 단계
- 버블링 단계

<br/>

## 이벤트 전파 방지 방법
이벤트 전파 방지는 꼭 필요한 경우가 아니라면 사용하지 않는 것이 좋습니다. 서비스 내에서 이용자의 클릭 행동 패턴을 분석할 경우 이벤트 전파 방지된 부분은 Dead Zone 이 되어 분석이 제대로 되지 않을 수도 있기 때문입니다.
### stopPropagation()
이벤트 전파를 막기 위해서 `stopPropagation()` 이라는 웹 API 를 사용할 수 있습니다.

```javascript
function InnerFunc(event) {
  event.stopPropagation()
}
```

### e.preventDefault()
`e.preventDefault()` 는 이벤트 전파 뿐만 아니라 기본 이벤트 동작 자체를 취소합니다.

### e.stopImmediatePropagation()
e.stopImmediatePropagation() 는 이벤트 전파는 물론이고 요소에 할당된 다른 핸들러의 동작을 막을 수 있습니다.

<br/>

## 이벤트 위임(Event Delegation)
이벤트 위임은 다수의 자식 요소에 각각 이벤트 핸들러를 바인딩하는 대신, 하나의 부모 요소에 이벤트 핸들러를 바인딩하는 방법입니다. 즉, 상위 요소에 이벤트 리스터를 추가하여 하위 요소의 이벤트를 관리하는 패턴입니다.

실제로 이벤트를 일으킨 요소를 알아내기 위해 `event.target` 을 사용합니다. event.target 은 실제 이벤트가 걸린 DOM 엘리먼트 객체이고, `tagName`, `className`, `id` 속성을 통해 태그의 정보를 분류할 수 있습니다.
_<div style="font-size: 0.7rem; color: #20C997">* tagName은 이벤트가 발생한 요소의 태그 이름을 나타내는 프로퍼티로 대문자로 반환됩니다.</div>_
```html
<body>
  <ul id="parent-list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
    <li>Item 4</li>
    <li>Item 5</li>
  </ul>

  <script>
    const parentList = document.getElementById("parent-list")
    function clickItem(evnet) {
      if (event.target.tagName === "LI") {
        console.log("Clicked on : ", event.target.textContent)
      }
    }
    parentList.addEventListener("click", clickItem)
  </script>
</body>
```

### event 객체
각 핸들러는 아래와 같은 event 객체의 프로퍼티에 접근 가능합니다.
- `event.target` : 이벤트가 발생한 가장 안쪽의 요소
- `event.currentTarget`(=`this`) : 이벤트를 핸들링하는 현재 요소(=핸들러가 할당된 요소)
- `event.eventPhase` : 이벤트 흐름 단계(1: 캡처링, 2: 타깃, 3: 버블링)

<br/>

event.target 과 this 는 다음과 같은 차이가 있습니다.
- event.target 은 실제 이벤트가 시작된 타깃 요소로 버블링이 진행되어도 변하지 않습니다.
- this 는 현재 요소로 현재 실행중인 핸들러가 할당된 요소를 참조합니다.

### 이벤트 중첩을 피하는 방법
이벤트 중첩을 피하기 위해 이벤트 위임을 사용할 수 있습니다.

이벤트 중첩은 여러 HTML 요소가 중첩되어 있는 상황에서 각 요소에 이벤트가 중첩되어 발생하는 것을 의미합니다.

- stopPropagation() 메서드
- addEventListener() 의 세 번째 매개변수 사용
  - 세 번째 매개변수에 {capture: true} 를 전달하여 이벤트 캡처링 활성화.
    - addEventListener 메소드의 세번째 매개변수에 false 또는 미설정하면 버블링이 활성화 됨
  - 이벤트를 캡처링 단계에서 처리하여 버블링 막는 것
- 이벤트 위임(Event Delegation)

<br/>

## React 에서 이벤트 위임을 효과적으로 구현하기 위한 방법?
React의 경우 모든 이벤트가 이벤트 위임 처리되고 있습니다. 즉, React 에서는 `on + 이벤트` 로 사용한 이벤트가 DOM 노드에 할당되는 것이 아닌, document 레벨에서 이루어지고 있습니다.

따라서 `ref` 를 이용해 DOM 에 접근하여 이벤트 위임을 구현하는 것은 사실상 이점이 없습니다.

### 효율적으로 이벤트 핸들러 바인딩하기

다음의 코드는 이벤트 위임이 발생하지 않았고, 3000개의 이벤트 리스너가 생성됩니다. 각각의 요소마다 개별적으로 이벤트 리스너를 생성하면 메모리 사용량이 증가하고 성능이 저하될 수 있습니다.

```javascript
// selectedItems = new Set([1, 2, 3])
// ids = [0, 1, 2, ..., 2999]
{ids.map((id) => (
  <FancyButton
    key={id}
    id={id}
    label={id}
    isSelected={selectedItems.has(id)}
    onClick={() => handleClick(id)}
  />
))}
```

<br/>
다음의 코드는 부모 요소에 하나의 이벤트 리스너를 사용하고 있습니다. 클릭된 요소를 식별하여 처리하는 방법으로 이벤트를 관리하고 있습니다.

```javascript
const handleClick = (event) => {
  const id = event.target.id
  setSelectedItems((prevState) => new Set([...prevState, id]))
}
<div className="container" onClick={handleClick}>
  {ids.map((id) => (
    <FancyButton
      key={id}
      id={id}
      label={id}
      isSelected={selectedItems.has(id)}
    />
  ))}
</div>
```
이 방법은 요소가 동적으로 추가되는 경우에도 효율적으로 이벤트를 처리할 수 있습니다. 이를 통해 코드의 가독성을 높이고 메모리 사용을 줄여 성능을 향상시킬 수 있습니다.

<br/>

## 참고
- https://ko.javascript.info/bubbling-and-capturing
- https://poiemaweb.com/js-event
- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B2%84%EB%B8%94%EB%A7%81-%EC%BA%A1%EC%B3%90%EB%A7%81#%F0%9F%93%8C_e.target_%EC%9C%BC%EB%A1%9C_%EC%A1%B0%EA%B1%B4_%EA%B1%B8%EC%96%B4_%EB%B0%A9%EC%A7%80
- https://dev.to/thawsitt/should-i-use-event-delegation-in-react-nl0