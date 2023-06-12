<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/route</p>

# route.js

Route Handlers 는  [Web Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) 와 [Response API](https://developer.mozilla.org/en-US/docs/Web/API/Response)를 사용하여 특정 경로(route)에 대해서 사용자 정의 요청 핸들러를 생성할 수 있도록 합니다. 

---

## HTTP Methods

route 파일은 특정 경로(route) 에 대해서 사용자 정의 핸들러를 생성할 수 있도록 합니다. 다음과 같은 [HTTP 메서드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)가 지원됩니다. : `GET` , `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`

```jsx
//app/api/route.ts

export async function GET(request: Request) {}

export async function HEAD(request: Request) {}

export async function POST(request: Request) {}

export async function PUT(request: Request) {}

export async function DELETE(request: Request) {}

export async function PATCH(request: Request) {}

//If 'OPTIONS' is not defined, Next.js will automatically implement 'OPTION' and
//set the appropriate Response 'Allow' header depending on the other methods defined
//in the route handler.

export async function OPTIONS(request: Request) {}
```


##### 알아두면 좋은 정보
Route Handlers는 app 디렉토리 안에서만 가능합니다. Route Handlers 는 모든 use case를 다룰 수 있기 때문에 API Routes(**pages**) 와 Route Handlers(**app**) 를 함께 사용할 필요는 없습니다. 

---

## Parameters

<br>

### `request` (optional)

`request` 객체는 **Web Request API**의 확장인 [**NextRequest**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/NextRequest.md) 객체입니다. 이 객체를 사용하면 들어오는 요청에 대해 더욱 세밀하게 제어할 수 있게 해주는데 이 `NextRequest` 객체를 통해 쿠키(`cookie`)와 파싱된 URL 객체(`nextURl`)에 쉽게 접근할 수 있습니다. 

<br>

### `context` (optional)

```jsx
//app/dashboard/[team]/route.js

export async function GET(request, context: { params }){
	const team = params.team; //'1'
}
```

현재 **`context`**의 유일한 값은 **`params`**이며, 현재 경로(route)의 **동적 라우트 매개변수**를 포함하는 객체입니다. 

| Example | URL | params |
| --- | --- | --- |
| app/dashboard/[team]/route.js | /dashboard/1 | { team: ‘1’ } |
| app/shop/[tag]/[item]/route.js | /shop/1/2 | { tag: ‘1’, item: ‘2’ } |
| app/blog/[…slug]/route.js | /blog/1/2 | { slug: [ ‘1’, ‘2’ ] } |

---

## NextResponse

Route Handlers는 `NextResponse` 객체를 반환함으로써 Web Response API를 확장할 수 있습니다. 이를 통해 쿠키 설정, 헤더 설정, 리다이렉트 및 rewrite (리라이트) 를 쉽게 수행할 수 있습니다. [API reference](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/APIReference/Functions/NextResponse.md)를 참고해보세요. 