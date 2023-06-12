# layout.js

<p>공식 문서 : https://nextjs.org/docs/app/api-reference/file-conventions/layout</p>

<br>

**layout** 은 경로 사이에서 공유되는 UI 입니다. 

```jsx
//app/dashboard/layout.tsx
export default function DashboardLayout({
	children,
}: {
	children: React.ReactNode;
}) {
	return <section>{children}</section>
}
```

---

## Props

### `children` (required)

Layout 컴포넌트는 children prop을 사용하고 승인해야 합니다. 랜더링 되는 동안에  **`chilrden`**은 레이아웃이 wrapping되고 있는 경로 segment로 채워집니다. 이는 주로  child [**Layout**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Pages_and_Layouts.md)(존재한다면,)이나 [**Page**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Pages_and_Layouts.md)의 컴포넌트 일 수 있으나 해당이되는 경우 [**Loading**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Loading_UI_and_Streaming.md) 이나 [**Error**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Error_Handling.md)와 같은 다른 특별한 파일 일 수도 있습니다. 

<br>

### `params` (required)

[“**dynamic route parameters**(동적인 경로 변수) 객체”](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Dynamic_Routes.md) 는 root segment 에서 해당 레이아웃까지 이동합니다.

<br>

| Example | URL | params |
| --- | --- | --- |
| app/dashboard/[team]/layout.js | /dashboard/1 | { team: ‘1’ } |
| app/shop/[tag]/[item]/layout.js | /shop/1/2 | { tag: ‘1’, item: ‘2’ } |
| app/blog/[…slug]/layout.js | /blog/1/2 | { slug: [’1’, ‘2’] } |

예시 : 

```jsx
//app/shop/[tag]/[item]/layout.js
export default function ShopLayout({
	children,
params,
}: {
	children: React.ReactNode;
	params: {
		tag: string;
		item: string;
	};
}){
	// URL -> /shop/shoes/nike-air-max-97
	// `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
	return <section>{children}</section>
}
```

---

##### 알아두면 좋은 정보
> 레이아웃은 `searchParams` 를 받지 않습니다. 

[Pages](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/FileConventions/page.js.md) 와는 다르게, Layout 컴포넌트는 `searchParams`  prop을 받지 않습니다. 이는 [공통된 레이아웃이 탐색 중에 재 렌더링 되지 않기 때문에](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Routing.md#partial-rendering), 탐색 사이에 오래된  `searchParams` 유지될 수 있기 때문입니다. 

클라이언트 측 탐색을 사용할 경우 **Next.js** 는 자동으로 페이지의 일부를 두 경로 사이의 공통된 레이아웃 아래의 페이지 부분만 랜더링 합니다. 

예를 들어, 다음의 디렉토리 구조에서 `dashboard/layout.tsx` 는 `/dashboard` 와 `/dashboard/settings` 의  공통 레이아웃입니다.

<br>

```bash
app
└── dashboard
        ├── layout.tsx
        ├── page.tsx
        └── settings
                └── page.tsx
```

<br>

`/dashboard`에서 `/dashboard/settings` 로 탐색할 때,`dashboard/settings/page.tsx`는 세그먼트가 변경되었기 때문에 서버에서 렌더링 됩니다. 반면에 `dashboard/layout.tsx`는 두 경로 간의 공통 레이아웃 이기 때문에 재 랜더링 되지 않습니다.

이러한 성능 최적화를 사용하면 레이아웃을 공유하는 페이지 간의 탐색을 빠르게 만듭니다. 
이는 공통된 레이아웃을 포함하는 전체 경로를 실행할 필요 없이 페이지의 데이터 가져오기와 렌더링만 실행하면 되기 때문에 해당 페이지만 렌더링되어 더 효율적이고 빠른 탐색이 가능해집니다. 

`dashboard/layout.tsx` 는 재 랜더링 하지 않기 때문에 레이아웃 서버 컴포넌트에서 searchParams props를 읽으면 탐색할 때 상태가 지속됩니다. ( 최신 정보가 아닌 오래된 정보를 사용할 가능성이 있다는 의미입니다. )

- 대신, 클라이언트 컴포넌트에서 Page의 `searchParams` prop이나 `useSearchParams` 훅을 사용하십시오. 이는 클라이언트에서 최신 `searchParams`와 함께  재 렌더링됩니다.

---

## Root Layouts

- `app` 디렉토리는 `app/layout.js` 경로를 포함해야 합니다.
- root layout 은 `<html>`과 `<body>` 태그를 정의해야 합니다.
    - root layout에 `<title>` 및 `<meta>` 와 같은 `<head>` 태그를 수동적으로 추가할 수 없습니다. 대신에, `<head>` 요소의 스트리밍 및 중복 처리 같은 advanced 요구 사항을 자동적으로 다뤄주는 **Metadata API**를 사용해야 합니다.
- 여러 root layout을 생성하기 위해 **route groups**를 사용할 수 있습니다.
    - **여러 root layout 사이에서 Navigating** 은 **full page load** (client-side navigation과는 반대로)를 일으킬 수 있습니다.  예를 들어 `app/(shop)/layout.js` 를 사용하는 `/cart`에서 `app/(marketing)/layout.js`를 사용하는 `/blog` 로의 navigating은 full page load를 야기합니다. 이것은 **오직** 여러 root layout 에만 적용됩니다.