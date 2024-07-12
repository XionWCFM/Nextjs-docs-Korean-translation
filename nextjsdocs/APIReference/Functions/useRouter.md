# useRouter API Reference

<p>공식 문서 : https://nextjs.org/docs/app/api-reference/functions/use-router</p>

`useRouter` 훅을 사용하여 클라이언트 컴포넌트 내에서 프로그래밍 방식으로 경로를 변경할 수 있습니다.

**권장 사항:** useRouter 사용에 특별한 요구 사항이 없다면 이동에 &lt;Link &gt; 컴포넌트를 사용하세요.

```jsx
//app/example-client-component.tsx
"use client";

import { useRouter } from "next/navigation";

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push("/dashboard")}>
      Dashboard
    </button>
  );
}
```

---

### `useRouter()`

- `router.push(href: string)` : 제공된 경로에 대한 클라이언트 사이드 이동을 수행합니다. [브라우저의 history](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 스택에 새 항목을 추가합니다.
- `router.replace(href: string)` : [브라우저의 history](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 스택에 새 항목을 추가하지 않고 제공된 경로에 대한 클라이언트 사이드 이동을 수행합니다.
- `router.refresh()` : 현재 경로를 새로 고칩니다. 서버에 새로운 요청을 만들고, 데이터 요청을 다시 가져오고, 서버 컴포넌트를 다시 렌더링합니다. 클라언트는 업데이트된 React 서버 컴포넌트 payload를 병합합니다. 이때 영향을 받지 않는 클라이언트 사이드 React (예: `useState`) 또는 브라우저 상태 (예: scroll position)를 잃지 않습니다.
- `router.prefetch(href: string)` : 더 빠른 클라이언트 사이드 전환을 위해 제공된 경로를 [미리 가져옵니다.](../../BuildingYourApplication/Routing/Linking_and_Navigating.md#prefetching)
- `router.back()` : [soft navigation](../../BuildingYourApplication/Routing/Linking_and_Navigating.md#soft-navigation)을 사용하여 브라우저의 history 스택에서 이전 경로로 다시 이동합니다.
- `router.forward()` : [soft navigation](../../BuildingYourApplication/Routing/Linking_and_Navigating.md#soft-navigation)을 사용하여 브라우저의 history 스택에서 다음 페이지로 이동합니다.

##### 알아두면 좋은 정보

- 만약 새로운 경로가 미리 불러오기되어 있고, [동적 세그먼트(dynamic segments)](../../BuildingYourApplication/Routing/Defining_Routes.md#dynamic-segments)를 포함하지 않거나 현재 경로와 동일한 동적 매개 변수를 가질 경우 `push()` 와 `replace()` 메서드는 [soft navigation](../../BuildingYourApplication/Routing/Linking_and_Navigating.md#soft-navigation)을 수행합니다.
- `next/link`는 뷰포트에 나타나는 대로 경로를 자동으로 미리 가져옵니다.
- `refresh()` 는 fetch 요청이 캐시된 경우 동일한 결과를 재생산할 수 있습니다. 쿠키(`cookies`) 및 헤더(`headers`)와 같은 다른 동적 기능도 응답을 변경할 수 있습니다.

**Migrating from the `pages` directory:**

- 새 `useRouter` 훅은  `next/router`가 아니라  `next/navigation`에서 import 해야합니다.
- pathname 문자열이 제거되면서 [`usePathname()`](./usePathname.md)으로 대체되었습니다.
- query 객체가 제거되면서  [`useSearchParams()`](./useSeartchParams.md) 로 대체되었습니다.
- 현재 `router.events` 는 지원되지 않습니다. [아래](#router-events)를 참조해주세요.

[View the full migration guide](../../BuildingYourApplication/Upgrading/App_Router_Migration.md#step-5-migrating-routing-hooks).

---

### Router Events

usePathname 및 useSearchParams와 같은 다른 클라이언트 컴포넌트 훅을 조합하여 페이지 변경을 감지할 수 있습니다. 이 커스텀 훅을 레이아웃에 추가할 수 있습니다.

```jsx
"use client";

import { usePathname, useSearchParams } from "next/navigation";

function useNavigationEvent() {
  const pathname = usePathname();
  const searchParams = useSearchParams();

  useEffect(() => {
    const url = pathname + searchParams.toString();
    // You can now use the current URL
  }, [pathname, searchParams]);
}
```
