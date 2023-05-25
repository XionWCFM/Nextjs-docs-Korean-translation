원본 링크 : https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering

<br>

# **Static and Dynamic Rendering**

Next.js에서는 경로를 정적으로 렌더링하거나 동적으로 렌더링할 수 있습니다.

- **정적 경로**에서는 컴포넌트가 빌드 시점에 서버에서 렌더링됩니다. 작업 결과는 캐시되어 다음 요청에 재사용됩니다.
- **동적 경로**에서는 요청 시점에 컴포넌트가 서버에서 렌더링됩니다.

<br>

---

<br>

## **[Static Rendering (Default)](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#static-rendering-default)**

기본적으로 Next.js는 성능을 개선하기 위해 경로를 정적으로 렌더링합니다. 즉, 모든 렌더링 작업이 미리 완료되어 사용자와 지리적으로 더 가까운 콘텐츠 전송 네트워크(CDN)에서 제공될 수 있습니다.

<br>

---

<br>

## **[Static Data Fetching (Default)](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#static-data-fetching-default)**

기본적으로 Next.js는 캐싱 동작을 특별히 선택 해제하지 않은 `fetch()` 요청의 결과를 캐싱합니다. 즉, `cache` 옵션을 설정하지 않은 fetch 요청은 `force-cache` 옵션을 사용합니다.

경로의 모든 가져오기 요청이 `revalidate` 옵션을 사용하는 경우 재검증 중에 경로가 정적으로 다시 렌더링됩니다.

데이터 가져오기 요청 캐싱에 대해 자세히 알아보려면 [Caching and Revalidating](https://nextjs.org/docs/app/building-your-application/data-fetching/caching) 페이지를 참조하세요.

<br>

---

<br>

## **[Dynamic Rendering](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-rendering)**

정적 렌더링 중에 동적 함수 또는 동적 `fetch()` 요청(캐싱 없음)이 발견되면 Next.js는 요청 시점에 전체 경로를 동적으로 렌더링하는 것으로 전환합니다. 캐시된 데이터 요청은 동적 렌더링 중에 계속 재사용할 수 있습니다.

이 표에는 [dynamic functions](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)와 [caching](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#static-data-fetching-default)이 경로의 렌더링 동작에 미치는 영향이 요약되어 있습니다:

| Data Fetching   | Dynamic Functions | Rendering |
| --------------- | ----------------- | --------- |
| Static (Cached) | No                | Static    |
| Static (Cached) | Yes               | Dynamic   |
| Not Cached      | No                | Dynamic   |
| Not Cached      | Yes               | Dynamic   |

동적 함수는 데이터 가져오기의 캐시 여부에 관계없이 항상 경로를 동적 렌더링으로 선택한다는 점에 유의하세요. 즉, 정적 렌더링은 데이터 가져오기 동작뿐만 아니라 경로에 사용되는 동적 함수에 따라 달라집니다.

> Note: 향후 Next.js는 전체 경로가 아닌 경로의 레이아웃과 페이지를 독립적으로 정적 또는 동적으로 렌더링할 수 있는 하이브리드 서버 측 렌더링을 도입할 예정입니다.

<br>

### **[Dynamic Functions](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)**

동적 함수는 사용자의 쿠키, 현재 요청 헤더 또는 URL의 검색 매개변수와 같이 요청 시점에만 알 수 있는 정보에 의존합니다. Next.js에서 이러한 동적 함수는 다음과 같습니다:

- 서버 컴포넌트에서 `[cookies()](https://nextjs.org/docs/app/api-reference/functions/cookies)` 또는 `[headers()](https://nextjs.org/docs/app/api-reference/functions/headers)` 를 사용하면 요청 시 전체 경로가 동적 렌더링으로 선택됩니다.
- 클라이언트 컴포넌트에서 `[useSearchParams()](https://nextjs.org/docs/app/api-reference/functions/use-params)`를 사용하면 정적 렌더링을 건너뛰고 대신 클라이언트에서 가장 가까운 상위 서스펜스 경계까지 모든 클라이언트 컴포넌트를 렌더링합니다.
  - `useSearchParams()`를 사용하는 클라이언트 컴포넌트를 `<Suspense/>` 경계로 감싸는 것이 좋습니다. 이렇게 하면 그 위에 있는 모든 클라이언트 컴포넌트가 정적으로 렌더링될 수 있습니다. [Example](https://nextjs.org/docs/app/api-reference/functions/use-search-params#static-rendering).
- `[searchParams](https://nextjs.org/docs/app/api-reference/file-conventions/page#searchparams-optional)` [Pages](https://nextjs.org/docs/app/api-reference/file-conventions/page) 프로퍼티를 사용하면 요청 시 페이지가 동적 렌더링으로 선택됩니다.

<br>

---

<br>

## **[Dynamic Data Fetching](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-data-fetching)**

Dynamic data fetches are `fetch()` requests that specifically opt out of caching behavior by setting the `cache` option to `'no-store'` or `revalidate` to `0`.

The caching options for all `fetch` requests in a layout or page can also be set using the [segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config) object.

To learn more about Dynamic Data Fetching, see the [Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching) page.

동적 데이터 가져오기는 `cache` 옵션을 `'no-store'`으로 설정하거나 `revalidate`을 `0`으로 설정하여 캐싱 동작을 특별히 opt out하는 `fetch()` 요청입니다.

레이아웃 또는 페이지의 모든 `fetch` 요청에 대한 캐싱 옵션은  [segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config) 객체를 사용하여 설정할 수도 있습니다.

동적 데이터 가져오기에 대해 자세히 알아보려면 [Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching) 페이지를 참조하세요.
