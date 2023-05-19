# 데이터 가져오기

> **참고사항**
> - 이 새로운 데이터 가져오기 모델은 현재 React 팀에서 개발 중입니다. 서버 컴포넌트에서 `async`와 `await`를 도입하고 클라이언트 컴포넌트를 위한 새로운 `use()` 훅을 소개하는 [support for promises React RFC](https://github.com/acdlite/rfcs/blob/first-class-promises/text/0000-first-class-support-for-promises.md) 문서를 읽는 것을 권장합니다.
> - 실제로 시도해 볼 수는 있지만, 아직 안정화되지 않았습니다. 우리는 최신 개발 동향을 반영하기 위해 이 문서를 업데이트할 예정입니다.

React와 Next.js 13에서는 애플리케이션에서 데이터를 가져오고 관리하는 새로운 방법을 소개했습니다. 새로운 데이터 가져오기 시스템은 `app` 디렉토리에서 작동하며 [`fetch()` 웹 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)를 기반으로 구축되었습니다.

`fetch()`는 원격 리소스를 가져오는 데 사용되는 웹 API로, 프로미스를 반환합니다. React는 `fetch`를 확장하여 [자동 요청 중복 제거 기능](https://nextjs.org/docs/app/building-your-application/data-fetching#automatic-fetch-request-deduping)을 제공하며, Next.js는 `fetch` 옵션 객체를 확장하여 각 요청이 자체적으로 [캐싱과 유효성 재검증](https://nextjs.org/docs/app/building-your-application/data-fetching/caching)을 설정할 수 있게 합니다.

---

## [서버 컴포넌트에서의 async와 await](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#async-and-await-in-server-components)

[React RFC](https://github.com/acdlite/rfcs/blob/first-class-promises/text/0000-first-class-support-for-promises.md)를 통해 서버 컴포넌트에서 `async`와 `await`을 사용하여 데이터를 가져올 수 있습니다.

```TS
// app/page.tsx

async function getData() {
  const res = await fetch('https://api.example.com/...');
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.
 
  // Recommendation: handle errors
  if (!res.ok) {
    // This will activate the closest `error.js` Error Boundary
    throw new Error('Failed to fetch data');
  }
 
  return res.json();
}
 
export default async function Page() {
  const data = await getData();
 
  return <main></main>;
}
```

> **비동기 서버 컴포넌트 타입스크립트 에러**
> - `async` 서버 컴포넌트는 사용된 위치에서 `Promise<Element>'는 유효한 JSX 요소 타입이 아닙니다`라는 오류를 일으킬 수 있습니다.
> - 이는 TypeScript의 알려진 문제로서 현재 수정 작업이 진행 중입니다.
> - 임시 해결책으로 해당 컴포넌트 위에 `{/* @ts-expect-error Async Server Component */}`를 추가하여 해당 컴포넌트의 타입 체크를 비활성화할 수 있습니다.

### 서버 컴포넌트 함수

Next.js는 서버 컴포넌트에서 데이터를 가져올 때 필요한 유용한 서버 함수를 제공합니다.

- [cookies()](https://nextjs.org/docs/app/api-reference/functions/cookies)
- [headers()](https://nextjs.org/docs/app/api-reference/functions/headers)

---

## [클라이언트 컴포넌트에서의 use](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#use-in-client-components)

`use()`는 새로운 React 함수로, 개념적으로 `await`과 유사한 **프로미스를 받습니다.** `use()`는 컴포넌트, 훅, 그리고 서스펜스와 호환되는 방식으로 함수가 반환한 **프로미스를 처리합니다.** use에 대해 더 자세히 알아보려면 [React RFC](https://github.com/acdlite/rfcs/blob/first-class-promises/text/0000-first-class-support-for-promises.md#usepromise)를 참조하세요.

현재 클라이언트 컴포넌트에서 `use()` 내부에 `fetch`를 래핑하는 것은 권장되지 않으며, 여러 번의 재렌더링을 유발할 수 있습니다. 현재 클라이언트 컴포넌트에서 데이터를 가져와야 할 경우, [SWR](https://swr.vercel.app/) 또는 [React Query](https://tanstack.com/query/v4)와 같은 타사 라이브러리를 사용하는 것을 권장합니다.

> **참고**: `fetch`와 `use`가 클라이언트 컴포넌트에서 동작할 때 추가 예제를 제공할 예정입니다.

---

## [정적 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#static-data-fetching)

기본적으로 `fetch`는 데이터를 자동으로 가져와 영구적으로 [데이터를 캐시](https://nextjs.org/docs/app/building-your-application/data-fetching/caching)합니다.

```TS
fetch('https://...'); // cache: 'force-cache' is the default
```

### [데이터 유효성 재검사](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#revalidating-data)

[캐시된 데이터](https://nextjs.org/docs/app/building-your-application/data-fetching/caching)를 일정 시간 간격으로 유효성 재검사하려면 `fetch()`에서 `next.revalidate` 옵션을 사용하여 리소스의 `cache` 수명(초 단위)을 설정할 수 있습니다.

```TS
fetch('https://...', { next: { revalidate: 10 } });
```

자세한 내용은 [데이터 유효성 재검사](https://nextjs.org/docs/app/building-your-application/data-fetching/revalidating)를 참조하세요.

> 참고: `revalidate` 또는 `cache: 'force-cache'`를 통한 fetch 수준에서의 캐싱은 공유 캐시를 통해 요청 간에 데이터를 저장합니다. 이를 사용하여 사용자별 데이터(예: `cookie()`나 `headers()`에서 유도된 데이터)에는 사용하지 않는 것이 좋습니다.

---

## [동적 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#dynamic-data-fetching)

매 요청마다 최신 데이터를 `fetch` 하려면 `cache: 'no-store'` 옵션을 사용하세요.

```TS
fetch('https://...', { cache: 'no-store' });
```

---

## [데이터 가져오기 패턴](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#data-fetching-patterns)

### [병렬 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#parallel-data-fetching)

클라이언트-서버 간의 지연을 최소화하기 위해 데이터를 병렬로 가져오는 다음 패턴을 권장합니다.

```TS
// app/artist/[username]/page.tsx

import Albums from './albums';
 
async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`);
  return res.json();
}
 
async function getArtistAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`);
  return res.json();
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Initiate both requests in parallel
  const artistData = getArtist(username);
  const albumsData = getArtistAlbums(username);
 
  // Wait for the promises to resolve
  const [artist, albums] = await Promise.all([artistData, albumsData]);
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  );
}
```

서버 컴포넌트에서 `await`을 호출하기 전에 `fetch`를 시작함으로써 각 요청은 동시에 요청을 가져오기 시작할 수 있습니다. 이렇게 하면 컴포넌트를 설정하여 지연을 피할 수 있습니다.

두 요청을 병렬로 시작함으로써 시간을 절약할 수 있지만, 사용자는 두 프로미스가 모두 해결될 때까지 렌더링된 결과를 볼 수 없습니다.

렌더링 작업을 분할하고 가능한 빠르게 일부 결과를 표시하여 사용자 경험을 향상시키기 위해, [서스펜스 바운더리](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)를 추가할 수 있습니다.

```TS
// artist/[username]/page.tsx

import { getArtist, getArtistAlbums, type Album } from './api';
 
export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Initiate both requests in parallel
  const artistData = getArtist(username);
  const albumData = getArtistAlbums(username);
 
  // Wait for the artist's promise to resolve first
  const artist = await artistData;
 
  return (
    <>
      <h1>{artist.name}</h1>
      {/* Send the artist information first,
          and wrap albums in a suspense boundary */}
      <Suspense fallback={<div>Loading...</div>}>
        <Albums promise={albumData} />
      </Suspense>
    </>
  );
}
 
// Albums Component
async function Albums({ promise }: { promise: Promise<Album[]> }) {
  // Wait for the albums promise to resolve
  const albums = await promise;
 
  return (
    <ul>
      {albums.map((album) => (
        <li key={album.id}>{album.name}</li>
      ))}
    </ul>
  );
}
```

컴포넌트 구조를 개선하기 위한 자세한 내용은 [사전 로딩 패턴](https://nextjs.org/docs/app/building-your-application/data-fetching/caching#preload-pattern-with-cache)을 살펴보세요.

### [순차적 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#sequential-data-fetching)

데이터를 순차적으로 가져오기 위해서는 해당 데이터가 필요한 컴포넌트 내에서 직접 `fetch`를 수행하거나, 필요한 컴포넌트 내에서 `fetch`의 결과를 `await`할 수 있습니다.

```TS
// app/artist/page.tsx

// ...
 
async function Playlists({ artistID }: { artistID: string }) {
  // Wait for the playlists
  const playlists = await getArtistPlaylists(artistID);
 
  return (
    <ul>
      {playlists.map((playlist) => (
        <li key={playlist.id}>{playlist.name}</li>
      ))}
    </ul>
  );
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Wait for the artist
  const artist = await getArtist(username);
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <Playlists artistID={artist.id} />
      </Suspense>
    </>
  );
}
```

컴포넌트 내에서 데이터를 가져오면 이전 요청이나 세그먼트가 완료될 때까지 다음 요청이나 세그먼트의 데이터 가져오기와 렌더링이 시작되지 않습니다.

### [라우트에서 렌더링 차단](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#sequential-data-fetching)

[레이아웃](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts)에서 데이터를 가져오면 해당 데이터 로딩이 완료될 때까지 그 아래에 있는 모든 라우트 세그먼트의 렌더링은 시작되지 않습니다.


`pages` 디렉토리에서 서버 렌더링을 사용하는 페이지는 `getServerSideProps`가 완료될 때까지 브라우저 로딩 스피너가 표시되고, 그 후 해당 페이지의 React 컴포넌트가 렌더링됩니다. 이는 "전부 또는 아무것도 없는" 데이터 가져오기로 설명할 수 있습니다. 즉, 페이지에 대한 모든 데이터를 가지고 있거나 전혀 없는 상태입니다.

`app` 디렉토리에서는 추가적인 옵션을 탐색할 수 있습니다.

1. 첫째, 서버로부터 데이터를 가져오는 함수의 결과를 스트리밍하는 동안 서버에서 즉시 로딩 상태를 표시하는 데 `loading.js`를 사용할 수 있습니다.

2. 둘째, 데이터를 가져오는 위치를 컴포넌트 트리의 아래로 이동하여 페이지의 필요한 부분에 대해서만 렌더링 차단을 수행할 수 있습니다. 예를 들어, 루트 레이아웃에서 데이터를 가져오는 대신 특정 컴포넌트로 데이터 가져오기를 이동할 수 있습니다.

가능한 경우, 사용하는 세그먼트에서 데이터를 가져오는 것이 가장 좋습니다. 이렇게 하면 로딩 중인 부분만 로딩 상태를 표시할 수 있고 전체 페이지를 로딩 상태로 표시하지 않을 수도 있습니다.

---

## [fetch() 없이 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#data-fetching-without-fetch)

ORM이나 데이터베이스 클라이언트와 같은 서드파티 라이브러리를 사용하는 경우에는 직접 `fetch` 요청을 사용하고 구성할 수 없는 경우도 있을 수 있습니다.

`fetch`를 사용할 수 없지만 레이아웃이나 페이지의 캐싱 또는 유효화 재검사 동작을 제어하고 싶은 경우에는 세그먼트의 [기본 캐싱 동작](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#default-caching-behavior)을 활용하거나 [세그먼트 캐시 구성](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#segment-cache-configuration)을 사용할 수 있습니다.

### [기본 캐싱 동작](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#default-caching-behavior)

`fetch`를 직접 사용하지 않는 데이터 가져오기 라이브러리는 라우트의 캐싱에 영향을 주지 않으며, 라우트 세그먼트에 따라 정적이거나 동적일 수 있습니다.

세그먼트가 정적(기본값)인 경우, 요청의 결과는 (구성에 따라) 다른 세그먼트와 함께 캐싱되고 유효화 재검사를 하게 됩니다. 세그먼트가 동적인 경우, 요청의 결과는 캐시되지 않고, 세그먼트가 렌더링될 때마다 모든 요청마다 다시 가져옵니다.

> **참고 사항**: [`cookies()`](https://nextjs.org/docs/app/api-reference/functions/cookies)와 [`headers()`](https://nextjs.org/docs/app/api-reference/functions/headers)와 같은 동적 함수는 라우트 세그먼트를 동적으로 만듭니다.

### [세그먼트 캐시 구성](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching#segment-cache-configuration)

일시적인 해결책으로, 서드파티 쿼리의 캐싱 동작을 구성할 수 있을 때까지, [세그먼트 구성](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#configrevalidate)을 사용하여 전체 세그먼트의 캐시 동작을 사용자 정의할 수 있습니다.

```TS
// app/page.tsx

import prisma from './lib/prisma';
 
export const revalidate = 3600; // revalidate every hour
 
async function getPosts() {
  const posts = await prisma.post.findMany();
  return posts;
}
 
export default async function Page() {
  const posts = await getPosts();
  // ...
}
```
