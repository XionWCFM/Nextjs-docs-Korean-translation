# **Route Segment Config Options**

"Route Segment Config Options"는 [페이지](), [레이아웃]() 또는 [라우트 핸들러]()의 동작을 직접 구성할 수 있는 변수를 내보내는 것을 허용합니다. :

|layout.js or page.js|
|:--|
```javascript
export const dynamic = 'auto'
export const dynamicParams = true
export const revalidate = false
export const fetchCache = 'auto'
export const runtime = 'nodejs'
export const preferredRegion = 'auto'
export function generateStaticParams(...)
```
**알아두면 좋은 점 :**

- config options의 값은 정적으로 분석 가능해야 합니다. 예를 들어, `revalidate = 600`은 유효하지만, `revalidate = 60 * 10`은 유효하지 않습니다.


## `dynamic`
레이아웃 또는 페이지의 동적 동작을 완전히 정적 또는 완전히 동적으로 변경합니다.

|layout.js or page.js|
|:--|
```javascript
export const dynamic = 'auto'
// 'auto' | 'force-dynamic' | 'error' | 'force-static'
```


> **Migration Note** : `app` 디렉토리의 새로운 모델은 `pages` 디렉토리의 `getServerSideProps` 및 `getStaticProps`의 바이너리 all-or-nothing 모델 대신에, `fetch` 요청 수준에서의 세분화된 캐싱 제어를 선호합니다. `dynamic` 옵션은 이전 모델로 다시 옵트인하는 방법으로 편의를 제공하며, 더 간단한 마이그레이션 경로를 제공합니다.

- `'auto'`(default): 컴포넌트가 동적 동작에 참여하는 것을 방지하지 않으면서 최대한 캐시를 유지하는 기본 옵션입니다.

- `'force-dynamic'`: 모든 `fetch` 요청의 캐싱을 비활성화하고 항상 재검증하여 레이아웃 또는 페이지의 동적 렌더링과 동적 데이터 가져 오기를 강제로 설정합니다. 이 옵션은 다음과 동일합니다 :

    - `pages` 디렉토리의`getServerSideProps()` 함수
    - 레이아웃(layout)이나 페이지(page)의 모든 `fetch()` 요청의 옵션을 `{cache: 'no-store', next: {revalidate: 0}}`로 설정하기
    - 섹션(segment) 구성을 다음과 같이 설정하기: `export const fetchCache = 'force-no-store'`

- `'error'` :  [동적 함수(dynamic functions)]()나 [동적 데이터(dynamic fetches)]() 가져오기를 사용하는 컴포넌트가 있을 경우 에러를 발생시켜 렌더링 및 데이터 가져오기를 정적으로 처리하도록 하는 옵션입니다. 이 옵션은 다음과 같은 의미입니다 :
    - `getStaticProps()`는 `pages`디렉토리에서 정적인 데이터를 가져오는 함수입니다.
    - 레이아웃이나 페이지의 모든 `fetch()` 요청 옵션을 `{ cache : 'force-cache'}`로 설정하는 것입니다.
    - 세그먼트(segment) 구성을 `fetchCache = 'only-cache', dynamicParams = false`로 설정하는 것입니다.
    - 참고 : `dynamic = 'error'`는 `dynamicParams`의 기본값을 `true`에서 `false`로 변경합니다. `generateStaticParams`에서 생성된 동적 매개변수가 아닌 동적 매개변수에 대해 페이지를 동적으로 렌더링하려면 `dynamicParams = true`를 수동으로 설정해야 합니다.

- `'force-static'` : `cookies()`, `headers()`, `useSearchParams()` 함수가 빈 값을 반환하도록 강요하여 레이아웃이나 페이지의 정적 렌더링과 정적 데이터 가져 오기를 강요하는 것입니다.

**알아두면 좋은 점** :

`getServerSideProps`와 `getStaticProps`에서 `dynamic: 'force-dynamic'` 및 `dynamic: 'error'`로 [마이그레이션(migrate)]()하는 방법에 대한 지침은 [업그레이드 가이드]()에서 찾을 수 있습니다.


dynamicParams 부터
