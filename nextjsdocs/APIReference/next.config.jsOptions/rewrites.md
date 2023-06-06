# rewrites

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/rewrites](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites)

rewrites는 들어오는 요청 경로를 다른 대상 경로로 매핑할 수 있도록 해줍니다.

rewrites는 URL 프록시 역할을 하며 대상 경로를 가리고, 사용자가 사이트에서 위치를 변경하지 않은 것처럼 보이게 합니다. 반면, redirects는 새로운 페이지로 경로를 변경하고 URL 변경을 보여줍니다.

rewrites를 사용하기 위해 `next.config.js` 파일에서 `rewrites` 키를 사용할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/about",
        destination: "/",
      },
    ];
  },
};
```

rewrites는 클라이언트 측 라우팅에 적용되며, 위의 예시에서 `<Link href="/about">`은 rewrite가 적용됩니다.

`rewrites`는 배열 또는 배열 객체(아래 참조)를 반환하는 비동기 함수입니다. 각 객체는 `source`와 `destination` 속성을 가집니다.

- `source` : `String` - 들어오는 요청 경로 패턴입니다.
- `destination` : `String` - 라우팅하고자 하는 경로입니다.
- `basePath` : `false` 또는 `undefined` - false인 경우 일치할 때 basePath는 포함되지 않으며, 외부 rewrites에만 사용할 수 있습니다.
- `locale` : `false` 또는 `undefined` - 일치할 때 locale을 포함하지 않아야 하는지 여부입니다.
- `has`는 `type`, `key`, `value` 속성을 가진 [has 객체](rewrites.md#header-cookie-and-query-matching)의 배열입니다.
- `missing`은 `type`, `key`, `value` 속성을 가진 [missing 객체](rewrites.md#header-cookie-and-query-matching)의 배열입니다.

`rewrites` 함수가 배열을 반환하는 경우, 파일 시스템(페이지 및 `/public` 파일)을 확인한 후 동적 라우트 이전에 `rewrites`가 적용됩니다. `rewrites` 함수가 특정 형태의 배열 객체를 반환하는 경우, Next.js의 `v10.1`부터 이 동작을 변경하고 더 정밀하게 제어할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return {
      beforeFiles: [
        // headers/redirects 이후에 확인되며
        // _next/public 파일을 포함한 모든 파일 이전에 확인됩니다.
        // 페이지 파일을 덮어쓸 수 있습니다.
        {
          source: "/some-page",
          destination: "/somewhere-else",
          has: [{ type: "query", key: "overrideMe" }],
        },
      ],
      afterFiles: [
        // pages/public 파일 확인 이후에,
        // dynamic routes 이전에 확인됩니다.
        {
          source: "/non-existent",
          destination: "/somewhere-else",
        },
      ],
      fallback: [
        // pages/public 파일과 dynamic routes 확인 이후에 확인됩니다.
        {
          source: "/:path*",
          destination: `https://my-old-site.com/:path*`,
        },
      ],
    };
  },
};
```

> Note : `beforeFiles`의 rewrites는 소스와 일치한 후에 파일 시스템/동적 라우트를 즉시 확인하지 않고 모든 `beforeFiles`가 확인될 때까지 계속 진행됩니다.

Next.js에서 라우트가 확인되는 순서는 다음과 같습니다.

1. [headers](https://nextjs.org/docs/pages/api-reference/next-config-js/headers)가 확인되고 적용됩니다.
2. [redirects](https://nextjs.org/docs/pages/api-reference/next-config-js/redirects)가 확인되고 적용됩니다.
3. `beforeFiles` rewrites가 확인되고 적용됩니다.
4. [public 디렉토리](https://nextjs.org/docs/pages/building-your-application/optimizing/static-assets)의 정적 파일, `\_next/static` 파일 및 비동적 페이지가 확인되고 제공됩니다.
5. `afterFiles` rewrites가 확인되고 적용됩니다. 일치하는 rewrites가 있으면 각 일치 후 동적 라우트/정적 파일을 확인합니다.
6. `fallback` rewrites가 확인되고 적용됩니다. 이러한 rewrites는 404 페이지 렌더링 전에 적용되며 동적 라우트/모든 정적 에셋이 확인된 후에 적용됩니다. `getStaticPaths`에서 [fallback: true 또는 'blocking'](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths#fallback-true)을 사용하는 경우, `next.config.js`에서 정의된 fallback `rewrites`가 실행되지 않습니다.

<br><hr><br>

## Rewrite 매개변수

rewrite에서 매개변수를 사용할 때, `destination`에 매개변수가 사용되지 않은 경우 매개변수는 기본적으로 쿼리로 전달됩니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/old-about/:path*",
        destination: "/about", // :path 매개변수가 사용되지 않았으므로 쿼리로 전달됩니다.
      },
    ];
  },
};
```

