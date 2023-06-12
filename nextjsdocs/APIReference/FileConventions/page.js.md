# page

<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/page</p>

page 는 경로에 대한 고유한 UI 입니다. 
> page는 특정 경로에 대한 유일한 화면으로 Next.js에서 페이지를 라우트에 매하여 페이지를 생성할 때 사용하는 개념입니다.
>

```jsx
//app/blog/[slug]/page.js

export default function Page({ params, searchParams }) {
  return <h1>My Page</h1>
}
```

---

<br>

### Props

<br>

#### `params` (optional)

루트 세그먼트(root segment) 부터 해당 페이지까지의 [**동적 경로 매개변수 (dynamic route parameters )**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Dynamic_Routes.md) 를 포함하는 객체입니다. 

> Next.js에서 라우트가 동적 경로 매개변수를 사용할 때 이를 파싱하여 객체로 반환해주는 개념입니다. 이렇게 반환된 객체를 통해 동적 경로 매개 변수에 접근할 수 있습니다.
> 

예시:

| Example | URL | params |
| --- | --- | --- |
|  app/shop/[slug]/page.js | /shop/1 | { slug:  ‘1’  } |
| app/shop/[category]/[item]/page.js | /shop/1/2 | { category: ‘1’,  item: ‘2’  } |
| app/shop/[…slug]/page.js | /shop/1/2 | { slug: [ ’1’, ‘2’ ] } |

<br>

#### `searchParams` (optional)

현재 URL의 검색 매개변수[(search parameters)](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#parameters)를 포함하는 객체입니다. 


예시:

| URL | searchParams |
| --- | --- |
| /shop?a=1 | { a: ‘1’ } |
| /shop?a=1&b=2 | { a: ‘1’, b: ‘2’ } |
| /shop?a=1&a=2 | { a: [ ’1’, ‘2’ ] } |

##### Good to know

- `searchParams` 는 사전에 알 수 없는 값들로 구성되어 있다  [**동적 API ( Dynamic API )** ](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md)입니다.  이를 사용하면 페이지가 요청 시 [**동적 랜더링( dynamic rendering)**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Rendering/Static_and_Dynamic.md)을 수행하도록 설정됩니다.

> 페이지가 로드되기 전에는 그 안에 들어갈 값들이 미리 결정되어 있지 않다는 의미로 페이지가 요청 될 때마다 동적으로 결정됩니다. ‘searchParams’는 URL의 검색 매개변수를 담고 있는 객체로 URL에 따라 값이 동적으로 변합니다.
> 
- **`searchParams`** 는 **`URLSearchParams`** 인스턴스가 아닌 일반적인 JavaScript 객체를 반환합니다.

> URLSearchParams 객체의 메서드는 사용할 수 없지만 일반 객체처럼 속성값을 읽을 수 있습니다.
>