원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/basePath

<br>

# **basePath**

도메인의 하위 경로 아래에 Next.js 애플리케이션을 배포하려면 `basePath` 구성 옵션을 사용할 수 있습니다.

`basePath`를 사용하면 애플리케이션의 경로 prefix를 설정할 수 있습니다. 예를 들어 `''`(기본값인 빈 문자열) 대신 `/docs`를 사용하려면 `next.config.js`를 열고 `basePath` 구성을 추가합니다:

next.config.js

```jsx
module.exports = {
  basePath: "/docs",
};
```

> **Note**: 이 값은 빌드 시점에 설정해야 하며 클라이언트 측 번들에 인라인으로 설정되므로 재빌드하지 않고는 변경할 수 없습니다.

<br>

## **[Links](https://nextjs.org/docs/app/api-reference/next-config-js/basePath#links)**

`next/link` 및 `next/router`를 사용하여 다른 페이지로 링크할 때 `basePath`가 자동으로 적용됩니다.

예를 들어 `/about`을 사용하면 `basePath`가 `/docs`로 설정되어 있으면 자동으로 `/docs/about`이 됩니다.

```jsx
export default function HomePage() {
  return (
    <>
      <Link href="/about">About Page</Link>
    </>
  );
}
```

<br>

HTML을 출력합니다:

```html
<a href="/docs/about">About Page</a>
```

이렇게 하면 `basePath` 값을 변경할 때 애플리케이션의 모든 링크를 변경할 필요가 없습니다.

<br>

## **[Images](https://nextjs.org/docs/app/api-reference/next-config-js/basePath#images)**

[`next/image`](https://nextjs.org/docs/pages/api-reference/components/image) 컴포넌트를 사용할 때는 `src` 앞에 `basePath`를 추가해야 합니다.

예를 들어 `/docs/me.png`를 사용하면 `basePath`가 `/docs`로 설정된 경우 이미지가 올바르게 제공됩니다.

```jsx
import Image from "next/image";

function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src="/docs/me.png"
        alt="Picture of the author"
        width={500}
        height={500}
      />
      <p>Welcome to my homepage!</p>
    </>
  );
}

export default Home;
```
