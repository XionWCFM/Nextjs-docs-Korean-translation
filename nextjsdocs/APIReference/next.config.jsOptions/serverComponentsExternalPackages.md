# 서버 컴포넌트 외부 패키지

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/serverComponentsExternalPackages](https://nextjs.org/docs/app/api-reference/next-config-js/serverComponentsExternalPackages)

[서버 컴포넌트](../../GettingStarted/React_Essentials.md#server-components) 및 [라우트 핸들러](../../BuildingYourApplication/Routing/Route_Handlers.md) 내에서 사용되는 종속성은 Next.js에 의해 자동으로 번들링됩니다.

특정 종속성이 Node.js의 특정 기능을 사용하는 경우, 서버 컴포넌트 번들링에서 해당 종속성을 제외하고 네이티브 Node.js `require`를 사용할 수 있습니다.

```jsx
// next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    serverComponentsExternalPackages: ["@acme/ui"],
  },
};

module.exports = nextConfig;
```

Next.js는 현재 호환성 작업중이며 자동으로 제외되는 [인기 있는 패키지의 짧은 목록](https://github.com/vercel/next.js/blob/canary/packages/next/src/lib/server-external-packages.json)을 포함합니다.

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
