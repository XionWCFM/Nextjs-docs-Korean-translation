원본링크 : https://nextjs.org/docs/app/api-reference/next-config-js/exportPathMap

<br>

# **exportPathMap (Deprecated)**

> 이 기능은 `next export`에만 제공되며 현재 `pages`가 있는 `getStaticPaths` 또는 `app`이 있는 `generateStaticParams`를 위해 더 이상 사용되지 않습니다.

<br>

### **Examples**

- [Static Export](https://github.com/vercel/next.js/tree/canary/examples/with-static-export)

`exportPathMap`을 사용하면 내보낼 때 사용할 페이지 대상에 대한 요청 경로 매핑을 지정할 수 있습니다. `exportPathMap`에 정의된 경로는 [`next dev`](https://nextjs.org/docs/app/api-reference/next-cli#development) 도구를 사용할 때에도 사용할 수 있습니다.

<br>

다음 페이지에서 앱에 대한 사용자 지정 `exportPathMap`을 만드는 예제부터 시작하겠습니다:

- `pages/index.js`
- `pages/about.js`
- `pages/post.js`

<br>

`next.config.js`를 열고 다음 `exportPathMap` 구성을 추가합니다:
next.config.js

```jsx
module.exports = {
  exportPathMap: async function (
    defaultPathMap,
    { dev, dir, outDir, distDir, buildId }
  ) {
    return {
      "/": { page: "/" },
      "/about": { page: "/about" },
      "/p/hello-nextjs": { page: "/post", query: { title: "hello-nextjs" } },
      "/p/learn-nextjs": { page: "/post", query: { title: "learn-nextjs" } },
      "/p/deploy-nextjs": { page: "/post", query: { title: "deploy-nextjs" } },
    };
  },
};
```

> **Note**: `exportPathMap`의 `query` 필드는 [자동으로 정적으로 최적화된 페이지](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization) 또는 [`getStaticProps` 페이지](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)와 함께 사용할 수 없으며, 빌드 시 HTML 파일로 렌더링되므로 `next export` 시 추가 쿼리 정보를 제공할 수 없습니다.

<br>

그러면 페이지가 HTML 파일로 내보내집니다, 예, `/about`은 `/about.html`이 됩니다.

<br>

`exportPathMap`은 두 개의 인수를 받는 `async` 함수로, 첫 번째 인수는 Next.js에서 사용하는 기본 맵인 `defaultPathMap`입니다. 두 번째 인수는 객체입니다:

- `dev` - 개발 중에 `exportPathMap`이 호출되면 `true`. `next export`를 실행하면 `false`입니다. 개발에서 `exportPathMap`은 경로를 정의하는 데 사용됩니다.
- `dir` - 프로젝트 디렉터리의 절대 경로
- `outDir` - `out/` 디렉터리의 절대 경로(`[-o`로 구성 가능](https://nextjs.org/docs/app/api-reference/next-config-js/exportPathMap#customizing-the-output-directory)). `dev`가 `true`이면 `outDir`의 값은 `null`이 됩니다.
- `distDir` - `.next/` 디렉터리의 절대 경로(`[distDir](https://nextjs.org/docs/pages/api-reference/next-config-js/distDir)` config로 구성 가능)
- `buildId` - 생성된 빌드 ID

<br>

반환되는 객체는 `key`가 `pathname`이고 `value`이 다음 필드를 허용하는 객체인 페이지 맵입니다:

- `page`: `String` - 렌더링할 `pages` 디렉토리 내의 페이지
- `query`: `Object` - 사전 렌더링할 때 `getInitialProps`에 전달된 `query` 객체입니다. 기본값은 `{}`입니다.

> 내보낸 경로명은 `pathname`(예: `/readme.md`)일 수도 있지만, 콘텐츠를 제공할 때 `Content-Type` 헤더를 `.html`과 다른 경우 `text/html`로 설정해야 할 수 있습니다.

<br>

---

<br>

## **[Adding a trailing slash](https://nextjs.org/docs/app/api-reference/next-config-js/exportPathMap#adding-a-trailing-slash)**

페이지를 `index.html` 파일로 내보내고 trailing slashes가 필요하도록 Next.js를 구성할 수 있으며, `/about`은 `/about/index.html`이 되고 `/about/`을 통해 라우팅할 수 있습니다. 이는 Next.js 9 이전의 기본 동작이었습니다.

<br>

다시 전환하여 trailing slashes를 추가하려면 `next.config.js`를 열고 `trailingSlash` 구성을 활성화합니다:
next.config.js

```jsx
module.exports = {
  trailingSlash: true,
};
```

<br>

---

<br>

## **[Customizing the output directory](https://nextjs.org/docs/app/api-reference/next-config-js/exportPathMap#customizing-the-output-directory)**

[`next export`](https://nextjs.org/docs/pages/building-your-application/deploying/static-exports)는 기본 출력 디렉토리로 사용되며, 다음과 같이 `-o` 인수를 사용하여 이를 사용자 지정할 수 있습니다:

Terminal

```bash
next export -o outdir
```

> Warning: `exportPathMap` 사용은 더 이상 사용되지 않으며 `pages` 내부의 `getStaticPaths`로 재정의됩니다. 함께 사용하지 않는 것이 좋습니다.
