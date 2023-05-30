원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/eslint

<br>

# **eslint**

프로젝트에서 ESLint가 감지되면 오류가 있는 경우 Next.js는 **프로덕션 빌드**(`next build`)에 실패합니다.

애플리케이션에 ESLint 오류가 있는 경우에도 Next.js가 프로덕션 코드를 생성하도록 하려면 built-in linting 단계를 완전히 비활성화하면 됩니다. 워크플로우의 별도 부분(예: CI 또는 pre-commit hook)에서 실행하도록 ESLint를 이미 구성한 경우가 아니라면 이 방법을 권장하지 않습니다.

<br>

`next.config.js`를 열고 `eslint` 구성에서 `ignoreDuringBuilds` 옵션을 활성화합니다:

next.config.js

```jsx
module.exports = {
  eslint: {
    // Warning: This allows production builds to successfully complete even if
    // your project has ESLint errors.
    ignoreDuringBuilds: true,
  },
};
```
