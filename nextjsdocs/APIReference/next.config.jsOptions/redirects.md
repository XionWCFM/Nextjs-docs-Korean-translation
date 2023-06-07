# redirects

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/redirects](https://nextjs.org/docs/app/api-reference/next-config-js/redirects)

redirects는 들어오는 요청 경로를 다른 대상 경로로 리다이렉션할 수 있도록 해줍니다.

redirects를 사용하기 위해 `next.config.js` 파일에서 `redirects` 키를 사용할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async redirects() {
    return [
      {
        source: "/about",
        destination: "/",
        permanent: true,
      },
    ];
  },
};
```

`redirects`는 배열을 반환하는 비동기 함수입니다. 반환되는 배열은 `source`, `destination`, `permanent` 속성을 가진 객체들을 포함해야 합니다.

- `source`는 들어오는 요청 경로 패턴입니다.
- `destination`은 라우팅하고자 하는 경로입니다.
- `permanent` : `true` 혹은 `false` - `true`로 설정하면 308 상태 코드가 사용되며, 이는 클라이언트 및 검색 엔진에게 리다이렉션을 영구적으로 캐시하도록 지시합니다. 반대로 `false`로 설정하면 307 상태 코드가 사용되며, 이는 일시적인 리다이렉션을 나타내며 캐시되지 않습니다.

> **Next.js는 왜 307, 308을 사용하나요?** 기존에는 일시적인 리다이렉션을 나타내기 위해 302가 사용되고 영구적인 리다이렉션을 나타내기 위해 301이 사용되었지만, 많은 브라우저에서 리다이렉션의 요청 방법을 `GET`으로 변경했습니다. 예를 들어, 브라우저가 `POST /v1/users`에 대한 요청을 만들고 위치 `/v2/users`로 `302` 상태 코드가 반환되었다면, 예상되는 `POST /v2/users` 대신 `GET /v2/users`와 같은 후속 요청이 발생할 수 있습니다. Next.js는 307 임시 리다이렉션 및 308 영구 리다이렉션 상태 코드를 사용하여 요청 방법을 명시적으로 보존합니다.

- `basePath` : `false` 또는 `undefined` - false인 경우 일치할 때 `basePath`는 포함되지 않으며, 외부 redirects에만 사용할 수 있습니다.
- `locale` : `false` 또는 `undefined` - 일치할 때 locale을 포함하지 않아야 하는지 여부입니다.
- `has`는 `type`, `key`, `value` 속성을 가진 [has 객체](./redirects.md#header-cookie-and-query-matching)의 배열입니다.
- `missing`은 `type`, `key`, `value` 속성을 가진 [missing 객체](./redirects.md#header-cookie-and-query-matching)의 배열입니다.

리다이렉트는 페이지와 `/public` 파일을 포함한 파일 시스템보다 먼저 확인됩니다.

리다이렉트는 클라이언트 측 라우팅(`Link`, `router.push`)에는 적용되지 않습니다. 단, [미들웨어](../../BuildingYourApplication/Routing/Middleware.md)가 존재하고 경로와 일치하는 경우에는 적용됩니다.

리다이렉트가 적용되면 요청에 제공된 쿼리 값이 리다이렉트 대상으로 전달됩니다. 예를 들어, 다음과 같은 리다이렉트 구성을 살펴보세요.

```jsx
{
  source: '/old-blog/:path*',
  destination: '/blog/:path*',
  permanent: false
}
```

`/old-blog/post-1?hello=world`가 요청되면 클라이언트는 `/blog/post-1?hello=world`로 리다이렉트됩니다.

<br><hr><br>

## 경로 매칭

경로 매칭이 허용됩니다. 예를 들어, `/old-blog/:slug`는 `/old-blog/hello-world`와 일치합니다(중첩된 경로는 허용되지 않습니다).

```jsx
// next.config.js

