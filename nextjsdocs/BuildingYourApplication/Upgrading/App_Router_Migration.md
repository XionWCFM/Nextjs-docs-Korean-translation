# **앱 라우터(App Router) 점진적 적용 가이드**

공식 문서 : [https://nextjs.org/docs/app/building-your-application/upgrading/app-router-migration](https://nextjs.org/docs/app/building-your-application/upgrading/app-router-migration)

> ※ 링크가 추가되어 있으나 해당되는 내용이 없는 경우가 있습니다. 이러한 경우에는 하이퍼링크 텍스트의 뒷부분에 '※' 표시를 달아두겠습니다. - 번역자 드림

이 가이드는 다음을 수행하는 것을 돕습니다.

- [Next.js 애플리케이션의 버전을 12에서 13(stable)으로 업데이트합니다. ※](https://nextjs.org/docs/pages/building-your-application/upgrading#nextjs-version)
- [pages와 app 디렉토리 모두에서 작동하는 기능들을 업그레이드합니다. ※](https://nextjs.org/docs/pages/building-your-application/upgrading#upgrading-new-features)
- [기존의 애플리케이션을 pages에서 app으로 점진적으로 이전합니다. ※](https://nextjs.org/docs/pages/building-your-application/upgrading#migrating-from-pages-to-app)

<br><hr><br>

## **업그레이드**

### **Node.js 버전**

이제 Next.js의 최소 Node.js 버전은 v16.8입니다. 자세한 내용은 [Node.js 문서](https://nodejs.org/docs/latest-v16.x/api/)를 참조하십시오.

### **Next.js 버전**

Next.js 버전 13으로 업데이트하려면 선호하는 패키지 매니저를 사용하여 다음 명령을 실행하십시오.

```bash
npm install next@latest react@latest react-dom@latest
```

### **ESLint 버전**

ESLint를 사용중이라면, ESLint 버전을 업그레이드해야 합니다.

```bash
npm install -D eslint-config-next@latest
```

> Note : ESLint 변경 사항이 적용되려면 VS Code에서 ESLint 서버를 재시작해야 할 수 있습니다. 명령 팔레트(Mac의 경우 `cmd+shift+p`, Windows의 경우 `ctrl+shift+p`)를 열고 `"ESLint: Restart ESLint Server"`를 검색합니다.

<br><hr><br>

## **다음 단계**

업데이트를 마친 후, 다음 단계를 위해 다음의 섹션을 참고하세요.

- [새로운 기능 업그레이드 ※](./Upgrading.md#upgrading-new-features) : 향상된 이미지 컴포넌트, 링크 컴포넌트와 같은 새로운 기능으로 업그레이드하는 데 도움이 되는 가이드입니다.
- [pages에서 app 디렉토리로 이전 ※](./Upgrading.md#migrating-from-pages-to-app) : `pages`에서 `app` 디렉토리로 점진적으로 이전하는 데 도움이 되는 단계별 가이드입니다.

<br><hr><br>

## **새로운 기능 업그레이드하기**

Next.js 13는 새로운 기능과 규칙을 가진 새로운 [앱 라우터](../Routing/Routing.md)를 도입했습니다. 새로운 라우터는 `app` 디렉토리에서 사용할 수 있으며, `pages` 디렉토리와 공존합니다.

Next.js 13으로 업그레이드하는 것은 새로운 [앱 라우터](../Routing/Routing.md#the-app-directory)를 사용해야 한다는 것을 의미하지 않습니다. 업그레이드 후에도 `pages`와 함께 새로운 기능을 계속 사용할 수 있습니다. 업데이트된 [이미지 컴포넌트 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#image-component), [링크 컴포넌트 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#link-component), [스크립트 컴포넌트 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#script-component), [폰트 최적화 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#font-optimization) 등이 그 예입니다.

### **`<Image/>` 컴포넌트**

Next.js 12에서는 임시로 `next/future/image`를 도입하여 이미지 컴포넌트를 개선했습니다. 이러한 개선사항에는 줄어든 클라이언트 측 JavaScript, 손쉬운 이미지 확대 및 스타일, 더 나은 접근성 및 웹 브라우저의 기본 이미지 지연 로딩이 포함됩니다.

버전 13에서는 이러한 새로운 동작이 `next/image`의 기본 설정으로 변경되었습니다.

새 이미지 컴포넌트로 이전하는 데 도움이 되는 두 가지 codemod가 있습니다.

- [`next-image-to-legacy-image` codemod](./Codemods.md#rename-instances-of-nextimage) : `next/image`를 `next/legacy/image`로 안전하게 변경합니다. 기존 구성 요소는 동일한 동작을 유지합니다.
- [`next-image-experimental` codemod](./Codemods.md#migrate-next-image-experimental-experimental) : 인라인 스타일을 위험하게 추가하고 사용되지 않는 props를 제거합니다. 이는 기존 구성 요소의 동작을 변경하여 새로운 기본값과 일치하게 합니다. 이 codemod를 사용하려면 먼저 `next-image-to-legacy-image` codemod를 실행해야 합니다.

### **`<Link>` 컴포넌트**

[<Link> 컴포넌트](../Routing/Linking_and_Navigating.md#link-component)는 더이상 자식 요소로 `<a>` 태그를 추가할 필요가 없습니다. 이 동작은 [버전 12.2](https://nextjs.org/blog/next-12-2)의 실험적인 옵션으로 추가되었으며 이제는 기본값입니다. Next.js 13에서 `<Link>`는 항상 `<a>`를 렌더링하며 하위 태그로 props를 전달할 수 있도록 해줍니다.

예시 :

```JSX
import Link from 'next/link'

// Next.js 12: `<a>` 요소는 중첩되어 있어야 하며, 그렇지 않으면 제외됩니다.
<Link href="/about">
  <a>About</a>
</Link>

// Next.js 13: `<Link>`는 항상 내부적으로 `<a>`를 렌더링합니다.
<Link href="/about">
  About
</Link>
```

링크를 Next.js 13으로 업그레이드하기 위하여 [`new-link` codemod](./Codemods.md#remove-a-from-links-next-link)를 사용할 수 있습니다.

### **`<Script>` 컴포넌트**

[`next/script`](../../APIReference/Components/Script.md)의 동작 방식이 업데이트되어 `pages`와 `app` 모두를 지원하지만, 원활한 이전을 보장하기 위해 일부 사항의 변경이 필요합니다.

- 이전에 `_document.js`에 포함시켰던 `beforeInteractive` 스크립트를 루트 레이아웃 파일 (`app/layout.tsx`)로 이동하세요.

- 실험적으로 추가된 `worker` 전략은 아직 `app`에서 작동하지 않으며, 해당 전략으로 표시된 스크립트는 제거하거나 다른 전략(예: `lazyOnload`)을 사용하도록 수정해야합니다.

- `onLoad`, `onReady` 및 `onError` 핸들러는 서버 컴포넌트에서 작동하지 않으므로, [클라이언트 컴포넌트](../../GettingStarted/React_Essentials.md)로 이동하거나 완전히 제거해야 합니다.

### **폰트 최적화**

이전에는 Next.js가 [폰트 CSS를 인라인으로 처리하여](../Optimizing/Fonts.md) 폰트 최적화를 지원했습니다. 버전 13에서는 새로운 [`next/font`](../Optimizing/Fonts.md) 모듈이 도입되어 뛰어난 성능과 개인정보 보호를 보장하면서 폰트 로딩 경험을 커스터마이즈할 수 있게 됐습니다. `next/font`는 `pages`와 `app` 디렉토리에서 모두 지원됩니다.

[CSS를 인라인으로 삽입하는 것](../Optimizing/Fonts.md)은 여전히 `pages`에서 작동하지만, `app`에서는 작동하지 않습니다. 대신 [`next/font`](../Optimizing/Fonts.md)를 사용해야합니다.

`next/font`를 사용하는 방법은 [Font Optimization](../Optimizing/Fonts.md) 페이지를 참조하십시오.

<br><hr><br>

## **`pages`에서 `app`으로 이전하기**

앱 라우터로 이동하면 Next.js가 기반으로 하는 서버 컴포넌트, 서스펜스 등 React 기능을 처음 사용하는 경우가 될 수 있습니다. [특수 파일](../Routing/Routing.md#file-conventions) 및 [레이아웃](../Routing/Pages_and_Layouts.md#layouts) 같은 새로운 Next.js 기능과 결합될 때, 이전 (Migration)은 새로운 개념, 심상 모형(mental model) 및 동작 변경을 새롭게 익혀야 함을 의미합니다.

우리는 이전을 더 작은 단계로 분할하여 복합 복잡성 (combined complexity) 을 줄이는 것을 권장합니다. `app` 디렉토리는 페이지별로 점진적으로 이전할 수 있도록 의도적으로 설계되었습니다.

- `app` 디렉토리는 중첩된 라우트와 레이아웃을 지원합니다. [더 알아보기](../Routing/Routing.md).
- 중첩된 폴더를 사용하여 [라우트를 정의](../Routing/Defining_Routes.md)하고, 특수한 `page.js` 파일을 사용하여 경로 세그먼트를 공개적으로 액세스할 수 있도록 만들 수 있습니다. [더 알아보기 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#step-4-migrating-pages).
- [특수 파일 규칙](../Routing/Routing.md#file-conventions)을 사용하여 각 경로 세그먼트에 대한 UI를 만들 수 있습니다. 가장 일반적인 특수 파일은 `page.js` 및 `layout.js`입니다.
  - `page.js`를 사용하여 경로별로 고유한 UI를 정의합니다.
  - `layout.js`를 사용하여 여러 경로에서 공유되는 UI를 정의합니다.
  - `.js`, `.jsx` 또는 `.tsx` 파일 확장명을 사용할 수 있습니다.
- `app` 디렉토리 내에 컴포넌트, 스타일, 테스트 등을 함께 둘 수 있습니다. [더 알아보기](../Routing/Routing.md).
- `getServerSideProps`와 `getStaticProps`와 같은 데이터 가져오기 함수는 `app` 내부의 [새로운 API](../DataFetching/DataFetching.md)로 대체되었습니다. `getStaticPaths`는 [`generateStaticParams`](../../APIReference/Functions/generateStaticParams.md)로 대체되었습니다.
- `pages/_app.js` 및 `pages/_document.js`는 `app/layout.js` 루트 레이아웃 하나로 대체되었습니다. [더 알아보기](../Routing/Pages_and_Layouts.md#root-layout-required).
- `pages/_error.js`는 더 세분화된 `error.js` 특수 파일로 대체되었습니다. [더 알아보기](../Routing/Error_Handling.md).
- `pages/404.js`는 [`not-found.js`](../../APIReference/FileConventions/not-found.js.md) 파일로 대체되었습니다.
- `app` 디렉토리 내에 컴포넌트, 스타일, 테스트 등을 함께 둘 수 있습니다. [더 알아보기](../Routing/Routing.md).
- `pages/api/*`는 현재 `pages` 디렉토리 내에 남아 있습니다.

<br>

### **1단계 : `app` 디렉토리 생성**

최신 버전의 Next.js로 업데이트합니다(버전 13.4 또는 그 이상의 버전이 필요합니다).

```bash
npm install next@latest
```

그런 다음, 프로젝트의 루트 디렉토리 (또는 `src/` 디렉토리)에 새로운 `app` 디렉토리를 만듭니다.
<br>

### **2단계 : 루트 레이아웃 생성**

`app/layout.tsx` 파일을 `app` 디렉토리 안에 새롭게 생성합니다. 이는 `app` 내부의 모든 라우트에 적용되는 [루트 레이아웃](../Routing/Pages_and_Layouts.md#root-layout)입니다.

```tsx
// app/layout.tsx
export default function RootLayout({
  // 레이아웃은 반드시 children prop을 전달받아야 합니다.
  // 이는 중첩된 레이아웃 또는 페이지로 채워질 것입니다.
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

- `app` 디렉토리는 반드시 루트 레이아웃을 포함하여야 합니다.
- 루트 레이아웃은 반드시 `<html>`, `<body>` 태그를 정의하여야 합니다. Next.js는 이들을 자동적으로 생성하지 않습니다.
- 루트 레이아웃은 `pages/_app.tsx`, `pages/_documents.tsx` 파일을 대체합니다.
- `.js`, `.jsx`, `.tsx` 확장자를 레이아웃 파일로 사용할 수 있습니다.

`<head>` 태그를 관리하려면 [내장된 SEO 지원 기능](../Optimizing/Metadata.md#seo)을 사용할 수 있습니다.

```tsx
// app/layout.tsx
export const metadata = {
  title: "Home",
  description: "Welcome to Next.js",
};
```

#### **`_document.js`와 `_app.js`의 이전**

기존에 `_app` 또는 `_document` 파일이 있는 경우 내용(예: 전역 스타일)을 루트 레이아웃 (`app/layout.tsx`)에 복사할 수 있습니다. `app/layout.tsx`의 스타일은 `pages/*`에 적용되지 않습니다. 이전 도중에 `pages/*` 경로가 손상되는 것을 방지하기 위해 `_app`/`_document`를 유지해야 합니다. 전체적으로 이전이 완료되면 안전하게 삭제할 수 있습니다.

만약 React Context providers를 사용하고 있다면, 이들은 [클라이언트 컴포넌트](../../GettingStarted/React_Essentials.md)로 이동해야 합니다.

#### **`getLayout()` 패턴을 Layouts로 이전(선택사항)**

Next.js는 `pages` 디렉토리에서 페이지별 레이아웃을 구현하기 위해 [페이지 컴포넌트에 속성](https://nextjs.org/docs/pages/building-your-application/routing/pages-and-layouts#layout-pattern#per-page-layouts)을 추가하는 것을 권장했습니다. 이 패턴은 `app` 디렉토리의 [중첩된 레이아웃](../Routing/Pages_and_Layouts.md#layouts)의 네이티브 지원으로 대체될 수 있습니다.

<details>
<summary>before after 예시 보기</summary>

**Before**

```jsx
// components/DashboardLayout.js
export default function DashboardLayout({ children }) {
  return (
    <div>
      <h2>My Dashboard</h2>
      {children}
    </div>
  );
}
```

```jsx
// pages/dashboard/index.js
import DashboardLayout from "../components/DashboardLayout";

export default function Page() {
  return <p>My Page</p>;
}

Page.getLayout = function getLayout(page) {
  return <DashboardLayout>{page}</DashboardLayout>;
};
```

**After**

1. `pages/dashboard/index.js`로부터 `Page.getLayout` 속성을 제거하고 [`pages`를 `app` 디렉토리로 이전하는 단계 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#migrating-pages)를 따릅니다.

```jsx
// app/dashboard/page.js
export default function Page() {
  return <p>My Page</p>;
}
```

2. `pages` 디렉토리의 동작을 유지하기 위해 `DashboardLayout`의 내용을 새로운 [클라이언트 컴포넌트](../../GettingStarted/React_Essentials.md#client-components)로 옮깁니다.

```jsx
// app/dashboard/DashboardLayout.js
"use client"; // 이 지시어는 모든 import 이전에 파일 상단에 있어야 합니다.

// 이것은 클라이언트 컴포넌트입니다.
export default function DashboardLayout({ children }) {
  return (
    <div>
      <h2>My Dashboard</h2>
      {children}
    </div>
  );
}
```

3. `DashboardLayout`을 `app` 디렉토리 안의 새로운 `layout.js` 파일로 import합니다.

```jsx
// app/dashboard/layout.js
import DashboardLayout from "./DashboardLayout";

// 이것은 서버 컴포넌트입니다.
export default function Layout({ children }) {
  return <DashboardLayout>{children}</DashboardLayout>;
}
```

4. `DashboardLayout.js` (클라이언트 컴포넌트)에서 비상호작용 부분을 `layout.js` (서버 컴포넌트)로 점진적으로 이동시켜 클라이언트로 전송되는 컴포넌트 JavaScript 양을 줄일 수 있습니다.
</details>

<br>

### **3단계 : `next/head` 이전**

`pages` 폴더에서는, `next/head` React 컴포넌트가 `<head>` HTML 엘리먼트를 관리하는 데 사용됩니다. 예를 들어, `title`과 `meta` 태그를 설정할 수 있습니다. `app` 디렉토리에서는 `next/head` 대신 새로운 [내장 SEO 지원 기능](../Optimizing/Metadata.md#seo)이 제공됩니다.

**Before**

```tsx
// pages/index.tsx
import Head from "next/head";

export default function Page() {
  return (
    <>
      <Head>
        <title>My page title</title>
      </Head>
    </>
  );
}
```

**After**

```tsx
// app/page.tsx
export const metadata = {
  title: "My Page Title",
};

export default function Page() {
  return "...";
}
```

[모든 metadata 옵션을 봅니다.](../../APIReference/FileConventions/Metadata/Metadata.md)

<br>

### **4단계 : pages 이전**

- `app` 디렉토리에 있는 페이지는 기본적으로 서버 컴포넌트입니다. 이는 `pages` 디렉토리와는 다르게 pages가 클라이언트 컴포넌트인 것과 다릅니다.
- [데이터 가져오기(fetching)](../DataFetching/DataFetching.md) 방법이 달라졌습니다. `getServerSideProps`, `getStaticProps`, `getInitialProps`는 더 간단한 API로 대체되었습니다.
- `app` 디렉토리는 중첩된 폴더를 사용하여 [라우트를 정의](../Routing/Defining_Routes.md)하고, 특별한 `page.js` 파일을 사용하여 라우트 세그먼트를 공개적으로 사용 가능하게 만듭니다.

| `pages` 디렉토리 | `app` 디렉토리        | 라우트       |
| ---------------- | --------------------- | ------------ |
| index.js         | page.js               | /            |
| about.js         | about/page.js         | /about       |
| blog/\[slug\].js | blog/\[slug\]/page.js | /blog/post-1 |

이전의 단계를 다음의 주요 2단계로 나누는 것을 권장합니다.

- 1단계 : 기본 내보내기(export default)된 페이지 컴포넌트를 새로운 클라이언트 컴포넌트로 이동합니다.
- 2단계 : 새로운 클라이언트 컴포넌트를 `app` 디렉토리 내의 새로운 `page.js` 파일로 가져옵니다.

> Note : 이것은 가장 유사한 동작을 가지고 있어 가장 쉬운 이전 경로입니다.

1단계 : 새 클라이언트 컴포넌트 생성

1. `app` 디렉토리 내에 새로운 별도 파일 (예: `app/home-page.tsx` 또는 유사한 이름)을 생성하여 클라이언트 컴포넌트를 export 하세요. 클라이언트 컴포넌트를 정의하려면 파일 상단 (모든 import 전)에 `'use client'` 지시문을 추가하세요.

2. `pages/index.js`의 기본 내보내기 페이지 구성 요소를 `app/home-page.tsx`로 이동합니다.

```tsx
// app/home-page.tsx
"use client";

// 이것은 클라이언트 컴포넌트입니다. 데이터를 props로 받으며,
//  pages 디렉토리의 페이지 컴포넌트와 마찬가지로
//  state와 effects에 접근할 수 있습니다.
export default function HomePage({ recentPosts }) {
  return (
    <div>
      {recentPosts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

2단계 : 새 페이지 생성

1. `app` 디렉토리 내에 `app/page.tsx` 파일을 만듭니다. 이는 기본적으로 서버 컴포넌트입니다.
2. `home-page.tsx` 클라이언트 컴포넌트를 페이지에 import 합니다.
3. 만약 `pages/index.js`에서 데이터를 가져오고 있었다면, 새로운 [데이터 가져오기 API](../DataFetching/Fetching.md)를 사용하여 데이터 가져오기 로직을 직접 서버 컴포넌트로 이동합니다. 자세한 내용은 [데이터 가져오기 업그레이드 가이드 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#step-6-migrating-data-fetching-methods)를 참조하세요.

```tsx
// app/page.tsx
// 클라이언트 컴포넌트로 import합니다.
import HomePage from "./home-page";

async function getPosts() {
  const res = await fetch("https://...");
  const posts = await res.json();
  return posts;
}

export default async function Page() {
  // 서버 컴포넌트 안에 직접 데이터를 가져옵니다.
  const recentPosts = await getPosts();
  // 가져온 데이터를 클라이언트 컴포넌트로 전달합니다.
  return <HomePage recentPosts={recentPosts} />;
}
```

1. 만약 이전 페이지가 `useRouter`를 사용하였다면, 새로운 라우팅 Hook으로 업데이트해야 합니다. [더 알아보기](../../APIReference/Functions/useRouter.md)

2. 개발 서버를 시작하고 [http://localhost:3000](http://localhost:3000/)에 방문하십시오. 이제 `app` 디렉토리를 통해 제공되는 기존 인덱스 경로를 볼 수 있어야 합니다.

<br>

### **5단계 : 라우팅 Hooks 이전**

새로운 라우터가 추가되어 `app` 디렉토리에서 새로운 동작을 지원합니다.

`app`에서는 `next/navigation`에서 가져온 새로운 세 가지 Hook([useRouter()](../../APIReference/Functions/useRouter.md), [usePathname()](../../APIReference/Functions/usePathname.md), [useSearchParams()](../../APIReference/Functions/useParams.md))을 사용해야 합니다.

- 새로운 `useRouter` Hook은 `next/navigation`에서 import되며, `next/router`로부터 import되던 `pages`의 `useRouter` Hook과 다르게 동작합니다.
  - `next/router`로부터 import된 `useRouter` Hook은 `app` 디렉토리에서 지원되지 않지만 `pages` 디렉토리에서는 계속 사용할 수 있습니다.
- 새로운 `useRouter`는 `pathname` 문자열을 반환하지 않습니다. 대신, 별도의 `usePathname` Hook을 사용하세요.
- 새로운 `useRouter`는 `query` 객체를 반환하지 않습니다. 대신, 별도의 `useSearchParams` Hook을 사용하세요.
- `useSearchParams`, `usePathname`을 동시에 사용하여 페이지 변경을 감시할 수 있습니다. 자세한 내용은 [Router Events](../../APIReference/Functions/useRouter.md#router-events)를 참조하세요.
- 이러한 새로운 Hook은 클라이언트 컴포넌트에서만 지원됩니다. 서버 컴포넌트에서는 사용할 수 없습니다.

```tsx
// app/example-client-component.tsx
"use client";

import { useRouter, usePathname, useSearchParams } from "next/navigation";

export default function ExampleClientComponent() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams();

  // ...
}
```

또한, 새로운 useRouter Hook은 다음과 같이 변경되었습니다.

- `fallback`이 [대체 ※](https://nextjs.org/docs/pages/building-your-application/upgrading#replacing-fallback)되어 `isFallback`이 제거되었습니다.
- `locale`, `locales`, `defaultLocales`, `domainLocales` 값은 `app` 디렉토리에서 내장 i18n Next.js 기능이 더 이상 필요하지 않기 때문에 제거되었습니다. [i18n에 대해 자세히 알아보세요](https://nextjs.org/docs/pages/building-your-application/routing/internationalization).
- `basePath`는 제거되었습니다. 대안은 `useRouter`의 일부가 아닙니다. 아직 구현되지 않았습니다.
- `asPath`는 새로운 라우터에서 `as`의 개념이 제거되었기 때문에 제거되었습니다.
- `isReady`가 제거되었습니다. [정적 렌더링](../Rendering/Rendering.md#static-rendering) 중 [useSearchParams()](../../APIReference/Functions/useParams.md) Hook을 사용하는 컴포넌트는 프리렌더링 단계를 건너뛰고 대신 런타임에 클라이언트에서 렌더링됩니다.

[useRouter() API 참조를 확인하세요](../../APIReference/Functions/useRouter.md).

<br>

### **6단계 : 데이터 페칭 메서드 이전**

`pages` 디렉토리에서는 `getServerSideProps`와 `getStaticProps`를 사용하여 데이터를 가져옵니다. 그러나 `app` 디렉토리 내에서는 이전 데이터 가져오기 함수들이 `fetch()`와 `async` React 서버 컴포넌트를 기반으로 한 [더 간단한 API](../DataFetching/DataFetching.md)로 대체됩니다.

```tsx
// app/page.tsx
export default async function Page() {
  // 이 요청은 수동으로 무효화될 때까지 캐시되어야 합니다.
  // `getStaticProps`와 유사합니다.
  // 기본값은 `force-cache`이며, 생략할 수 있습니다.
  const staticData = await fetch(`https://...`, { cache: "force-cache" });

  // 이 요청은 매 요청마다 다시 fetch해야 합니다.
  // `getServerSideProps`와 유사합니다.
  const dynamicData = await fetch(`https://...`, { cache: "no-store" });

  // 이 요청은 10초의 유효기간을 가지고 캐시되어야 합니다.
  // `revalidate` 옵션을 사용한 `getStaticProps`와 유사합니다.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  });

  return <div>...</div>;
}
```

#### **서버 사이드 렌더링(`getServerSideProps`)**

`pages` 디렉토리에서는 `getServerSideProps`를 사용하여 데이터를 서버에서 가져와 해당 파일에서 기본 내보내기된 React 구성 요소로 props를 전달합니다.
페이지의 초기 HTML은 서버에서 프리렌더링되어 브라우저에 전송되고, 브라우저에서 페이지가 "수화(hydration)"되어 상호작용할 수 있는 형태로 만들어집니다.

```JSX
// pages/dashboard.js
// `pages` 디렉토리

export async function getServerSideProps() {
  const res = await fetch(`https://...`);
  const projects = await res.json();

  return { props: { projects } };
}

export default function Dashboard({ projects }) {
  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  );
}
```

`app` 디렉토리에서는 [서버 컴포넌트](../../GettingStarted/React_Essentials.md#server-components)를 사용하여 React 컴포넌트 내에 데이터 가져오기를 함께 배치할 수 있습니다. 이를 통해 적은 JavaScript를 클라이언트에 보내면서 서버에서 렌더링된 HTML을 유지할 수 있습니다.

`cache` 옵션을 `no-store`로 설정하면, 가져온 데이터가 [캐시되지 않도록](../DataFetching/Caching.md) 할 수 있습니다. 이는 `pages` 디렉토리에서의 `getServerSideProps`와 유사합니다.

```tsx
// app/dashboard/page.tsx
// `app` 디렉토리

// 함수명은 자유롭게 작성할 수 있습니다.
async function getProjects() {
  const res = await fetch(`https://...`, { cache: "no-store" });
  const projects = await res.json();

  return projects;
}

export default async function Dashboard() {
  const projects = await getProjects();

  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  );
}
```

#### **요청(Request) 객체에 접근하기**

`pages` 디렉토리에서는 Node.js HTTP API를 기반으로 하는 요청 기반 데이터를 검색할 수 있습니다.

예를 들어, `getServerSideProps`에서 `req` 객체를 가져와서 요청의 쿠키와 헤더를 검색하는 데 사용할 수 있습니다.

```JSX
// pages/index.js
// `pages` 디렉토리

export async function getServerSideProps({ req, query }) {
  const authHeader = req.getHeaders()['authorization'];
  const theme = req.cookies['theme'];

  return { props: { ... }}
}

export default function Page(props) {
  return ...
}
```

`app` 디렉토리에서는 다음과 같은 새로운 읽기 전용 함수를 사용하여 요청 데이터를 검색할 수 있습니다.

- [`headers()`](../../APIReference/Functions/headers.md): Web Headers API를 기반으로하며, [서버 컴포넌트](../../GettingStarted/React_Essentials.md) 내부에서 요청 헤더를 검색하는 데 사용할 수 있습니다.

- [`cookies()`]: Web Cookies API를 기반으로하며, [서버 컴포넌트](../../GettingStarted/React_Essentials.md) 내부에서 쿠키를 검색하는 데 사용할 수 있습니다.

```tsx
// app/page.tsx
// Next.js 13
import { cookies, headers } from 'next/headers';

async function getData() {
  const authHeader = headers().get('authorization');

  return ...;
}

export default async function Page() {
  // `cookies()` 또는 `headers()`를 서버 컴포넌트 안에서 직접 사용하거나,
  // 데이터 가져오기 함수 안에서 사용할 수 있습니다.
  const theme = cookies().get('theme');
  const data = await getData();
  return ...;
}
```

#### **정적 사이트 생성(`getStaticProps`)**

`pages` 디렉토리에서는 빌드할 때 `getStaticProps` 함수를 사용하여 페이지를 사전 렌더링합니다. 이 함수는 외부 API 또는 데이터베이스에서 직접 데이터를 가져와 빌드 중에 페이지 전체에 이 데이터를 전달할 수 있습니다.

```JSX
// pages/index.js
// `pages` 디렉토리

export async function getStaticProps() {
  const res = await fetch(`https://...`);
  const projects = await res.json();

  return { props: { projects } };
}

export default function Index({ projects }) {
  return projects.map((project) => <div>{project.name}</div>);
}
```

`app` 디렉토리에서 [fetch()](../../APIReference/Functions/fetch.md)를 사용한 데이터 가져오기는 기본적으로 `cache: 'force-cache'`로 설정되어 있어, 요청 데이터가 수동으로 무효화될 때까지 캐시됩니다. 이것은 `pages` 디렉토리에서의 `getStaticProps`와 유사합니다.

```JSX
// app/page.js
// `app` 디렉토리

// 함수명은 자유롭게 작성할 수 있습니다.
async function getProjects() {
  const res = await fetch(`https://...`);
  const projects = await res.json();

  return projects;
}

export default async function Index() {
  const projects = await getProjects();

  return projects.map((project) => <div>{project.name}</div>);
}
```

#### **동적 경로 (getStaticPaths)**

`pages` 디렉토리에서 `getStaticPaths` 함수는 빌드 시 미리 렌더링할 동적 경로를 정의하는 데 사용됩니다.

```JSX
// pages/posts/[id].js
// `pages` 디렉토리

export async function getStaticPaths() {
  return {
    paths: [{ params: { id: '1' } }, { params: { id: '2' } }]
  };
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://.../posts/${params.id}`);
  const post = await res.json();

  return { props: { post } };
}

export default function Post({ post }) {
  return <PostLayout post={post}>
}
```

`app` 디렉토리에서 `getStaticPaths`는 [`generateStaticParams`](../../APIReference/Functions/generateStaticParams.md)로 대체됩니다.

`generateStaticParams`는 `getStaticPaths`와 유사한 방식으로 작동하지만, 라우트 파라미터를 반환하기 위한 단순화된 API를 가지며 [레이아웃](../Routing/Pages_and_Layouts.md) 내부에서 사용할 수 있습니다. `generateStaticParams`의 반환 형식은 중첩된 `param` 객체의 배열이나 해결된(resolved) 경로 문자열 대신 세그먼트의 배열입니다.

`app` 디렉토리에서 새 모델을 사용할 때 `getStaticPaths`보다 `generateStaticParams`라는 이름을 사용하는 것이 더 적절합니다. `get` 접두사는 `getStaticProps` 및 `getServerSideProps`가 더 이상 필요하지 않기 때문에 더 구체적인 `generate`로 대체되며, `Paths` 접미사는 여러 동적 세그먼트를 가진 중첩 라우팅에 더 적합한 `Params`로 대체됩니다.

```JSX
// app/posts/[id]/page.js
// `app` 디렉토리

export async function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }]
}

async function getPost(params) {
  const res = await fetch(`https://.../posts/${params.id}`);
  const post = await res.json();

  return post;
}

export default async function Post({ params }) {
  const post = await getPost(params);

  return <PostLayout post={post}>
}
```

<br><hr><br>

#### **`fallback` 대체하기**

`pages` 디렉토리에서 `getStaticPaths`에서 반환된 `fallback` 속성은 빌드 시점에 미리 렌더링되지 않은 페이지의 동작을 정의하는 데 사용됩니다. 이 속성은 페이지가 생성되는 동안 대기하는 동안 대체 페이지를 표시하려면 `true`로 설정하고, 404 페이지를 표시하려면 `false`로 설정하거나 요청 시 페이지를 생성하려면 `blocking`으로 설정할 수 있습니다.

```JSX
// pages/posts/[id].js
// `pages` 디렉토리

export async function getStaticPaths() {
  return {
    paths: [],
    fallback: 'blocking'
  };
}

export async function getStaticProps({ params }) {
  ...
}

export default function Post({ post }) {
  return ...
}
```

`app` 디렉토리에서 [`config.dynamicParams 속성`](../../APIReference/FileConventions/Route_Segment_Config.md#configdynamicparams)은 [`generateStaticParams`](../../APIReference/Functions/generateStaticParams.md) 밖의 파라미터를 어떻게 처리할지를 제어합니다.

- `true`: (기본값) `generateStaticParams`에 포함되지 않은 동적 세그먼트는 요청 시 생성됩니다.
- `false`: `generateStaticParams`에 포함되지 않은 동적 세그먼트는 404를 반환합니다.

이는 `pages` 디렉토리의 `getStaticPaths`에서의 `fallback: true | false | 'blocking'` 옵션을 대체합니다. `fallback: 'blocking'` 옵션은 흐름상 `'blocking'`과 `true`의 차이가 미미하기 때문에 `dynamicParams`에 포함되지 않았습니다.

```JSX
// app/posts/[id]/page.js
// `app` 디렉토리

export const dynamicParams = true;

export async function generateStaticParams() {
  return [...]
}

async function getPost(params) {
  ...
}

export default async function Post({ params }) {
  const post = await getPost(params);

  return ...
}
```

[`dynamicParams`](../../APIReference/FileConventions/Route_Segment_Config.md#configdynamicparams) 속성이 `true`로 설정되어 있으면(기본값), 생성되지 않은 라우트 세그먼트가 요청되면 서버에서 렌더링되고 성공적으로 [정적 데이터](../DataFetching/DataFetching.md#static-and-dynamic-data-fetches)로 캐시됩니다.

#### **점진적 정적 재생성 (`revalidate`와 함께 쓰는 `getStaticProps`)**

`pages` 디렉토리에서 `getStaticProps` 함수를 사용하면 `revalidate` 필드를 추가하여 일정 시간 후에 페이지를 자동으로 재생성할 수 있습니다. 이것은 [점진적 정적 생성 (Incremental Static Regeneration, ISR)](../DataFetching/Fetching.md#static-data)이라고 하며, 재배포하지 않고 정적 콘텐츠를 업데이트하는 데 도움이 됩니다.

```JSX
// pages.index.js
// `pages` 디렉토리

export async function getStaticProps() {
  const res = await fetch(`https://.../posts`);
  const posts = await res.json();

  return {
    props: { posts },
    revalidate: 60,
  };
}

export default function Index({ posts }) {
  return (
    <Layout>
      <PostList posts={posts} />
    </Layout>
  );
}
```

`app` 디렉토리에서 [`fetch()`](../../APIReference/Functions/fetch.md)를 사용한 데이터 가져오기는 `revalidate`를 사용할 수 있으며, 이는 지정된 시간 동안 요청을 캐시합니다.

```JSX
// app/page.js
// `app` 디렉토리

async function getPosts() {
  const res = await fetch(`https://.../posts`, { next: { revalidate: 60 } });
  const data = await res.json();

  return data.posts;
}

export default async function PostList() {
  const posts = await getPosts();

  return posts.map((post) => <div>{post.name}</div>);
}
```

#### **API Routes**

API Routes는 `pages/api` 디렉토리에서 변경없이 계속 작동합니다. 하지만 `app` 디렉토리 내의 [Route Handlers](../Routing/Route_Handlers.md)로 대체되었습니다.

Route Handlers는 웹 [요청](https://developer.mozilla.org/en-US/docs/Web/API/Request) 및 [응답](https://developer.mozilla.org/en-US/docs/Web/API/Response) API를 사용하여 특정 경로에 대한 사용자 정의 요청 핸들러를 생성할 수 있도록 해줍니다.

```tsx
// app/api/route.ts
export async function GET(request: Request) {}
```

> Note : 이전에 클라이언트에서 외부 API를 호출하기 위해 API routes를 사용했었다면, 이제는 보안적인 이유로 [서버 컴포넌트](../../GettingStarted/React_Essentials.md#server-components)를 사용하여 데이터를 안전하게 가져올 수 있습니다. [데이터 가져오기](../DataFetching/Fetching.md)에 대해 더 알아보세요.

<br>

### 7단계 : 스타일링

`pages` 디렉토리에서, 전역 스타일시트는 `pages/_app.js` 에만 적용할 수 있습니다. 그러나 `app` 디렉토리를 사용하면 이러한 제약이 해제됩니다. 전역 스타일은 어떠한 레이아웃, 페이지 또는 컴포넌트에도 추가할 수 있습니다.

- [CSS Modules](../Styling/CSS_Modules.md)
- [Tailwind CSS](../Styling/Tailwind_CSS.md)
- [Global Styles](../Styling/CSS_Modules.md#global-styles)
- [CSS-in-JS](../Styling/CSS-in-js.md)
- [External Stylesheet](../Styling/CSS_Modules.md#external-stylesheets)
- [Sass](../Styling/Sass.md)

#### Tailwind CSS

Tailwind CSS를 사용하고 있다면, `tailwind.config.js` 파일에 `app` 디렉토리를 추가하여야 합니다.

```js
// tailwind.config.js
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx,mdx}", // <-- Add this line
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
  ],
};
```

또한, 전역 스타일을 `app/layout.js` 파일에 import하여야 합니다.

```JSX
// app/layout.js
import '../styles/globals.css';

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

[Tailwind CSS를 사용한 스타일링](../Styling/Tailwind_CSS.md)에 대해 알아봅니다.

<br><hr><br>

## **Codemods**

Next.js는 기능이 사용되지 않게 될 때 코드베이스를 업그레이드하는 데 도움이 되는 Codemod 변환을 제공합니다. 자세한 내용은 [Codemods](./Codemods.md)를 참조하십시오.
