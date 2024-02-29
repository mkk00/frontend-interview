# 💁🏻‍♂️ 브라우저의 렌더링 원리에 대해 설명해주세요.

브라우저는 사용자의 요청에 따라 서버로부터 HTML, CSS, Javascript 등을 응답받습니다. HTML, CSS 파일은 렌더링 엔진의 HTML parser, CSS parser 에 의해 파싱(Parsing)되어 DOM Tree, CSSOM Tree 로 변환됩니다. 이후 DOM, CSSOM 을 조합하여 Render Tree 를 구축합니다. 브라우저는 이렇게 생성된 Render Tree 를 기반으로 웹페이지를 출력하게 됩니다.

## 브라우저의 구조
### 사용자 인터페이스
### 브라우저 엔진
자료 저장소에 캐싱된 데이터를 찾거나 렌더링 엔진 에 URI 값 전달합니다.

### 렌더링 엔진
; Blink(크롬), Webkit(사파리), Gecko(파이어폭스)

렌더링 엔진은 CRP(Critical Rendering Path) 프로세스를 거쳐 웹 페이지를 표시합니다.
1. **HTML 파싱 : `DOM Tree` 생성**
2. **CSS 파싱 : `CSSOM Tree` 생성**
3. **Javascript 실행**
    - 중간에 script 코드 있을 경우 HTML 파싱 중단됨!
4. **`렌더 트리(Render Tree)` 생성**
    - DOM + CSSOM
5. **렌더 트리 배치(레이아웃)**
6. **렌더 트리 페인팅**
<br/>

## script 의 위치
Javascript 코드는 <u>body 요소의 가장 아래</u>에 위치시키는 것이 가장 좋습니다.

### DOM 생성의 지연
Javascript 는 렌더링 엔진이 아닌 `자바스크립트 엔진(V8)`이 처리합니다.

HTML parser 는 script 태그와 만나면 DOM 생성 프로세스를 중지합니다. 즉, 렌더링 엔진에서 자바스크립트 엔진으로 제어 권한이 넘어가게 됩니다. Javascript 의 실행이 완료되면 다시 렌더링 엔진으로 넘어가 DOM 생성을 재개합니다.

또한, DOM 생성이 완료되지 않은 상태에서 Javascript 가 DOM 을 조작하면 에러를 발생시킬 수 있습니다. 또한 script 로딩 지연은 DOM 에 영향을 미칠 수 있습니다.

### 어쩔 수 없이 script 태그가 하단에 위치하지 않는다면?
만약 script 태그가 상단 부분에 위치하게 된다면 `defer`, `async` 문법을 통해서 동기적으로 처리할 수 있습니다.

### 인라인 스타일을 지양해야하는 이유
- 성능 저하
  - 리액트의 경우 불필요한 리렌더링이 발생할 수 있습니다.
- 재사용성 저하
- 유지보수성 저하
- 유연성 및 가독성 저하

## 참고
- https://poiemaweb.com/js-browser
- https://velog.io/@gonasooc/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%9B%90%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94
- https://ramincoding.tistory.com/entry/Web-%EC%9B%B9-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EA%B0%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80
- https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/browser-rendering.md