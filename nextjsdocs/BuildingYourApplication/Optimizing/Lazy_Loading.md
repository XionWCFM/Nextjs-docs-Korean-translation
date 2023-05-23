원본 링크: [https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading)

# Lazy Loading

Next.js에서 [Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading)은 route를 렌더링하는데 필요한 JavaScript의 양을 줄임으로써 응용 프로그램의 초기 로드 성능을 향상시킵니다.

Lazy loading은 클라이언트 구성 요소 및 가져온 라이브러리의 로딩을 지연시켜, 필요할 때만 클라이언트 번들에 포함할 수 있습니다.
예를 들어, 사용자가 모달을 클릭하여 열 때까지 로딩을 지연시키는 게 좋을 것입니다.

Next.js에서 Lazy loading을 구현하는 방법은 두 가지가 있습니다.

1. next/dynamic 함께 동적 가져오기([Dynamic Imports](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#nextdynamic)) 사용하기
2. [Suspense](https://react.dev/reference/react/Suspense)와 함께 `[React.lazy()](https://react.dev/reference/react/lazy)` 사용하기

기본적으로, 서버 구성 요소는 자동으로 코드 분할([code split](https://developer.mozilla.org/en-US/docs/Glossary/Code_splitting))되며, 스트리밍([streaming](../Routing/Loading_UI_and_Streaming.md))을 사용하여 서버에서 클라이언트로 UI 조각을 점진적으로 전송할 수 있습니다. 반면, Lazy loading은 클라이언트 요소에 적용됩니다.

## next/dynamic

`next/dynamic` 은 `[React.lazy()](https://react.dev/reference/react/lazy)` 와 [Suspense](https://react.dev/reference/react/Suspense)의 합성체입니다. `app` 과 `pages` 디렉터리에서 동일하게 동작하여 점진적인 마이그레이션(incremental migration)을 가능하게 합니다.

## Examples

### Importing Client Components

`app/page.js`

```js
"use client";

import { useState } from "react";
import dynamic from "next/dynamic";

// Client Components:
const ComponentA = dynamic(() => import("../components/A"));
const ComponentB = dynamic(() => import("../components/B"));
const ComponentC = dynamic(() => import("../components/C"), { ssr: false });

export default function ClientComponentExample() {
  const [showMore, setShowMore] = useState(false);

  return (
    <div>
      {/* Load immediately, but in a separate client bundle */}
      <ComponentA />

      {/* Load on demand, only when/if the condition is met */}
      {showMore && <ComponentB />}
      <button onClick={() => setShowMore(!showMore)}>Toggle</button>

      {/* Load only on the client side */}
      <ComponentC />
    </div>
  );
}
```

### Skipping SSR

`React.lazy()` 와 Suspense를 사용할 때, 클라이언트 구성 요소는 기본적으로 사전 렌더링(SSR)됩니다.

만약 클라이언트 구성 요소의 사전 렌더링을 비활성화하고 싶다면, `ssr` 옵션을 `false` 로 설정할 수 있습니다.

```js
const ComponentC = dynamic(() => import("../components/C"), { ssr: false });
```

### Importing Server Components

서버 구성 요소를 동적으로 가져오는 경우, 서버 구성 요소 자체가 아니라 서버 구성 요소의 하위에 있는 클라이언트 구성 요소만 Lazy loading됩니다.

`app/page.js`

```js
import dynamic from "next/dynamic";

// Server Component:
const ServerComponent = dynamic(() => import("../components/ServerComponent"));

export default function ServerComponentExample() {
  return (
    <div>
      <ServerComponent />
    </div>
  );
}
```

### Loading External Libraries

`[import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)` 함수를 사용하여 필요할 때 외부 라이브러리를 로드할 수 있습니다. 이 예제에서는 모호한 검색(fuzzy search)을 위해 외부 라이브러리 `fuse.js`를 사용합니다. 이 모듈은 사용자가 검색 입력란에 타이핑하기 시작한 후에만 클라이언트에서 로드됩니다.

`app/page.js`

```js
"use client";

import { useState } from "react";

const names = ["Tim", "Joe", "Bel", "Lee"];

export default function Page() {
  const [results, setResults] = useState();

  return (
    <div>
      <input
        type="text"
        placeholder="Search"
        onChange={async (e) => {
          const { value } = e.currentTarget;
          // Dynamically load fuse.js
          const Fuse = (await import("fuse.js")).default;
          const fuse = new Fuse(names);

          setResults(fuse.search(value));
        }}
      />
      <pre>Results: {JSON.stringify(results, null, 2)}</pre>
    </div>
  );
}
```

### Adding a custom loading component

`app/page.js`

```js
import dynamic from "next/dynamic";

const WithCustomLoading = dynamic(
  () => import("../components/WithCustomLoading"),
  {
    loading: () => <p>Loading...</p>,
  }
);

export default function Page() {
  return (
    <div>
      {/* The loading component will be rendered while  <WithCustomLoading/> is loading */}
      <WithCustomLoading />
    </div>
  );
}
```

### Importing Named Exports

이름이 지정된 내보내기를 동적으로 가져오려면, `[import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)` 함수가 반환하는 Promise에서 해당 내보내기를 반환하면 됩니다.

`components/hello.js`

```js
"use client";

export function Hello() {
  return <p>Hello!</p>;
}
```

`app/page.js`

```js
import dynamic from "next/dynamic";

const ClientComponent = dynamic(() =>
  import("../components/ClientComponent").then((mod) => mod.Hello)
);
```
