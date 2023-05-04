# 연결 및 탐색(Linking and Navigating)

Next.js router는 클라이언트 측 탐색([client-side navigation](https://beta.nextjs.org/docs/routing/linking-and-navigating#how-navigation-works))과 함께 서버 중심의 라우팅([server-centric routing](https://beta.nextjs.org/docs/routing/fundamentals#server-centric-routing-with-client-side-navigation))을 사용합니다. 이는 즉각적인 로딩 상태와 동시 렌더링을 지원합니다. 즉, 탐색이 client-side state를 유지하고, 비용이 많이 드는 재렌더링을 피하며, 중단이 가능하고, race conditions을 일으키지 않습니다.

router 사이를 탐색하는 방법에는 두 가지가 있습니다.

• `[<Link>` Component](https://beta.nextjs.org/docs/routing/linking-and-navigating#link-component)

• `[useRouter` Hook](https://beta.nextjs.org/docs/routing/linking-and-navigating#userouter-hook)

이 페이지에서는 <`Link`>와 useRouter()를 사용하는 방법을 살펴봅니다. 그리고  탐색이 작동하는 방식에 대해 자세히 알아볼 것입니다.

## <Link>Component

<`Link`>는 HTML <a> 요소를 확장한 React 컴포넌트입니다.  라우터들(routes)간의 prefetching 및 클라이언트 측 탐색(client-side navigation)을 제공합니다. 이 방법은 Next.js에서 라우터들(routes)간 탐색하는 기본적인 방법입니다.

### 사용법

<`Link`>를 사용하려면 “next/link”에서 임포트(import)하고 컴포넌트에 href 프로퍼티를 전달하세요.

```tsx
//app/page.tsx
import Link from 'next/link';

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>;
}
```

<`Link`>에 전달할 수 있는 옵션 프로퍼티가 있습니다. 자세한 내용은 [API reference](https://beta.nextjs.org/docs/api-reference/components/link)를 참조하세요.

## 예시: 동적 세그먼트(****[Dynamic Segments](https://beta.nextjs.org/docs/routing/linking-and-navigating#example-linking-to-dynamic-segments)****)에 연결하기

동적 세그먼트에 연결시, 템플릿 리터럴([template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)) 및 인터폴레이션([interpolation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals))을 사용하여 링크 목록을 생성할 수 있습니다. 예를 들어 블로그 글 리스트를 생성할 수 있습니다.

```tsx
//app/blog/PostList.jsx
import Link from 'next/link';

export default function PostList({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  );
}
```

## useRouter() Hook

useRouter() Hook을 사용하면 클라이언트 컴포넌트([Client Components](https://beta.nextjs.org/docs/rendering/server-and-client-components)) 내에서 프로그래밍 방식으로 라우터(routes)를 변경할 수 있습니다.

`[useRouter](https://beta.nextjs.org/docs/routing/linking-and-navigating#userouter-hook)`를 사용하려면 “next/navigation”에서 임포트하고 클라이언트 컴포넌트([Client Components](https://beta.nextjs.org/docs/rendering/server-and-client-components).)내부에서 훅을 호출하세요.

```tsx
'use client';

import { useRouter } from 'next/navigation';

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  );
}
```

useRouter는 push(), refresh() 등의 메서드를 제공합니다. 자세한 내용은 API 참조를 참조하세요.

<aside>
💡 권장 사항: useRouter를 사용해야 하는 특별한 사항이 아니라면 <`Link`> 컴포넌트를 사용하여 라우터 를 탐색하세요.

</aside>

## 내비게이션 작동 방식

- <`Link`>를 사용하거나 `router.push()`를 호출하면 라우터 전환이 시작됩니다.
- 라우터는 브라우저의 주소창에 URL을 업데이트합니다.
- 라우터는 클라이언트 사이드 캐시([client-side cache](https://beta.nextjs.org/docs/routing/linking-and-navigating#client-side-caching-of-rendered-server-components))에서 변경되지 않은 세그먼트(예: 공유 레이아웃)를 재사용하여 불필요한 작업을 피합니다. 이를 부분 렌더링이라고도 합니다.
- 소프트 탐색 조건([conditions of soft navigation](https://beta.nextjs.org/docs/routing/linking-and-navigating#conditions-for-soft-navigation))이 충족되면 라우터는 서버가 아닌 캐시에서 새 세그먼트를 가져옵니다. 그렇지 않은 경우 라우터는 하드 탐색([hard navigation](https://beta.nextjs.org/docs/routing/linking-and-navigating#hard-navigation))을 수행하고 서버에서 서버 컴포넌트 페이로드를 가져옵니다.
- 생성된 경우 페이로드를 가져오는 동안 서버에서 로딩 UI가 표시됩니다.
- 라우터는 캐시된 페이로드 또는 새로운 페이로드를 사용하여 클라이언트에서 새 세그먼트를 렌더링합니다.

렌더링된 서버 컴포넌트의 클라이언트 사이드 캐싱(****[Client-side Caching](https://beta.nextjs.org/docs/routing/linking-and-navigating#client-side-caching-of-rendered-server-components)****)

<aside>
💡 알아두면 좋은 정보: 이 클라이언트 사이드 캐시(client-side cache)는 서버 사이드 Next.js HTTP 캐시(server-side [Next.js HTTP cache](https://beta.nextjs.org/docs/data-fetching/fundamentals#caching-data))와 다릅니다.

</aside>

새 라우터에는 서버 컴포넌트의 렌더링 결과(페이로드)를 저장하는 인-메모리 클라이언트 사이드 캐시(**in-memory client-side cache**)를 가집니다. 캐시는 라우트 세그먼트별로 분할되어 어느 수준에서든 무효화를 허용하고 동시 렌더링 전반에 걸쳐 일관성을 보장합니다.

사용자가 앱을 탐색할 때, 라우터는 이전에 패치된 세그먼트들과 미리 패치된 세그먼트의 페이로드를  캐시에 저장합니다.

즉, 특정 경우 라우터는 서버에 새로 요청하는 대신 캐시를 재사용할 수 있습니다. 이렇게 하면 데이터를 다시 가져오고 불필요하게 컴포넌트를 다시 렌더링하지 않아도 되므로 성능이 향상됩니다.

## 캐시 무효화하기

`router.refresh`()`를 사용하여 라우터를 새로 고칠 수 있습니다. 이렇게 하면 서버에 새 요청을 보내 데이터 요청을 다시 가져오고 서버 컴포넌트를 다시 렌더링합니다. 자세한 내용은 API 참조를 참조하세요. 앞으로는 ****객체 변이(mutation)****을 하면 캐시가 자동으로 무효화됩니다.

프리패칭(****[Prefetching](https://beta.nextjs.org/docs/routing/linking-and-navigating#prefetching)****)

프리패칭(****[Prefetching](https://beta.nextjs.org/docs/routing/linking-and-navigating#prefetching)****)은 방문하기 전에 백그라운드에서 라우트를 미리 로드하는 방법입니다. 프리페치된 경로의 렌더링 결과는 라우터의 클라이언트 사이드 캐시(router's client-side cache)에 추가됩니다. 따라서 프리페치된 라우트로 거의 즉시 이동할 수 있습니다.

기본적으로 라우트들은 <`Link`> 컴포넌트를 사용할 때 뷰포트(viewport)에 표시되는 대로 프리패치됩니다. 이는 페이지가 처음 로드될 때 또는 스크롤할 때 발생할 수 있습니다. 라우트들은 또한 `[useRouter](https://beta.nextjs.org/docs/routing/linking-and-navigating#userouter-hook)`() 훅의 프리페치 메서드를 사용하여 프로그래밍 방식으로  프리패치할 수 있습니다.

### 정적 및 동적 라우트:

라우트가 정적 라우트인 경우, 라우트 세그먼트에 대한 모든 서버 컴포넌트 페이로드가 프리채치됩니다.

라우트가 동적인 경우, 첫 번째 공유 레이아웃부터 첫 번째 loading.js 파일까지 페이로드가 프리패치됩니다. 이렇게 하면 전체 라우트를 동적으로 프리페치하는 데 드는 비용이 줄어들고 동적 라우트에 대한 로딩 상태(loading states)를 즉시 확인할 수 있습니다.

### 알아두면 좋습니다:

- 프리페칭은 프로덕션 환경에서만 활성화됩니다.
- 프리페칭은 <`Link`>에 `prefetch={false}`를 전달하여 비활성화할 수 있습니다.

## Hard Navigation의 경우

탐색 시 캐시가 무효화되고 서버가 데이터를 새로 고침하여 변경된 세그먼트를 다시 렌더링합니다.

## Soft Navigation의 경우

탐색 시 변경된 세그먼트에 대한 캐시가 재사용되며(캐시가 있는 경우) 서버에 데이터를 새로 요청하지 않습니다.

### soft Navigation의 조건

탐색시 탐색 중인 경로가 미리 가져온 경로이고 동적 세그먼트를 포함하지 않거나

현재 경로와 동일한 동적 매개 변수가 있는 경우 Next.js는 소프트 탐색을 사용합니다.

예컨대 [team]이라는 동적인 세그먼트가 포함된 경로를 고려해보세요

/dashboard/[team]/* 과 같은 예제에서 아래 캐시된 세그먼트는 오로지 [team] 파라미터가 변할때만 무효화됩니다.

예를 들어 dashboard/team-red/* 에서 dashboard/team-red/*로 이동하는 것은 soft navigation이 됩니다.

반면 dashboard/team-red에서 dasheboard/team-blue로 이동하는 것은 hard navigation이 됩니다.

> 역주 : *를 와일드 카드처럼 이해하시면 될 것 같습니다.
> 

> dashboard/team-red/ 뒤에 올수 있는 거의 모든 경우의 수를 지칭합니다.
> 

## Back/Forward Navigation (뒤로 / 앞으로 탐색)

back and forward navigation(popstate event)는 soft navigation을 합니다.

즉 클라이언트 측 캐시가 재사용되고 탐색이 거의 즉각적으로 이루어집니다.

## **Focuse and scroll Management**

초점 및 스크롤 관리

기본적으로 Next.js는 탐색에서 변경된 세그먼트를 보기 위해 초점을 설정하고 스크롤 합니다.

이 접근성과 사용성 기능의 중요성은 고급 라우팅 패턴이 구현될 때 더욱 분명해질것입니다.

[< Pages and Layouts ](./Intercepting_Routes.md)

[Loading UI>](./Loading_UI.md)