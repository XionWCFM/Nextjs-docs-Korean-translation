원본링크 : [https://nextjs.org/docs/app/api-reference/next-config-js/compress](https://nextjs.org/docs/app/api-reference/next-config-js/compress)

# compress

Next.js는 렌더링된 콘텐츠와 정적 파일을 압축하기 위해 [gzip](https://tools.ietf.org/html/rfc6713#section-3) 압축을 제공합니다. 일반적으로 [nginx](https://www.nginx.com/)와 같은 HTTP 프록시에서 압축을 활성화하여 `Node.js` 프로세스의 부하를 오프로드하는 것이 좋습니다.

**압축**을 비활성화하려면 `next.config.js`를 열고 `compress` 구성을 비활성화합니다:

```jsx
module.exports = {
  compress: false,
};
```
