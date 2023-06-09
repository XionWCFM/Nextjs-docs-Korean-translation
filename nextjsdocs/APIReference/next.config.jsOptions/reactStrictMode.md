# reactStrictMode

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/reactStrictMode](https://nextjs.org/docs/app/api-reference/next-config-js/reactStrictMode)

> 권장됨 : 향후의 React에 대비하기 위해 Next.js 애플리케이션에서 Strict Mode를 활성화하는 것을 강력히 권장합니다.

React의 Strict Mode는 애플리케이션에서 잠재적인 문제를 강조하기 위한 개발 모드 전용 기능입니다. 이는 안전하지 않은 생명 주기(Lifecycles), 레거시 API 사용 및 여러 다른 기능을 식별하는 데 도움이 됩니다.

Next.js 런타임은 Strict Mode를 준수합니다. Strict Mode를 사용하려면 next.config.js에서 다음 옵션을 구성하십시오.

```jsx
// next.config.js

module.exports = {
  reactStrictMode: true,
};
```

만약 애플리케이션 전체에 Strict Mode를 사용할 준비가 되지 않았다면, 그래도 괜찮습니다! `<React.StrictMode>`를 사용하여 페이지별로 점진적으로 마이그레이션할 수 있습니다.
