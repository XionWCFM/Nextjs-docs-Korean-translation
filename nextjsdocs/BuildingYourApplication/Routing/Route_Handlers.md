https://beta.nextjs.org/docs/routing/route-handlers
위 문서에 대한 번역을 진행합니다.

번역시점은 2023-05-07으로 공식문서의 추가적인 업데이트가 있을 수 있습니다.

번역기와 챗지피티에 의존해서 번역하고있습니다.

번역체를 자연스러운 어투로 옮기는 과정에서 오역이 발생할 수 있는 점 미리 알립니다.


# Route Handlers

라우트 핸들러를 사용하면 웹 요청 및 `response API`를 이용하여 지정된 라우트에 대한 `custom request handlers`를 만들 수 있습니다.


## Good to know
라우트 핸들러는 앱 디렉토리 내에서만 사용할 수 있습니다.
라우트 핸들러는 페이지 디렉토리 내의 API 경로와 동일하므로 API 경로와 라우트 핸들러를 함께 사용할 필요가 없습니다.
Convention
라우트 핸들러는 앱 디렉토리 내의 route.js|ts 파일에 정의됩니다.

`app/api/route.ts`

```js
export async function GET(request: Request) {}
```

`Route Handlers` 는 `page.js` `layout.js`와 비슷하게 앱 디렉토리 내에서 중첩될 수 있습니다.

하지만 `page.js`와 동일한 경로 세그먼트 수준에는 `route.js` 파일이 존재할 수 없습니다.

 

## Supported HTTP Methods
지원되는 HTTP 메서드는 다음과 같습니다.

**GET, POST, PUT, PATCH, DELETE , HEAD, OPTIONS**

지원되지 않는 메서드가 호출되는 경우 Next.js는 405 에러를 반환합니다. (메서드 허용되지 않음)

 

## Extended NextRequest and NextResponse APIs
기본 요청 및 응답을 지원할 뿐만 아니라 Next.js는 advanced use cases를 위한 편리한 helpers를 제공하기 위해 NextRequest, NextResponse로 이를 확장합니다.

 

# Behavior
## Static Route Handlers

`Route Handlers`는 기본적으로 응답 객체와 함께 GET 메서드를 사용할 때 정적으로 평가됩니다.

 
```ts
app/items/route.ts

import { NextResponse } from 'next/server';

export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  });
  const data = await res.json();

  return NextResponse.json({ data })
}
```

## TypeScript Warning
Response.json()이 유효하지만 native typescript types는 오류를 표시할 수 있습니다.
이런 경우 당신은 typed responses를 위해 NextResponse.json()을 사용할 수 있습니다.
static fetching은 robots.txt , ress.xml , sitemap.xml 및 기타 경로에 대한 custom route handlers를 생성하는데 유용할 수 있습니다. 예시를 참고하세요

## Dynamic Route Handlers
 

Route Handlers는 다음과 같은 경우에 동적으로 평가됩니다.
 

- Get 메서드와 함께 Request Object를 사용하는 경우

- 다른 HTTP 메서드를 사용하는 경우

- 쿠키, 헤더와 같은 동적 함수를 사용하는 경우

- Segment Config Options의 Dynamic mode를 개발자가 직접 지정해준 경우

예를 들면

```ts
app/products/api/route.ts

import { NextResponse } from 'next/server';

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const id = searchParams.get('id');
  const res = await fetch(`https://data.mongodb-api.com/product/${id}`, {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  });
  const product = await res.json();

  return NextResponse.json({ product })
}
```
마찬가지로 POST 메서드를 사용하면 라우트 핸들러가 동적으로 평가됩니다.

```ts
app/items/route.ts

import { NextResponse } from 'next/server';

export async function POST() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
    body: JSON.stringify({ time: new Date().toISOString() }),
  });

  const data = await res.json();

  return NextResponse.json(data);
}
```
## Note
이전에는 양식 제출 처리와 같은 사용 사례에서 API Routes를 사용할 수 있었습니다.
라우트 핸들러는 이러한 사용사례에 적합한 솔루션이 아닐 수 있기때문에
준비가 되면 이를 위한 변형을 사용할 것을 권장할 예정입니다.
 

## Route Resolution
- 경로를 가장 낮은 수준의 라우팅 프리미티브로 간주할 수 있습니다.

- 페이지와 같은 레이아웃이나 클라이언트측 탐색에 참여하지 않습니다.

- `page.js`와 같은 경로에 `route.js` 파일이 있을 수 없습니다.

| Page               | Route               | Result   |
| ------------------| ------------------- | -------- |
| app/page.js       | app/route.js        | Conflict |
| app/page.js       | app/api/route.js    | Valid    |
| app/[user]/page.js | app/api/route.js    | Valid    |

각 `route.js` 혹은 `page.js` 파일은 해당 경로에 대한 모든 HTTP verbs를 대신합니다.

```jk
app/page.js
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}

// ❌ Conflict
// `app/route.js`
export async function POST(request) {}
```
 

# Examples
 
다음 예제는 라우트 핸들러를 다른 next.js API 및 기능과 결합하는 방법을 보여줍니다.

 

## Revalidating Static Data 
다음 revalidate 옵션을 사용하여 정적 데이터 가져오기의 유효성을 재검증할 수 있습니다.

```ts
app/items/route.ts

import { NextResponse } from 'next/server';

export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    next: { revalidate: 60 } // Revalidate every 60 seconds
  });
  const data = await res.json();

  return NextResponse.json(data)
}
또는 세그먼트 구성 재검증 옵션을 사용할 수도 있습니다.

