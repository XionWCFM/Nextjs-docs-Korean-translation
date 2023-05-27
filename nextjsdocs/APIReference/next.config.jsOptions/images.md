번역 완료 : https://nextjs.org/docs/app/api-reference/next-config-js/images

<br>

# **images**

Next.js 기본 제공 이미지 최적화 API를 사용하는 대신 클라우드 제공업체를 사용하여 이미지를 최적화하려면 다음을 사용하여 `next.config.js`를 구성할 수 있습니다:
next.config.js

```jsx
module.exports = {
  images: {
    loader: "custom",
    loaderFile: "./my/image/loader.js",
  },
};
```

<br>

이 `loaderFile`은 Next.js 애플리케이션의 루트에 상대적인 파일을 가리켜야 합니다. 파일은 문자열을 반환하는 기본 함수를 내보내야 합니다(예: 문자열):

```jsx
export default function myImageLoader({ src, width, quality }) {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`;
}
```

또는  [`loader` prop](https://nextjs.org/docs/pages/api-reference/components/image#loader)를 사용하여 `next/image`의 각 인스턴스에 함수를 전달할 수 있습니다.

<br>

---

<br>

## **[Example Loader Configuration](https://nextjs.org/docs/app/api-reference/next-config-js/images#example-loader-configuration)**

- [Akamai](https://nextjs.org/docs/app/api-reference/next-config-js/images#akamai)

- [Cloudinary](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudinary)

- [Cloudflare](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudflare)

- [Contentful](https://nextjs.org/docs/app/api-reference/next-config-js/images#contentful)

- [Fastly](https://nextjs.org/docs/app/api-reference/next-config-js/images#fastly)

- [Gumlet](https://nextjs.org/docs/app/api-reference/next-config-js/images#gumlet)

- [ImageEngine](https://nextjs.org/docs/app/api-reference/next-config-js/images#imageengine)

- [Imgix](https://nextjs.org/docs/app/api-reference/next-config-js/images#imgix)

- [Thumbor](https://nextjs.org/docs/app/api-reference/next-config-js/images#thumbor)

<br>

### **[Akamai](https://nextjs.org/docs/app/api-reference/next-config-js/images#akamai)**

```jsx
// Docs: https://techdocs.akamai.com/ivm/reference/test-images-on-demand
export default function akamaiLoader({ src, width, quality }) {
  return `https://example.com/${src}?imwidth=${width}`;
}
```

<br>

### **[Cloudinary](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudinary)**

```jsx
// Demo: https://res.cloudinary.com/demo/image/upload/w_300,c_limit,q_auto/turtles.jpg
export default function cloudinaryLoader({ src, width, quality }) {
  const params = ["f_auto", "c_limit", `w_${width}`, `q_${quality || "auto"}`];
  return `https://example.com/${params.join(",")}${src}`;
}
```

<br>

### **[Cloudflare](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudflare)**

```jsx
// Docs: https://developers.cloudflare.com/images/url-format
export default function cloudflareLoader({ src, width, quality }) {
  const params = [`width=${width}`, `quality=${quality || 75}`, "format=auto"];
  return `https://example.com/cdn-cgi/image/${params.join(",")}/${src}`;
}
```

<br>

### **[Contentful](https://nextjs.org/docs/app/api-reference/next-config-js/images#contentful)**

```jsx
// Docs: https://www.contentful.com/developers/docs/references/images-api/
export default function contentfulLoader({ src, quality, width }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set("fm", "webp");
  url.searchParams.set("w", width.toString());
  url.searchParams.set("q", quality.toString() || "75");
  return url.href;
}
```

<br>

---

<br>

## **[Fastly](https://nextjs.org/docs/app/api-reference/next-config-js/images#fastly)**

```jsx
// Docs: https://developer.fastly.com/reference/io/
export default function fastlyLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set("auto", "webp");
  url.searchParams.set("width", width.toString());
  url.searchParams.set("quality", quality.toString() || "75");
  return url.href;
}
```

<br>

---

<br>

## **[Gumlet](https://nextjs.org/docs/app/api-reference/next-config-js/images#gumlet)**

```jsx
// Docs: https://docs.gumlet.com/reference/image-transform-size
export default function gumletLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set("format", "auto");
  url.searchParams.set("w", width.toString());
  url.searchParams.set("q", quality.toString() || "75");
  return url.href;
}
```

<br>

### **[ImageEngine](https://nextjs.org/docs/app/api-reference/next-config-js/images#imageengine)**

```jsx
// Docs: https://support.imageengine.io/hc/en-us/articles/360058880672-Directives
export default function imageengineLoader({ src, width, quality }) {
  const compression = 100 - (quality || 50)
  const params = [`w_${width}`, `cmpr_${compression}`)]
  return `https://example.com${src}?imgeng=/${params.join('/')`
}
```

<br>

### **[Imgix](https://nextjs.org/docs/app/api-reference/next-config-js/images#imgix)**

```jsx
// Demo: https://static.imgix.net/daisy.png?format=auto&fit=max&w=300
export default function imgixLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  const params = url.searchParams;
  params.set("auto", params.getAll("auto").join(",") || "format");
  params.set("fit", params.get("fit") || "max");
  params.set("w", params.get("w") || width.toString());
  params.set("q", quality.toString() || "50");
  return url.href;
}
```

<br>

### **[Thumbor](https://nextjs.org/docs/app/api-reference/next-config-js/images#thumbor)**

```jsx
// Docs: https://thumbor.readthedocs.io/en/latest/
export default function thumborLoader({ src, width, quality }) {
  const params = [`${width}x0`, `filters:quality(${quality || 75})`];
  return `https://example.com${params.join("/")}${src}`;
}
```
