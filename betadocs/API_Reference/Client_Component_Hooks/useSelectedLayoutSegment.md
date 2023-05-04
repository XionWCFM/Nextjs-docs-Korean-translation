# `useSelectedLayoutSegment`

`useSelectedLayoutSegment`는 호출된 Layout **한 단계 아래**의 라우트 세그먼트를 읽을 수 있는 클라이언트 컴포넌트 훅입니다.

이는 활성 하위 세그먼트에 따라 스타일이 변경되는 탭과 같은 탐색 UI에 유용합니다.

```jsx
//app/example-client-component.tsx

"use client";

import { useSelectedLayoutSegment } from "next/navigation";

export default function ExampleClientComponent() {
  const segment = useSelectedLayoutSegment();

  return <>Active segment: {segment}</>;
}
```

<br>

> ### Good to know
>
> - `useSelectedLayoutSegment`는 [클라이언트 컴포넌트](../../Building_Your_Application/Rendering/Server_and_Client_Components.md) 훅이고, 레이아웃은 기본적으로 [서버 컴포넌트](../../Building_Your_Application/Rendering/Server_and_Client_Components.md)입니다. 따라서 `useSelectedLayoutSegment`는 일반적으로 레이아웃에서 가져온 클라이언트 컴포넌트를 통해 호출됩니다.
> - `useSelectedLayoutSegment`는 한 단계 아래의 세그먼트만 반환합니다. 모든 활성 세그먼트를 반환하려면 [useSelectedLayoutSegments](./useSelectedLayoutSegments.md)를 참조하세요.

<br>

## API 참조

```jsx
const segment = useSelectedLayoutSegment();
```

<br>

### 매개변수

`useSelectedLayoutSegment`는 매개변수를 받지 않습니다.

<br>

### 반환값

`useSelectedLayoutSegment`는 활성 세그먼트의 문자열을 반환하고, 존재하지 않는 경우 `null`을 반환합니다.

예를 들어, 아래의 레이아웃과 URL이 주어졌을 때, 반환된 세그먼트는 다음과 같습니다.

| Layout                    | Visited URL                    | Returned Segment |
| ------------------------- | ------------------------------ | ---------------- |
| `app/layout.js`           | `/`                            | `null`           |
| `app/layout.js`           | `/dashboard`                   | `'dashboard'`    |
| `app/dashboard/layout.js` | `/dashboard`                   | `null`           |
| `app/dashboard/layout.js` | `/dashboard/settings`          | `'settings'`     |
| `app/dashboard/layout.js` | `/dashboard/analytics`         | `'analytics'`    |
| `app/dashboard/layout.js` | `/dashboard/analytics/monthly` | `'analytics'`    |

<br>

## 예시

<br>

### 활성 링크 컴포넌트 만들기

`useSelectedLayoutSegment`를 사용하여 활성 세그먼트에 따라 스타일이 변경되는 활성 링크 컴포넌트를 만들 수 있습니다. 예를 들어, 블로그의 사이드바에서 특집 포스트 목록을 다음과 같이 만들 수 있습니다.

```jsx
//app/blog/blog-nav-link.tsx

"use client";

import Link from "next/link";
import { useSelectedLayoutSegment } from "next/navigation";

// 이 *클라이언트* 컴포넌트를 블로그 레이아웃으로 가져옵니다.

export default function BlogNavLink({
  slug,
  children,
}: {
  slug: string,

  children: React.ReactNode,
}) {
  // /blog/hello-world로 이동하면 선택된 레이아웃 세그먼트에 'hello-world'가 반환됩니다.
  const segment = useSelectedLayoutSegment();
  const isActive = slug === segment;

  return (
    <Link
      href={`/blog/${slug}`}
      // 링크의 활성 여부에 따라서 스타일을 바꿉니다.
      style={{ fontWeight: isActive ? "bold" : "normal" }}
    >
      {children}
    </Link>
  );
}
```

```jsx
//app/blog/layout.tsx
// 클라이언트 구성요소를 상위 레이아웃(서버 구성요소)으로 가져옵니다.
import { BlogNavLink } from "./blog-nav-link";
import getFeaturedPosts from "./get-featured-posts";

export default async function Layout({
  children,
}: {
  children: React.ReactNode,
}) {
  const featuredPosts = await getFeaturedPosts();
  return (
    <div>
      {featuredPosts.map((post) => (
        <div key={post.id}>
          <BlogNavLink slug={post.slug}>{post.title}</BlogNavLink>
        </div>
      ))}
      <div>{children}</div>
    </div>
  );
}
```

[useSearchParams](./useSearchParams.md)

[useSelectedLayoutSegments](./useSelectedLayoutSegments.md)
