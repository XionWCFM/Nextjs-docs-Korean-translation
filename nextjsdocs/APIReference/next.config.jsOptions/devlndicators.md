원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/devIndicators

<br>

# **devIndicators**

코드를 편집하고 Next.js가 애플리케이션을 컴파일하는 경우 페이지 오른쪽 하단에 컴파일 표시기가 나타납니다.

> Note: 이 표시기는 개발 모드에서만 표시되며 프로덕션 모드에서 앱을 빌드하고 실행할 때는 표시되지 않습니다.

<br>

채팅 런처와 충돌하는 경우와 같이 이 표시기가 페이지에서 잘못 배치될 수 있습니다. 위치를 변경하려면 `next.config.js`를 열고 `devIndicators` 객체에서 `buildActivityPosition`을 `bottom-right`(기본값), `bottom-left`, `top-right` 또는 `top-left`으로 설정하세요:

next.config.js

```jsx
module.exports = {
  devIndicators: {
    buildActivityPosition: "bottom-right",
  },
};
```

<br>

경우에 따라 이 표시기가 유용하지 않을 수 있습니다. 이를 제거하려면 `next.config.js`를 열고 `devIndicators` 객체에서 `buildActivity` 구성을 비활성화하세요:

next.config.js

```jsx
module.exports = {
  devIndicators: {
    buildActivity: false,
  },
};
```
