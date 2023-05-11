원본 링크: [https://nextjs.org/docs/app/api-reference/next-config-js/pageExtensions](https://nextjs.org/docs/app/api-reference/next-config-js/pageExtensions)

---

기본적으로 Next.js는 확장자가. tsx,. ts,. jsx,. js인 파일을 허용합니다. 마크다운(. md,. mdx)과 같은 다른 확장자를 허용하도록 수정할 수 있습니다.

```jsx
const withMDX = require('@next/mdx')();
 
/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ['ts', 'tsx', 'mdx'],
  experimental: {
    mdxRs: true,
  },
};
 
module.exports = withMDX(nextConfig);
```

Next.js의 고급 사용자 정의 설정을 위해 프로젝트 디렉토리의 루트 (package.json 옆)에 next.config.js 또는 next.config.mjs 파일을 생성할 수 있습니다.

next.config.js는 JSON 파일이 아닌 일반적인 Node.js 모듈입니다. Next.js 서버 및 빌드 단계에서 사용되며 브라우저 빌드에 포함되지 않습니다.

다음은 next.config.js 예제입니다.

```jsx
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* config options here */
};
 
module.exports = nextConfig;
```

만약 ECMAScript 모듈이 필요하다면, next.config.mjs를 사용할 수 있습니다:

```jsx
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* config options here */
};
 
export default nextConfig;
```

또한 함수를 사용할 수 있습니다.

```jsx
module.exports = (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* config options here */
  };
  return nextConfig;
};
```

Next.js 12.1.0부터 async함수를 사용할 수 있습니다:

```jsx
module.exports = async (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* config options here */
  };
  return nextConfig;
};
```

phase은 구성이 로드되는 현재 컨텍스트입니다. 사용 가능한 단계를 확인할 수 있습니다. 단계는 next/constants에서 가져올 수 있습니다.

```jsx
const { PHASE_DEVELOPMENT_SERVER } = require('next/constants');
 
module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* development only config options here */
    };
  }
 
  return {
    /* config options for all phases except development here */
  };
};
```

주석 처리된 줄은 next.config.js에서 정의된 구성을 입력할 수 있는 위치입니다. 하지만 이러한 구성 중 어떤 것도 필수는 아니며, 각 구성이 무엇을 하는지 이해할 필요도 없습니다. 대신 이 섹션에서 활성화하거나 수정해야 할 기능을 검색하고, 해당 기능에 대한 지침을 찾아보세요.

대상 Node.js 버전에서 사용할 수 없는 새로운 JavaScript 기능을 사용하지 마세요. next.config.js는 Webpack, Babel 또는 TypeScript에서 구문 분석되지 않습니다.