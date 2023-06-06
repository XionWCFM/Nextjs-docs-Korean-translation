# transpilePackages

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/transpilePackages](https://nextjs.org/docs/app/api-reference/next-config-js/transpilePackages)

Next.js는 자동으로 로컬 패키지(모노레포와 같은)나 외부 종속성(`node_modules`)에서 종속성을 트랜스파일하고 번들링할 수 있습니다. 이는 `next-transpile-modules` 패키지를 대체합니다.

```jsx
// next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  transpilePackages: ["@acme/ui", "lodash-es"],
};

module.exports = nextConfig;
```

<br><hr><br>

## 버전 히스토리

| Version   | Changes                  |
| --------- | ------------------------ |
| `v13.0.0` | `transpilePackages 추가` |
