<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config</p>

# Route Segment Config

Route Segment  옵션은 [Page](), [Layout]() 또는 [Route Handler]()의 동작을 구성할 수 있도록 다음 변수들을 직접 내보내는 것을 허용합니다. 

| Option | Type | Default |
| --- | --- | --- | 
| dynamic | `'auto' or 'force-'dynamic' or 'error' or 'force-static'` | `'auto'` |
| dynamicParams | `boolean` | `true` |
| revalidate | `false or 'force-cache' or 0 or number` | `false` |
| fetchCache | `'auto' or 'daefault-cache' or 'only-cache' or 'force-cache' or 'force-no-store' or 'default-no-store' or 'only-no-store'` | `'auto'` |
| runtime | `'nodejs' or 'edge'` | `nodejs` |
| preferredRegion | `'auto' or 'global' or 'home' or string or string[]` | `'auto'` |



```jsx
//layout.js OR page.js OR route.js

export const dynamic = 'auto';
export const dynamicParams = true;
export const revalidate = false;
export const fetchCache = 'auto';
export const runtime = 'nodejs';
export const preferredRegion = 'all';
 
export default function MyComponent() {}
```

##### 알아두면 좋은 정보

현재 구성 옵션의 값은 정적으로(statically) 분석 가능해야 합니다. 예를 들어 **`revalidate = 600`**은 유효하지만 **`revalidate = 60 * 10`** 은 유효하지 않습니다. 

---

## Options

 **`dynamic`**

레이아웃이나 페이지의 동적 동작(dynamic behavior)을 완전히 정적(static)이나 완전히 동적(dynamic) 으로 변경할 수 있습니다. 

```jsx
//layout.js OR page.js OR route.js

export const dynamic = 'auto';
//'auto' | 'force-dynamic' | 'error' | 'force-static'
```

참고 : `App` 디렉토리의 새 모델은 `pages` 디렉토리의 `getServerSideProps` 및 `getStaticProps` 의 이진(all-or-nothing) 모델보다는 `fetch` 요청 수준에서 세밀한 캐싱 제어를 선호합니다. `dynamic` 옵션은 편의성을 위해 이전 모델로 다시 돌아갈 수 있는 방법이며, 더 간단한 마이그레이션 경로를 제공합니다.

> 마이그레이션(migration)은 기존 시스템이나 소프트웨어에서 새로운 시스템이나 소프트웨어로의 전환을 의미합니다. 일반적으로 이전 버전에서 새로운 버전으로의 업그레이드, 데이터 이전, 코드 변경 등을 포함합니다. 마이그레이션은 기존 시스템을 더 효율적이거나 현대적인 기술에 맞게 업데이트하는 과정이며, 주로 호환성 문제를 해결하고 기능 개선을 위해 수행됩니다. 

- `‘auto’` (default) : 기본 옵션은 가능한 한 많은 캐싱(cache)을 수행하면서도 모든 컴포넌트가 동적 동작을 선택할 수 있도록 방지하지 않는 것입니다.
- `‘force-dynamic’` : 모든 fetch 요청의 캐싱을 비활성화하고 항상 다시 유효성을 검사하여 레이아웃이나 페이지의 동적 렌더링과 동적 데이터 가져오기를 강제로 실행합니다. 이 옵션은 다음과 동일합니다. :
    - pages 디렉토리 안의 `getStaticProps()`
    - 레이아옷이나 페이지의 모든 fetch() 요청의 옵션을 { cache: ‘force-cache’ } 로 설정
    - segment 구성을 `fetchCache = ‘only-cache’`,  `dynamicParams=false`로 설정
- `‘error’` : [동적 함수](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md)나 [동적 데이터 가져오기](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md)를 사용하는 컴포넌트가 있을 경우 오류를 
발생시켜 레이아웃이나 페이지의 정적 렌더링 및 정적 데이터 가져오기를 강제로 수행합니다. 이 옵션은 다음과 동일합니다.
    - `pages`디렉토리의 `getStaticProps()` 메서드 사용
    - 레이아웃이나 페이지 내의 모든 `fetch()` 요청의 옵션을 `{cache: 'force-cache'}`로 설정.
    - 세그먼트 설정을 `fetchCache = 'only-cache', dynamicParams = false.`
    - 참고 : `dynamic = ‘error’` 는 `dynamicParmas` 의 기본 값을 `true` 에서 `false`로 변화 시킨다. `generateStaticParmas`에서 생성되지 않은 동적 매개변수(dynamic params)에 대해 동적 랜더링을 다시 사용하려면 `dynamicParams = true` 로 수동으로 설정할 수 있습니다.


