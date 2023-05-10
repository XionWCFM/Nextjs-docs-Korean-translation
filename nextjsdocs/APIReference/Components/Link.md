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
<hr/>
<br/>

## Props

다음은 Link 컴포넌트에 사용할 수 있는 속성 요약입니다:

<table><thead><tr><th>Prop</th><th>Example</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td><a href="#href" class="relative"><code class="inline">href</code></a></td><td><code class="inline">href="/dashboard"</code></td><td>String or Object</td><td>Yes</td></tr><tr><td><a href="#replace" class="relative"><code class="inline">replace</code></a></td><td><code class="inline">replace={false}</code></td><td>Boolean</td><td>-</td></tr><tr><td><a href="#prefetch" class="relative"><code class="inline">prefetch</code></a></td><td><code class="inline">prefetch={false}</code></td><td>Boolean</td><td>-</td></tr></tbody></table>

> 알아두면 좋은 정보: `className` 또는 `target="_blank"`와 같은 `<a>` 태그 속성들은 `<Link>` 컴포넌트의 props로 추가될 수 있으며 기본 `<a>` 요소로 전달됩니다.

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

<br/>
<hr/>
<br/>

## Other Props

### `<scroll>`

탐색 후 페이지 맨 위로 스크롤합니다. 기본값은 `true`입니다.

### `<shallow>`

`getStaticProps`, `getServerSideProps` 또는 `getInitialProps`를 다시 실행하지 않고 현재 페이지의 경로를 업데이트합니다. 기본값은 `false`입니다.

### `<locale>`

활성 로케일(locale)은 자동으로 앞에 추가됩니다. `locale`을 사용하면 다른 로케일(locale)을 제공할 수 있습니다. `false`일 때 `href`는 기본 동작이 비활성화되어 있으므로 로케일을 포함해야 합니다.

<br/>
<hr/>
<br/>

## 경로에 동적 세그먼트가 있는 경우

Next.js 9.5.3 이후 모든 경로 캐치를 포함하여 동적 경로에 연결할 때 수행할 작업이 없습니다. 그러나 보간이나 URL 개체를 사용하여 링크를 생성하는 것이 매우 일반적이고 편리해질 수 있습니다.

예를 들어, 동적 경로 `pages/blog/[slug].js`는 다음 링크와 일치합니다:

```js
import Link from 'next/link';

function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${encodeURIComponent(post.slug)}`}>
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  );
}

export default Posts;
```

<br/>
<hr/>
<br/>

## 자식이 `<a>`태그인 경우

```js
import Link from 'next/link';

function Legacy() {
  return (
    <Link href="/about" legacyBehavior>
      <a>About Us</a>
    </Link>
  );
}

export default Legacy;
```

<br/>
<hr/>
<br/>

## 자식이 `<a>` 태그를 감싸는 커스텀 컴포넌트인 경우

만약 `<Link>` 컴포넌트의 자식이 `<a>` 태그를 감싸는 커스텀 컴포넌트일 경우 반드시 `passHref`를 `<Link>` 컴포넌트에 추가해야 합니다. 이는 [styled-components](https://styled-components.com/)와 같은 라이브러리들을 사용하는 경우 반드시 필요한 작업입니다. 이 속성 없다면 `<a>` 태그는 `href` 속성을 가지지 않을 것이며 이는 여러분의 사이트 접근성과 SEO에 영향을 미칠 것입니다. 만약 [Eslint](https://nextjs.org/docs/pages/building-your-application/configuring/eslint#eslint-plugin)를 사용하는 경우 `passHref`를 올바르게 사용할 수 있도록 `next/link-passhref` 규칙이 내장되어 있습니다.

```js
import Link from 'next/link';
import styled from 'styled-components';

// This creates a custom component that wraps an <a> tag
const RedLink = styled.a`
  color: red;
`;

function NavLink({ href, name }) {
  return (
    <Link href={href} passHref legacyBehavior>
      <RedLink>{name}</RedLink>
    </Link>
  );
}

