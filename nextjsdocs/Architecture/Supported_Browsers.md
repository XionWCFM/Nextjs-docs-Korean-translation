# Supported Browsers

<br>

Next.js는 설정이 필요 없는 최신 브라우저를 지원합니다.

<br>

- Chrome 64+
- Edge 79+
- Firefox 67+
- Opera 51+
- Safari 12+

<br>

---

<br>

## Browserslist

<br>

특정 브라우저나 기능을 타겟팅하려는 경우 Next.js는 `package.json` 파일에서 [Browserslist](https://browsersl.ist/) 구성을 지원합니다. Next.js는 기본적으로 다음 Browserslist 구성을 사용합니다:

```json
{
  "browserslist": [
    "chrome 64",
    "edge 79",
    "firefox 67",
    "opera 51",
    "safari 12"
  ]
}
```

<br>

---

<br>

## Polyfills

<br>

다음을 포함하여 [널리 사용되는 폴리필](https://github.com/vercel/next.js/blob/canary/packages/next-polyfill-nomodule/src/index.js)을 주입합니다:

- [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) — Replacing: `whatwg-fetch` and `unfetch`.
- [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) — Replacing: the [url package (Node.js API)](https://nodejs.org/api/url.html).
- [Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) — Replacing: `object-assign`, `object.assign`, and `core-js/object/assign`.

<br>

종속성에 이러한 폴리필이 포함된 경우 중복을 방지하기 위해 프로덕션 빌드에서 자동으로 제거됩니다.

<br>

또한 번들 크기를 줄이기 위해 Next.js는 이러한 폴리필이 필요한 브라우저에 대해서만 폴리필을 로드합니다. 전 세계 대부분의 웹 트래픽은 이러한 폴리필을 다운로드하지 않습니다.

<br>

### Custom Polyfills

자체 코드 또는 외부 npm 종속성에서 대상 브라우저에서 지원하지 않는 기능(예: IE 11)이 필요한 경우 직접 폴리필을 추가해야 합니다.

이 경우 [사용자 정의 <App>](https://nextjs.org/docs/pages/building-your-application/routing/custom-app) 또는 개별 컴포넌트에서 필요한 특정 폴리필에 대한 최상위 임포트를 추가해야 합니다.

<br>

---

<br>

## JavaScript Language Features

<br>

Next.js를 사용하면 최신 JavaScript 기능을 바로 사용할 수 있습니다. [ES6 기능](https://github.com/lukehoban/es6features) 외에도 Next.js는 다음과 같은 기능도 지원합니다:

<br>

- [Async/await](https://github.com/tc39/proposal-async-await) (ES2017)
- [Object Rest/Spread Properties](https://github.com/tc39/proposal-object-rest-spread) (ES2018)
- [Dynamic import()](https://github.com/tc39/proposal-dynamic-import) (ES2020)
- [Optional Chaining](https://github.com/tc39/proposal-optional-chaining) (ES2020)
- [Nullish Coalescing](https://github.com/tc39/proposal-nullish-coalescing) (ES2020)
- [Class Fields](https://github.com/tc39/proposal-class-fields) and [Static Properties](https://github.com/tc39/proposal-static-class-features) (part of stage 3 proposal)
- and more!

<br>

### Server-Side Polyfills

클라이언트 측의 `fetch()` 외에도 Next.js는 Node.js 환경에서 `fetch()`를 폴리필합니다. `isomorphic-unfetch` 또는 `node-fetch`와 같은 폴리필을 사용하지 않고도 서버 코드(예: `getStaticProps` / `getServerSideProps`)에서 `fetch()`를 사용할 수 있습니다.

<br>

### TypeScript Features

Next.js에는 TypeScript 지원이 내장되어 있습니다. [여기에서 자세히 알아보세요.](https://nextjs.org/docs/pages/building-your-application/configuring/typescript)

<br>

### Customizing Babel Config (Advanced)

babel 구성을 사용자 지정할 수 있습니다. [여기에서 자세히 알아보세요.](https://nextjs.org/docs/pages/building-your-application/configuring/babel)
