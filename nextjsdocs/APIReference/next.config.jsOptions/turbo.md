# **turbo (Experimental)**

> 경고: 이 기능은 실험 단계이며 `next --turbo`에서만 작동합니다.
> 

---

## **[webpack loaders](https://nextjs.org/docs/app/api-reference/next-config-js/turbo#webpack-loaders)**

현재 Turbopack은 웹팩 로더 API의 하위 집합을 지원하므로 일부 웹팩 로더를 사용하여 Turbopack에서 코드를 변환할 수 있습니다.

로더를 구성하려면 설치한 로더의 이름과 옵션을 `next.config.js`에 추가하여 파일 확장명을 로더 목록에 매핑합니다:

```jsx
// next.config.js

module.exports = {
  experimental: {
    turbo: {
      loaders: {
        // Option format
        '.md': [
          {
            loader: '@mdx-js/loader',
            options: {
              format: 'md',
            },
          },
        ],
        // Option-less format
        '.mdx': ['@mdx-js/loader'],
      },
    },
  },
};
```

그런 다음 위의 구성이 주어지면 앱에서 변환된 코드를 사용할 수 있습니다:

```jsx
import MyDoc from './my-doc.mdx';

 export default function Home() {
  return <MyDoc />;
}
```

---

## **[Resolve Alias](https://nextjs.org/docs/app/api-reference/next-config-js/turbo#resolve-alias)**

`next.config.js` 를 통해 터보팩은 웹팩의 [resolve.alias](https://webpack.js.org/configuration/resolve/#resolvealias) 구성과 유사하게 별칭을 통해 모듈 해상도를 수정하도록 구성할 수 있습니다.

별칭 확인을 구성하려면 `next.config.js`에서 가져온 패턴을 새 대상에 매핑합니다:

```jsx
// next.config.js

module.exports = {
  experimental: {
    turbo: {
      resolveAlias: {
        underscore: 'lodash',
        mocha: { browser: 'mocha/browser-entry.js' },
      },
    },
  },
};
```

이 별칭은 `underscore` 패키지를 `lodash` 패키지로 가져오는 것을 별칭으로 지정합니다. 즉, `import underscore from 'underscore'` 을 하면 `underscore` 대신 `lodash` 모듈이 로드됩니다.

터보팩은 또한 이 필드를 통해 Node.js의 [conditional exports](https://nodejs.org/docs/latest-v18.x/api/packages.html#conditional-exports)와 유사한 조건부 앨리어싱을 지원합니다.

현재는 `browser` 조건만 지원됩니다. 위의 경우, 터보팩이 브라우저 환경을 대상으로 할 때 `mocha` 모듈을 가져오는 별칭은 `mocha/browser-entry.js` 로 지정됩니다.

웹팩에서 터보팩으로 앱을 마이그레이션하는 방법에 대한 자세한 정보 및 지침은 [Turbopack's documentation on webpack compatibility](https://turbo.build/pack/docs/migrating-from-webpack) 문서를 참조하세요.