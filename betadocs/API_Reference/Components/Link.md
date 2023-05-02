# next/link

`<Link>`는 HTML의 `<a>` 요소를 확장한 리액트 컴포넌트로 [미리 불러오기](https://beta.nextjs.org/docs/routing/linking-and-navigating#prefetching)(prefetching)와 경로 간의 클라이언트 측 탐색을 제공합니다. `<Link>`는 Next.js에서 경로간 이동하기 위한 가장 기본적인 방법입니다.

```tsx
// app/page.tsx
import Link from 'next/link';

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>;
}
```

<br/>

## Props

다음은 Link 컴포넌트에 사용할 수 있는 속성 요약입니다:

<table><thead><tr><th>Prop</th><th>Example</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td><a href="#href" class="relative"><code class="inline">href</code></a></td><td><code class="inline">href="/dashboard"</code></td><td>String or Object</td><td>Yes</td></tr><tr><td><a href="#replace" class="relative"><code class="inline">replace</code></a></td><td><code class="inline">replace={false}</code></td><td>Boolean</td><td>-</td></tr><tr><td><a href="#prefetch" class="relative"><code class="inline">prefetch</code></a></td><td><code class="inline">prefetch={false}</code></td><td>Boolean</td><td>-</td></tr></tbody></table>

> <b>알아두면 좋아요:</b> `className` 또는 `target="_blank"`와 같은 `<a>` 태그 속성들은 `<Link>` 컴포넌트에 props로 추가할 수 있으며 `<a>` 요소로 전달될 것입니다.

### <code>href</code> (required)

이동할 경로 또는 URL.

```jsx
<Link href="/dashboard">Dashboard</Link>
```

`href`는 또한 객체(object)를 받을 수 있습니다. 예를 들면:

```jsx
// "/aobut?name=test"로 이동합니다.
<Link
  href={{
    pathname: '/about',
    query: { name: 'test' },
  }}
>
  About
</Link>
```

### `replace`

기본값은 `false`입니다. `true`일 때 브라우저의 히스토리 스택에 새 URL을 추가하는 대신 `next/link`가 현재 기록 상태를 대체합니다.

```tsx
import Link from 'next/link';

export default function Page() {
  return (
    <Link href="/dashboard" replace>
      Dashboard
    </Link>
  );
}
```

<br />

### `prefetch`

기본값은 `true`입니다. `true`일 때 `next/link`가 배경에 있는 페이지(href로 표시)를 프리페치합니다. 이 기능은 클라이언트 측 탐색의 성능을 향상시키는데 유용합니다. 뷰포트(초기 또는 스코롤을 통해)의 모든 `<Link />`가 사전 로드됩니다.

`prefetch={false}`를 전달하여 프리페치를 사용하지 않도록 설정할 수 있습니다. 프리페치는 프로덕션에서만 사용할 수 있습니다.

```tsx
import Link from 'next/link';

export default function Page() {
  return (
    <Link href="/dashboard" prefetch={false}>
      Dashboard
    </Link>
  );
}
```

### Legacy Props

- `as`: `app` 디렉토리의 URL을 마스킹하는 개념은 [고급 라우팅 패턴](https://beta.nextjs.org/docs/routing/fundamentals#advanced-routing-patterns)<sup>advanced routiong patterns</sup>(자세한 내용은 추후 추가문서로 제공)으로 처리됩니다. `pages` 디렉터리의 [이전 동적 경로](https://nextjs.org/docs/tag/v9.5.2/api-reference/next/link#dynamic-routes)<sup>old dynamic routes</sup>와의 호환성을 위해 `as` 속성을 Link 컴포넌트의 prop으로 전달할 수 있습니다. 그러나 이제 동적 세그먼트를 `hrep` prop으로 전달할 수 있으므로 `as`를 사용하는 것은 권장하지 않습니다.
- `legacyBehavior`: `<a>`요소는 이제 더이상 `<Link>` 컴포넌트의 자식으로 필요하지 않습니다. 레거시 동작을 사용하려면 `leagacyBehavior` prop을 추가하거나 업그레이드할 `<a>`를 제거합니다. [codemod](https://beta.nextjs.org/docs/upgrade-guide/codemods#new-link)를 이용하면 코드를 자동으로 업그레이드할 수 있습니다.
- `passHref`: `<Link>`가 올바른 `href`를 가진 기본 `<a>` 요소를 렌더링하므로 필요하지 않습니다. 여기에는 다음과 같은 경우가 포함됩니다:
  - `<Link href="/about">About</Link>`
  - `<Link href="/about"><CostomComponent /></Link>`
  - `<Link href="/about"><strong>About</strong></Link>`