- `‘force-static’`*: 레이아웃(layout)이나 페이지의 정적 렌더링(static rendering)과 정적 데이터 가져오기(static data fetching)를 강제로 수행하기 위해 [cookies()](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/cookies.md) , [headers()](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/headers.md), [useSearchParams()](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/useSearchParams.md) 함수가 빈 값을 반환하도록 만드는 옵션입니다.

<br>

##### 알아두면 좋은 정보
`getServerSideProps` 와 `getStaticProps` 에서 `dynamic : ‘force-dynamic’` 과 `dynamic: ‘error’` 로 [‘마이그레이션 하는 방법’](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Upgrading/App_Router_Migration.md)에서 찾을 수 있습니다.


<br>

### `dynamicParmas`

dynamic segment가 방문 했을 때 무슨 일이 있는지 컨트롤 하는 것은 [generateStaticParmas](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/generateStaticParams.md)에 구성되어 있지 않다. 

```jsx
//layout.js/page.js/route.js

export const dynamicParmas = true; // true | false
```

- true (default) :  generateStaticParams 를 포함하지 않는 Dynamic segments는 요구에 구성되어 있습니다.
- false : generateStaticParams를 포함하지 않는 Dynamic segements 는 404를 반환합니다.

<br>

##### 알아두면 좋은 정보
* 해당 옵션은 `pages` 디렉토리의 `getStaticPaths` 의  `fallback:true | false | blocking` 옵션을 대체합니다. 
* `dynamicParams = true` 일 때, 해당 segment는 [Streaming Server Rendering](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Loading_UI_and_Streaming.md) 을 사용합니다. 
* 만약 `dynamic = ‘error’` 와 `dynamic = ‘force-static’`을 사용한다면 이는`dynamicParams`의 기본 값을 `false`로 변경합니다.


<br>

### `revalidate`

레이아웃 페이지의 기본 재검증 시간을 설정합니다. 이 옵션은 개별 fetch 요청에서 설정된 재검증 값을 덮어쓰지 않습니다. 

```jsx
//layout.js/page.js/route.js

export const revalidate = false;
//false | 'force-cache' | 0 | number
```

- **false** : (default) `‘force-cache’`로 `cache`옵션을 설정한 모든 `fetch` 요청 또는 [동적 함수](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md) 사용 전에 발견된 요청을 캐시하는 기본적인 휴리스틱을 사용합니다. `revalidate: Infinity`  와 의미적으로 동일하며, 이는 해당 리소스가 무기한으로 cache 되어야 함을 의미합니다. 개별 `fetch` 요청에서 `cache: ‘no-store’` 또는 `revalidae: 0` 을 사용하여 캐시를 피하고 해당 경로(route)를 동적으로 랜더링 되도록 할 수 있습니다. 또는 `revalidate` 를 route의 기본 값보다 작은 양수로 설정하여 route의 재검증 빈도를 높일 수 있습니다.

- **0** : 동적 함수나 동적 데이터 가져오기(data fetches)가 발견되지 않더라도 항상 레이아웃 (layout) 또는 페이지를 [동적으로 렌더링](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md)하도록 보장합니다. 이 옵션은 캐시 옵션을 설정하지 않은 `fetch` 요청의 기본값을 `'no-store’`로 변경합니다만, `‘force-cache’`로 설정되거나 양수 `재유효성(revalidate)`을 사용하는 `fetch`요청은 그대로 유지됩니다.

- **number** : (초 단위) 레이아웃이나 페이지의 기본 재검증 주를 `n`초로 설정합니다.

<br>

#### Revalidation Frequency  (재검증 주기)

- 동일한 경로 패턴을 가진 각 레이아웃과 페이지의 가장 낮은 `revalidate` 이 전체 경로(route)의 재 검증 주기를 결정합니다.  이는 자식 페이지도 부모 레이아웃과 동일한 빈도로 재 검증되도록 보장합니다.
- 개별 `fetch` 요청은 route의 기본 `revalidate`보다 낮은 revalidate 를 설정하여 전체 경로의 재 검증 빈도를 높일 수 있습니다. 이를 통해 특정 기준에 따라 특정 route에 대해 더 자주 재 검증을 선택적으로 수행할 수 있습니다.

<br>

### `fetchCache`

