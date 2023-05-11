# 이미지 최적화

<details>
<summary>예시</summary>
<div>

- [이미지 컴포넌트](https://github.com/vercel/next.js/tree/canary/examples/image-component)

</div>
</details>

Next.js 이미지 컴포넌트는 HTML의 <img> 엘리먼트를 확장하여 다음과 같은 기능을 제공합니다:

* __향상된 성능:__ 최신 이미지 형식을 사용하여 각 디바이스에 대한 적절한 크기의 이미지 제공
* __시각적 안정성:__ 자동으로 [누적 레이아웃 이동(CLS)](https://nextjs.org/learn/seo/web-performance/cls) 방지
* __더 빠른 페이지 로드:__ 이미지가 뷰포트에 들어갈 때만 로드되며 선택적 blur-up placeholder가 있습니다.
* __자산(Assets) 유연성:__ 원격 서버에 저장된 이미지에 대해서도 필요한 크기로 즉시 이미지 크기 조정

<br><hr><br>

## [사용법](#사용법)
로컬 이미지를 사용하기 위해서는 `.jpg`, `.png`또는 `.webp`파일을 `import`합니다:

```javascript
import Image from 'next/image';
```
이제 이미지에 대한 src를 정의할 수 있게 되었습니다. (로컬 혹은 원격 모두 가능합니다.)

<br><hr><br>


### [로컬 이미지](#로컬-이미지)
로컬 이미지를 사용하려면 .jpg, .png 또는 .webp 파일을 가져옵니다:

```javascript
import profilePic from '../assets/me.png';
```
동적인 `await import()` 또는 `require()`는 지원되지 않습니다. `import`는 빌드타임 때 분석할 수 있도록 정적이어야 합니다.

Next.js는 가져온 파일을 기반으로 이미지의 `width`및 `height`를 [자동으로 결정합니다.](#이미지-크기-조정) 이 값은 이미지가 로드되는 동안 [누적 레이아웃 이동(CLS)](https://nextjs.org/learn/seo/web-performance/cls)을 방지하는 데 사용됩니다.

```javascript
import Image from 'next/image';
import profilePic from '../public/me.png';
 
export default function Page() {
  return (
    <Image
      src={profilePic}
      alt="Picture of the author"
      // width={500} automatically provided
      // height={500} automatically provided
      // blurDataURL="data:..." automatically provided
      // placeholder="blur" // Optional blur-up while loading
    />
  );
} 
```
<br><hr><br>

### [원격 이미지](#원격-이미지)
원격 이미지를 사용하려면 상대 경로든 절대 경로든 `src` 속성이 URL 문자열이어야 합니다.

Next.js는 빌드 과정 중에 원격 파일에 액세스할 수 없기 때문에 및 [`width`](../../APIReference/Components/Image.md), [`height`](../../APIReference/Components/Image.md#) 속성과 선택적으로 [`blurDataURL`](../../APIReference/Components/Image.md#) 속성을 수동으로 제공해야 합니다 .

`width`및 `height`속성은 이미지의 적절한 종횡비를 추론하고 이미지가 로드될 때 레이아웃이 이동하는 것을 방지하기 위해 사용됩니다. `width`및 `height`은 이미지 파일의 실제 렌더링 크기를 결정 하지 않습니다. [이미지 크기 조정](#이미지-크기-조정)에 대해 자세히 알아보세요.

```javascript
import Image from 'next/image';
 
export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  );
}
```

이미지 최적화를 안전하게 허용하려면 `next.config.js` 파일에서 지원되는 URL 패턴 목록을 정의하세요. 악의적인 사용을 방지하기 위해 가능한 한 구체적으로 작성하십시오. 예를 들어 다음과 같은 구성은 특정 AWS S3 버킷에서만 이미지를 허용합니다.

```javascript
// next.config.js

module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 's3.amazonaws.com',
        port: '',
        pathname: '/my-bucket/**',
      },
    ],
  },
};
```

[`remotePatterns`](../../APIReference/Components/Image.md) 구성에 대해 자세히 알아보세요. 이미지 `src`에 대한 상대 URL을 사용하려면 [`loader`](../../APIReference/Components/Image.md)를 사용하세요.

<br>

### [도메인](#도메인)
경우에 따라 내장 Next.js 이미지 최적화 API를 사용하면서 원격 이미지를 최적화할 수 있습니다. 이렇게 하기 위해서는, `loader`를 기본 설정으로 둔 채로 이미지 `src` 속성에 대한 절대 URL을 입력하면 됩니다.

악의적인 사용자로부터 애플리케이션을 보호하려면 `next/image` 컴포넌트를 사용할 때 함께 사용할 원격 호스트 이름 목록을 정의해야 합니다.

> [`remotePatterns`](../../APIReference/Components/Image.md) 구성에 대해 자세히 알아보세요 .

<br>

### [Loaders](#loaders)
[이전 예제](#원격-이미지)에서는 원격 이미지를 위해 부분 URL(`"/me.png"`)을 제공했습니다. 이는 loader 아키텍처로 인해 가능한 일이었습니다.

loader는 이미지의 URL을 생성하는 함수입니다. 이것은 제공된 `src`를 수정하고, 다른 크기로 이미지를 요청하는 여러 URL을 생성합니다. 이러한 다중 URL은 자동 [srcset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/srcset) 생성에 사용되어, 사이트 방문자가 보는 화면 크기에 적합한 크기의 이미지가 제공됩니다.

Next.js 애플리케이션의 기본 loader는 내장된 이미지 최적화 API를 사용하여 웹의 어느 곳에서든 이미지를 최적화한 후 Next.js 웹 서버에서 직접 제공합니다. 이미지를 CDN이나 이미지 서버에서 직접 제공하려면 몇 줄의 JavaScript로 자신만의 loader 함수를 작성할 수 있습니다.

[loader의 속성](../../APIReference/Components/Image.md)을 사용하여 이미지별로 loader를 정의하거나 [`loaderFile` 구성](../../APIReference/Components/Image.md)을 사용하여 애플리케이션 수준에서 loader를 정의할 수 있습니다.

<br><hr><br>


## [Priority](#priority)

각 페이지에서 [최대 콘텐츠풀 페인트 (LCP)](https://web.dev/lcp/#what-elements-are-considered)가 될 이미지에 `priority` 속성을 추가해야 합니다. 이렇게 하면 Next.js가 해당 이미지에 특별히 로딩 우선순위를 부여하여 (예를 들면 preload 태그나 priority 힌트 등을 통해) LCP를 유의미하게 높일 수 있는 결과를 이끕니다.

일반적으로 LCP 엘리먼트는 페이지의 뷰포트 내에서 가장 큰 이미지나 텍스트 블록입니다. `next dev`를 실행할 때, LCP 요소가 `priority` 속성이 없는 `<Image>`인 경우 콘솔에 경고 메시지가 표시됩니다.

LCP 이미지를 식별한 후 다음과 같이 속성을 추가할 수 있습니다.

```javascript

import Image from 'next/image';
import profilePic from '../public/me.png';
 
export default function Page() {
  return <Image src={profilePic} alt="Picture of the author" priority />;
}
```

priority에 대한 자세한 내용은 [next/image 컴포넌트 문서](../../APIReference/Components/Image.md)를 참조하십시오.

<br><hr><br>

## [이미지 크기 조정](#이미지-크기-조정)
이미지가 일반적으로 가장 성능을 저하시키는 방법 중 하나는 이미지가 로드되면서 페이지의 다른 요소를 밀어내는 _레이아웃 이동(layout shift)_입니다. 이 성능 문제는 사용자에게 매우 불쾌한 성능 문제이며, 이에 따라 Core Web Vital 중 하나인 [누적 레이아웃(Cumulative Layout Shift)](https://web.dev/cls/)가 존재합니다. 옮기다. 이미지 기반 레이아웃 이동을 피하는 방법은 [항상 이미지 크기를 지정](https://web.dev/optimize-cls/#images-without-dimensions)하는 것입니다. 이렇게 하면 브라우저는 이미지가 로드되기 전에 이미지를 위한 충분한 공간을 정확하게 남겨둘 수 있습니다.

`next/image`는 좋은 성능 결과를 보장하도록 설계되었기 때문에, 레이아웃 이동에 기여하는 방식으로 사용할 수 없으며 따라서 무조건 다음 세 가지 방법 중 하나로 크기를 지정해야 합니다.

1. [정적 가져오기(static import)](#로컬-이미지)를 사용하여 자동으로 크기 조정
2. [`width`](../../APIReference/Components/Image.md)와 [`height`](../../APIReference/Components/Image.md)속성을 포함하여 명시적으로 크기 지정 
3. 이미지가 확장되어 부모 요소를 채우는 [fill](../../APIReference/Components/Image.md)을 사용하여 암시적으로 크기 지정


>__이미지의 크기를 모른다면 어떻게 해야 하나요?__ <br>
>이미지 크기에 대한 지식 없이 소스에서 이미지에 접근하는 경우 다음과 같은 몇 가지 작업을 수행할 수 있습니다.<br>
>* __`fill` 사용__<br>
>[`fill`](../../APIReference/Components/Image.md) 속성을 사용하면 부모 요소에 따라 이미지 크기를 조정할 수 있습니다. 미디어 쿼리의 어떠한 break points도 만족시키도록 CSS를 사용하여 이미지의 부모요소에게 [`sizes`](../../APIReference/Components/Image.md) 속성과 함께 페이지의 공간을 제공하는 것을 고려하십시오. 또한 `fill`과 [`object-fit`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit), `contain`, 또는 `cover`, 및 [`object-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position)를 사용하여 이미지가 공간을 어떻게 차지할지를 정의합니다.<br>
>* __이미지 정규화__<br>
>소스를 제어할 수 있는 경우, 이미지 파이프라인을 수정하여 이미지를 특정 크기로 정규화하는 것을 고려할 수 있습니다.<br>
>* __API 호출 수정__<br>
>애플리케이션이 API 호출(예를 들면 CMS)을 사용하여 이미지 URL을 검색하는 경우, URL과 함께 이미지 크기를 반환하도록 API 호출을 수정할 수 있습니다.

이미지 크기 지정을 위해 제안된 방법 중 어느 것도 작동하지 않는 경우, `next/image` 컴포넌트는 페이지에서 표준 `<img>` 엘리먼트와 함께 잘 작동하도록 설계되었습니다.<br>

<br><hr><br>

## [스타일링](#스타일링)
이미지 컴포넌트의 스타일 지정은 일반 `<img>` 엘리먼트의 스타일 지정과 비슷하지만 유의해야 할 몇 가지 지침을 염두에 두어야 합니다.

* `styled-jsx`이 아닌, `className` 또는 `style`을 사용하세요.
  * 대부분의 경우, `className` 속성을 사용하는 것이 좋습니다. 이는 가져온 [CSS 모듈](../Styling/CSS_Modules.md), [전역 스타일시트](../Styling/CSS_Modules.md) 등일 수 있습니다.
  * 인라인 스타일을 부과하기 위해 `style` 속성을 사용할 수도 있습니다.
  * [styled-jsx](../Styling/CSS-in-js.md)는 현재 컴포넌트로 스코프가 지정되어 있기 때문에 (스타일을 `global`로 표시하지 않는 한).사용할 수 없습니다 
* `fill`을 사용하는 경우, 부모 요소에 `position: relative`이 있어야 합니다.
  * 이는 해당 레이아웃 모드에서 이미지 엘리먼트를 적절하게 렌더링하는 데 필요합니다.
* `fill`을 사용하는 경우, 부모 요소에 `display: block`이 있어야 합니다.
  * 이는 `<div>`엘리먼트의 기본값이지만 그렇지 않은 경우에는 지정해야 합니다.

예제는 [이미지 컴포넌트 Demo](https://image-component.nextjs.gallery/)를 참조하십시오.

### [예](#예)
다양한 스타일과 함께 사용된 이미지 컴포넌트의 예시는 [이미지 컴포넌트 Demo](https://image-component.nextjs.gallery/)에서 확인할 수 있습니다.

## [기타 속성](#기타-속성)
[`next/image` 컴포넌트에 사용할 수 있는 모든 속성을 참조할 수 있습니다.](../../APIReference/Components/Image.md) 

<br><hr><br>

## [구성](#구성)
`next/image` 컴포넌트와 Next.js 이미지 최적화 API는 [`next.config.js` 파일](../../APIReference/next.config.jsOptions/)에서 구성할 수 있습니다. 이러한 구성을 통해 [원격 이미지 활성화](../../APIReference/Components/Image.md)하거나, [사용자 정의 이미지 breakpoints를 정의](../../APIReference/Components/Image.md)하거나, [캐싱 동작 변경](../../APIReference/Components/Image.md) 등을 수행할 수 있습니다.

[__자세한 내용은 전체 이미지 구성 설명서를 참조하십시오.__](../../APIReference/Components/Image.md)

<br><hr><br>

## [API 참조](#api-참조)
next/image API에 대하여 더 알아보세요.

<br>

> ### [<이미지>](../../APIReference/Components/Image.md)
>    내장된 `next/image` 컴포넌트를 사용하여 Next.js 애플리케이션의 이미지를 최적화하세요.