destination에 매개변수가 사용된 경우, 매개변수는 자동으로 쿼리로 전달되지 않습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/docs/:path*",
        destination: "/:path*", // :path 매개변수가 사용되었으므로 쿼리로 자동으로 전달되지 않습니다.
      },
    ];
  },
};
```

`destination`에서 이미 매개변수가 사용된 경우, 대상에 직접 쿼리를 지정하여 매개변수를 수동으로 전달할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/:first/:second",
        destination: "/:first?second=:second",
        // :first 매개변수가 사용되었기 때문에 :second 매개변수는 destination에
        // 자동으로 쿼리로 전달되지 않지만 위의 예시처럼 수동으로 전달할 수 있습니다.
      },
    ];
  },
};
```

> Note : [자동 정적 최적화](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization)에서 가져온 정적 페이지 또는 rewrites를 통해 [사전 렌더링](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)된 매개변수는 하이드레이션 후에 클라이언트에서 구문 분석(parse)되어 쿼리로 제공됩니다.

<br><hr><br>

## 경로 매칭

경로 매칭이 허용됩니다. 예를 들어, `/blog/:slug`는 `/blog/hello-world`와 일치합니다(중첩된 경로는 허용되지 않습니다).

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/blog/:slug",
        destination: "/news/:slug", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
      },
    ];
  },
};
```

<br><hr><br>

### 와일드카드 경로 매칭

와일드카드 경로를 일치시키려면 매개변수 뒤에 `*`을 사용할 수 있습니다. 예를 들어, `/blog/:slug*`는 `/blog/a/b/c/d/hello-world`와 일치합니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/blog/:slug*",
        destination: "/news/:slug*", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
      },
    ];
  },
};
```

<br><hr><br>

### 정규 표현식 경로 매칭

정규 표현식 경로를 일치시키려면 매개변수 뒤에 정규식을 괄호로 감쌉니다. 예를 들어, `/blog/:slug(\d{1,})`는 `/blog/123`과 일치하지만 `/blog/abc`와는 일치하지 않습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/old-blog/:post(\\d{1,})",
        destination: "/blog/:post", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
      },
    ];
  },
};
```

다음 문자들 `(`, `)`, `{`, `}`, `:`, `*`, `+`, `?`는 정규 표현식 경로 일치에 사용되므로 `source`에서 특수한 값으로 사용되지 않을 때에는 그 앞에 `\\`를 추가하여 이스케이프(escape)해야 합니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        // /english(default)/something이 요청되었을 때 일치합니다.
        source: "/english\\(default\\)/:slug",
        destination: "/en-us/:slug",
      },
    ];
  },
};
```

<br><hr><br>

## 헤더, 쿠키, 쿼리 매칭

헤더, 쿠키 또는 쿼리 값이 `has` 필드와 일치하거나 `missing` 필드와 일치하지 않을 때에만 rewrite가 일치하도록 설정할 수 있습니다. `source`와 모든 `has` 아이템이 일치하고 모든 `missing` 아이템이 일치하지 않아야 rewrite가 적용됩니다.

`has`와 `missing` 아이템은 다음과 같은 필드를 가질 수 있습니다.

- `type` : `String` - 반드시 `header`, `cookie`, `host`, 또는 `query` 중 하나여야 합니다.
- `key` : `String` - 일치 여부를 확인하기 위해 선택한 타입의 키입니다.
- `value` : `String` 또는 `undefined` - 확인할 값입니다. undefined인 경우 어떤 값이든 일치합니다. 정규 표현식 형식의 문자열을 사용하여 값의 특정 부분을 포착할 수도 있습니다. 예를 들어, 값으로 `first-(?<paramName>.*)`을 사용하면 `first-second`와 일치하며, destination에서 `:paramName`과 함께 `second`를 사용할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      // 헤더에 'x-rewrite-me'가 있다면 rewrite가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "header",
            key: "x-rewrite-me",
          },
        ],
        destination: "/another-page",
      },
      // 헤더에 'x-rewrite-me'가 없다면 rewrite가 적용됩니다.
      {
        source: "/:path*",
        missing: [
          {
            type: "header",
            key: "x-rewrite-me",
          },
        ],
        destination: "/another-page",
      },
      // source, 쿼리, 쿠키가 일치하면 rewrite가 적용됩니다.
      {
        source: "/specific/:path*",
        has: [
          {
            type: "query",
            key: "page",
            // page 값은 명명된 캡처 그룹 (예: (?<page>home))을 사용하지 않고
            // value가 제공되므로 destination에서 사용할 수 없습니다.
            value: "home",
          },
          {
            type: "cookie",
            key: "authorized",
            value: "true",
          },
        ],
        destination: "/:path*/home",
      },
      // 헤더에 `x-authorized`가 있고 일치하는 값을 포함하고 있다면 rewrite가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "header",
            key: "x-authorized",
            value: "(?<authorized>yes|true)",
          },
        ],
        destination: "/home?authorized=:authorized",
      },
      // host가 `example.com`이면 rewrite가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "host",
            value: "example.com",
          },
        ],
        destination: "/another-page",
      },
    ];
  },
};
```

<br><hr><br>

## 외부 URL로의 rewrite

<details>
<summary>예시</summary>

- [점진적 Next.js 도입](https://github.com/vercel/next.js/tree/canary/examples/custom-routes-proxying)
- [여러 Zone 사용](https://github.com/vercel/next.js/tree/canary/examples/custom-routes-proxying)
</details>

rewrites를 사용하면 외부 URL로 rewrite할 수 있습니다. 이는 Next.js를 점진적으로 도입하는 데 특히 유용합니다. 다음은 메인 앱의 `/blog` 경로를 외부 사이트로 redirection하는 rewrite 예시입니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: "/blog",
        destination: "https://example.com/blog",
      },
      {
        source: "/blog/:slug",
        destination: "https://example.com/blog/:slug", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
      },
    ];
  },
};
```

