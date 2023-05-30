원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/assetPrefix

<br>

# **assetPrefix**

> Attention: [Vercel에 배포](https://nextjs.org/docs/pages/building-your-application/deploying)하면 Next.js 프로젝트에 대한 글로벌 CDN이 자동으로 구성됩니다. Asset Prefix를 수동으로 설정할 필요가 없습니다.

> Note: Next.js 9.5 이상에서는 사용자 정의 가능한 [기본 경로](https://nextjs.org/docs/app/api-reference/next-config-js/basePath)에 대한 지원이 추가되었으며, 이는 `/docs`와 같은 하위 경로에서 애플리케이션을 호스팅하는 데 더 적합합니다. 이 사용 사례에는 사용자 정의 Asset Prefix를 사용하지 않는 것이 좋습니다.

[CDN](https://en.wikipedia.org/wiki/Content_delivery_network)을 설정하려면 asset prefix를 설정하고 Next.js가 호스팅되는 도메인으로 확인하도록 CDN의 오리진을 구성하면 됩니다.

<br>

`next.config.js`를 열고 `assetPrefix` 구성을 추가합니다:

next.config.js

```jsx
const isProd = process.env.NODE_ENV === "production";

module.exports = {
  // 운영 환경에서는 CDN을 사용하고 개발 환경에서는 로컬호스트를 사용하세요.
  assetPrefix: isProd ? "https://cdn.mydomain.com" : undefined,
};
```

<br>

Next.js는 `/_next/` 경로(`.next/static/` 폴더)에서 로드하는 JavaScript 및 CSS 파일에 asset prefix를 자동으로 사용합니다. 예를 들어, 위의 구성에서는 다음과 같이 JS 청크를 요청합니다:

```
/_next/static/chunks/4b9b41aaa062cbbfeff4add70f256968c51ece5d.4d708494b3aed70c04f0.js
```

<br>

대신 다음과 같이 됩니다.:

```
https://cdn.mydomain.com/_next/static/chunks/4b9b41aaa062cbbfeff4add70f256968c51ece5d.4d708494b3aed70c04f0.js
```

특정 CDN에 파일을 업로드하기 위한 정확한 구성은 선택한 CDN에 따라 달라집니다. CDN에서 호스팅해야 하는 폴더는 `.next/static/`의 콘텐츠뿐이며, 위의 URL 요청에 표시된 대로 `_next/static/`으로 업로드해야 합니다. 서버 코드 및 기타 구성이 공개되어서는 안 되므로 **나머지 `.next/` 폴더는 업로드하지 마세요.**

<br>

`assetPrefix`는 `_next/static`에 대한 요청을 포함하지만, 다음 경로에는 영향을 미치지 않습니다:

- [public](https://nextjs.org/docs/pages/building-your-application/optimizing/static-assets) 폴더에 있는 파일; CDN을 통해 해당 자산을 제공하려면 접두사를 직접 도입해야 합니다.
- `/_next/data/`에서 `getServerSideProps` 페이지를 요청합니다. 이러한 요청은 정적이 아니므로 항상 기본 도메인에 대해 이루어집니다.
- `/_next/data/`에서 `getStaticProps` 페이지를 요청합니다. 이러한 요청은 일관성을 위해 [Incremental Static Generation](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)을 사용하지 않더라도 항상 기본 도메인에 대해 이루어집니다.
