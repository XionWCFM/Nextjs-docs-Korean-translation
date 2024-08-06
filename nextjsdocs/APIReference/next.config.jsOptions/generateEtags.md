원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/generateEtags

<br>

# **generateEtags**

Next.js는 기본적으로 모든 페이지에 대해 [etags](https://en.wikipedia.org/wiki/HTTP_ETag)를 생성합니다. 캐시 전략에 따라 HTML 페이지에 대한 etag 생성을 비활성화할 수 있습니다.

<br>

`next.config.js`를 열고 `generateEtags` 옵션을 비활성화합니다:
next.config.js

```jsx
module.exports = {
  generateEtags: false,
};
```
