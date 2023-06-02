# useReportWebVitals

공식 문서 : [https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals)

`useReportWebVitals` 훅을 사용하여 [Core Web Vitals](https://web.dev/vitals/)를 보고할 수 있으며, 다른 분석 서비스와 함께 사용할 수 있습니다.

```jsx
// app/_components/web-vitals.js

"use client";

import { useReportWebVitals } from "next/web-vitals";

export function WebVitals() {
  useReportWebVitals((metric) => {
    console.log(metric);
  });
}
```

```jsx
// app/layout.js

import { WebVitals } from "./_components/web-vitals";

export default function Layout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  );
}
```

> `useReportWebVitals` 훅은 `"use client"` 지시문을 필요로 하기 때문에, 성능 면에서 가장 우수한 접근 방식은 루트 레이아웃이 가져오는 별도의 컴포넌트를 생성하는 것입니다. 이렇게 하면 클라이언트 영역이 오로지 `WebVitals` 컴포넌트에 국한됩니다.

<br><hr><br>

## useReportWebVitals

훅의 인자로 전달되는 `metric` 객체는 다수의 속성으로 구성됩니다.

- `id` : 현재 페이지 로드의 맥락에서 해당 metric에 대한 고유 식별자

- `name` : 성능 metric의 이름입니다. 웹 애플리케이션에 특정한 [Web Vitals](./useReportWebVitals#web-vitals)(TTFB, FCP, LCP, FID, CLS)의 이름이 가능한 값으로 포함됩니다.

- `delta` : metric의 현재 값과 이전 값의 차이입니다. 값은 일반적으로 밀리초 단위이며, 시간에 따른 metric 값의 변화를 나타냅니다.

- `entries` : metric과 관련된 [성능 항목](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry)의 배열입니다. 이 항목들은 metric과 관련된 성능 이벤트에 대한 자세한 정보를 제공합니다.

- `navigationType` : metric의 수집을 발생시킨 [navigation 유형](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming/type)을 나타냅니다. `"navigate"`, `"reload"`, `"back_forward"`, `"prerender"`와 같은 값을 사용할 수 있습니다.

- `rating` : metric 값의 정성적인 등급으로, 성능에 대한 평가를 제공합니다. 가능한 값은 `"good"`, `"needs-improvement"`, `"poor"`입니다. 등급은 일반적으로 사전 정의된 임계값과 비교하여 metric 값이 수용 가능한지 또는 최적이 아닌지를 판단하여 결정됩니다.

- `value` : 성능 항목의 실제 값 또는 기간으로, 일반적으로 밀리초 단위입니다. 이 값은 metric으로 추적되는 성능 측면의 양적인 측정 값을 제공합니다. 값의 출처는 측정되는 특정 metric에 따라 다를 수 있으며, 다양한 [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance_API)에서 가져올 수 있습니다.

<br><hr><br>

## Web Vitals

[Web Vitals](https://web.dev/vitals/)는 웹 페이지의 사용자 경험을 포착하기 위한 유용한 metric의 집합입니다. 다음과 같은 Web Vital이 모두 포함되어 있습니다.

- [Time to First Byte](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_first_byte) (TTFB)
- [First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint) (FCP)
- [Largest Contentful Paint](https://web.dev/lcp/) (LCP)
- [First Input Delay](https://web.dev/fid/) (FID)
- [Cumulative Layout Shift](https://web.dev/cls/) (CLS)
- [Interaction to Next Paint](https://web.dev/inp/) (INP)

`name` 속성을 사용하여 이러한 metric의 모든 결과를 처리할 수 있습니다.

```tsx
// app/components/web-vitals.tsx

"use client";

import { useReportWebVitals } from "next/web-vitals";

export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case "FCP": {
        // FCP 결과를 처리합니다.
      }
      case "LCP": {
        // LCP 결과를 처리합니다.
      }
      // ...
    }
  });
}
```

<br><hr><br>

## Vercel에서의 사용법

[Vercel Speed Insights](https://vercel.com/docs/concepts/speed-insights)는 Vercel 배포에서 자동으로 구성되며, `useReportWebVitals`를 사용하지 않아도 됩니다. 이 훅은 로컬에서 개발하는 경우 또는 다른 분석 서비스를 이용하는 경우에 유용합니다.

<br><hr><br>

## 외부 시스템으로 결과 전송하기

사용자의 실제 성능을 측정하고 추적하기 위해 결과를 원하는 엔드포인트로 전송할 수 있습니다.

예시:

```jsx
useReportWebVitals((metric) => {
  const body = JSON.stringify(metric);
  const url = "https://example.com/analytics";

  // 가능한 경우 `navigator.sendBeacon()`을 사용하고, 그렇지 않은 경우 `fetch()`를 사용합니다.
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, body);
  } else {
    fetch(url, { body, method: "POST", keepalive: true });
  }
});
```

> Note : Google Analytics를 사용하는 경우, id 값을 사용하여 수동으로 metric 분포를 구성할 수 있습니다(백분위수 계산 등).

```jsx
useReportWebVitals((metric) => {
  // Google Analytics를 초기화한 경우, 다음 예제와 같이 `window.gtag`를 사용하세요.
  // https://github.com/vercel/next.js/blob/canary/examples/with-google-analytics/pages/_app.js
  window.gtag("event", metric.name, {
    value: Math.round(
      metric.name === "CLS" ? metric.value * 1000 : metric.value
    ), // value는 반드시 정수여야 합니다.
    event_label: metric.id, // 현재 페이지 로드에서 고유한 id
    non_interaction: true, // 이탈률(Bounce Rate)에 영향을 주지 않도록 합니다.
  });
});
```

[Google Analytics로 결과를 전송하는 방법에 대해 자세히 알아봅니다.](https://github.com/GoogleChrome/web-vitals#send-the-results-to-google-analytics)