export default NavLink;
```

[Emotion](https://emotion.sh/docs/introduction)의 JSX 프라그마 기능(@jsxjsx)을 사용하는 경우 <a> 태그를 직접 사용하더라도 반드시 passHref를 사용해야 합니다.

컴포넌트가 올바르게 탐색을 트리거하려면 클릭 속성을 지원해야 합니다.

<br/>
<hr/>
<br/>

## 자식이 기능적(functional) 컴포넌트인 경우

`<Link>` 컴포넌트의 자식이 기능적인 컴포넌트라면, `passHref`와 `legacyBehavior`를 사용하는 것 외에도 컴포넌트를 <code>[React.forwardRef](https://react.dev/reference/react/forwardRef)</code>로 감싸야합니다:

```jsx
import Link from 'next/link';

// `onClick`, `href`, and `ref` need to be passed to the DOM element
// for proper handling
const MyButton = React.forwardRef(({ onClick, href }, ref) => {
  return (
    <a href={href} onClick={onClick} ref={ref}>
      Click Me
    </a>
  );
});

function Home() {
  return (
    <Link href="/about" passHref legacyBehavior>
      <MyButton />
    </Link>
  );
}

export default Home;
```

<br/>
<hr/>
<br/>

## URL를 가진 객체

`<Link>` 컴포넌트는 또한 URL 객체를 받을 수 있으며 URL 문자열을 만들기 위해 자동으로 형식을 지정합니다. 방법은 다음과 같습니다:

```js
import Link from 'next/link';

function Home() {
  return (
    <ul>
      <li>
        <Link
          href={{
            pathname: '/about',
            query: { name: 'test' },
          }}
        >
          About us
        </Link>
      </li>
      <li>
        <Link
          href={{
            pathname: '/blog/[slug]',
            query: { slug: 'my-post' },
          }}
        >
          Blog Post
        </Link>
      </li>
    </ul>
  );
}

export default Home;
```

위의 예에는 다음 링크가 있습니다:

- 사전 정의된 라우트: `/about?name=test`
- [동적 라우트](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes) : `/blog/my-post`

[Node.js URL module 문서](https://nodejs.org/api/url.html#url_url_strings_and_url_objects)에 정의된 대로 모든 속성을 사용할 수 있습니다.

<br/>
<hr/>
<br/>

## pust 대신 URL 바꾸기

`<Link>` 컴포넌트의 기본적인 행동은 새로운 URL을 `history` 스택안으로 `push`한다. 다음 예와 같이 `replace` 속성을 사용한다면 새 항목을 추가하지 않도록 할 수 있습니다.

```js
<Link href="/about" replace>
  About us
</Link>
```

<br/>
<hr/>
<br/>

## Next.js 13 미들웨어 포함

인증 또는 다른 페이지로 사용자를 다시 쓰는 것과 관련된 다른 목적으로 미들웨어를 사용하는 것이 일반적입니다. `<Link />` 컴포넌트가 미들웨어를 통해 다시 쓰기와 함께 링크를 적절하게 프리페치하려면 Next.js에 표시할 URL과 프리페치할 URL을 모두 알려주어야 합니다. 이는 프리페치할 올바른 경로를 알기 위해 미들웨어로의 불필요한 페치를 방지하기 위해 필요합니다.

예를 들어, 인증 및 방문자 보기가 있는 `/dashboard` 경로를 제공하려는 경우 미들웨어에서 다음과 유사한 것을 추가하여 사용자를 올바른 페이지로 리다이렉션할 수 있습니다:

```js
export function middleware(req) {
  const nextUrl = req.nextUrl;
  if (nextUrl.pathname === '/dashboard') {
    if (req.cookies.authToken) {
      return NextResponse.rewrite(new URL('/auth/dashboard', req.url));
    } else {
      return NextResponse.rewrite(new URL('/public/dashboard', req.url));
    }
  }
}
```

이 경우 `<Link />` 컴포넌트에서 다음 코드를 사용할 수 있습니다:

```js
import Link from 'next/link';
import useIsAuthed from './hooks/useIsAuthed';

export default function Page() {
  const isAuthed = useIsAuthed();
  const path = isAuthed ? '/auth/dashboard' : '/dashboard';
  return (
    <Link as="/dashboard" href={path}>
      Dashboard
    </Link>
  );
}
```

> 노트: 동적 라우트를 사용하는 경우 `as` 및 `href` 속성들을 조정해야 합니다. 예를 들어 미들웨어를 통해 다르게 표시하려는 `/dashboard/[user]`와 같은 동적 라우트가 있는 경우 다음과 같이 기록합니다:
> <code><Link href={{ pathname: '/dashboard/authed/[user]', query: { user: username } }} as="/dashboard/[user]">Profile</Link></code>
