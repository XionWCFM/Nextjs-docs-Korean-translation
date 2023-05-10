# 메타데이터

## [동적 이미지 생성](#동적-이미지-생성)
> 요구 사항 : Next.js v12.2.3 이상.

`ImageResponse` 생성자를 사용하면 JSX 및 CSS를 사용하여 동적 이미지를 생성할 수 있습니다. 이것은 Open Graph 이미지, Twitter 카드 등과 같은 소셜 미디어 이미지를 만드는 데 유용합니다.

<br>

`ImageResponse`는 [Edge Runtime](../../BuildingYourApplication/Rendering/Edge_and_Node.js_Runtimes.md)을 사용하고 Next.js는 edge의 캐시된 이미지에 적절한 헤더를 자동으로 추가하여 성능을 개선하고 재계산을 줄이는 데 도움을 줍니다.

<br>

이것을 사용하기 위해서는 `next/server`에서 `ImageResponse`를 가져옵니다.


```javascript
// app/about/route.js
import { ImageResponse } from 'next/server';

export default function () {
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          textAlign: 'center',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        Hello world!
      </div>
    ),
    {
      width: 1200,
      height: 600,
    },
  );
}
```

`ImageResponse`는 [Route Handlers](../Routing/Route_Handlers.md) 및 파일 기반 메타데이터를 포함한 다른 Next.js API와 잘 통합됩니다. 예를 들어, 
`opengraph-image.tsx` 파일에서 `ImageResponse`를 사용하여 빌드 시간에 또는 요청 시간에 동적으로 Open Graph 이미지를 생성할 수 있습니다.

<br>

### [스타일링](#스타일링)

`ImageResponse`는 flexbox 및 absolute 포지션 지정, 사용자 정의 글꼴, 텍스트 줄 바꿈, 가운데 정렬 및 중첩된 이미지를 비롯한 일반적인 CSS 속성을 지원합니다. [지원되는 CSS 속성의 전체 목록을 참조하십시오.](../../APIReference/Functions/imageResponse.md)

