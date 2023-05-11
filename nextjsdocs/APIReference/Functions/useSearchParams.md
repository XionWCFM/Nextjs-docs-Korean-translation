# useSearchParams API Reference

<p>공식 문서 : https://nextjs.org/docs/app/api-reference/functions/use-search-params</p>

`useSearchParams`는 현재 url의 쿼리 문자열(queary string)을 읽을 수 있는 클라이언트 컴포넌트 훅입니다.
`useSearchParams`는 [`URLSearchParms`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 인터페이스의 읽기 전용 버전을 반환합니다.

```jsx
//app/dashboard/search-bar.tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParams.get("search");

  // URL -> `/dashboard?search=my-project`
  // `search` -> 'my-project'
  return <>Search: {search}</>;
}
```

### API Reference

```jsx
const searchParams = useSearchParams();
```

##### Parameters

useSearchParams 는 어떠한 매개변수도 가지지 않습니다.

##### Returns

`URLSearchParms`인터페이스의 읽기 전용 버전을 반환합니다.
URL의 쿼리 문자열을 읽기 위한 유틸리티 메서드가 포함되어있습니다.

- [`URLSearchParams.get()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/get): 검색 매개변수(search parameter)와 관련된 첫 번째 값을 반환합니다.
  For example:
  <table><thead><tr><th>URL</th><th><code>searchParams.get("a")</code></th></tr></thead><tbody><tr><td><code>/dashboard?a=1</code></td><td><code>'1'</code></td></tr><tr><td><code>/dashboard?a=</code></td><td><code>''</code></td></tr><tr><td><code>/dashboard?b=3</code></td><td><code>null</code></td></tr><tr><td><code>/dashboard?a=1&amp;a=2</code></td><td><code>'1'</code> <em>- use <a href="https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/getAll" rel="noopener noreferrer nofollow" target="_blank"><code>getAll()</code><span class="inline-flex"><svg data-testid="geist-icon" fill="none" height="14" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="14" style="color: currentcolor;"><path d="M7 17L17 7"></path><path d="M7 7h10v10"></path></svg></span></a> to get all values</em></td></tr></tbody></table>

- [`URLSearchParams.has()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/has): 주어진 매개변수가 존재하는지 나타내는 불린값을 반환합니다.
  For example:
  <table><thead><tr><th>URL</th><th><code>searchParams.has("a")</code></th></tr></thead><tbody><tr><td><code>/dashboard?a=1</code></td><td><code>true</code></td></tr><tr><td><code>/dashboard?b=3</code></td><td><code>false</code></td></tr></tbody></table>

• [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)의 다른 읽기 전용 메서드에 대해 알아보세요.
[`getAll()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/getAll), [`keys()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/keys), [`values()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/values), [`entries()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/entries), [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/forEach), and [`toString()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/toString)

##### 알아두면 좋은 정보

- `useSearchParams`는 [부분 렌더링](../../APIReference/FileConventions/layout.js.md#알아두면-좋은-정보) 중 오래된 값 사용을 방지하기 위해 [서버 컴포넌트](../../GettingStarted/React_Essentials.md)에서 지원되지 않는 [클라이언트 컴포넌트](../../GettingStarted/React_Essentials.md) 훅입니다.
- 애플리케이션에 `/pages` 디렉토리가 포함된 경우, `useSearchParams`는 `ReadonlyURLSearchParams | null` 를 반환합니다. `getServerSideProps`를 사용하지 않는 페이지의 사전 렌더링(pre-rendering) 과정에서 검색 매개변수를 알 수 없으므로 `null` 값은 이전 버전과의 호환성을 위한 것입니다.

---

### Behavior

#### Static Rendering

라우트가 [정적으로 렌더링](../../BuildingYourApplication/Rendering/Static_and_Dynamic.md#static-rendering-default)되는 경우 `useSearchParams()`를 호출하면 가장 가까운 [`Suspense` boundary](../../BuildingYourApplication/Routing/Loading_UI_and_Streaming.md#example-using-suspense-boundaries) 까지의 트리가 클라이언트 사이드에서 렌더링됩니다.

이렇게하면 `searchParams` 를 사용하는 동적 부분이 클라이언트 사이드에서 렌더링되는 동안 페이지의 일부가 정적으로 렌더링될 수 있습니다.

`Suspense 경계` 에서 `useSearchParams` 를 사용하는 컴포넌트를 래핑하여 클라이언트 사이드에서 렌더링되는 라우트의 일부를 줄일 수 있습니다.

For example:

```jsx
//app/dashboard/search-bar.tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParams.get("search");

  // 정적 렌더링을 사용할 때는 서버에 로그가 기록되지 않습니다.
  console.log(search);

  return <>Search: {search}</>;
}
```

```jsx
//app/dashboard/page.tsx
import { Suspense } from "react";
import SearchBar from "./search-bar";

// 이 컴포넌트는 Suspense 경계에 대한 폴백으로 전달됐습니다.
// 초기 HTML에서 검색 표시 줄 대신 렌더링됩니다.
// React 하이드레이션 중 값이 이용가능해지면 전달된 fallback'이 <SearchBar> 컴포넌트로 대체됩니다.
function SearchBarFallback() {
  return <>placeholder</>;
}

export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

#### Dynamic Rendering

라우트가 [동적으로 렌더링](../../BuildingYourApplication/Rendering/Static_and_Dynamic.md#dynamic-rendering)되는 경우 클라이언트 컴포넌트의 초기 서버 렌더링 중에 서버에서 `useSearchParams` 를 사용할 수 있습니다.

참고: [동적 라우트 세그먼트 구성 옵션(`dynamic` route segment config option)](../../APIReference/FileConventions/Route_Segement_Config.md#dynamic)을 `force-dynamic`으로 설정하면 동적 렌더링을 강제화할 수 있습니다.

For example:

```jsx
//app/dashboard/search-bar.tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParams.get("search");

  // 초기 렌더링 서버에서 로깅되며, 이후 탐색에서는 클라이언트에서 로깅됩니다.
  console.log(search);

  return <>Search: {search}</>;
}
```

```jsx
//app/dashboard/page.tsx
import SearchBar from "./search-bar";

export const dynamic = "force-dynamic";

export default function Page() {
  return (
    <>
      <nav>
        <SearchBar />
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

#### Server Components

##### Pages

[페이지](../../APIReference/FileConventions/page.js.md)(서버 컴포넌트)에서 검색 매개변수에 액세스하려면 [`searchParams`](../../APIReference/FileConventions/page.js.md#searchparams-optional) prop을 사용하세요.

##### Layouts

페이지와 달리 [레이아웃](../../APIReference/FileConventions/layout.js.md)(서버 컴포넌트)은 [`searchParams` prop](../../APIReference/FileConventions/page.js.md#searchparams-optional)을 받지 않습니다.
[탐색 중에 공유 레이아웃이 다시 렌더링되지 않아](../../BuildingYourApplication/Routing/Routing.md) 탐색 간에 오래된 `searchParams` 가 발생할 수 있기 때문입니다. [자세한 설명](../../APIReference/FileConventions/layout.js.md#good-to-know)을 참고하세요.

- 대신 클라이언트 컴포넌트에서 Page의 [`searchParams` prop](../../APIReference/FileConventions/page.js.md#searchparams-optional)이나  [`useSearchParams`](./useSeartchParams.md) 훅을 사용하세요. 이렇게 하면 최신 `searchParams` 로 클라이언트에서 다시 렌더링됩니다.

---

### Examples

#### Updating `searchParams`

[`useRouter`](./useRouter.md) 또는 [`Link`](../../APIReference/Components/Link.md) 를 사용하여 새로운 `searchParams` 을 설정할 수 있습니다.
탐색이 수행된 후 현재 [`page.js`](../../APIReference/FileConventions/page.js.md)는 업데이트된 [`searchParams` prop](../../APIReference/FileConventions/page.js.md#searchparams-optional)을 받습니다.

For example:

```jsx
//app/example-client-component.tsx
export default function ExampleClientComponent() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams()!;

  // 현재 searchParams와 제공된 key/value 쌍을 병합하여 새로운 searchParams 문자열을
  // 가져옵니다.
  const createQueryString = useCallback(
    (name: string, value: string) => {
      const params = new URLSearchParams(searchParams);
      params.set(name, value);

      return params.toString();
    },
    [searchParams],
  );

  return (
    <>
      <p>Sort By</p>

      {/* using useRouter */}
      <button
        onClick={() => {
          // <pathname>?sort=asc
          router.push(pathname + '?' + createQueryString('sort', 'asc'));
        }}
      >
        ASC
      </button>

      {/* using <Link> */}
      <Link
        href={
          // <pathname>?sort=desc
          pathname + '?' + createQueryString('sort', 'desc')
        }
      >
        DESC
      </Link>
    </>
  );
}

```
