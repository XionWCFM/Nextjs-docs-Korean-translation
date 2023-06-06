# 런타임 구성

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/runtime-configuration](https://nextjs.org/docs/app/api-reference/next-config-js/runtime-configuration)

> Note : 이 기능은 레거시로 간주되며, [자동 정적 최적화(Automatic Static Optimization)](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization), [출력 파일 추적(Output File Tracing)](https://nextjs.org/docs/pages/api-reference/next-config-js/output#automatically-copying-traced-files), 또는 [React 서버 컴포넌트](../../GettingStarted/React_Essentials.md#server-components)와 함께 작동하지 않습니다. 초기화 오버헤드를 피하기 위해 [환경 변수](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)를 사용하십시오.

앱에 런타임 구성을 추가하려면 `next.config.js` 파일을 열고 `publicRuntimeConfig`와 `serverRuntimeConfig`를 추가합니다.

```jsx
// next.config.js

module.exports = {
  serverRuntimeConfig: {
    // 서버 측에서만 사용 가능합니다.
    mySecret: "secret",
    secondSecret: process.env.SECOND_SECRET, // 환경 변수를 통해 전달합니다.
  },
  publicRuntimeConfig: {
    // 서버 측, 클라이언트 측 모두 사용 가능합니다.
    staticFolder: "/static",
  },
};
```

서버 전용 런타임 구성은 `serverRuntimeConfig`의 하위에 위치시킵니다.

클라이언트 및 서버 측 코드 모두에서 접근 가능한 것은 `publicRuntimeConfig`의 하위에 위치시킵니다.

> `publicRuntimeConfig`에 의존하는 페이지는 **반드시** `getInitialProps` 또는 `getServerSideProps`를 사용하거나, 애플리케이션에서 `getInitialProps`를 사용하는 [사용자 지정 앱](https://nextjs.org/docs/pages/building-your-application/routing/custom-app)을 가져야만 [자동 정적 최적화](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization)에서 제외될 수 있습니다. 런타임 구성은 서버 측 렌더링되지 않은 페이지(또는 페이지의 구성 요소)에서는 사용할 수 없습니다.

앱에서 런타임 구성에 접근하려면 다음과 같이 `next/config`를 사용합니다.

```jsx
import getConfig from "next/config";
import Image from "next/image";

// serverRuntimeConfig, publicRuntimeConfig만 변수로 저장합니다.
const { serverRuntimeConfig, publicRuntimeConfig } = getConfig();
// 서버 측에서만 사용 가능합니다.
console.log(serverRuntimeConfig.mySecret);
// 서버 측, 클라이언트 측 모두 사용 가능합니다.
console.log(publicRuntimeConfig.staticFolder);

function MyImage() {
  return (
    <div>
      <Image
        src={`${publicRuntimeConfig.staticFolder}/logo.png`}
        alt="logo"
        layout="fill"
      />
    </div>
  );
}

export default MyImage;
```
