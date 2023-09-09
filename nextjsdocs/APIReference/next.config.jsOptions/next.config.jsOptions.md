# **next.config.js Options**

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js](https://nextjs.org/docs/app/api-reference/next-config-js)

Next.js는 프로젝트 루트 디렉토리에 위치한 `next.config.js` 파일을 통하여 구성될 수 있습니다. (예: `package.json`)

```js
// next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  /* 구성 옵션이 위치하는 곳입니다. */
}
 
module.exports = nextConfig
```

`next.config.js`는 JSON 파일이 아닌 일반 Node.js 모듈입니다. Next.js 서버와 빌드 단계에 사용되며, 브라우저 빌드에는 포함되지 않습니다.

[ECMAScript 모듈](https://nodejs.org/api/esm.html)이 필요하다면, `next.config.mjs`를 사용할 수 있습니다.

```mjs
// next.config.mjs

/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* 구성 옵션이 위치하는 곳입니다. */
}
 
export default nextConfig
```

함수를 활용할 수도 있습니다.

```mjs
// next.config.mjs

module.exports = (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* 구성 옵션이 위치하는 곳입니다. */
  }
  return nextConfig
}
```

Next.js 버전 12.1.0부터는 async 함수를 사용할 수 있습니다.

```js
// next.config.js

module.exports = async (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* 구성 옵션이 위치하는 곳입니다. */
  }
  return nextConfig
}
```

`phase`는 구성이 로드된 현재 컨텍스트를 나타냅니다. [사용가능한 phase](https://github.com/vercel/next.js/blob/5e6b008b561caf2710ab7be63320a3d549474a5b/packages/next/shared/lib/constants.ts#L19-L23)를 확인할 수 있습니다. `phase`는 `next/constants`로부터 import할 수 있습니다.

```js
const { PHASE_DEVELOPMENT_SERVER } = require('next/constants')
 
module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* 개발 단계에만 적용되는 구성 옵션이 위치하는 곳입니다. */
    }
  }
 
  return {
    /* 개발 단계를 제외한 모든 단계에 적용되는 구성 옵션이 위치하는 곳입니다. */
  }
}
```

주석 처리된 줄은 `next.config.js`에 의해 허용된 구성을 입력할 수 있는 곳입니다. 구성 목록은 [이 파일](https://github.com/vercel/next.js/blob/canary/packages/next/src/server/config-shared.ts)에 정의되어 있습니다.

그러나 그 어떠한 구성도 필수가 아니며 각각의 구성이 어떤 역할을 하는지 이해하는 것은 중요하지 않습니다. 대신에 이 섹션에서 활성화하거나 수정해야 하는 기능에 대하여 찾아보십시오. 무엇을 해야할지 보여줄 것입니다.

> 대상 Node.js 버전에서 사용할 수 없는 새로운 JavaScript 기능을 사용하는 것을 피하십시오. `next.config.js`는 Webpack, Babel, TypeScript에 의하여 파싱되지 않을 것입니다.

이 페이지는 모든 사용가능한 구성 옵션을 정리합니다.

> [appDir](./appDir.md)
>
> 레이아웃, 스트리밍 등을 사용하기 위하여 앱 라우터를 활성화합니다.

> [assetPrefix](./assetPrefix.md)
>
> CDN을 구성하기 위해 assetPrefix 구성 옵션을 사용하는 방법을 알아봅니다.

> [basePath](./basePath.md)
>
> 도메인의 하위경로 하에서 Next.js 애플리케이션을 배포하기 위해 'basePath'를 사용합니다.

> [compress](./compress.md)
>
> Next.js는 렌더링된 콘텐츠와 정적 파일을 압축하기 위하여 gzip 압축을 제공하며, 서버 타겟에만 작동합니다. 더 알아봅니다.

> [devIndicators](./devlndicators.md)
>
> 최적화된 페이지는 정적으로 최적화되었는지 알 수 있도록 하는 지표를 포함합니다. 여기에서 적용을 해제할 수 있습니다.

> [distDir](./distDir.md)
>
> 기본 .next 디렉토리 대신 사용할 커스텀 빌드 디렉토리를 설정합니다.

> [env](./env.md)
>
> 빌드 시 Next.js 애플리케이션에 환경 변수를 추가하고 이것에 접근하는 법을 알아봅니다.

> [eslint](./eslint.md)
>
> Next.js는 기본적으로 빌드 중에 발생한 ESLint 오류와 경고를 보고합니다. 이것을 적용하지 않는 방법을 알아봅니다.

> [exportPathMap](./exportPathMap.md)
>
> `next export`를 사용했을 때 HTML 파일로써 내보내기될 페이지를 커스터마이징합니다.

> [generateBuildId](./generateBuildld.md)
>
> 빌드 id를 구성합니다. 이것은 현재 빌드를 식별하기 위하여 사용됩니다.

> [generateEtags](./generateEtags.md)
>
> Next.js는 기본적으로 모든 페이지에 대하여 etag를 생성합니다. etag 생성을 비활성화하는 방법을 알아봅니다.

> [headers](./headers.md)
>
> Next.js 앱에 커스텀 HTTP 헤더를 추가합니다.

> [httpAgentOptions](./httpAgentOptions.md)
>
> Next.js는 기본적으로 자동으로 HTTP Keep-Alive를 사용합니다. HTTP Keep-Alive를 비활성화하는 방법을 알아봅니다.

> [images](./images.md)
>
> next/image 로더에 대한 구성을 커스텀합니다.

> [incrementalCacheHandlerPath](./incrementalCacheHandlerPath.md)
>
> 데이터 저장 및 재확인을 위해 사용되는 Next.js 캐시를 구성합니다.

> [mdxRs](./mdxRs.md)
>
> 앱 라우터 내에서 MDX 파일을 컴파일하기 위하여 Rust 컴파일러를 사용합니다.

> [onDemandEntries](./onDemandEntries.md)
>
> Next.js가 개발 시 생성된 페이지를 어떻게 폐기하고 메모리에 보존할지 구성합니다.

> [output](./output.md)
>
> Next.js는 애플리케이션의 편리한 배포를 위하여 자동적으로 각각의 페이지에 어떠한 파일이 필요한지 추적합니다. 

> [pageExtensions](./pageExtensions.md)
>
> 페이지 라우터 안의 페이지를 해석할 때에 Next.js에 의해 사용되는 페이지 확장자의 범위를 확장합니다.

> [poweredByHeader](./poweredByHeader.md)
>
> Next.js는 자동으로 `x-powered-by` 헤더를 추가합니다. 이것을 적용하지 않는 방법을 알아봅니다.

> [productionBrowserSourceMaps](./productionBrowserSourceMaps.md)
>
> 프로덕션 빌드 중 브라우저 소스맵 생성을 활성화합니다.

> [reactStrictMode](./reactStrictMode.md)
>
> 완전한 Next.js 런타임은 이제 엄격 모드에 부응합니다. 적용하는 방법에 대하여 알아봅니다.

> [redirects](./redirects.md)
>
> Next.js 앱에 redirects를 추가합니다.

> [rewrites](./rewrites.md)
>
> Next.js 앱에 rewrites를 추가합니다.

> [serverComponentsExternalPackages](./serverComponentsExternalPackages.md)
>
> 서버 컴포넌트 번들링으로부터 특정 의존성의 적용을 해제하고 Node.js의 `require`를 사용합니다.

> [trailingSlash](./trailingSlash.md)
>
> Next.js 페이지의 후행 슬래시 해석 여부를 구성합니다.

> [transpilePackages](./transpilePackages.md)
>
> 로컬 패키지(모노레포와 같은) 또는 외부 의존성으로부터 자동적으로 의존성을 트랜스파일 및 번들링합니다.

> [turbo](./turbo.md)
>
> Next.js를 터보팩 특정 옵션으로 구성합니다.

> [typedRoutes](./typedRoutes.md)
>
> 정적으로 타이핑된 링크에 대한 실험적인 지원을 활성화합니다.

> [typescript](./typescript.md)
>
> Next.js는 기본적으로 TypeScript 에러를 보고합니다. 이러한 동작의 적용을 해제하는 방법을 알아봅니다.

> [urlImports](./urlImports.md)
>
> Next.js가 외부 URL로부터 모듈을 가져오는 것을 허용하도록 구성합니다. (실험적 기능)

> [webpack](./webpack.md)
>
> Next.js에 의해 사용되는 웹팩 구성을 커스터마이징하는 법을 알아봅니다.

> [webVitalsAttribution](./webVitalsAttribution.md)
>
> Web Vital 이슈를 짚어내기 위해 webVitalsAttribution 옵션을 사용하는 법을 알아봅니다.