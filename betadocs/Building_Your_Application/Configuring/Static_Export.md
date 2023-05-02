원본 링크 : [https://beta.nextjs.org/docs/configuring/static-export](https://beta.nextjs.org/docs/configuring/static-export)

# Static_Export

<aside>
💡 참고: 정적 배포(static exports)를 지원하는 기능이 Next.js 13.3에 추가되었습니다.
</aside>


Next.js는 정적 사이트 또는 싱글 페이지 애플리케이션(SPA)으로 시작하여 나중에 필요에 따라 서버를 사용하는 기능으로 업그레이드 할 수 있도록 가능하게 합니다.

next build를 실행할 때, Next.js는 각 경로(route)별로 HTML 파일을 생성합니다. 엄격한 SPA를 개별 HTML 파일로 분할함으로써, Next.js는 클라이언트 측에서 불필요한 JavaScript 코드를 로드하지 않고 번들 크기를 줄이며, 빠른 페이지 로드를 가능하게 합니다.

## 구성(Configuration)

정적 배포(static export)를 활성화하려면 next.config.js 내에서 출력(output) 모드를 변경하십시오.

```jsx
next.config.js;

/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  output: "export",
};

module.exports = nextConfig;
```

next build를 실행한 후에는, Next.js가 애플리케이션의 HTML/CSS/JS 자산을 포함하는 out 폴더를 생성합니다.

## 지원되는 기능(Supported Features)

Next.js의 핵심은 정적 익스포트를 지원하도록 설계되었습니다.

### 서버 컴포넌트(Server Components)

정적 배포(static export)를 생성하기 위해 next build를 실행하면, 애플리케이션 디렉토리 내에서 사용되는 서버 컴포넌트는 기존 정적 사이트(static-site) 생성과 유사하게 빌드 중에 실행됩니다.

결과 컴포넌트(resulting component)는 초기 페이지 로드를 위해 정적 HTML로 렌더링되며, 클라이언트에서 라우트 간 탐색을 위한 정적 페이로드(static payload)로 렌더링됩니다. 서버 컴포넌트가 동적 서버 함수(dynamic server functions)를 사용하지 않는 경우, 정적 배포(static export)를 사용할 때 Server Components에 대해 변경할 필요는 없습니다.

```jsx
app / page.tsx;

export default async function Page() {
  // 이 fetch는 'next build' 수행 중에 서버 측에서 실행됩니다.
  const res = await fetch("https://api.example.com/...");
  const data = await res.json();

  return <main>...</main>;
}
```

브라우저에서 동적 데이터(dynamic data)를 가져오려면, 정적 배포(static export)를 사용하여 클라이언트 컴포넌트를 사용하는 것도 지원됩니다.

### Dynamic Route Segments

동적 라우트 세그먼트가 ‘`next build`’ 중에 알려져 있는 경우, `generateStaticParams`를 사용하여 어떤 세그먼트를 생성할지 선택할 수 있습니다. 예를 들어, 다음 페이지는 모든 게시글에 대한 경로를 생성합니다.

```jsx
app / blog / [id] / page.js;

export async function generateStaticParams() {
  const posts = await fetch("https://.../posts").then((res) => res.json());

  return posts.map((post) => ({
    id: post.id,
  }));
}

export default function Page({ params }: { params: { id: string } }) {
  return <div>{params.id}</div>;
}
```

`next build` 중에 경로 세그먼트를 모를 경우, 클라이언트 컴포넌트를 사용하여 브라우저에서 동적 데이터를 가져올 수 있습니다.

### Dynamic Fetching with Client Components

`next build` 중에 알 수 없는 값을 기반으로 동적 데이터를 표시하려면 클라이언트 컴포넌트를 사용할 수 있습니다:

```jsx
app / blog / [id] / page.js;

("use client");

import useSWR from "swr";

const fetcher = (url: string) => fetch(url).then((r) => r.json());

export default function Page({ params }: { params: { id: string } }) {
  const { data, error } = useSWR(
    `https://jsonplaceholder.typicode.com/posts/${params.id}`,
    fetcher
  );
  if (error) return "Failed to load";
  if (!data) return "Loading...";

  return data.title;
}
```

---

경로 전환은 클라이언트 측에서 발생하기 때문에, 이는 전통적인 SPA와 유사한 방식으로 동작합니다. 예를 들어, 다음 인덱스 경로는 클라이언트에서 다른 게시글로 이동할 수 있도록 합니다:

```jsx
app / page.tsx;

import Link from "next/link";

export default function Page() {
  return (
    <>
      <h1>Index Page</h1>
      <hr />
      <ul>
        <li>
          <Link href="/post/1">Post 1</Link>
        </li>
        <li>
          <Link href="/post/2">Post 2</Link>
        </li>
      </ul>
    </>
  );
}
```

### 이미지 최적화(Image Optimization)

`next/image`를 통한 이미지 최적화는 `next.config.js`에서 사용자 정의 이미지 로더(custom image loader)를 정의함으로써 정적 배포(static export)와 함께 사용할 수 있습니다. 예를 들어, Cloudinary와 같은 서비스를 사용하여 이미지를 최적화할 수 있습니다:

```jsx
next.config.js;

