# **robots.txt**

검색 엔진 크롤러가 사이트에서 어떤 URL에 액세스할 수 있는지 알려주기 위해 **`app`** 디렉터리의 **루트**에 [로봇 배제 표준](https://en.wikipedia.org/wiki/Robots.txt#Standard)과 일치하는 **`robots.txt`** 파일을 추가하거나 생성하세요.

## **[정적 `robots.txt`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#static-robotstxt)**

app/robots.txt

```
User-Agent: *Allow: /Disallow: /private/Sitemap: https://acme.com/sitemap.xml
```

## [로봇 파일 생성](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#generate-a-robots-file)

**[Robots](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#robots-object)** 반환하는 **`robots.js`** 또는 **`robots.ts`** 파일을 추가하세요.

app/robots.ts

```
import { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: {
      userAgent: '*',
      allow: '/',
      disallow: '/private/',
    },
    sitemap: 'https://acme.com/sitemap.xml',
  }
}
```

출력 결과:

```
User-Agent: *Allow: /Disallow: /private/Sitemap: https://acme.com/sitemap.xml
```

### [로봇 객체](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#robots-object)

```
type Robots = {
  rules:
    | {
        userAgent?: string | string[]
        allow?: string | string[]
        disallow?: string | string[]
        crawlDelay?: number
      }
    | Array<{
        userAgent: string | string[]
        allow?: string | string[]
        disallow?: string | string[]
        crawlDelay?: number
      }>
  sitemap?: string | string[]
  host?: string
}
```

## [버전 히스토리](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#version-history)

| 버전    | 변경 사항          |
| ------- | ------------------ |
| v13.3.0 | robots introduced. |
