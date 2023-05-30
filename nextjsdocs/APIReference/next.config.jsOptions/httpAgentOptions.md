번역 완료 : https://nextjs.org/docs/app/api-reference/next-config-js/httpAgentOptions

<br>

# **httpAgentOptions**

18 이전 Node.js 버전에서는 Next.js가 자동으로 `fetch()`를 [node-fetch](https://nextjs.org/docs/architecture/supported-browsers#polyfills)로 폴리필링하고 기본적으로 [HTTP Keep-Alive](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Keep-Alive)를 활성화합니다.

<br>

서버 측의 모든 `fetch()` 호출에 대해 HTTP Keep-Alive를 비활성화하려면 `next.config.js`를 열고 `httpAgentOptions` 구성을 추가합니다:

next.config.js

```jsx
module.exports = { httpAgentOptions: { keepAlive: false } };
```
