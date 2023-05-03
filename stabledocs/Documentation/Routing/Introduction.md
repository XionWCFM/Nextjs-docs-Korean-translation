# Routing

<br>

Next.js에는 페이지 개념을 기반으로 구축된 파일 시스템 기반 router가 있습니다.

페이지 디렉터리에 파일이 추가되면 자동으로 route로 사용할 수 있습니다.

페이지 디렉터리 내의 파일은 가장 일반적인 패턴을 정의하는 데 사용할 수 있습니다.

<br>

### Index routes

<br>

라우터는 `index`라는 이름의 파일을 디렉터리의 루트로 자동 라우팅합니다.

- `pages/index.js` → `/`
- `pages/blog/index.js` → `/blog`

<br>

### Nested routes

<br>

라우터는 중첩 파일(nested files)을 지원합니다. 중첩된 폴더 구조를 만들면, 파일은 여전히 동일한 방식으로 자동으로 라우팅됩니다.

- `pages/blog/first-post.js` → `/blog/first-post`
- `pages/dashboard/settings/username.js` → `/dashboard/settings/username`

<br>

### Dynamic route segments

<br>

동적 세그먼트를 일치시키려면, 대괄호 구문을 사용할 수 있습니다. 이를 통해서 명명된 매개변수를 일치시킬 수 있습니다.

- `pages/blog/[slug].js` → `/blog/:slug` (`/blog/hello-world`)
- `pages/[username]/settings.js` → `/:username/settings` (`/foo/settings`)
- `pages/post/[...all].js` → `/post/\*` (`/post/2020/id/title`)

> 동적 경로의 작동 방식에 대해 자세히 알아보려면 [동적 경로 문서](./Dynamic_Routes.md)를 확인하세요.

<br>

## Linking between pages

<br>

Next.js 라우터를 사용하면 단일 페이지 애플리케이션(SPA)과 유사하게 페이지 간에 client-side route을 수행할 수 있습니다.

<br>

이 client-side route을 수행하기 위해 `Link`라는 React 컴포넌트가 제공됩니다.

```jsx
import Link from "next/link";

function Home() {
  return (
    <ul>
      <li>
        <Link href="/">Home</Link>
      </li>
      <li>
        <Link href="/about">About Us</Link>
      </li>
      <li>
        <Link href="/blog/hello-world">Blog Post</Link>
      </li>
    </ul>
  );
}

export default Home;
```

위의 예에서는 여러 개의 링크를 사용합니다. 각 링크는 알려진 페이지에 대한 경로(`href`)를 매핑합니다:

- `/` → `pages/index.js`
- `/about` → `pages/about.js`
- `/blog/hello-world` → `pages/blog/[slug].js`

[정적 생성](../Basic_Features/Data_Fetching/getStaticProps.md)을 사용하는 페이지의 경우 viewport(초기 또는 스크롤을 통해)의 모든 `<Link />`는 기본적으로 해당 데이터를 포함하여 미리 가져오기됩니다. [서버 렌더링](../Basic_Features/Data_Fetching/getServerSideProps.md) 경로에 대한 해당 데이터는 `<Link />`를 클릭할 때만 가져옵니다.

<br>

## Linking to dynamic paths

<br>

보간(interpolation)을 사용하여 경로를 생성할 수도 있는데, 이는 dynamic route segments에 유용합니다. 예를 들어 컴포넌트에 prop으로 전달된 게시물 목록을 표시할 수 있습니다:

```jsx
import Link from "next/link";

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

> 이 예제에서는 경로가 utf-8과 호환되도록 하기 위해 [encodeURIComponent](encodeURIComponent링크)가 사용됩니다.

<br>

또는 URL 객체를 사용할 수도 있습니다:

<br>

```jsx
import Link from "next/link";

function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link
            href={{
              pathname: "/blog/[slug]",
              query: { slug: post.slug },
            }}
          >
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  );
}

export default Posts;
```

이제 보간을 사용하여 경로를 만드는 대신 `href`에 URL 객체를 사용합니다.

- `pathname`은 `pages` 디렉터리에 있는 페이지의 이름입니다. 이 경우 `/blog/[slug]`입니다.
- `query`는 동적 세그먼트가 있는 객체입니다. 이 경우 `slug`입니다.

<br>

## Injecting the router

<br>

> Examples - [Dynamic Routing](https://github.com/vercel/next.js/tree/canary/examples/dynamic-routing)

<br>

React 컴포넌트에서 라우터 객체에 접근하려면 [useRouter](../../API_Reference/NextRouter.md) 또는 [withRouter](../../API_Reference/NextRouter.md)를 사용할 수 있습니다.

일반적으로 [useRouter](../../API_Reference/NextRouter.md)를 사용하는 것을 권장합니다.

<br>

## Learn more

<br>

라우터는 여러 부분으로 나뉩니다:

<br>

[next/link](../../API_Reference/NextLink.md) <br>
클라이언트 측 탐색을 처리합니다.

[next/router](../../API_Reference/NextRouter.md) <br>
페이지에서 라우터 API를 활용할 수 있습니다.

<br>

[이전 문서: Basic Features: Handling Scripts](../Basic_Features/Handling_Scripts)

<br>

[다음 문서: Dynamic Routes](./Dynamic_Routes.md)
