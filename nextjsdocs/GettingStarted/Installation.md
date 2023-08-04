원본링크 : https://nextjs.org/docs/getting-started/installation
# Installation

시스템 필요요소

- **[Node.js 16.8](https://nodejs.org/ko)** 이상의 버전
- macOS, Windows(WSL 포함) 와 Linux에서 사용할 수 있습니다.

## Automatic Installation
create-next-app을 사용해서 새로운 Next.js 프로젝트를 시작하는 것을 추천합니다. 이는 모든 기능들이 자동으로 설정 및 설치 되어 있기 때문입니다. 

```bash
npx create-next-app@latest
```

설치시에 다음과 같은 안내문을 볼 수 있습니다.
```bash
What is your project named? my-app
Would you like to use TypeScript? No / Yes
Would you like to use ESLint? No / Yes
Would you like to use Tailwind CSS? No / Yes
Would you like to use `src/` directory? No / Yes
Would you like to use App Router? (recommended) No / Yes
Would you like to customize the default import alias? No / Yes
What import alias would you like configured? @/*
```
안내문 이후에 ```create-next-app```은 프로젝트 이름으로 폴더를 생성하고 필요한 dependencies을 설치합니다.

> Good to Know:<br>
>  - 이제 Next.js 는 기본적으로 TypeScript, ESLint 및 Tailwind CSS 구성과 함께 제공됩니다.
> -  선택적으로 프로젝트 루트에 [src directory](../BuildingYourApplication/Configuring/src_Directory.md)를 사용하여 애플리케이션의 코드를 구성 파일과 분리할 수 있습니다.

## Manual Installation
수동으로 새로운 Next.js app을 생성하고 싶다면, 필요한 패키지들을 설치해야 합니다.
```bash
npm install next@latest react@latest react-dom@latest
```
`package.json` 파일을 열고 `scripts`에 아래와 같이 추가합니다.
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```
각각의 scripts는 애플리케이션 개발의 여러 단계를 나타냅니다.
- `dev` : `next dev`를 실행하여 개발 모드에서 Next.js를 시작합니다.
- `build` : `next build`를 실행하여 제품용 애플리케이션을 빌드 합니다.
- `start` : `next start`를 통해 Next.js 제품용 서버를 시작합니다
- `lint` :  `next lint`를 통해 Next.js의 기본 제공 ESLint 구성을 설정합니다.

## Creating directories
Next.js는 파일 시스템 라우팅을 사용하므로, 해플리케이션의 라우팅은 파일 구조에 따라 결정됩니다.

### The `app` directory
새로운 어플리케이션에는 App Router를 사용하는 것을 추천합니다. App Router는 React의 최신 기능과 커뮤니티의 피드백을 기반으로 발전된 Pages Router 기능을 사용할 수 있습니다.

`app` 폴더를 생성한 후에 `layout.tsx` 와 `page.tsx` 파일을 생성합니다. 사용자가 어플리케이션의 `/` 루트 디렉토리에 방문할 때 렌더링 됩니다.

\<html> 및 \<body> 태그를 사용하여 `app/layout.tsx` 내에 [root layout](../BuildingYourApplication/Routing/Pages_and_Layouts.md)을 생성합니다.

```tsx
// app/layoout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```
마지막으로 `app/page.tsx`를 작성함으로서 home page를 생성합니다.
```jsx
// app/page.tsx
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```
>Good to know: 만약 `layout.tsx`를 생성하지 않았다면, Next.js는 `next dev`를 통해 개발 서버로 실행될 때, 이 파일을 자동으로 생성합니다.

[using the App Router](../BuildingYourApplication/Routing/Defining_Routes.md)에 대해서 더 학습할 수 있습니다.

### The `pages` directory (optional)
App Router 대신 Pages Router를 더 선호하신다면, 프로젝트의 루트에 `pages/` 폴더를 생성할 수 있습니다.

그런 다음 `pages` 폴더에 `index.tsx`파일을 추가합니다. 이것이 홈페이지가(`/`) 됩니다.'
```jsx
// pages/index.tsx
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```
다음으로 `_app.tsx` 파일을 `pages` 폴더 내부에 생성합니다. 이는 전역 layout을 정의합니다. ([custom App file](https://nextjs.org/docs/pages/building-your-application/routing/custom-app)에 대해 더 알아보기)
```jsx
// pages/_app.tsx
import type { AppProps } from 'next/app'
 
export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}
```
마지막으로 `_document.tsx`파일을 `pages` 폴대 내부에 생성합니다. 이는 서버의 초기 응답을 제어합니다.
```jsx
// pages/_document.tsx
import { Html, Head, Main, NextScript } from 'next/document'
 
export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```
[using the Pages Router](../BuildingYourApplication/Routing/Pages_and_Layouts.md)에 대해서 더 학습할 수 있습니다.
>  **Good to Know** : 두가지 라우터 방식을 같은 프로젝트에서 사용할 수 있지만 `app` 라우터가 `pages` 보다 우선시 되어 처리됩니다. 따라서 새로운 프로젝트에서 하나의 라우터 방식만을 사용하는 것을 추천합니다.

### The Public folder(optional)

`public` 폴더에 이미지, 폰트 등과 같은 static file을 저장합니다. `public` 폴더 내의 파일은 기본 URL(`/`)에서 시작하는 코드에서 참조할 수 있습니다.

## Run the Development Server 
1. `npm run dev` 로 개발 서버를 시작합니다.
2. `http://localhost:3000`을 통해 당신의 어플리케이션을 볼 수 있습니다.
3. `app/layout.tsx` (혹은 `pages/index.tsx`) 파일을 수정하고 저장하세요. 이후 업데이트 된 결과를 브라우저에서 확인하 수 있습니다.

# Next Step
Next.js 프로젝트의 파일 및 폴더에 대해 알아보세요.