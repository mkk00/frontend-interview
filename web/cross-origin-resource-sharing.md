# 💁🏻 CORS에 대처하는 방법에 대해 설명해주세요.
`CORS(Cross-Origin-Resource-Sharing)` 는 `교차 출처 리소스 공유` 로 서로 다른 출처라도 리소스 요청, 응답을 허용 또는 비허용할 수 있도록 하는 정책입니다.

예전에는 모든 처리가 같은 도메인 안에서 가능했으나, 클라이언트와 API 는 다른 도메인인 경우가 많아짐에 따라 CORS 정책이 등장했습니다.

CORS 이 필요한 이유는 이러한 제약이 없다면 해커가 `CSRF(Cross-Site Request Forgery)`, `XSS(Cross-Site Scripting)` 등의 방법으로 웹페이지에서 악의적인 코드를 실행하여 개인 정보를 가로챌 수 있기 때문입니다.

### 출처(origin) 를 구성하는 요소
출처를 구성하는 요소는 다음과 같습니다. 이 중 하나라도 다르면 CORS 에러를 만나게 됩니다.
- 프로토콜
- 도메인(호스트 이름)
- 포트

### 예시
`https:(=Protocol)` //` www.myBlogPage.com(=Hostname)` : `3000(=Port)` / `review/write(=Pathname)` `?sort=asc...(=Search)` `#hash(=Hash)`
- 도메인(Hostname) : myBlogPage.com
- 출처(Origin) : https://www.myBlogPage.com:3000

<br/>

![https://docs.tosspayments.com/resources/glossary/cors#cors%EA%B5%90%EC%B0%A8-%EC%B6%9C%EC%B2%98-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EA%B3%B5%EC%9C%A0](/image/tosspayments-cors-url-img.png)
출처 : [토스페이먼츠 개발자센터 포스팅 : CORS(교차 출처 리소스 공유)](https://docs.tosspayments.com/resources/glossary/cors#cors%EA%B5%90%EC%B0%A8-%EC%B6%9C%EC%B2%98-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EA%B3%B5%EC%9C%A0)

<br/>

## CORS 에러 대응 방법
### 서버에서 `Access-Control-Allow-Origin` 응답 헤더 세팅하기
서버에서 HTTP 응답 헤더를 설정해서 요청을 수락할 출처를 명시적으로 지정할 수 있습니다.

```javascript
'Access-Control-Allow-Origin' : https://naver.com
```

`*` 을 설정하면 출처에 상관없이 모든 리소스에 접근할 수 있는 와일드카드이기 때문에 보안에 취약해집니다.

### 프록시 서버 사용하기
리소스를 직접 요청하는 대신 `프록시 서버`를 이용하여 웹 애플리케이션에서 리소스로의 요청을 전달하는 방법을 통해 CORS 에러를 방지할 수 있습니다.

`프록시(Proxy)`란 클라이언트와 서버 사이의 중간에 위치하여 클라이언트의 요청을 대신 받아 서버로 전달하거나, 서버의 응답을 받아서 클라이언트로 전달하는 시스템을 말합니다.

다만, 프록시 서버를 직접 운영해야 합니다. 무료 서비스는 악용되는 사례가 많기 때문에 실전에서 사용할 수 없습니다.

### Chrome 확장 프로그램
`Allow CORS: Access-Control-Allow-Origin` 크롬 확장 프로그램을 설치하면 로컬 환경에서 API 테스트 시 CORS 문제를 해결할 수 있습니다.

[Allow CORS: Access-Control-Allow-Origin 바로가기](https://chromewebstore.google.com/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf?pli=1)

<br/>

## Next.js CORS 에러
Next.js 에서는 `next.config.js` 파일에서 `Rewrite` 기능을 사용해 프록시 설정을 대체할 수 있습니다. Rewrite 는 특정 URL로의 요청을 서버 측에서 처리하도록 설정할 수 있습니다.

```javascript
//next.config.js
async rewrites() {
  return [
    {
      source: '/api/:path*',
      destination: `${baseURL}/:path*`
    }
  ]
}
...
```

<br/>

## 출처
- https://docs.tosspayments.com/resources/glossary/cors#cors%EA%B5%90%EC%B0%A8-%EC%B6%9C%EC%B2%98-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EA%B3%B5%EC%9C%A0
- https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F