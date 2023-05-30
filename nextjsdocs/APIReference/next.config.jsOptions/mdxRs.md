공유 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/mdxRs

<br>

# **mdxRs**

`@next/mdx`와 함께 사용합니다. 새로운 Rust 컴파일러를 사용하여 MDX 파일을 컴파일합니다.

next.config.js

```jsx
const withMDX = require("@next/mdx")();

/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ["ts", "tsx", "mdx"],
  experimental: {
    mdxRs: true,
  },
};

module.exports = withMDX(nextConfig);
```
