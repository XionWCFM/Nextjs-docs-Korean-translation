# **revalidateTag**

`revalidateTag` 를 사용하면 특정 캐시 태그와 관련된 데이터를 재검증할 수 있습니다. 이 기능은 재검증 기간이 만료될 때까지 기다리지 않고 캐시된 데이터를 업데이트하려는 시나리오에 유용합니다.

```jsx
// app/api/revalidate/route.js

import { NextResponse, revalidateTag } from 'next/server';

export async function GET(request) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

> 알아두면 유용합니다:
> 
> - `revalidateTag` 는  [Node.js and Edge runtimes](https://nextjs.org/docs/pages/building-your-application/rendering/edge-and-nodejs-runtimes) 모두에서 사용할 수 있습니다.

## **[Parameters](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#parameters)**

```jsx
revalidateTag(tag: string): void;
```

- `tag`: 재검증하려는 데이터와 연결된 캐시 태그를 나타내는 문자열입니다.

다음과 같이 `fetch` 에 태그를 추가할 수 있습니다 :

```jsx
fetch(url, { next: { tags: [...] } });
```

---

## **[Returns](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#returns)**

`revalidateTag` 는 어떠한 값도 반환하지 않습니다.

## **[Examples](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#examples)**

### **[Node.js Runtime](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#nodejs-runtime)**

```jsx
// app/api/revalidate/route.js

import { NextResponse } from 'next/server';
import { revalidateTag } from 'next/cache';

export async function GET(request) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

### **[Edge Runtime](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#edge-runtime)**

```jsx
// app/api/revalidate/route.js

import { NextResponse } from 'next/server';
import { revalidateTag } from 'next/cache';

export const runtime = 'edge';

export async function GET(request) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```