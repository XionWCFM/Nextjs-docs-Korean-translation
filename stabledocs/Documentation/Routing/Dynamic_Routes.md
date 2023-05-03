# Dynamic Routes

> Examples - [Dynamic Routing](https://github.com/vercel/next.js/tree/canary/examples/dynamic-routing)

<br>

복잡한 애플리케이션의 경우 사전 정의된 라우트를 사용해서 라우트를 정의하는 것만으로는 충분하지 않을 수 있습니다.
Next.js에서는 페이지(`[param]`)에 괄호를 추가하여 동적 라우트(일명 URL 슬러그, 예쁜 URL 등)를 만들 수 있습니다.

<br>

다음 페이지 `pages/post/[pid].js`를 살펴보세요:

```jsx
import { useRouter } from "next/router";

const Post = () => {
  const router = useRouter();
  const { pid } = router.query;

  return <p>Post: {pid}</p>;
};

export default Post;
```

`post/1`, `/post/abc` 등과 같은 모든 라우트는 `pages/post/[pid].js`에 의해 일치됩니다. 일치하는 라우트 매개변수는 페이지에 쿼리 매개변수로 전송되며 다른 쿼리 매개변수와 합쳐집니다.

<br>

예를 들어 `/post/abc` 라우트에는 다음과 같은 `query` 객체가 있습니다:

```jsx
{ "pid": "abc" }
```

마찬가지로 `/post/abc?foo=bar` 라우트에는 다음과 같은 `query` 객체가 있습니다:

```jsx
{ "foo": "bar", "pid": "abc" }
```

그러나 라우트 매개변수는 같은 이름의 쿼리 매개변수를 재정의합니다. 예를 들어 `/post/abc?pid=123` 라우트에는 다음과 같은 `query` 개체가 있습니다:

```jsx
{ "pid": "abc" }
```

여러 개의 동적 라우트 세그먼트도 같은 방식으로 작동합니다. 페이지 `pages/post/[pid]/[comment].js`는 `/post/abc/a-comment` 라우트와 일치하며 해당 `query` 객체가 됩니다:

```jsx
{ "pid": "abc", "comment": "a-comment" }
```

동적 라우트에 대한 클라이언트 측 내비게이션(Client-side navigations)은 `next/link`로 처리됩니다. 위에 사용된 경로에 대한 링크를 원한다면 다음과 같이 표시될 것입니다:

```jsx
import Link from "next/link";

function Home() {
  return (
    <ul>
      <li>
        <Link href="/post/abc">Go to pages/post/[pid].js</Link>
      </li>
      <li>
        <Link href="/post/abc?foo=bar">Also goes to pages/post/[pid].js</Link>
      </li>
      <li>
        <Link href="/post/abc/a-comment">
          Go to pages/post/[pid]/[comment].js
        </Link>
      </li>
    </ul>
  );
}

export default Home;
```

자세한 내용은 [Linking between pages](./Introduction.md#linking-between-pages)에 대한 문서를 참조하세요.

<br>
<br>

## Catch all routes

<br>

> Examples - [Catch All Routes](https://github.com/vercel/next.js/tree/canary/examples/dynamic-routing)

<br>

동적 경로는 괄호 안에 점(`...`) 세 개를 추가하여 모든 경로를 포착하도록 확장할 수 있습니다.
예를 들어:

- `pages/post/[...slug].js`는 `/post/a`뿐만 아니라 `/post/a/b`, `/post/a/b/c` 등과도 일치합니다.
- `pages/post/[...slug].js`는 `/post`와 일치하지 않습니다.

> 참고: `slug`가 아닌 다른 이름을 사용할 수 있습니다. ex) `[...param]`

<br>

일치하는 매개변수는 페이지에 쿼리 매개변수(예제에서는 `slug`)로 전송되며, 항상 배열이므로 `/post/a` 라우트에는 다음과 같은 `query` 객체가 있습니다:

```jsx
{ "slug": ["a"] }
```

그리고 `/post/a/b` 및 기타 일치하는 라우트의 경우, 다음과 같이 배열에 새 매개변수가 추가됩니다:

```jsx
{ "slug": ["a", "b"] }
```

<br>
<br>

## Optional catch all routes

<br>

매개변수를 대괄호(`[[...slug]]`) 안에 포함하여 모든 경로를 선택 사항으로 설정할 수 있습니다.

예를 들어 `pages/post/[[...slug]].js`는 `/post`, `/post/a`, `/post/a/b` 등과 일치합니다.

catch all route과 optional catch all route의 주요 차이점은 optional catch all을 사용하면 매개변수가 없는 경로도 일치한다는 점입니다(위 예제에서는 `/post`).

`query` 객체는 다음과 같습니다:

```jsx
{ } // GET `/post` (empty object)
{ "slug": ["a"] } // `GET /post/a` (single-element array)
{ "slug": ["a", "b"] } // `GET /post/a/b` (multi-element array)
```

<br>
<br>

## Caveats

- 사전 정의된 라우트는 동적 라우트보다, 동적 라우트는 catch all route보다 우선합니다. 다음 예제를 살펴보세요:

  - `pages/post/create.js` - `/post/create`와 일치합니다.
  - `pages/post/[pid].js` - `/post/1`, `/post/abc` 등과 일치합니다. 하지만 `/post/create`는 아닙니다.
  - `pages/post/[...slug].js` - `/post/1/2`, `/post/a/b/c` 등과 일치합니다. 하지만 `/post/create`, `/post/abc`는 아닙니다.

- [자동 정적 최적화(Automatic Static Optimization)](../Advanced_Features/Automatic_Static_Optimization.md)에 의해 정적으로 최적화되는 페이지는 경로 매개변수가 제공되지 않은 상태에서 수화됩니다. 즉, `query`는 빈 개체(`{}`)가 됩니다.
  hydration 후 Next.js는 애플리케이션 업데이트를 트리거하여 `query` 객체에 라우트 매개변수를 제공합니다.

<br>
<br>

## Related

<br>

다음에 수행할 작업에 대한 자세한 내용은 다음 섹션을 참조하세요:

<br>

[next/link](../../API_Reference/NextLink.md) <br>
클라이언트 측 탐색을 처리합니다.

[next/router](../../API_Reference/NextRouter.md) <br>
페이지에서 라우터 API를 활용할 수 있습니다.

<br>

[이전 문서: Introduction](./Introduction.md)

<br>

[다음 문서: Imperatively](./Imperatively.md)