/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
  images: {
    loader: "custom",
    loaderFile: "./app/image.ts",
  },
  experimental: {
    appDir: true,
  },
};

module.exports = nextConfig;
```

---

이 사용자 정의 로더는 원격 소스에서 이미지를 가져오는 방법을 정의합니다. 예를 들어, 다음 로더는 Cloudinary를 위한 URL을 구성합니다:

```jsx
app / image.ts;

export default function cloudinaryLoader({
  src,
  width,
  quality,
}: {
  src: string,
  width: number,
  quality?: number,
}) {
  const params = ["f_auto", "c_limit", `w_${width}`, `q_${quality || "auto"}`];
  return `https://res.cloudinary.com/demo/image/upload/${params.join(
    ","
  )}${src}`;
}
```

---

그런 다음 애플리케이션에서 next/image를 사용하여 Cloudinary의 이미지에 대한 상대적인 경로를 정의할 수 있습니다:

```jsx
app / page.tsx;

import Image from "next/image";

export default function Page() {
  return <Image alt="turtles" src="/turtles.jpg" width={300} height={300} />;
}
```

### Route Handlers

라우트 핸들러(Route Handlers)는 **`next build`**를 실행할 때 정적인 응답을 렌더링합니다. `GET` HTTP 동사만 지원됩니다. 이는 동적 또는 정적 데이터에서 정적 HTML, JSON, TXT 또는 다른 파일을 생성하는 데 사용할 수 있습니다. 예를 들어:

```jsx
app / data.json / route.ts;

import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({ name: "Lee" });
}
```

위 파일인 **`app/data.json/route.ts`**은 **`next build`** 동안 정적 파일로 렌더링되어 **`{ name: 'Lee' }`**을 포함하는 **`data.json`**이 생성됩니다.

들어오는 요청에서 동적 값이 필요한 경우 정적 배포(static export)를 사용할 수 없습니다.

### Browser APIs

클라이언트 컴포넌트는 `next build` 중에 HTML로 사전 렌더링됩니다. window, localStorage 및 navigator와 같은 웹 API는 서버에서 사용할 수 없으므로 브라우저에서만 안전하게 이러한 API에 액세스해야 합니다. 예를 들면 다음과 같습니다:

```jsx
'use client';

import { useEffect } from 'react';

export default function ClientComponent() {
  useEffect(() => {
    // You now have access to `window`
    console.log(window.innerHeight);
  }, [])

  return ...;
}
```

## 지원되지 않는 기능들(Unsupported Features)

정적 배포 출력 모드(static export output mode)를 활성화 한 후, app 내부의 모든 라우트는 다음 Route Segment Config에 선택되어 들어갑니다:

```jsx
export const dynamic = "error";
```

이 설정으로는 런타임 서버가 없기 때문에 헤더나 쿠키와 같은 서버 기능을 사용하면 애플리케이션에서 오류가 발생합니다. 이렇게 함으로써 로컬 개발 환경이 정적으로 내보내는 것과 동일한 동작을 보장합니다. 만약 서버 기능을 사용해야 한다면, 정적으로 내보내는 것은 불가능합니다.

정적인 배포(static export)를 사용하는 경우 다음과 같은 추가적인 동적 기능은 지원되지 않습니다:

- `rewrites` in `next.config.js`
- `redirects` in `next.config.js`
- `headers` in `next.config.js`
- Middleware
- Incremental Static Regeneration

## 배포(Deploying)

정적 배포(static export)를 사용하면, Next.js를 HTML/CSS/JS 정적 파일을 제공할 수 있는 모든 웹 서버에 배포하고 호스팅할 수 있습니다.

next build를 실행하면, Next.js는 정적 익스포트를 out 폴더로 생성합니다. 이제 next export를 사용할 필요가 없습니다. 예를 들어, 다음과 같은 라우트가 있다고 가정해봅시다:

- `/`
- `/blog/[id]`

After running `next build`, Next.js will generate the following files:

- `/out/index.html`
- `/out/404.html`
- `/out/blog/post-1.html`
- `/out/blog/post-2.html`

만약 Nginx와 같은 정적 호스트를 사용하고 있다면, 들어오는 요청을 올바른 파일로 재작성할 수 있도록 구성할 수 있습니다:

```jsx
(nginx.conf)

server {
  listen 80;
  server_name acme.com;

  root /var/www;

  location / {
      try_files /out/index.html =404;
  }

  location /blog/ {
      rewrite ^/blog/(.*)$ /out/blog/$1.html break;
  }

  error_page 404 /out/404.html;
  location = /404.html {
      internal;
  }
}
```
