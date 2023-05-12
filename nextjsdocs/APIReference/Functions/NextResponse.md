원본 링크: [https://nextjs.org/docs/app/api-reference/functions/next-response](https://nextjs.org/docs/app/api-reference/functions/next-response)
# **NextResponse**

NextResponse는 추가적인 편의 메서드를 통해 [Web Response API](https://developer.mozilla.org/en-US/docs/Web/API/Response) 를 확장합니다.

---

## **[cookies](https://nextjs.org/docs/app/api-reference/functions/next-response#cookies)**

응답의 [Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) 헤더를 읽거나 변경합니다.

### **[set(name, value)](https://nextjs.org/docs/app/api-reference/functions/next-response#setname-value)**

이름이 지정되면 응답에 지정된 값으로 쿠키를 설정합니다.

```jsx
// Given incoming request /home
let response = NextResponse.next();
// Set a cookie to hide the banner
response.cookies.set('show-banner', 'false');
// Response will have a `Set-Cookie:show-banner=false;path=/home` header
return response;
```

### **[get(name)](https://nextjs.org/docs/app/api-reference/functions/next-response#getname)**

쿠키 이름이 주어지면 쿠키의 값을 반환합니다. 쿠키를 찾을 수 없으면 `undefined` 가 반환됩니다. 여러 개의 쿠키가 발견되면 첫 번째 쿠키가 반환됩니다.

```jsx
// Given incoming request /home
let response = NextResponse.next();
// { name: 'show-banner', value: 'false', Path: '/home' }
response.cookies.get('show-banner');
```

### **[getAll()](https://nextjs.org/docs/app/api-reference/functions/next-response#getall)**

쿠키 이름이 주어지면 쿠키의 값을 반환합니다. 이름이 지정되지 않으면 응답에 있는 모든 쿠키를 반환합니다.

```jsx
// Given incoming request /home
let response = NextResponse.next();
// [
//   { name: 'experiments', value: 'new-pricing-page', Path: '/home' },
//   { name: 'experiments', value: 'winter-launch', Path: '/home' },
// ]
response.cookies.getAll('experiments');
// Alternatively, get all cookies for the response
response.cookies.getAll();
```

### **[delete(name)](https://nextjs.org/docs/app/api-reference/functions/next-response#deletename)**

쿠키 이름이 주어지면 응답에서 쿠키를 삭제합니다.

```jsx
// Given incoming request /home
let response = NextResponse.next();
// Returns true for deleted, false is nothing is deleted
response.cookies.delete('experiments');
```

---

## **[json()](https://nextjs.org/docs/app/api-reference/functions/next-response#json)**

주어진 JSON 본문으로 응답을 생성합니다.

app/api/route.js

```jsx
import { NextResponse } from 'next/server'; 

export async function GET(request) {
  return NextResponse.json({ hello: 'Next.js' });
}
```

---

## **[redirect()](https://nextjs.org/docs/app/api-reference/functions/next-response#redirect)**

[URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)로 리디렉션되는 응답을 생성합니다.

```jsx
import { NextResponse } from 'next/server'; 

return NextResponse.redirect(new URL('/new', request.url));
```

[URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)은 `NextResponse.redirect()` 메서드에서 사용하기 전에 생성 및 수정할 수 있습니다. 예를 들어 `request.nextUrl` 속성을 사용하여 현재 URL을 가져온 다음 다른 URL로 리디렉션하도록 수정할 수 있습니다.

```jsx
import { NextResponse } from 'next/server';
 
// Given an incoming request...
const loginUrl = new URL('/login', request.url);
// Add ?from=/incoming-url to the /login URL
loginUrl.searchParams.set('from', request.nextUrl.pathname);
// And redirect to the new URL
return NextResponse.redirect(loginUrl);
```

---

## **[rewrite()](https://nextjs.org/docs/app/api-reference/functions/next-response#rewrite)**

원본 URL 표시를 유지하면서 주어진 [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)을 재작성(프록시)하는 응답을 생성합니다.

```jsx
import { NextResponse } from 'next/server';
 
// Incoming request: /about, browser shows /about
// Rewritten request: /proxy, browser shows /about
return NextResponse.rewrite(new URL('/proxy', request.url));
```

---

## **[next()](https://nextjs.org/docs/app/api-reference/functions/next-response#next)**

`next()` 메서드는 조기에 반환하고 라우팅을 계속할 수 있으므로 미들웨어에 유용합니다.

```jsx
import { NextResponse } from 'next/server'; 

return NextResponse.next();
```

응답을 생성할 때 `headers` 를 전달할 수도 있습니다 :

```jsx
import { NextResponse } from 'next/server';
 
// Given an incoming request...
const newHeaders = new Headers(request.headers);
// Add a new header
newHeaders.set('x-version', '123');
// And produce a response with the new headers
return NextResponse.next({
  request: {
    // New request headers
    headers: newHeaders,
  },
});
```