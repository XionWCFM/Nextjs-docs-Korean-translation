# Custom Webpack Config

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/webpack](https://nextjs.org/docs/app/api-reference/next-config-js/webpack)

> Note : 웹팩 구성 변경은 semver에 포함되지 않으므로 위험 부담은 스스로 감수하여 진행하십시오.

애플리케이션에 사용자 정의 웹팩 구성을 추가하기 전에, Next.js가 이미 사용 사례를 지원하는지 확인하십시오.

- [CSS imports](https://nextjs.org/docs/pages/building-your-application/styling)
- [CSS modules](https://nextjs.org/docs/pages/building-your-application/styling/css-modules)
- [Sass/SCSS imports](https://nextjs.org/docs/pages/building-your-application/styling/sass)
- [Sass/SCSS modules](https://nextjs.org/docs/pages/building-your-application/styling/sass)
- [preact](https://github.com/vercel/next.js/tree/canary/examples/using-preact)
- [바벨 구성 커스터마이징](https://nextjs.org/docs/pages/building-your-application/configuring/babel)

자주 요청되는 몇 가지 기능은 플러그인으로 제공됩니다.

- [@next/mdx](https://github.com/vercel/next.js/tree/canary/packages/next-mdx)
- [@next/bundle-analyzer](https://github.com/vercel/next.js/tree/canary/packages/next-bundle-analyzer)

webpack 사용을 확장하기 위해 next.config.js 내에서 config를 확장하는 함수를 다음과 같이 정의할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  webpack: (
    config,
    { buildId, dev, isServer, defaultLoaders, nextRuntime, webpack }
  ) => {
    // 중요 : 수정된 구성을 반환합니다.
    return config;
  },
};
```

> webpack 함수는 서버와 클라이언트 각각에 대해 실행되어 총 두 번 실행됩니다. 이를 통해 isServer 속성을 사용하여 클라이언트와 서버 구성을 구별할 수 있습니다.

`webpack` 함수의 두 번째 인자는 다음과 같은 속성을 가진 객체입니다.

- `buildId` : `String` - 빌드 ID로, 빌드 간의 고유한 식별자로 사용됩니다.
- `dev` : `Boolean` - 개발 중인지 여부를 나타냅니다.
- `isServer` : `Boolean` - 서버 측 컴파일인 경우 `true`이며, 클라이언트 측 컴파일인 경우 `false`입니다.
- `nextRuntime` : `String | undefined` - 서버 측 컴파일의 대상 런타임입니다. `"edge"` 또는 `"nodejs"` 중 하나이며, 클라이언트 측 컴파일의 경우 `undefined`입니다.
- `defaultLoaders` : `Object` - Next.js 내부에서 사용되는 기본 로더
  - `babel` : `Object` - 기본 `babel-loader` 구성

`defaultLoaders.babel` 사용법 예시 :

```js
// babel-loader에 의존하는 로더를 추가하는 구성 예제
// @next/mdx 플러그인 소스로부터 발췌한 소스입니다.
// https://github.com/vercel/next.js/tree/canary/packages/next-mdx
module.exports = {
  webpack: (config, options) => {
    config.module.rules.push({
      test: /\.mdx/,
      use: [
        options.defaultLoaders.babel,
        {
          loader: "@mdx-js/loader",
          options: pluginOptions.options,
        },
      ],
    });

    return config;
  },
};
```

<br><hr><br>

## `nextRuntime`

주목할 점은 `nextRuntime`이 `"edge"` 또는 `"nodejs"`일 때 `isServer`가 `true`인 것입니다. nextRuntime `"edge"`는 현재 edge 런타임 내 미들웨어와 서버 컴포넌트 전용입니다.
