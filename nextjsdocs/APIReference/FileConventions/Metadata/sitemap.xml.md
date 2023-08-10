# **sitemap.xml**

사이트맵 XML 형식을 일치시키는 **`app`** 디렉터리의 **루트**에 [사이트맵 XML 형식](https://www.sitemaps.org/protocol.html)의 **`sitemap.xml`** 파일을 추가하거나 생성하여 검색 엔진 크롤러가 사이트를 더 효율적으로 크롤링할 수 있도록 도와주세요.

## **[정적 `sitemap.xml`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#static-sitemapxml)**

app/sitemap.xml

```
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
</urlset>
```

## [사이트맵 생성](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#generate-a-sitemap)

[Sitemap](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-return-type)을 반환하는 **`sitemap.js`** 또는 **`sitemap.ts`** 파일을 추가하세요.

app/sitemap.ts

```
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://acme.com',
      lastModified: new Date(),
    },
    {
      url: 'https://acme.com/about',
      lastModified: new Date(),
    },
    {
      url: 'https://acme.com/blog',
      lastModified: new Date(),
    },
  ]
}
```

출력 결과:

acme.com/sitemap.xml

```
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
</urlset>
```

### [사이트맵 반환 유형](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-return-type)

```
type Sitemap = Array<{
  url: string
  lastModified?: string | Date
}>
```

> **알아두면 좋은 점**
>
> - **미래에는 여러 개의 사이트맵과 사이트맵 인덱스를 지원할 예정입니다.**

## [버전 히스토리](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#version-history)

| 버전    | 변경 사항           |
| ------- | ------------------- |
| v13.3.0 | sitemap introduced. |
