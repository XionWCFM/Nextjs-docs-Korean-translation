# **파비콘, 아이콘 및 애플 아이콘**

**`파비콘`**, **`아이콘`**, 또는 **`애플 아이콘`** 파일 규약을 사용하면 애플리케이션에 아이콘을 설정할 수 있습니다.

이 규약들은 웹 브라우저 탭, 휴대폰 홈 화면 및 검색 엔진 결과와 같은 위치에 나타나는 앱 아이콘을 추가하는 데 유용합니다.

앱 아이콘을 설정하는 두 가지 방법이 있습니다:

- [이미지 파일 사용 (.ico, .jpg, .png)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#image-files-ico-jpg-png)
- [코드를 사용하여 아이콘 생성 (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)

## **[이미지 파일 (.ico, .jpg, .png)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#image-files-ico-jpg-png)**

앱 아이콘을 설정하기 위해 이미지 파일을 사용하려면 **`/app`** 디렉토리 내에 **`favicon`**, **`icon`**, 또는 **`apple-icon`** 이미지 파일을 배치하면 됩니다. **`favicon`** 이미지는 반드시 **`app/`** 디렉토리의 최상위 수준에 위치해야 합니다.

Next.js는 파일을 평가하고 앱의 **`<head>`** 요소에 적절한 태그를 자동으로 추가합니다.

| 파일 규약                                                                                              | 지원되는 파일 형식            | 유효한 위치 |
| ------------------------------------------------------------------------------------------------------ | ----------------------------- | ----------- |
| [파비콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#favicon)        | .ico                          | app/        |
| [아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)           | .ico, .jpg, .jpeg, .png, .svg | app/\*_/_   |
| [애플아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#apple-icon) | .jpg, .jpeg, .png             | app/\*_/_   |

### [파비콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#favicon)

**`favicon.ico`** 이미지 파일을 루트 **`/app`** 경로 세그먼트에 추가하세요.

\<head> 출력 결과

```
<link rel="icon" href="/favicon.ico" sizes="any" />
```

### [아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)

**`icon.(ico|jpg|jpeg|png|svg)`** 이미지 파일을 추가하세요.

\<head> 출력 결과

```
<link  rel="icon"  href="/icon?<generated>"  type="image/<generated>"  sizes="<generated>"/>
```

### [애플아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#apple-icon)

**`apple-icon.(jpg|jpeg|png)`** 이미지 파일을 추가하세요.

<head> 출력 결과

```
<link  rel="apple-touch-icon"  href="/apple-icon?<generated>"  type="image/<generated>"  sizes="<generated>"/>
```

> **유용한 정보**
>
> - **파일 이름에 숫자 접미사를 추가하여 여러 개의 아이콘을 설정할 수 있습니다. 예를 들어, `icon1.png`, `icon2.png` 등입니다. 숫자 파일은 사전식으로 정렬됩니다.**
> - Favicons can only be set in the root `/app` segment. If you need more granularity, you can use [아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon).
> - **파비콘은 루트 `/app` 세그먼트에서만 설정할 수 있습니다. 더 세분화된 설정이 필요한 경우 [아이콘](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)을 사용할 수 있습니다.**
> - **적절한 `<link>` 태그 및 `rel`, `href`, `type`, `sizes` 와 같은 속성은 아이콘 유형 및 평가된 파일의 메타데이터에 따라 결정됩니다.**
>   - **예를 들어, 32x32px `.png` 파일은 `type="image/png"` 및 `sizes="32x32"` 속성을 가집니다.**
> - **`sizes="any"` 는 `favicon.ico` 의 출력에 추가되어 [브라우저 버그를 피하기 위해](https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs) `.ico` 아이콘이 `.svg` 보다 우선되는 경우를 피합니다.**

## **[코드를 사용하여 아이콘 생성하기 (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)**

[literal image files](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#image-files-ico-jpg-png)을 사용하는 것 외에도 코드를 사용하여 아이콘을 **동적으로 생성**할 수 있습니다.

**`icon`** 또는 **`apple-icon`** 경로를 생성하여 함수를 기본 내보내기하는 방식으로 앱 아이콘을 생성하세요.

| 파일 규약  | 지원되는 파일 형식 |
| ---------- | ------------------ |
| icon       | .js, .ts, .tsx     |
| apple-icon | .js, .ts, .tsx     |

아이콘을 생성하는 가장 쉬운 방법은 `next/server`에서 제공하는 **[ImageResponse](https://nextjs.org/docs/app/api-reference/functions/image-response)** API를 사용하는 것입니다.

app/icon.tsx

```
import { ImageResponse } from 'next/server'

// Route segment config
export const runtime = 'edge'

// Image metadata
export const size = {
  width: 32,
  height: 32,
}
export const contentType = 'image/png'

// Image generation
export default function Icon() {
  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 24,
          background: 'black',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          color: 'white',
        }}
      >
        A
      </div>
    ),
    // ImageResponse options
    {
      // For convenience, we can re-use the exported icons size metadata
      // config to also set the ImageResponse's width and height.
      ...size,
    }
  )
}
```

\<head> 출력 결과

```
<link rel="icon" href="/icon?<generated>" type="image/png" sizes="32x32" />
```

> **알아두면 좋은 점**
>
> - **기본적으로 생성된 아이콘은 [정적으로 최적화됩니다](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic#static-rendering-default) (빌드 시 생성되고 캐시됨), 동적 함수나 캐시되지 않은 데이터를 사용하지 않는 한입니다.**
> - **[[generateImageMetadata](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata)]를 사용하여 동일한 파일에서 여러 개의 아이콘을 생성할 수 있습니다.**
> - **`favicon` 아이콘을 생성할 수 없습니다. 대신 [icon](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)이나 [favicon.ico](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#favicon) 파일을 사용하세요.**

### **[속성 (Props)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#props)**

기본 내보내기 함수는 다음과 같은 속성을 받습니다:

### _[params](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#params-optional) (선택 사항)_

루트 세그먼트에서부터 **`icon`** 또는 **`apple-icon`** 세그먼트가 위치한 곳까지의 **[동적 경로 매개변수](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)** 객체를 포함하는 객체입니다.

app/shop/[slug]/icon.tsx

```
export default function Icon({ params }: { params: { slug: string } }) {  // ...}
```

| 경로                          | URL       | params                  |
| ----------------------------- | --------- | ----------------------- |
| app/shop/icon.js              | /shop     | undefined               |
| app/shop/[slug]/icon.js       | /shop/1   | { slug: '1' }           |
| app/shop/[tag]/[item]/icon.js | /shop/1/2 | { tag: '1', item: '2' } |
| app/shop/[...slug]/icon.js    | /shop/1/2 | { slug: ['1', '2'] }    |

### **[반환값 (Returns)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#returns)**

기본 내보내기 함수는 **`Blob`** | **`ArrayBuffer`** | **`TypedArray`** | **`DataView`** | **`ReadableStream`** | **`Response`** 를 반환해야 합니다.

> 알아두면 좋은 점: ImageResponse는 이 반환 유형을 만족시킵니다.

### **[구성 내보내기 (Config exports)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#config-exports)**

**`icon`** 또는 **`apple-icon`** 경로에서 **`size`** 와 **`contentType`** 변수를 내보내어 아이콘의 메타데이터를 선택적으로 구성할 수 있습니다.

| Option                                                                                                   | Type                                                                                                                |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [사이즈](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#size)             | { width: number; height: number }                                                                                   |
| [콘텐츠 유형](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#contenttype) | string - [image MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#image_types) |

### **[size](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#size)**

icon.tsx / apple-icon.tsx

```
export const size = { width: 32, height: 32 } export default function Icon() {}
```

\<head> 출력 결과

```
<link rel="icon" sizes="32x32" />
```

### **[contentType](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#contenttype)**

icon.tsx / apple-icon.tsx

```
export const contentType = 'image/png' export default function Icon() {}
```

\<head> 출력 결과

```
<link rel="icon" type="image/png" />
```

### **[경로 세그먼트 구성 (Route Segment Config)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#route-segment-config)**

`icon`과 `apple-icon`은 동일한 [페이지 및 레이아웃과 같은 경로 세그먼트 구성 옵션](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)을 사용할 수 있는 특수화된 [경로 핸들러 (Route Handlers)](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)입니다.

| 옵션                                                                                                               | 타입                                             | 기본값   |
| ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ | -------- |
| [dynamic](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic)                 | 'auto', 'force-dynamic', 'error', 'force-static' | 'auto'   |
| [revalidate](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate)           | false , 'force-cache', 0, number                 | false    |
| [runtime](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#runtime)                 | 'nodejs', 'edge'                                 | 'nodejs' |
| [preferredRegion](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#preferredregion) | 'auto' , 'global', 'home', string, string[]      | 'auto'   |

app/icon.tsx

TypeScript

```
export const runtime = 'edge' export default function Icon() {}
```

## [버전 히스토리](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#version-history)

| 버전    | 변경 사항                              |
| ------- | -------------------------------------- |
| v13.3.0 | favicon icon and apple-icon introduced |
