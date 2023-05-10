# 글꼴 최적화
[`next/font`](../../APIReference/Components/Font.md)는 개인 정보 보호 및 성능 향상을 위해 자동으로 글꼴(사용자 정의 글꼴 포함)을 최적화하고 외부 네트워크 요청을 제거합니다.

> 🎥 보기 : `next/font` 자세한 사용법 알아보기 → [유튜브(6분)](https://www.youtube.com/watch?v=L8_98i_bMMA) 

`next/font`에는 모든 글꼴 파일에 대한 자동 자체 호스팅이 내장되어 있습니다. 다시 말해서, 기본 CSS `size-adjust` 속성 덕분에 레이아웃 이동 없이 웹 글꼴을 최적으로 로드할 수 있도록 해줍니다.

이 새로운 글꼴 시스템은 성능과 개인 정보 보호를 염두에 두고 모든 Google 글꼴을 편리하게 사용할 수 있도록 해줍니다. CSS 및 글꼴 파일은 빌드 타임에 다운로드되며 나머지 정적 assets과 함께 자체 호스팅됩니다. __브라우저에서 Google로 요청을 보내지 않습니다.__

<br><hr><br>

## [구글 글꼴](#구글-글꼴)
모든 Google 글꼴을 자동으로 자체 호스팅합니다. 글꼴은 배포에 포함되며 배포와 동일한 도메인에서 제공됩니다. __브라우저에서 Google로 요청을 보내지 않습니다.__

`next/font/google`에서 사용하려는 글꼴을 함수로 가져와서 시작하세요. 최상의 성능과 유연성을 위해 [variable fonts](https://fonts.google.com/variablefonts) 사용을 권장합니다 .

``` typescript
// app/layout.tsx

import { Inter } from 'next/font/google';

// variable font를 로드하는 경우, 글꼴 두께를 지정할 필요가 없습니다.
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export default function RootLayout({ children }: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```
variable font를 사용할 수 없는 경우, 글꼴 두께를 지정해야 합니다.

```typescript
// app/layout.tsx

import { Roboto } from 'next/font/google';

const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
  display: 'swap'
})

export default function RootLayout({ children }: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  );
}
```
배열을 사용하여 여러 글꼴 두께 및(혹은) 스타일을 지정할 수 있습니다.
```typescript
const roboto = Roboto({
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  subsets: ['latin'],
  display: 'swap',
});
```
> __알아두면 좋은 정보 :__ 두 개 이상의 단어로 된 글꼴 이름에는 밑줄(_)을 사용하세요. 예를 들어, `Roboto Mono`는 `Roboto_Mono`로 가져와야 합니다.

### [서브셋 지정](#서브셋-지정)
Google 글꼴은 자동으로 [서브셋](https://fonts.google.com/knowledge/glossary/subsetting)됩니다. 이렇게 하면 글꼴 파일의 크기가 줄어들고 성능이 향상됩니다. 어떤 서브셋을 미리 로드할지 정의해야 합니다. `[preload](../../APIReference/Components/Font.md)`가 `true`인 상태에서 서브셋을 지정하지 않으면 경고가 발생합니다.

이는 함수 호출에 추가하여 수행할 수 있습니다.

```typescript
app/layout.tsx

const inter = Inter({ subsets: ["latin"] });
```

자세한 내용은 [글꼴 API 참조](../../APIReference/Components/Font.md)를 확인하십시오 .

### [여러 글꼴 사용](#여러-글꼴-사용)
애플리케이션에서 여러 글꼴을 가져와서 사용할 수 있습니다. 두 가지 방법을 사용할 수 있습니다.

첫 번째 접근 방식은 글꼴을 내보내고 가져오고 `className`을 필요한 곳에 적용하는 유틸리티 함수를 만드는 것입니다. 이렇게 하면 글꼴이 렌더링될 때만 미리 로드됩니다.

```typescript
// app/fonts.ts

import { Inter, Roboto_Mono } from 'next/font/google';

export const inter = Inter({
  subsets: ['latin'],
  display: 'swap'
});

export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
});
```
```typescript
// app/layout.tsx

import { inter } from './fonts';

export default function Layout() {
  return (
    <html lang="en" className={inter.className}>
      <body>
        <div>{children}
      </body>
    </html>
  );
}
```
```typescript
// app/page.tsx

import { roboto_mono } from './fonts';

export default function Page() {
  return (
    <>
      <h1 className={roboto_mono.className}>My page</h1>
    </>
  );
}
```

위의 예에서, `Inter`은 전역적으로 적용되며 `Roboto Mono`를 필요에 따라 가져와서 적용할 수 있습니다.

또 다른 방법으로는 [CSS 변수](../../APIReference/Components/Font.md)를 생성하고 선호하는 CSS 솔루션과 함께 사용할 수 있습니다:

```typescript
// app/layout.tsx

import { Inter, Roboto_Mono } from 'next/font/google';
import styles from './global.css';

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
  display: 'swap',
});

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>
        <h1>My App</h1>
        <div>{children}
      </body>
    </html>
  );
}
```

```typescript
// app/global.css

html {
  font-family: var(--font-inter);
}

h1 {
  font-family: var(--font-roboto-mono);
}
```

위의 예에서 `Inter`은 전역적으로 적용되며, 모든 `<h1>`태그는 `Roboto Mono`로 스타일이 지정됩니다 .


> __권장 사항 :__ 각각의 새 글꼴은 클라이언트가 다운로드해야 하는 추가 리소스이므로 여러 글꼴의 사용을 보수적으로 하십시오.

<br><hr><br>

## [로컬 글꼴](#로컬-글꼴)

`next/font/local`을 가져와 로컬 글꼴 파일의 `src`를 지정합니다. 최상의 성능과 유연성을 위해 [variable fonts](https://fonts.google.com/variablefonts)를 사용하는 것이 좋습니다.

```typescript
// app/layout.tsx

import localFont from 'next/font/local';

// Font files can be colocated inside of `app`
const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap'
});

export default function RootLayout({ children }: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  );
}
```
하나의 글꼴 패밀리에 대해 여러 파일을 사용하려는 경우 `src`는 배열이 될 수 있습니다.
```typescript
const roboto = localFont({
  src: [
    {
      path: './Roboto-Regular.woff2',
      weight: '400',
      style: 'normal',
    },
    {
      path: './Roboto-Italic.woff2',
      weight: '400',
      style: 'italic',
    },
    {
      path: './Roboto-Bold.woff2',
      weight: '700',
      style: 'normal',
    },
    {
      path: './Roboto-BoldItalic.woff2',
      weight: '700',
      style: 'italic',
    },
  ],
});
```

자세한 내용은 [글꼴 API 참조]((../../APIReference/Components/Font.md))를 확인하십시오.

<br><hr><br>

## [Tailwind CSS 사용](#tailwind-css-사용)
`next/font`는 [CSS 변수](../../APIReference/Components/Font.md)를 통해 [Tailwind CSS](https://tailwindcss.com/)와 함께 사용할 수 있습니다.

아래 예에서는 `next/font/google`에서 `Inter` 글꼴을 사용합니다 (Google 또는 로컬 글꼴의 어떤 글꼴이든 사용할 수 있습니다). `inter`에 CSS 변수 이름을 정의하는 `variable` 옵션을 사용하여 글꼴을 로드합니다. 그런 다음, `inter.variable`를 사용하여 HTML 문서에 CSS 변수를 추가합니다.

```typescript
// app/layout.tsx

import { Inter, Roboto_Mono } from 'next/font/google';

const inter = Inter({
  variable: '--font-inter',
  display: 'swap'
});

const roboto_mono = Roboto_Mono({
  variable: '--font-roboto-mono',
  display: 'swap'
});

export default function RootLayout({ children }: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

마지막으로 CSS 변수를 [Tailwind CSS 구성](../Styling/Tailwind_CSS.md)에 추가합니다.

```typescript
// tailwind.config.js

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./app/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-inter)'],
        mono: ['var(--font-roboto-mono)'],
      },
    },
  },
  plugins: [],
};
```

이제 `font-sans` 및 `font-mono` 유틸리티 클래스를 사용하여 글꼴을 엘리먼트에 적용할 수 있습니다.

<br><hr><br>

## [사전 로드](#사전-로드)
사이트의 페이지에서 글꼴 함수가 호출되면, 해당 폰트가 전역으로 사용 가능하고 모든 경로에서 사전 로드되는 것은 아닙니다. 대신, 글꼴은 사용되는 파일 유형에 따라 관련 경로에서만 사전 로드됩니다.

* [고유한 페이지]()인 경우 해당 페이지의 고유한 라우트에 미리 로드됩니다.
* [레이아웃]()인 경우 레이아웃에 의해 래핑된 모든 라우트에 미리 로드됩니다.
* [루트 레이아웃]()인 경우 모든 라우트에 미리 로드됩니다.

<br><hr><br>

## [글꼴 재사용](#글꼴-재사용)

`localFont` 또는 Google 글꼴 함수을 호출할 때마다, 해당 글꼴은 애플리케이션에서 하나의 인스턴스로 호스팅됩니다. 따라서 여러 파일에서 동일한 글꼴 함수를 로드하면, 동일한 글꼴의 여러 인스턴스가 호스팅됩니다. 이 경우 다음을 수행하는 것이 권장됩니다.

* 하나의 공유 파일에서 글꼴 loader 함수를 호출합니다.
* 상수로 내보냅니다.
* 이 글꼴을 사용하려는 각 파일에서 상수를 가져옵니다.

## [API 참조](#api-참조)
next/font API에 대하여 더 알아보세요.

<br>

> ### [<글꼴>](../../APIReference/Components/Font.md) <br>
>  내장된 `next/font` 로더를 사용하여 웹 글꼴 로딩을 최적화하세요.