export const revalidate = 60
```

## Dynamic Functions
Route handlers 쿠키 및 헤더와 같은 next.js의 동적 함수와 함께 사용할 수 있습니다.

 

### Cookies
next/headers의 cookies로 쿠키를 읽을 수 있습니다.

이 서버 함수는 라우트 핸들러에서 직접 호출하거나 다른 함수 내부에 중첩할 수 있습니다.

이 cookies 인스턴스는 읽기 전용입니다.

쿠키를 설정하려면 Set-Cookie header를 사용하여 새 응답을 반환시켜야합니다.

```ts
app/api/route.ts

import { cookies } from 'next/headers'

export async function GET(request: Request) {
  const cookieStore = cookies();
  const token = cookieStore.get('token');

  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token}` }
  });
}
```

또는 기본 웹 API 위에 추상화를 사용하여 쿠키를 읽을 수 있습니다.(NextRequest)

```ts
app/api/route.ts

import { type NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const token = request.cookies.get('token')
}
 ```

## Headers
next/headers에서 header로 헤더를 읽을 수 있습니다. 이 서버 함수는 라우트 핸들러에서 직접 호출하거나 다른 함수 내부에 중첩할 수 있습니다.

이 headers 인스턴스는 read-only 입니다. 헤더를 설정하려면 새 헤더가 포함된 새 응답을 return 해야합니다.
```ts
import { headers } from 'next/headers';

export async function GET(request: Request) {
  const headersList = headers();
  const referer = headersList.get('referer');

  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'referer': referer }
  });
}
```
또는 기본 웹 API위에 추상화를 사용하여 헤더를 읽을 수 있습니다.(NextRequest)
```ts
import { type NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const requestHeaders = new Headers(request.headers)
}
```


## Redirects
```ts
import { redirect } from 'next/navigation';

export async function GET(request: Request) {
  redirect('https://nextjs.org/')
}
 ```

## Dynamic Route Segments
계속하기 전에 경로 정의 페이지를 읽어보시기 바랍니다.

라우트 핸들러는 동적 세그먼트를 사용하여 동적 데이터에서 요청 핸들러를 생성할 수 있습니다.

```ts
app/items/[slug]/route.js

export async function GET(request: Request, { params }: {
  params: { slug: string }
}) {
  const slug = params.slug; // 'a', 'b', or 'c'
}
```

| Route                | Example URL       | params         |
| --------------------| ----------------- | -------------- |
| app/items/[slug]/route.js | /items/a     | {slug:'a'}     |
| app/items/[slug]/route.js | /items/b     | {slug:'b'}     |
| app/items/[slug]/routes   | /items/c     | {slug:'c'}     |


## Note
`GenerateStaticParams()` 사용은 아직 라우트 핸들러와 함께 지원되지 않습니다.
 

# Streaming
```ts
app/api/route.ts

// https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream#convert_async_iterator_to_stream
function iteratorToStream(iterator: any) {
  return new ReadableStream({
    async pull(controller) {
      const { value, done } = await iterator.next()

      if (done) {
        controller.close()
      } else {
        controller.enqueue(value)
      }
    },
  })
}

function sleep(time: number) {
  return new Promise((resolve) => {
    setTimeout(resolve, time)
  })
}

const encoder = new TextEncoder()

async function* makeIterator() {
  yield encoder.encode('<p>One</p>')
  await sleep(200)
  yield encoder.encode('<p>Two</p>')
  await sleep(200)
  yield encoder.encode('<p>Three</p>')
}

export async function GET() {
  const iterator = makeIterator()
  const stream = iteratorToStream(iterator)

  return new Response(stream)
}
``` 

# Request Body
표준 web API 메서드를 사용하여 request body를 읽을 수 있습니다.
```ts
app/items/route.ts

import { NextResponse } from 'next/server';

export async function POST(request: Request) {
  const res = await request.json();
  return NextResponse.json({ res })
}

CORS
표준 Web API 메서드를 사용하여 응답에 CORS 헤더를 설정 할 수 있습니다.

app/api/route.ts

export async function GET(request: Request) {
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
    }
  });
}
``` 

# Edge and Node.js Runtimes
라우트 핸들러에는 스트리밍 지원을 포함해 Edge 및 Node.js 런타임을 원활하게 지원하는 Isomorphic(동형) 웹 API가 있어 Edge와 Node.js 런타임을 모두 원활하게 지원합니다.

라우트 핸들러는 페이지 및 레이아웃과 동일한 라우트 세그먼트 구성을 사용하기 때문에 범용 정적으로 재생성된 라우트 핸들러와 같이 오랫동안 기다려온 기능을 지원합니다.

runtime segment config option을 사용하여 런타임을 지정할 수 있습니다.

```js
export const runtime = 'edge'; // 'nodejs' is the default
 ```

# Non-UI Responses
라우트 핸들러를 사용하여 UI가 아닌 콘텐츠를 반환할 수 있습니다.

sitemap.sxml , robots.txt , favicon.ico, and open graph images 에는 모두 SEO 지원이 내장되어있습니다.

 
```ts
app/rss.xml/route.ts

export async function GET() {
  return new Response(`<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

<channel>
  <title>Next.js Documentation</title>
  <link>https://nextjs.org/docs</link>
  <description>The React Framework for the Web</description>
</channel>

</rss>`);
}
 ```

# Segment Config Options
라우트핸들러는 페이지 및 레이아웃과 동일한 라우트 세그먼트 구성을 사용합니다.


```ts
app/items/route.ts

export const dynamic = 'auto'
export const dynamicParams = true
export const revalidate = false
export const fetchCache = 'auto'
export const runtime = 'nodejs'
export const preferredRegion = 'auto'
```