>__알아두면 좋은 정보__: [Vercel OG Playground](https://og-playground.vercel.app/) 에서 관련 예제를 참고할 수 있습니다.

<br>

### [알아두면 좋은 정보](#알아두면-좋은-정보)

* `ImageResponse`는 [@vercel/og](https://vercel.com/docs/concepts/functions/edge-functions/og-image-generation), [Satori](https://github.com/vercel/satori) 및 Resvg를 사용하여 HTML 및 CSS를 PNG로 변환합니다.
* Edge 런타임만 지원됩니다. 기본 Node.js 런타임은 작동하지 않습니다.
* flexbox 및 CSS 속성의 서브셋만 지원됩니다. 고급 레이아웃(예: display: grid)은 작동하지 않습니다.
* 최대 번들 크기는 500KB입니다. 번들 크기에는 JSX, CSS, 글꼴, 이미지 및 기타 assets이 포함됩니다. 한도를 초과하면 assets의 크기를 줄이거나 런타임에 불러오는 것을 고려하십시오.
* `ttf`, `otf`, `woff` 글꼴 형식만 지원됩니다. 글꼴 구문 분석 속도를 최대화하려면 `woff`보다는 `ttf` or `otf`가 선호됩니다.

<br><hr><br>

## [SEO](#seo)
> __참고:__  `13.2`에서 도입된, 메타데이터를 통한 내장 SEO 지원은 이전 `head.js` 구현을 대체합니다. `head.js` 특수 파일은 `13.2`에서 더 이상 사용되지 않으며 향후 버전에서 제거될 예정입니다. [마이그레이션 가이드]()를 확인하세요.

Next.js를 사용하면 모든 레이아웃이나 페이지에서 명시적인 메타데이터 구성으로 메타데이터를 정의(예를 들면 HTML `head` 엘리먼트 속 `meta`와 `link`태그)할 수 있습니다. 

<br>

### [예](#예)
`generateMetadata`를 통한 정적 및 동적 메타데이터는 모두 서버 컴포넌트에서만 지원됩니다.

#### [정적 메타데이터](#정적-메타데이터)
```javascript
// app/page.tsx

import type { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Home',
  description: 'Welcome to Next.js',
};
 
export default function Page() {
  return '...';
}
```

#### [동적 메타데이터](#동적-메타데이터)
`generateMetadata`를 사용하여 동적 값이 필요한 메타데이터를 `fetch`할 수 있습니다.

``` javascript
// app/products/[id]/page.tsx

import type { Metadata } from 'next';

// 'fetch'로 직접 API 요청을 할 수 없는 경우, 'cache'를 사용할 수 있습니다. 
// 'fetch' 응답은 아래의 두 함수에서 하나의 API 요청으로 캐시되고 재사용됩니다.
// 자세한 내용은 다음 링크를 참고하세요: 
// https://beta.nextjs.org/docs/data-fetching/caching
async function getProduct(id) {
  const res = await fetch(`https://.../api/products/${id}`);
  return res.json();
}
 
export async function generateMetadata({ params }): Promise<Metadata> {
  const product = await getProduct(params.id);
  return { title: product.title };
}
 
export default async function Page({ params }) {
  const product = await getProduct(params.id);
  // ...
}
```
__알아두면 좋은 정보:__

* 라우트를 렌더링할 때, Next.js는 `generateMetadata`, `generateStaticParams`, 레이아웃, 페이지 및 서버 컴포넌트에서 동일한 데이터에 대한 `fetch` 요청을 [자동으로 중복 제거합니다.](../DataFetching/)`fetch`를 사용할 수 없는 경우, [React `cache`를 사용할 수](../DataFetching/Caching.md) 있습니다.

* Next.js는 UI를 클라이언트로 스트리밍하기 전에 `generateMetadata` 내부의 데이터 가져오기가 완료되기를 기다립니다. 이렇게 하면 [스트리밍된 응답](../Routing/Loading_UI_and_Streaming.md)의 첫 부분에 `<head>` 태그가 포함됩니다.

<br>

#### [JSON-LD](#json-ld)
[JSON-LD](https://json-ld.org/)는 검색 엔진이 콘텐츠를 이해하는 데 사용할 수 있는 구조화된 데이터 형식입니다. 예를 들어 사람, 이벤트, 단체, 영화, 책, 레시피 및 기타 여러 유형의 entities를 설명하는 데 사용할 수 있습니다.

현재 JSON-LD에 대한 권장 사항은 구조화된 데이터를 `layout.js`나 `page.js` 컴포넌트에서 `<script>` 태그로 렌더링하는 것입니다 . 예를 들어:

``` javascript

// app/products/[id]/page.tsx

export default async function Page({ params }) {
  const product = await getProduct(params.id);
 
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'Product',
    name: product.name,
    image: product.image,
    description: product.description,
  };
 
  return (
    <section>
      {/* 페이지에 JSON-LD를 추가하세요. */}
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      {/* ... */}
    </section>
  );
}
```
Google의 [Rich Results Test](https://search.google.com/test/rich-results) 또는 일반적인 [Schema Markup Validator](https://validator.schema.org/)를 사용하여 구조화된 데이터를 유효성 검사하고 테스트할 수 있습니다. 

[schema-dts](https://www.npmjs.com/package/schema-dts) 와 같은 다음과 같은 커뮤니티 패키지를 사용하여 TypeScript로 JSON-LD를 입력할 수 있습니다. :

``` javascript

import { Product, WithContext } from 'schema-dts';
 
const jsonLd: WithContext<Product> = {
  '@context': 'https://schema.org',
  '@type': 'Product',
  name: 'Next.js Sticker',
  image: 'https://nextjs.org/imgs/sticker.png',
  description: 'Dynamic at the speed of static.',
};
```

## [API 참조](#api-참조)
메타데이터 API 옵션 모두 보기

<br>

> ### [메타데이터 생성](../../APIReference/Functions/generateMetadata.md) <br>
>  검색 엔진 최적화(SEO)를 위해 Next.js 애플리케이션에 메타데이터를 추가하는 방법을 참조하세요.