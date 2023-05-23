원본 링크: [https://nextjs.org/docs/app/building-your-application/optimizing/analytics](https://nextjs.org/docs/app/building-your-application/optimizing/analytics)

# Anlaytics

[Next.js Speed Insights](https://vercel.com/analytics)는 다양한 지표를 사용하여 페이지의 성능을 분석하고 측정할 수 있는 기능을 제공합니다.
[Vercel deployments](https://vercel.com/docs/concepts/speed-insights?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)에서는 구성 없이도 실제 경험 점수([Real Experience Score](https://vercel.com/docs/concepts/speed-insights#core-web-vitals?utm_source=next-site&utm_medium=docs&utm_campaign=next-website))를 수집할 수 있습니다.
이 문서의 나머지 부분에서는 Next.js Speed Insights에서 사용하는 내장된 리레이어(relayer)에 대해 설명합니다.

## Web Vitals

웹 비탈([Web Vitals](https://web.dev/vitals/) )은 웹 페이지의 사용자 경험을 측정하기 위한 유용한 지표들의 모음입니다. 다음과 같은 웹 비탈들이 모두 포함되어 있습니다.

- [Time to First Byte](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_first_byte) (TTFB)
- [First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint) (FCP)
- [Largest Contentful Paint](https://web.dev/lcp/) (LCP)
- [First Input Delay](https://web.dev/fid/) (FID)
- [Cumulative Layout Shift](https://web.dev/cls/) (CLS)
- [Interaction to Next Paint](https://web.dev/inp/) (INP) *(experimental)*
