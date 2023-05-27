원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/distDir

<br>

# **distDir**

`.next` 대신 사용할 사용자 지정 빌드 디렉터리에 사용할 이름을 지정할 수 있습니다.

<br>

`next.config.js`를 열고 `distDir` 구성을 추가합니다:

next.config.js

```jsx
module.exports = {
  distDir: "build",
};
```

이제 `next build`를 실행하면 Next.js가 기본 `.next` 폴더 대신 `build`를 사용합니다.

> `distDir`은 프로젝트 디렉터리를 벗어나지 않아야 합니다. 예를 들어, `../build`는 잘못된 디렉토리입니다.
