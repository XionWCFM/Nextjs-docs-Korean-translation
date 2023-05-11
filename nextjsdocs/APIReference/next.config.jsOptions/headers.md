원본링크 : [https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-overriding-behavior](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-overriding-behavior)

# \***\*headers\*\***

### **Examples**

- [Headers](https://github.com/vercel/next.js/tree/canary/examples/headers)

### **Version History**

| Version | Changes         |
| ------- | --------------- |
| v10.2.0 | has 추가됨.     |
| v9.5.0  | Headers 추가됨. |

헤더를 사용하면 지정된 경로로 들어오는 요청에 대한 응답에 사용자 지정 HTTP 헤더를 설정할 수 있습니다.

사용자 지정 HTTP 헤더를 설정하려면 `next.config.js`에서 `headers` 키를 사용하면 됩니다:

```jsx
module.exports = {
  async headers() {
    return [
      {
        source: "/about",
        headers: [
          {
            key: "x-custom-header",
            value: "my custom header value",
          },
          {
            key: "x-another-custom-header",
            value: "my other custom header value",
          },
        ],
      },
    ];
  },
};
```

`headers`는 `source` 및 `headers` 속성을 가진 객체를 보유한 배열이 반환될 것으로 예상하는 비동기 함수입니다:

- `source`는 들어오는 요청 경로 패턴입니다.
- `headers`는 `key` 및 `value` 속성을 가진 응답 헤더 객체의 배열입니다.
- `basePath`: `false` 또는 `undefined` - 거짓이면 일치할 때 basePath가 포함되지 않으며, 외부 재작성에만 사용할 수 있습니다.
- `locale`: `false` 또는 `undefined` - 일치할 때 locale을 포함하지 말아야 하는지 여부입니다.
- `has`는 `type`, `key` 및 `value` 속성을 가진 [has 객체](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-cookie-and-query-matching)의 배열입니다.
- `missing`은 `type`, `key` 및 `value` 속성을 가진 [누락된 객체](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-cookie-and-query-matching)의 배열입니다.

헤더는 페이지 및 `/public` 파일을 포함하는 파일 시스템보다 먼저 검사됩니다.

---

## [Header Overriding Behavior](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-overriding-behavior)

두 헤더가 동일한 경로와 일치하고 동일한 헤더 키를 설정한 경우 마지막 헤더 키가 첫 번째 헤더 키를 재정의합니다. 아래 헤더를 사용하면 `/hello` 경로의 경우 설정된 마지막 헤더 값이 `world`이므로 헤더 `x-hello`가 `world`가 됩니다.

```jsx
module.exports = {
  async headers() {
    return [
      {
        source: "/:path*",
        headers: [
          {
            key: "x-hello",
            value: "there",
          },
        ],
      },
      {
        source: "/hello",
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
    ];
  },
};
```

---

## [Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#path-matching)

경로 일치가 허용됩니다 (예: `/blog/:slug`는 `/blog/hello-world`와 일치합니다(중첩된 경로 없음):

```jsx
module.exports = {
  async headers() {
    return [
      {
        source: "/blog/:slug",
        headers: [
          {
            key: "x-slug",
            value: ":slug", // 일치하는 매개 변수를 값에 사용할 수 있습니다.
          },
          {
            key: "x-slug-:slug", // 일치하는 매개 변수를 키에 사용할 수 있습니다.
            value: "my other custom header value",
          },
        ],
      },
    ];
  },
};
```

### [Wildcard Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#wildcard-path-matching)

와일드카드 경로를 일치시키려면 매개변수 뒤에 `*`를 사용할 수 있습니다(예: `/blog/:slug*`는 `/blog/a/b/c/d/hello-world`와 일치합니다):

```jsx
module.exports = {
  async headers() {
    return [
      {
        source: "/blog/:slug*",
        headers: [
          {
            key: "x-slug",
            value: ":slug*", // 일치하는 매개 변수를 값에 사용할 수 있습니다.
          },
          {
            key: "x-slug-:slug*", // 일치하는 매개 변수를 키에 사용할 수 있습니다.
            value: "my other custom header value",
          },
        ],
      },
    ];
  },
};
```

### [Regex Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#regex-path-matching)

정규식 경로를 일치시키려면 매개변수 뒤에 괄호로 정규식을 묶으면 됩니다(예: `/blog/:slug(\\d{1,})`는 `/blog/123`과 일치하지만 `/blog/abc`와는 일치하지 않습니다):

```jsx
module.exports = {
  async headers() {
    return [
      {
        source: "/blog/:post(\\d{1,})",
        headers: [
          {
            key: "x-post",
            value: ":post",
          },
        ],
      },
    ];
  },
};
```

다음 문자`(`, `)`, `{`, `}`, `:`, `*`, `+`, `?` 은 정규식 경로 일치에 사용되므로 `source`에서 특수하지 않은 값으로 사용할 때는 앞에 `\\`을 추가하여 이스케이프 처리해야 합니다:

```jsx
module.exports = {
  async headers() {
    return [
      {
        // 이것은 요청 중인 `/english(default)/something`과 일치합니다.
        source: "/english\\(default\\)/:slug",
        headers: [
          {
            key: "x-header",
            value: "value",
          },
        ],
      },
    ];
  },
};
```

---

## [Header, Cookie, and Query Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-cookie-and-query-matching)

헤더, 쿠키 또는 쿼리 값이 `has` 필드와 일치하거나 `missing`된 필드와 일치하지 않는 경우에만 헤더를 적용하는 방법을 사용할 수 있습니다. 헤더를 적용하려면 `source` 및 모든 `has` 항목이 일치해야 하며 `missing`된 항목이 모두 일치하지 않아야 합니다.

`has` 및 `missing` items에는 다음 필드가 있을 수 있습니다:

- `type`: `String` - `header`, `cookie`, `host`, 또는 `query`여야 합니다.
- `key`: `String` - 선택한 유형에서 일치시킬 키입니다.
- `value`: `String` 또는 `Unfined` - 정의되지 않은 경우 일치하는 값이 있는지 확인할 값입니다. 문자열과 같은 정규식을 사용하여 값의 특정 부분을 캡처할 수 있습니다. 예를 들어 `first-(?<paramName>.*)` 값이 `first-second`에 사용되는 경우 `second`는 `:paramName`을 사용하여 대상에서 사용할 수 있습니다.

```jsx
module.exports = {
  async headers() {
    return [
      // 헤더 `x-add-header`가 있는 경우,
      // `x-another-header` 헤더가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "header",
            key: "x-add-header",
          },
        ],
        headers: [
          {
            key: "x-another-header",
            value: "hello",
          },
        ],
      },
      // 헤더 `x-no-header`가 없는 경우,
      // `x-another-header` 헤더가 적용됩니다.
      {
        source: "/:path*",
        missing: [
          {
            type: "header",
            key: "x-no-header",
          },
        ],
        headers: [
          {
            key: "x-another-header",
            value: "hello",
          },
        ],
      },
      // 소스, 쿼리, 쿠키가 일치하는 경우,
      // 'x-authorized' 헤더가 적용됩니다.
      {
        source: "/specific/:path*",
        has: [
          {
            type: "query",
            key: "page",
            // 페이지 값이 제공된 이후에는
            // header key/values이 제공되고
            // 명명된 캡처 그룹을 사용하지 않습니다 e.g. (?<page>home)
            value: "home",
          },
          {
            type: "cookie",
            key: "authorized",
            value: "true",
          },
        ],
        headers: [
          {
            key: "x-authorized",
            value: ":authorized",
          },
        ],
      },
      // 헤더 `x-authorized`가 존재하고
      // 일치하는 값이 포함되어 있으면 `x-another-header`가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "header",
            key: "x-authorized",
            value: "(?<authorized>yes|true)",
          },
        ],
        headers: [
          {
            key: "x-another-header",
            value: ":authorized",
          },
        ],
      },
      // 호스트가 `example.com`인 경우,
      // 이 헤더가 적용됩니다.
      {
        source: "/:path*",
        has: [
          {
            type: "host",
            value: "example.com",
          },
        ],
        headers: [
          {
            key: "x-another-header",
            value: ":authorized",
          },
        ],
      },
    ];
  },
};
```

---

## [Headers with basePath support](https://nextjs.org/docs/app/api-reference/next-config-js/headers#headers-with-basepath-support)

헤더에 `[basePath` 지원](https://nextjs.org/docs/app/api-reference/next-config-js/basePath)을 활용하는 경우 헤더에 `basePath: false`를 추가하지 않는 한 각 `source`에는 자동으로 `basePath`가 접두사로 붙습니다:

```jsx
module.exports = {
  basePath: "/docs",

  async headers() {
    return [
      {
        source: "/with-basePath", // 문서가 /docs/with-basePath가 됩니다.
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
      {
        source: "/without-basePath", // basePath: false가 설정되어 있으므로 수정되지 않습니다.
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
        basePath: false,
      },
    ];
  },
};
```

---

## [Headers with i18n support](https://nextjs.org/docs/app/api-reference/next-config-js/headers#headers-with-i18n-support)

헤더와 함께 `[i18n` 지원](https://nextjs.org/docs/pages/building-your-application/routing/internationalization)을 활용할 때 헤더에 `locale: false`를 추가하지 않는 한 각 `source`는 구성된 `locales`을 처리하도록 자동으로 접두사가 붙습니다. `locale: false`를 사용하는 경우 `source`에 locale을 접두사로 추가해야 올바르게 일치합니다.

```jsx
module.exports = {
  i18n: {
    locales: ["en", "fr", "de"],
    defaultLocale: "en",
  },

  async headers() {
    return [
      {
        source: "/with-locale", // 모든 locales을 자동으로 처리합니다.
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
      {
        // local: false가 설정되어 있으므로 locales을 자동으로 처리하지 않습니다
        source: "/nl/with-locale-manual",
        locale: false,
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
      {
        // `en`이 기본 로캘이므로 '/'와 일치합니다.
        source: "/en",
        locale: false,
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
      {
        // /(en|fr|de)/(*)로 변환되므로 최상위 수준과 일치하지 않습니다
        // `/` 또는 /:path*와 같은 '/fr' 경로입니다
        source: "/(.*)",
        headers: [
          {
            key: "x-hello",
            value: "world",
          },
        ],
      },
    ];
  },
};
```

---

## [Cache-Control](https://nextjs.org/docs/app/api-reference/next-config-js/headers#cache-control)

`res.setHeader` 메서드를 사용하여 [Next.js API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)에서 `Cache-Control` 헤더를 설정할 수 있습니다:

```jsx
export default function handler(req, res) {
  res.setHeader("Cache-Control", "s-maxage=86400");
  res.status(200).json({ name: "John Doe" });
}
```

API 경로와 정적 에셋이 효과적으로 캐시되도록 하기 위해 프로덕션 환경에서 덮어쓰기 되므로 `next.config.js` 파일에서 `Cache-Control` 헤더를 설정할 수 없습니다.

[정적으로 생성](https://nextjs.org/docs/pages/building-your-application/routing/pages-and-layouts#static-generation-recommended)된 페이지의 캐시를 `revalidate`해야 하는 경우 페이지의 `[getStaticProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)` 함수에서 재검증 프로퍼티를 설정하여 재검증할 수 있습니다.

---

## [Options](https://nextjs.org/docs/app/api-reference/next-config-js/headers#options)

### [X-DNS-Prefetch-Control](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-dns-prefetch-control)

이 헤더는 DNS prefetching을 제어하여 브라우저가 외부 링크, 이미지, CSS, JavaScript 등에 대한 도메인 이름 확인을 사전에 수행할 수 있도록 합니다. 이 prefetching은 백그라운드에서 수행되므로 참조된 항목이 필요할 때 [DNS](https://developer.mozilla.org/en-US/docs/Glossary/DNS)가 해결될 가능성이 높습니다. 이렇게 하면 사용자가 링크를 클릭할 때 지연 시간이 줄어듭니다.

```jsx
{
  key: 'X-DNS-Prefetch-Control',
  value: 'on'
}
```

### [Strict-Transport-Security](https://nextjs.org/docs/app/api-reference/next-config-js/headers#strict-transport-security)

이 헤더는 브라우저에 HTTP 대신 HTTPS를 통해서만 액세스해야 함을 알립니다. 아래 구성을 사용하면 현재 및 향후 모든 하위 도메인이 `max-age` 2년 동안 HTTPS를 사용하게 됩니다. 이렇게 하면 HTTP를 통해서만 제공될 수 있는 페이지 또는 하위 도메인에 대한 액세스가 차단됩니다.

[Vercel](https://vercel.com/docs/concepts/edge-network/headers#strict-transport-security?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)에 배포하는 경우, `next.config.js`에서 `[headers](https://nextjs.org/docs/pages/api-reference/next-config-js/headers)`를 선언하지 않는 한 모든 배포에 자동으로 추가되므로 이 헤더는 필요하지 않습니다.

```jsx
{
  key: 'Strict-Transport-Security',
  value: 'max-age=63072000; includeSubDomains; preload'
}
```

### \***\*[X-XSS-Protection](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-xss-protection)\*\***

이 헤더는 반영된 크로스 사이트 스크립팅(XSS) 공격이 감지되면 페이지 로딩을 중지합니다. 사이트에서 인라인 자바스크립트(`'unsafe-inline'`)의 사용을 비활성화하는 강력한 [`Content-Security-Policy`](https://nextjs.org/docs/app/api-reference/next-config-js/headers#content-security-policy)를 구현하는 경우에는 이 보호 기능이 필요하지 않지만, CSP를 지원하지 않는 구형 웹 브라우저를 보호할 수 있습니다.

```jsx
{
  key: 'X-XSS-Protection',
  value: '1; mode=block'
}
```

### \***\*[X-Frame-Options](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-frame-options)\*\***

이 헤더는 사이트가 `iframe` 내에 표시되도록 허용할지 여부를 나타냅니다. 이를 통해 clickjacking 공격을 방지할 수 있습니다. 이 헤더는 최신 브라우저에서 더 잘 지원되는 CSP의 `frame-ancestors` 옵션으로 대체되었습니다.

```jsx
{
  key: 'X-Frame-Options',
  value: 'SAMEORIGIN'
}
```

### \***\*[Permissions-Policy](https://nextjs.org/docs/app/api-reference/next-config-js/headers#permissions-policy)\*\***

이 헤더를 사용하면 브라우저에서 사용할 수 있는 기능과 API를 제어할 수 있습니다. 이전에는 `Feature-Policy`으로 명명되었습니다. [여기](https://github.com/w3c/webappsec-permissions-policy/blob/main/features.md)에서 전체 권한 옵션 목록을 볼 수 있습니다.

```jsx
{
  key: 'Permissions-Policy',
  value: 'camera=(), microphone=(), geolocation=(), browsing-topics=()'
}
```

### \***\*[X-Content-Type-Options](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-content-type-options)\*\***

이 헤더는 `Content-Type` 헤더가 명시적으로 설정되지 않은 경우 브라우저가 콘텐츠 유형을 추측하려고 시도하는 것을 방지합니다. 이렇게 하면 사용자가 파일을 업로드하고 공유할 수 있는 웹사이트에 대한 XSS 익스플로잇을 방지할 수 있습니다. 예를 들어 사용자가 이미지를 다운로드하려고 하는데 실행 파일과 같은 다른 `Content-Type`으로 처리되면 악의적일 수 있습니다. 이 헤더는 브라우저 확장 프로그램 다운로드에도 적용됩니다. 이 헤더에 유효한 유일한 값은 `nosniff`입니다.

```jsx
{
  key: 'X-Content-Type-Options',
  value: 'nosniff'
}
```

### \***\*[Referrer-Policy](https://nextjs.org/docs/app/api-reference/next-config-js/headers#referrer-policy)\*\***

이 헤더는 현재 웹사이트(원본)에서 다른 웹사이트로 이동할 때 브라우저에 포함할 정보의 양을 제어합니다. [여기](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)에서 다양한 옵션에 대해 알아볼 수 있습니다.

```jsx
{
  key: 'Referrer-Policy',
  value: 'origin-when-cross-origin'
}
```

### \***\*[Content-Security-Policy](https://nextjs.org/docs/app/api-reference/next-config-js/headers#content-security-policy)\*\***

이 헤더는 cross-site scripting(XSS), 클릭재킹 및 기타 코드 인젝션 공격을 방지하는 데 도움이 됩니다. Content Security Policy(CSP)는 스크립트, 스타일시트, 이미지, 글꼴, 객체, 미디어(오디오, 비디오), 아이프레임 등을 포함한 콘텐츠의 허용된 출처를 지정할 수 있습니다.

[여기](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)에서 다양한 CSP 옵션에 대해 알아볼 수 있습니다.

템플릿 문자열을 사용하여 콘텐츠 보안 정책 지시문을 추가할 수 있습니다.

```jsx
// 보안 헤더를 정의하기 전에
// 템플릿 문자열을 사용하여 콘텐츠 보안 정책 지시문을 추가합니다.

const ContentSecurityPolicy = `
  default-src 'self';
  script-src 'self';
  child-src example.com;
  style-src 'self' example.com;
  font-src 'self';
`;
```

지시문에 `self`와 같은 키워드를 사용하는 경우 작은따옴표 `''`로 묶어야 합니다.

헤더의 값에서 새 줄을 공백으로 바꿉니다.

```jsx
{
  key: 'Content-Security-Policy',
  value: ContentSecurityPolicy.replace(/\s{2,}/g, ' ').trim()
}
```

### **[References](https://nextjs.org/docs/app/api-reference/next-config-js/headers#references)**

- [MDN](https://developer.mozilla.org/)
- [Varun Naik](https://blog.vnaik.com/posts/web-attacks.html)
- [Scott Helme](https://scotthelme.co.uk/)
- [Mozilla Observatory](https://observatory.mozilla.org/)
