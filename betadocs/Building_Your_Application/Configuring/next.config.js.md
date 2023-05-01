# next.config.js

Next.js는 next.config.js 파일을 프로젝트 디렉토리의 루트에 위치시키면 해당 파일을 통해 구성될 수 있습니다.

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  /* 여기에서는 config 옵션들을 설정할 수 있습니다. */
};

module.exports = nextConfig;
```

<aside>
💡 참고: ES Modules을 선호하는 경우 next.config.mjs도 지원됩니다.

</aside>

이 페이지는 /app 디렉토리에 대한 구성 옵션에 대한 문서입니다. 모든 가능한 옵션의 전체 목록을 보려면 next.config.js (stable) 문서를 참조해주세요.

## 실험적인 기능(Experimental)

| Key                              | Example      | Data Type |
| -------------------------------- | ------------ | --------- |
| appDir                           | true         | Boolean   |
| mdxRs                            | true         | Boolean   |
| typeRoutes                       | true         | Boolean   |
| serverComponentsExternalPackages | ['@acme/ui'] | String[]  |

## 안정화된 기능(Stable)

| Key               | Example              | Data type |
| ----------------- | -------------------- | --------- |
| transpilePackages | ['@acme/ui']         | String[]  |
| pageExtensions    | ['ts', 'tsx', 'mdx'] | String[]  |

## appDir

앱 라우터(app directory)는 레이아웃(layouts), 서버 컴포넌트(Server Components), 스트리밍(Streaming) 및 공간 데이터 가져오기(colocated data fetching)를 지원합니다.

app 디렉토리를 사용하면 자동으로 React Strict Mode가 활성화됩니다. app을 점진적으로 도입하는 방법을 알아보세요.

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    appDir: true,
  },
};

module.exports = nextConfig;
```

## mdxRs

@next/mdx와 함께 사용합니다. 새로운 Rust 컴파일러를 사용하여 MDX 파일을 컴파일합니다.

```jsx
next.config.js;

const withMDX = require("@next/mdx")();

/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ["ts", "tsx", "mdx"],
  experimental: {
    appDir: true,
    mdxRs: true,
  },
};

module.exports = withMDX(nextConfig);
```

## typedRoutes

이 기능은 정적 타입 링크에 대한 실험적인 지원을 제공합니다. 이 기능을 사용하려면 프로젝트에서 TypeScript를 사용하고 앱 디렉토리 플래그 (appDir: true)를 사용해야합니다.

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    appDir: true,
    typedRoutes: true,
  },
};

module.exports = nextConfig;
```

## serverComponentsExternalPackages

서버 컴포넌트(Server Components)와 라우트 핸들러(Route Handlers) 내에서 사용되는 종속성은 자동으로 Next.js에서 번들링됩니다.

만약 종속성이 Node.js 특정 기능을 사용하는 경우, 해당 종속성을 서버 컴포넌트 번들링에서 제외하고 기본 Node.js의 require를 사용할 수 있도록 선택할 수 있습니다.

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    serverComponentsExternalPackages: ["@acme/ui"],
  },
};

module.exports = nextConfig;
```

Next.js는 현재 호환성 작업을 진행 중인 인기 있는 패키지의 짧은 목록을 포함하고 있으며, 이러한 패키지는 자동으로 선택적으로 제외(opt-out)됩니다.

- `@prisma/client`
- `@sentry/nextjs`
- `@sentry/node`
- `autoprefixer`
- `aws-crt`
- `bcrypt`
- `cypress`
- `eslint`
- `express`
- `firebase-admin`
- `jest`
- `lodash`
- `mongodb`
- `next-mdx-remote`
- `next-seo`
- `postcss`
- `prettier`
- `prisma`
- `rimraf`
- `sharp`
- `shiki`
- `sqlite3`
- `tailwindcss`
- `ts-node`
- `typescript`
- `vscode-oniguruma`
- `webpack`

## transpilePackages

Next.js 13에서는 로컬 패키지(모노레포와 같은) 또는 외부 종속성(node_modules)에서 종속성을 자동으로 트랜스파일링하고 번들링할 수 있습니다. 이 기능은 next-transpile-modules 패키지를 대체합니다.

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  transpilePackages: ["@acme/ui", "lodash-es"],
};

module.exports = nextConfig;
```

## pageExtensions

기본적으로 Next.js는 다음 확장자를 가진 파일을 받아들입니다: .tsx, .ts, .jsx, .js. 이는 마크다운(.md, .mdx)과 같은 다른 확장자를 허용하도록 수정할 수 있습니다.

```jsx
next.config.js;

const withMDX = require("@next/mdx")();

/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ["ts", "tsx", "mdx"],
  experimental: {
    appDir: true,
    mdxRs: true,
  },
};

module.exports = withMDX(nextConfig);
```