---
---
- **(아래 내용)** 기본 동작을 특정하게 override 해야 할 경우에만 사용되는 advanced option 입니다.
    
    기본적으로 Next.js는 동적 함수가 사용되기 전에 도달 가능한 모든 `fetch( )`요청을 캐시하고, 동적 함수가 사용된 후에 발견된 fetch 요청을 캐시하지 않습니다. 
    
    `fetchCache` 를 사용하면 레이아웃이나 페이지에서 모든 fetch요청의 기본 캐시 옵션을 재정의할 수 있습니다. 
    
    ```jsx
    //layout.js/page/js/route.js
    
    export const fetchCache = 'auto'
    //'auto' | 'default-cache' | 'only-cache'
    // 'force-cache' | 'force-no-store' | 'default-no-store'
    ```
    
    - `auto` (default) : 동적 함수 이전에 `fetch` 요청을 캐시하고, 동적 함수 이후의 fetch 요청은 제공하는 캐시옵션에 따라 캐시하지 않습니다.
    - `default-cache` : fetch에 임의의 캐시 옵션을 전달 할 수 있지만, 옵션이 제공되지 않으면 캐시 옵션을 `force-cach` 로 설정합니다. 이는 동적 함수 이후의 fetch요청도 정적으로 간주됩니다.
    - `only-cache` 모든 fetch요청이 캐싱을 사용하도록 하기 위해 기본값을 `cache : 'force-cache'`  로 변경하며, fetch 요청 중 `cache: "no-store"` 를 사용하는 경우 오류를 발생시킵니다.
    - `force-cache` : 모든 `fetch` 요청이 캐싱을 하도록 하기 위해 모든 fetch요청의 캐시 옵션을 `force-cache` 로 설정합니다.
    - `default-no-store` : fetch에 임의의 캐시 옵션을 전달 할 수 있지만, 옵션이 제공되지 않으면 캐시 옵션을 `no-store` 로 설정합니다. 이는 동적 함수 이전의 fetch요청도 동적으로 간주됩니다.
    - `only-no-store` : 모든 fetch요청이 캐싱을 사용하지 않도록 하기 위해 기본값을 `cache: "no-store"` 로 변경하며, fetch요청 중 `cache: "force-cache"` 를 사용하는 경우 오류를 발생시킵니다.
    - `force-no-store` : 모든 fetch요청이 캐싱을 사용하지 않도록 하기 위해 모둔 fetch 요청의 캐시 옵션을 `no-store` 로 설정합니다. 이는 `force-cache` 옵션을 제공하더라도 모든 fetch요청이 매 요청마다 다시 가져와야 합니다.
    
    <br>

   ### Cross-route segment behavior : 교차 경로 세그먼트 동작
    
    - 단일 경로의 각 레이아웃과 페이지에서 설정된 옵션은 서로 호환되어야 합니다.
        - `only-cache` 와 `force-cache` 가 모두 제공되는 경우, `force-cache` 가 우선합니다. `only-no-store` 와 `force-no-store` 가 모두 제공되는 경우, `force-no-store` 가 우선합니다. force 옵션은 경로 전체의 동작을 변경하므로 `force-*` 를 사용하는 단일 세그먼트는 `only-*` 에 의해 발생하는 오류를 방지합니다.
        - 이러한 옵션의 목적은 경로 전체가 정적이거나 전적으로 동적임을 보장하는 것 입니다.
            - 단일 경로(single route)에서 `only-cache` 와 `only-no-store` 의 조합은 허용되지 않습니다.
            - 단일 경로(single route)에서 `force-cache` 와 `force-no-store` 의 조합은 허용되지 않습니다.
        - 부모가 `default-no-store` 를 제공하고 자식이 `auto` 또는 `*-cache` 를 제공하는 것은 동일한 fetch  가 다른 동작을 가질 수 있기 때문에 허용되지 않습니다.
    - 공유되는 부모 레이아웃을 `auto` 로 유지하고 자식 세그먼트에서 동작이 분기되는 곳에서 옵션을 사용자 정의하는 것이 일반적으로 권장됩니다.
    
---
---

<br>

### `runtime`

```jsx
//layout.js/page.js/route.js

export const runtime = 'node.js'
//'edge' | 'node.js'
```

- **nodejs (default)**
- **edge**

[Edge 와 Node.js 런타임](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Edge_and_Node.js_Runtimes.md)에서 더 자세히 알아보세요. 

<br>

### `preferredRegion`

```jsx
//layout.js/page.js/route.js

export const preferredRegion = 'auto';
//'all' | 'iad1' | ['iad1', 'sfo1'] 
```

`preferredRegion` 및 지원되는 regions 은 배포 플랫폼에 따라 달라집니다. 

##### 알아두면 좋은 정보
* preferredRegion 이 지정되지 않았다면 이는 가장 가까운 부모 레이아웃의 옵션을 상속받습니다. 
* root layout 은 모든 지역(region)을 기본 값으로 갖습니다.

<br>

### `generateStaticParams`

`generateStaticParams` 함수는 [동적 경로 세그먼트](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Dynamic_Routes.md)와 함께 사용하여, 요청 시간이 아닌 빌드 시간에 정적으로 생성될 라우트 세그먼트 매개변수 목록을 정의하는데 사용될 수 있습니다. 

더 자세한 내용은 [API 참조](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/generateStaticParams.md)에서 확인하세요.