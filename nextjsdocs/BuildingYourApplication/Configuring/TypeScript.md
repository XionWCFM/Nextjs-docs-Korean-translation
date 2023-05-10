# TypeScript

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/typescript](https://nextjs.org/docs/app/building-your-application/configuring/typescript)

Next.js는 React 애플리케이션을 만들기 위한 TypeScript 기반 개발 환경을 제공합니다.

내장된 TypeScript 지원으로 필요한 패키지를 자동으로 설치하고 적절한 설정을 구성할 수 있으며, 에디터용 [TypeScript 플러그인](https://nextjs.org/docs/app/building-your-application/configuring/typescript#using-the-typescript-plugin)도 제공됩니다.

> 🎥 **Watch**: 내장된 TypeScript 플러그인 알아보기 → [YouTube (3 minutes)](https://www.youtube.com/watch?v=pqMqn9fKEf8)

## New Projects (새 프로젝트 생성)

`create-next-app` 이제 기본적으로 TypeScript와 함께 제공됩니다.

```bash
npx create-next-app@latest
```

## Existing Projects (기존 프로젝트에 추가)

기존 프로젝트에 TypeScript를 추가하려면 파일 확장자를 `.ts` 또는 `.tsx`로 변경하면 됩니다. 그 후` next dev`와 `next build`를 실행하면 필요한 종속성이 자동으로 설치되며 권장 구성 옵션을 사용한 `tsconfig.json` 파일이 추가됩니다.

## The TypeScript Plugin

Next.js에는 VSCode 및 다른 코드 에디터에서 사용할 수 있는 고급 타입 체크 및 자동 완성을 위한 사용자 정의 TypeScript 플러그인 및 타입 체커가 포함되어 있습니다.

TypeScript 파일을 열고 처음으로 `next dev`를 실행하면 플러그인을 활성화할 것인지 묻는 메시지가 표시됩니다.

![](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Ftypescript-prompt.png&w=3840&q=75)

메시지를 놓치면 다음과 같은 방법으로 플러그인을 수동으로 활성화할 수 있습니다.

1. 명령 팔레트를 엽니다. (`Ctrl/⌘` + `Shift` + `P`)
2. "TypeScript: Select TypeScript Version"을 검색합니다.
3. "Use Workspace Version"을 선택합니다.

![](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Ftypescript-command-palette.png&w=3840&q=75)

이제 파일을 편집할 때 맞춤형 플러그인이 활성화됩니다. `next build`를 실행할 때 맞춤형 타입 체커가 사용됩니다. 또한 이러한 과정을 자동화하기 위해 VSCode 설정 파일이 자동으로 생성됩니다.

## Plugin Features

TypeScript 플러그인은 다음과 같은 기능을 수행할 수 있습니다.

- [세그먼트 구성 옵션(segment config options)](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)에 잘못된 값이 전달되는 경우 경고를 표시합니다.
- 사용 가능한 옵션과 문서를 제공합니다.
- `use client` 지시어가 올바르게 사용되도록 보장합니다.
- 클라이언트 컴포넌트에서만 클라이언트 훅(ex. `useState`)을 사용하는지 확인합니다.

> 참고: 이후 더 많은 기능이 추가될 예정입니다.

## Minimum TypeScript Version

[type modifiers on import names](https://devblogs.microsoft.com/typescript/announcing-typescript-4-5/#type-on-import-names)과 [performance improvements](https://devblogs.microsoft.com/typescript/announcing-typescript-4-5/#real-path-sync-native)와 같은 구문 기능을 얻으려면 TypeScript의 적어도 `v4.5.2`를 사용하는 것이 매우 권장됩니다.

## Statically Typed Links

Next.js는 `next/link`를 사용할 때 오타 및 기타 오류를 방지하기 위해 링크를 정적으로 타입화하여 페이지 간 탐색 시 유형 안전성을 향상시킵니다.

이 기능을 활성화하려면 `experimental.typedRoutes`가 사용 가능 상태여야 하고 프로젝트가 TypeScript를 사용해야 합니다.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    typedRoutes: true,
  },
};

module.exports = nextConfig;
```

Next.js는 `.next/types`에 현재 애플리케이션의 모든 기존 라우트 정보가 포함된 링크 정의를 생성합니다. TypeScript는 이를 사용하여 편집기에서 유효하지 않은 링크에 대한 피드백을 제공할 수 있습니다.

현재 실험적 지원은 동적 세그먼트를 포함한 모든 문자열 리터럴을 포함합니다. 그러나 문자열 리터럴이 아닌 경우, `href`를 `as Route`로 수동 캐스팅해야 합니다:

```js
import type { Route } from 'next';
import Link from 'next/link'

// No TypeScript errors if href is a valid route
<Link href="/about" />
<Link href="/blog/nextjs" />
<Link href={`/blog/${slug}`} />
<Link href={('/blog' + slug) as Route} />

// TypeScript errors if href is not a valid route
<Link href="/aboot" />
```

`next/link`을 래핑하는 사용자 정의 컴포넌트에서 `href`를 허용하려면 제네릭을 사용하세요:

```js
import type { Route } from 'next';
import Link from 'next/link';

function Card<T extends string>({ href }: { href: Route<T> | URL })
  return (
    <Link href={href}>
      <div>My Card</div>
    </Link>
  );
}
```

> 이것은 어떻게 작동하나요? <br /> next dev 또는 next build를 실행할 때, Next.js는 응용 프로그램 내에 있는 모든 존재하는 경로(유효한 모든 경로를 Link의 href 타입으로)에 대한 정보를 포함하는 .next 내부의 숨겨진 .d.ts 파일을 생성합니다. 이 .d.ts 파일은 tsconfig.json에 포함되며, TypeScript 컴파일러는 이 .d.ts 파일을 확인하여 에디터에서 유효하지 않은 링크에 대한 피드백을 제공합니다.

## End-to-End Type Safety

Next.js 13에서는 **타입 안정성이 향상되었습니다.** 이에는 다음이 포함됩니다:

1. **페이지와 데이터 가져오기(fetching function) 사이의 직렬화(serialization)가 없습니다** : 서버에서 직접 컴포넌트, 레이아웃 및 페이지에서 데이터를 가져올 수 있습니다. 이 데이터는 React에서 사용하기 위해 문자열로 변환되지 않아도 클라이언트 측으로 전달될 필요가 없습니다. 대신 `app`은 기본적으로 Server Components를 사용하므로 추가적인 단계 없이 `Date`, `Map`, `Set` 등의 값을 사용할 수 있습니다. 이전에는 서버와 클라이언트 간의 경계를 수동으로 Next.js 특정 타입으로 지정해야 했습니다.

2. **컴포넌트 간 데이터 흐름 최적화** :
   `_app` 컴포넌트를 루트 레이아웃으로 대체함으로써, 컴포넌트와 페이지 간의 데이터 흐름을 시각화하기가 더욱 쉬워졌습니다. 이전에는 개별 페이지와 `_app` 간의 데이터 흐름을 입력하는 것이 어려웠고, 혼란스러운 버그를 일으킬 수 있었습니다. Next.js 13에서 [공간적인 데이터 검색(co-located data fetching)](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching)을 사용하면 위와 같은 문제를 해결할 수 있습니다.

Next.js의 [데이터 가져오기(Data Fetching)](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching)는 데이터베이스나 콘텐츠 제공자 선택에 대해서는 규정적이지 않으면서 최대한 끝단의 타입 안전성(end-to-end type safety)을 제공합니다.

우리는 일반적인 TypeScript에서 예상하는 것과 같이 응답 데이터(response data)를 타입화할 수 있습니다. 예를 들어 :

```js
async function getData() {
  const res = await fetch("https://api.example.com/...");
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.
  return res.json();
}

export default async function Page() {
  const name = await getData();

  return "...";
}
```

완전한 end-to-end 타입 안전성을 보장하려면, 데이터베이스나 콘텐츠 제공 업체도 TypeScript를 지원해야 합니다. 이는 ORM 또는 타입 안전한 쿼리 빌더를 사용하여 구현할 수 있습니다.

## Async Server Component TypeScript Error

- async Server Components를 사용할 경우 해당 컴포넌트를 사용하는 곳에서 `'Promise<Element>' is not a valid JSX element` 타입 에러가 발생합니다.

- 이는 TypeScript의 알려진 이슈로 현재 수정 작업이 진행 중입니다.

- 일시적인 해결 방법으로 컴포넌트 위에 `{/* @ts-expect-error Async Server Component */}` 주석을 추가하여 해당 컴포넌트의 타입 체크를 비활성화할 수 있습니다.

```js
import { ExampleAsyncComponent } from "./ExampleAsyncComponent";
export default function Page() {
  return (
    <>
      {/* @ts-expect-error Async Server Component */}
      <ExampleAsyncComponent />
    </>
  );
}
```

- 이는 Layout 및 Page 컴포넌트에는 적용되지 않습니다.

- 우리는 여기에서 이 문제를 추적하고 있습니다. 이는 아마도 TypeScript 5.1 (Stable)에서 고칠 것입니다.

## Passing Data Between Server & Client Components

서버 컴포넌트와 클라이언트 컴포넌트 간에 props를 통해 데이터를 전달할 때는 데이터가 여전히 브라우저에서 사용할 수 있도록 직렬화됩니다(문자열로 변환). 그러나 이 데이터는 특별한 타입이 필요하지 않습니다. 컴포넌트 간에 다른 props를 전달하는 것과 동일하게 타입이 지정됩니다.

또한, 렌더링되지 않은 데이터는 서버와 클라이언트 사이를 넘나들지 않기 때문에 직렬화해야 하는 코드가 줄어듭니다. 이것은 서버 컴포넌트를 지원함으로써 가능해졌습니다.

## Path aliases and baseUrl

Next.js는 자동으로 `tsconfig.json`의 `"paths"`와 `"baseUrl"`옵션을 지원합니다.

이 기능에 대해 자세히 알아보려면 [모듈 경로 별칭 문서](https://nextjs.org/docs/pages/building-your-application/configuring/absolute-imports-and-module-aliases)를 참조하십시오

## Type checking next.config.js

`next.config.js` 파일은 Babel이나 TypeScript에 의해 파싱되지 않기 때문에 JavaScript 파일이어야합니다. 그러나 다음과 같이 JSDoc을 사용하여 IDE에서 일부 타입 체크를 추가할 수 있습니다 :

```js
// @ts-check

/**
 * @type {import('next').NextConfig}
 **/
const nextConfig = {
  /* config options here */
};

module.exports = nextConfig;
```

## Incremental type checking

v10.2.1부터 Next.js는 `tsconfig.json`에서 활성화할 경우 [증분형 타입 체크(Incremental type checking)](https://www.typescriptlang.org/tsconfig#incremental)를 지원합니다. 이 기능은 대규모 애플리케이션에서 타입 체크 속도를 높일 수 있습니다.

## Ignoring TypeScript Errors

Next.js는 프로젝트 내에 TypeScript 오류가 있을 때 **제작 빌드**(`next build`)를 실패시킵니다.

애플리케이션에 오류가 있더라도 Next.js가 프로덕션 코드를 생성하도록 하려면, 내장된 타입 체크 단계를 비활성화할 수 있습니다.

만약 이를 비활성화하면 빌드 또는 배포 프로세스 중에 타입 체크를 실행하는 것이 좋습니다. 그렇지 않으면 매우 위험할 수 있습니다.

`next.config.js`를 열고 `typescript` 구성에서 `ignoreBuildErrors` 옵션을 활성화합니다.

```js
module.exports = {
  typescript: {
    // !! WARN !!
    // Dangerously allow production builds to successfully complete even if
    // your project has type errors.
    // !! WARN !!
    ignoreBuildErrors: true,
  },
};
```