`trailingSlash: true`를 사용하는 경우, `source` 매개변수에 후행 슬래시를 추가해야 합니다. destination 서버가 후행 슬래시를 기대하는 경우, `destination` 매개변수에도 이를 포함해야 합니다.

```jsx
// next.config.js

module.exports = {
  trailingSlash: true,
  async rewrites() {
    return [
      {
        source: "/blog/",
        destination: "https://example.com/blog/",
      },
      {
        source: "/blog/:path*/",
        destination: "https://example.com/blog/:path*/",
      },
    ];
  },
};
```

<br><hr><br>

### Next.js의 점진적 도입

Next.js에서는 모든 Next.js 경로를 확인한 후 기존 웹사이트로 프록시(proxy)하도록 fallback을 설정할 수도 있습니다.

이러한 방식을 사용하면 Next.js로 추가 페이지를 마이그레이션할 때 rewrites 구성을 변경할 필요가 없습니다.

```jsx
// next.config.js

module.exports = {
  async rewrites() {
    return {
      fallback: [
        {
          source: "/:path*",
          destination: `https://custom-routes-proxying-endpoint.vercel.app/:path*`,
        },
      ],
    };
  },
};
```

<br><hr><br>

### basePath support를 활용한 rewrites

[basePath 지원](./basePath.md)을 활용하여 rewrites와 함께 사용할 때, rewrite에 `basePath: false`를 추가하지 않는 한 `source`와 `destination`에 자동으로 `basePath`로 접두어가 붙습니다.

```jsx
// next.config.js

module.exports = {
  basePath: "/docs",

  async rewrites() {
    return [
      {
        source: "/with-basePath", // 자동으로 /docs/with-basePath 로 변환됩니다.
        destination: "/another", // 자동으로 /docs/another 로 변환됩니다.
      },
      {
        // basePath: false 설정으로 인해
        // /without-basePath에 /docs 가 추가되지 않습니다.
        // Note : 내부 rewrites에 사용될 수 없습니다. 예 : `destination: '/another'`
        source: "/without-basePath",
        destination: "https://example.com",
        basePath: false,
      },
    ];
  },
};
```

<br><hr><br>

### i18n support를 활용한 rewrites

[`i18n` 지원](https://nextjs.org/docs/pages/building-your-application/routing/internationalization)을 활용하여 rewrites와 함께 사용할 때, rewrite에 `locale: false`를 추가하지 않는 한 구성된 `locales`를 처리하기 위해 `source`와 `destination`에 자동으로 접두어가 붙습니다. `locale: false`를 사용하는 경우 `source`와 `destination`을 locale과 함께 접두어로 추가해야 올바르게 일치합니다.

```jsx
// next.config.js

module.exports = {
  i18n: {
    locales: ["en", "fr", "de"],
    defaultLocale: "en",
  },

  async rewrites() {
    return [
      {
        source: "/with-locale", // 자동으로 모든 locale을 처리합니다.
        destination: "/another", // 자동으로 locale을 전달합니다.
      },
      {
        // locale: false가 설정되어 있기 때문에 자동으로 locale을 처리하지 않습니다.
        source: "/nl/with-locale-manual",
        destination: "/nl/another",
        locale: false,
      },
      {
        // 'en'이 locale 기본값이기 때문에 이는 '/'와 일치합니다.
        source: "/en",
        destination: "/en/another",
        locale: false,
      },
      {
        // locale: false가 설정되어 있어도 모든 locale과 일치시킬 수 있습니다.
        source: "/:locale/api-alias/:path*",
        destination: "/api/:path*",
        locale: false,
      },
      {
        // 이는 /(en|fr|de)/(.*)로 변환되므로 최상위 레벨과 일치하지 않을 것입니다.
        // `/` 또는 `/fr`과 같은 경로는 /:path*와 일치할 것입니다.
        source: "/(.*)",
        destination: "/another",
      },
    ];
  },
};
```

<br><hr><br>

## 버전 히스토리

| Version   | Changes        |
| --------- | -------------- |
| `v13.3.0` | `missing 추가` |
| `v10.2.0` | `has 추가`     |
| `v9.5.0`  | `Headers 추가` |
