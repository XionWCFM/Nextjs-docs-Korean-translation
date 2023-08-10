# **opengraph-image와 twitter-image**

`opengraph-image`와 `twitter-image` 파일 규약을 사용하면 경로 세그먼트에 대한 Open Graph 및 Twitter 이미지를 설정할 수 있습니다.

이 규약들은 사용자가 사이트에 대한 링크를 공유할 때 소셜 네트워크 및 메시징 앱에서 나타나는 이미지를 설정하는 데 유용합니다.

Open Graph 및 Twitter 이미지를 설정하는 두 가지 방법이 있습니다:

- **[이미지 파일 사용 (.jpg, .png, .gif)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif)**
- **[코드를 사용하여 이미지 생성 (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx)**

## **[이미지 파일 (.jpg, .png, .gif)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif)**

이미지 파일을 사용하여 경로 세그먼트의 공유 이미지를 설정하려면 해당 세그먼트에 **`opengraph-image`** 또는 **`twitter-image`** 이미지 파일을 배치하세요.

Next.js는 파일을 평가하고 앱의 **`<head>`** 요소에 적절한 태그를 자동으로 추가합니다.

| 파일 규약                                                                                                                        | 지원되는 파일 형식      |
| -------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| [opengraph-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-image)           | .jpg, .jpeg, .png, .gif |
| [twitter-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-image)               | .jpg, .jpeg, .png, .gif |
| [opengraph-image.alt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-imagealttxt) | .txt                    |
| [twitter-image.alt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-imagealttxt)     | .txt                    |

### **[opengraph-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-image)**

**`opengraph-image.(jpg|jpeg|png|gif)`** 이미지 파일을 원하는 경로 세그먼트에 추가하세요.

\<head> 출력 결과

```
<meta property="og:image" content="<generated>" />
<meta property="og:image:type" content="<generated>" />
<meta property="og:image:width" content="<generated>" />
<meta property="og:image:height" content="<generated>" />
```

### **[twitter-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-image)**

**`twitter-image.(jpg|jpeg|png|gif)`** 이미지 파일을 원하는 경로 세그먼트에 추가하세요.

\<head> 출력 결과

```
<meta name="twitter:image" content="<generated>" />
<meta name="twitter:image:type" content="<generated>" />
<meta name="twitter:image:width" content="<generated>" />
<meta name="twitter:image:height" content="<generated>" />
```

### **[opengraph-image.alt.txt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-imagealttxt)**

**`opengraph-image.alt.txt`** 파일을 **`opengraph-image.(jpg|jpeg|png|gif)`** 이미지와 같은 경로 세그먼트에 추가하세요. 이 파일은 이미지의 대체 텍스트를 포함합니다.

opengraph-image.alt.txt

```
About Acme
```

\<head> 출력 결과

```
<meta property="og:image:alt" content="About Acme" />
```

### **[twitter-image.alt.txt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-imagealttxt)**

**`twitter-image.alt.txt`** 파일을 **`twitter-image.(jpg|jpeg|png|gif)`** 이미지와 같은 경로 세그먼트에 추가하세요. 이 파일은 이미지의 대체 텍스트를 포함합니다.

twitter-image.alt.txt

```
About Acme
```

\<head> 출력 결과

```
<meta property="og:image:alt" content="About Acme" />
```

## **[코드를 사용하여 이미지 생성하기 (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx)**

[literal image files](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif)를 사용하는 것 외에도 코드를 사용하여 이미지를 **동적으로 생성**할 수 있습니다.

**`opengraph-image`** 또는 **`twitter-image`** 경로를 생성하여 함수를 기본 내보내기하는 방식으로 경로 세그먼트의 공유 이미지를 생성하세요.

| 파일 규약       | 지원되는 파일 형식 |
| --------------- | ------------------ |
| opengraph-image | .js, .ts, .tsx     |
| twitter-image   | .js, .ts, .tsx     |

> **알아두면 좋은 점**
>
> - **기본적으로 생성된 이미지는 [정적으로 최적화됩니다](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic#static-rendering-default) (빌드 시 생성되고 캐시됨), 동적 함수나 캐시되지 않은 데이터를 사용하지 않는 한입니다.**
> - [generateImageMetadata](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata)**를 사용하여 동일한 파일에서 여러 개의 이미지를 생성할 수 있습니다.**

**이미지를 생성하는 가장 쉬운 방법은 `next/server`의 [ImageResponse](https://nextjs.org/docs/app/api-reference/functions/image-response) API를 사용하는 것입니다.**

app/about/opengraph-image.tsx

```
import { ImageResponse } from 'next/server'

// Route segment config
export const runtime = 'edge'

// Image metadata
export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}

export const contentType = 'image/png'

// Font
const interSemiBold = fetch(
  new URL('./Inter-SemiBold.ttf', import.meta.url)
).then((res) => res.arrayBuffer())

// Image generation
export default async function Image() {
  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        About Acme
      </div>
    ),
    // ImageResponse options
    {
      // For convenience, we can re-use the exported opengraph-image
      // size config to also set the ImageResponse's width and height.
      ...size,
      fonts: [
        {
          name: 'Inter',
          data: await interSemiBold,
          style: 'normal',
          weight: 400,
        },
      ],
    }
  )
}
```

\<head> 출력 결과

```
<meta property="og:image" content="<generated>" />
<meta property="og:image:alt" content="About Acme" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

### [속성 (Props)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#props)

기본 내보내기 함수는 다음과 같은 속성을 받습니다:

### [params (선택사항)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#params-optional)

루트 세그먼트에서부터 **`opengraph-image`** 또는 **`twitter-image`** 세그먼트가 위치한 곳까지의 **[동적 경로 매개변수](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)** 객체를 포함하는 객체입니다.

app/shop/[slug]/opengraph-image.tsx

TypeScript

```
export default function Image({ params }: { params: { slug: string } }) {
  // ...
}
```

| Route                                    | URL       | params                  |
| ---------------------------------------- | --------- | ----------------------- |
| app/shop/opengraph-image.js              | /shop     | undefined               |
| app/shop/[slug]/opengraph-image.js       | /shop/1   | { slug: '1' }           |
| app/shop/[tag]/[item]/opengraph-image.js | /shop/1/2 | { tag: '1', item: '2' } |
| app/shop/[...slug]/opengraph-image.js    | /shop/1/2 | { slug: ['1', '2'] }    |

### **[반환값 (Returns)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#returns)**

기본 내보내기 함수는 **`Blob`** | **`ArrayBuffer`** | **`TypedArray`** | **`DataView`** | **`ReadableStream`** | **`Response`** 를 반환해야 합니다.

> 알아두면 좋은 점: ImageResponse는 이 반환 유형을 만족시킵니다.

### **[구성 내보내기 (Config exports)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#config-exports)**

**`opengraph-image`** 또는 **`twitter-image`** 경로에서 **`alt`**, **`size`**, **`contentType`** 변수를 내보내어 이미지의 메타데이터를 선택적으로 구성할 수 있습니다.

| 옵션                                                                                                           | 유형                                                                                                                |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [alt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#alt)                 | string                                                                                                              |
| [size](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#size)               | { width: number; height: number }                                                                                   |
| [contentType](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#contenttype) | string - [image MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#image_types) |

### **[alt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#alt)**

opengraph-image.tsx / twitter-image.tsx

```
export const alt = 'My images alt text' export default function Image() {}
```

\<head> output

```
<meta property="og:image:alt" content="My images alt text" />
```

### **[size](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#size)**

opengraph-image.tsx / twitter-image.tsx

```
export const size = { width: 1200, height: 630 } export default function Image() {}
```

\<head> 출력 결과

```
<meta property="og:image:width" content="1200" /><meta property="og:image:height" content="630" />
```

### **[contentType](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#contenttype)**

opengraph-image.tsx / twitter-image.tsx

TypeScript

```
export const contentType = 'image/png' export default function Image() {}
```

\<head> 출력 결과

```
<meta property="og:image:type" content="image/png" />
```

### **[경로 세그먼트 구성 (Route Segment Config)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#route-segment-config)**

**opengraph-image**와 **twitter-image**는 동일한 [페이지 및 레이아웃과 같은 경로 세그먼트 구성 옵션](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)을 사용할 수 있는 특수화된 [경로 핸들러 (Route Handlers)](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)입니다.

| 옵션                                                                                                               | 타입                                             | 기본값   |
| ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ | -------- |
| [dynamic](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic)                 | 'auto', 'force-dynamic', 'error', 'force-static' | 'auto'   |
| [revalidate](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate)           | false , 'force-cache', 0, number                 | false    |
| [runtime](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#runtime)                 | 'nodejs', 'edge'                                 | 'nodejs' |
| [preferredRegion](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#preferredregion) | 'auto', 'global', 'home', string, string[]       | 'auto'   |

app/opengraph-image.tsx

TypeScript

```
export const runtime = 'edge' export default function Image() {}
```

### **[예제](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#examples)**

### **[외부 데이터 사용](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#using-external-data)**

이 예제는 **`params`** 객체와 외부 데이터를 사용하여 이미지를 생성합니다.

> 알아두면 좋은 점: 기본적으로 이 생성된 이미지는 정적으로 최적화됩니다. 개별적으로 fetch 옵션이나 경로 세그먼트 옵션을 구성하여 이 동작을 변경할 수 있습니다.

app/posts/[slug]/opengraph-image.tsx

TypeScript

```
import { ImageResponse } from 'next/server'

export const runtime = 'edge'

export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}
export const contentType = 'image/png'

export default async function Image({ params }: { params: { slug: string } }) {
  const post = await fetch(`https://.../posts/${params.slug}`).then((res) =>
    res.json()
  )

  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 48,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    ),
    {
      ...size,
    }
  )
}
```

## [버전 히스토리](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#version-history)

| 버전    | 변경 사항                                     |
| ------- | --------------------------------------------- |
| v13.3.0 | opengraph-image and twitter-image introduced. |