module.exports = {
  async redirects() {
    return [
      {
        source: "/old-blog/:slug",
        destination: "/news/:slug", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
        permanent: true,
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
  async redirects() {
    return [
      {
        source: "/blog/:slug*",
        destination: "/news/:slug*", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
        permanent: true,
      },
    ];
  },
};
```

<br><hr><br>

### 정규 표현식 경로 매칭

정규 표현식 경로를 일치시키려면 매개변수 뒤에 정규식을 괄호로 감쌉니다. 예를 들어, `/post/:slug(\d{1,})`는 `/post/123`과 일치하지만 `/post/abc`와는 일치하지 않습니다.

```jsx
// next.config.js

module.exports = {
  async redirects() {
    return [
      {
        source: "/post/:slug(\\d{1,})",
        destination: "/news/:slug", // 일치하는 매개변수는 destination에서 사용할 수 있습니다.
        permanent: false,
      },
    ];
  },
};
```

다음 문자들 `(`, `)`, `{`, `}`, `:`, `*`, `+`, `?`는 정규 표현식 경로 일치에 사용되므로 `source`에서 특수한 값으로 사용되지 않을 때에는 그 앞에 `\\`를 추가하여 이스케이프(escape)해야 합니다.

```jsx
// next.config.js

module.exports = {
  async redirects() {
    return [
      {
        // '/english(default)/something'이 요청되었을 때 일치합니다.
        source: "/english\\(default\\)/:slug",
        destination: "/en-us/:slug",
        permanent: false,
      },
    ];
  },
};
```

<br><hr><br>

## 헤더, 쿠키, 쿼리 매칭

헤더, 쿠키 또는 쿼리 값이 `has` 필드와 일치하거나 `missing` 필드와 일치하지 않을 때에만 리다이렉트 되도록 설정할 수 있습니다. `source`와 모든 `has` 아이템이 일치하고 모든 `missing` 아이템이 일치하지 않아야 리다이렉트가 적용됩니다.

`has`와 `missing` 아이템은 다음과 같은 필드를 가질 수 있습니다.

- `type` : `String` - 반드시 `header`, `cookie`, `host`, 또는 `query` 중 하나여야 합니다.
- `key` : `String` - 일치 여부를 확인하기 위해 선택한 타입의 키입니다.
- `value` : `String` 또는 `undefined` - 확인할 값입니다. undefined인 경우 어떤 값이든 일치합니다. 정규 표현식 형식의 문자열을 사용하여 값의 특정 부분을 포착할 수도 있습니다. 예를 들어, 값으로 `first-(?<paramName>.*)`을 사용하면 `first-second`와 일치하며, destination에서 `:paramName`과 함께 `second`를 사용할 수 있습니다.

```jsx
// next.config.js

module.exports = {
  async redirects() {
    return [
      // 헤더에 'x-redirect-me'가 있다면 redirect가 적용됩니다.
      {
        source: "/:path((?!another-page$).*)",
        has: [
          {
            type: "header",
            key: "x-redirect-me",
          },
        ],
        permanent: false,
        destination: "/another-page",
      },
      // 헤더에 'x-do-not-redirect'가 있다면 redirect가 적용되지 않습니다.
      {
        source: "/:path((?!another-page$).*)",
        missing: [
          {
            type: "header",
            key: "x-do-not-redirect",
          },
        ],
        permanent: false,
        destination: "/another-page",
      },
      // source, 쿼리, 쿠키가 일치하면 redirect가 적용됩니다.
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
        permanent: false,
        destination: "/another/:path*",
      },
      // 헤더에 `x-authorized`가 있고 일치하는 값을 포함하고 있다면 redirect가 적용됩니다.
      {
        source: "/",
        has: [
          {
            type: "header",
            key: "x-authorized",
            value: "(?<authorized>yes|true)",
          },
        ],
        permanent: false,
        destination: "/home?authorized=:authorized",
      },
      // host가 `example.com`이면 redirect가 적용됩니다.
      {
        source: "/:path((?!another-page$).*)",
        has: [
          {
            type: "host",
            value: "example.com",
          },
        ],
        permanent: false,
        destination: "/another-page",
      },
    ];
  },
};
```

<br><hr><br>

### basePath support를 활용한 redirects

[basePath 지원](./basePath.md)을 활용하여 redirects와 함께 사용할 때, redirect에 `basePath: false`를 추가하지 않는 한 `source`와 `destination`에 자동으로 `basePath`로 접두어가 붙습니다.

```jsx
// next.config.js

module.exports = {
  basePath: "/docs",

  async redirects() {
    return [
      {
        source: "/with-basePath", // 자동으로 /docs/with-basePath 로 변환됩니다.
        destination: "/another", /// 자동으로 /docs/another 로 변환됩니다.
        permanent: false,
      },
      {
        // basePath: false 설정으로 인해 /docs가 추가되지 않습니다.
        source: "/without-basePath",
        destination: "https://example.com",
        basePath: false,
        permanent: false,
      },
    ];
  },
};
```

<br><hr><br>

### i18n support를 활용한 redirects

[`i18n` 지원](https://nextjs.org/docs/pages/building-your-application/routing/internationalization)을 활용하여 redirects와 함께 사용할 때, redirect에 `locale: false`를 추가하지 않는 한 구성된 `locales`를 처리하기 위해 `source`와 `destination`에 자동으로 접두어가 붙습니다. `locale: false`를 사용하는 경우 `source`와 `destination`에 locale을 접두어로 추가해야 올바르게 일치합니다.

```jsx
// next.config.js

module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },

  async redirects() {
    return [
      {
        source: '/with-locale', // 자동으로 모든 locale을 처리합니다.
        destination: '/another', // 자동으로 locale을 전달합니다.
        permanent: false,
      },
      {
        // locale: false가 설정되어 있기 때문에 자동으로 locale을 처리하지 않습니다.
        source: '/nl/with-locale-manual',
        destination: '/nl/another',
        locale: false,
        permanent: false,
      },
      {
        // 'en'이 defaultLocale이므로 '/'와 일치합니다.
        source: '/en',
        destination: '/en/another',
        locale: false,
        permanent: false,
      },
      // locale: false가 설정되어 있어도 모든 locale과 일치시킬 수 있습니다.
      {
        source: '/:locale/page',
        destination: '/en/newpage',
        permanent: false,
        locale: false,
      }
      {
        // 이는 /(en|fr|de)/(.*)로 변환되므로 최상위 레벨과 일치하지 않을 것입니다.
        // `/` 또는 `/fr`과 같은 경로는 /:path*와 일치할 것입니다.
        source: '/(.*)',
        destination: '/another',
        permanent: false,
      },
    ]
  },
}
```

일부 특수한 경우에 오래된 HTTP 클라이언트가 올바른 리다이렉션을 수행하려면 사용자 정의 상태 코드를 지정해야 할 수 있습니다. 이러한 경우에는 `permanent` 속성 대신 `statusCode` 속성을 사용할 수 있습니다. 하지만 statusCode와 permanent를 함께 사용할 수는 없습니다. IE11 호환성을 보장하기 위해 308 상태 코드에 대해 `Refresh` 헤더가 자동으로 추가됩니다.

<br><hr><br>

## 기타 Redirects

- [API Routes](https://nextjs.org/docs/pages/api-reference/functions/next-server) 내에서는 `res.redirect()`를 사용할 수 있습니다.
- [`getStaticProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props) 및 [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props) 내에서는 요청 시점에 특정 페이지로 리다이렉션할 수 있습니다.

<br><hr><br>

## 버전 히스토리

| Version   | Changes          |
| --------- | ---------------- |
| `v13.3.0` | `missing` 추가   |
| `v10.2.0` | `has` 추가       |
| `v9.5.0`  | `redirects` 추가 |
