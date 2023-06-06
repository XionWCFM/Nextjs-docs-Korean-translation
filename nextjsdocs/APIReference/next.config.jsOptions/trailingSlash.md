# 후행 슬래시(trailingSlash)

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/trailingSlash](https://nextjs.org/docs/app/api-reference/next-config-js/trailingSlash)

기본적으로 Next.js는 URL 끝에 슬래시가 있는 경우 슬래시가 없는 대응 URL로 리디렉션합니다. 예를 들어, `/about/`은 `/about`으로 리디렉션됩니다. 이 동작을 반대로 설정하여 슬래시가 없는 URL이 슬래시가 있는 대응 URL로 리디렉션되도록 구성할 수 있습니다.

`next.config.js` 파일을 열고 `trailingSlash` 구성을 추가합니다.

```jsx
// next.config.js

module.exports = {
  trailingSlash: true,
};
```

이 옵션을 설정하면 `/about`과 같은 URL은 `/about/`로 리디렉션됩니다.

<br><hr><br>

## 버전 히스토리

| Version  | Changes              |
| -------- | -------------------- |
| `v9.5.0` | `trailingSlash 추가` |
