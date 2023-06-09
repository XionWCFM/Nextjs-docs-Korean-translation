# webVitalsAttribution

공식 문서 : [https://nextjs.org/docs/app/api-reference/next-config-js/webVitalsAttribution](https://nextjs.org/docs/app/api-reference/next-config-js/webVitalsAttribution)

Web Vitals와 관련된 문제를 디버깅할 때 문제의 원인을 정확히 찾을 수 있다면 도움이 됩니다. 예를 들어, Cumulative Layout Shift (CLS)의 경우 단일 가장 큰 레이아웃 이동이 발생했을 때 첫 번째로 이동한 요소를 알고 싶을 수 있습니다. 또는 Largest Contentful Paint (LCP)의 경우, 페이지의 LCP에 해당하는 요소를 식별하고 싶을 수 있습니다. LCP 요소가 이미지인 경우 이미지 리소스의 URL을 알면 최적화해야 할 에셋(asset)을 찾는 데 도움이 될 수 있습니다.

Web Vitals 점수에서 가장 큰 영향을 미치는 요소를 찾는 것, 즉 [속성(attribution)](https://github.com/GoogleChrome/web-vitals/blob/4ca38ae64b8d1e899028c692f94d4c56acfc996c/README.md#attribution)을 통해 [PerformanceEventTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEventTiming), [PerformanceNavigationTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming) 및 [PerformanceResourceTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming)과 같은 더 자세한 정보를 얻을 수 있습니다.

Next.js에서는 기본적으로 속성(attribution)이 비활성화되어 있지만, `next.config.js` 파일에 다음과 같이 작성하여 각 지표(metric)별로 활성화할 수 있습니다.

```jsx
// next.config.js

experimental: {
  webVitalsAttribution: ["CLS", "LCP"];
}
```

유효한 속성(attribution) 값은 [`NextWebVitalsMetric`](https://github.com/vercel/next.js/blob/442378d21dd56d6e769863eb8c2cb521a463a2e0/packages/next/shared/lib/utils.ts#L43) 유형에 지정된 모든 `web-vitals` 지표(metric)입니다.
