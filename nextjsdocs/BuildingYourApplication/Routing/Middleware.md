https://nextjs.org/docs/app/building-your-application/routing/middleware

위 문서에 대한 번역을 진행합니다.

번역시점은 05-09로 공식문서의 추가적인 업데이트가 있을 수 있습니다.

번역기와 챗지피티에 의존해서 번역하고있습니다.

번역체를 자연스러운 어투로 옮기는 과정에서 오역이 발생할 수 있는 점 미리 알립니다.

# Middleware
미들웨어를 사용하면 요청이 완료되기 전에 코드를 실행할 수 있습니다.

그런 다음 들어오는 요청에 따라 요청 또는 응답 헤더를 rewriting, redirecting, modifying 하거나 직접 응답하여 응답을 수정할 수도 있습니다.

미들웨어는 캐시된 콘텐츠와 경로가 일치하기 전에 실행됩니다.

자세한 내용은 Matching Paths를 참조하세요

 

# Convention
프로젝트의 루트에서 middleware.ts(혹은 js) 파일을 사용하여 미들웨어를 정의합니다.

예컨대 `pages` 혹은 `app`과 same level에 있는 경우 `src` 안에서 정의합니다.


# Example
```ts
middleware.ts

import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
 
// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url));
}
 
// See "Matching Paths" below to learn more
export const config = {
  matcher: '/about/:path*',
};
```
Matching Paths
프로젝트의 모든 route에 대해 미들웨어가 호출되며 실행 순서는 다음과 같습니다.

1. next.config.js의 headers

2. next.config.js의 redirects

3. 미들웨어

4. next.config.js의 beforeFiles(rewrites)

5. 파일 시스템 경로(public/, _next/static, pages/ app/ 등등)

6. next.config.js의 afterFiles(rewrites)

7. 동적 라우트 (/blog/[slug])

8. next.config.js의 fallback(rewrites)

 

다음은 미들웨어가 실행될 경로를 정의하는 두가지 방법입니다.

1. Custom matcher config

2. Conditional Statements

# Matcher

`matcher`를 사용하면 특정 경로에서 실행될 수 있도록 미들웨어를 필터링할 수 있습니다.

``` ts
export const config = {
  matcher: '/about/:path*',
};
```
`matcher config`는 전체 정규식 regex를 허용합니다. 따라서 negative lockheads 또는 character matching이 지원됩니다.

특정 경로를 제외한 모든 경로를 일치시키는 negative lockheads의 예는 여기에서 확인할 수 있습니다.

```ts
export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
};
```

## Note 
matcher의 value는 빌드시 정적으로 분석할 수 있는 상수여야합니다.

변수와 같은 동적인 값은 무시됩니다. (역주 : constants한 값이여야 한다고 합니다. )

 

# Configured matchers
1. 항상 /로 시작해야합니다.

2. named parameters를 가질 수 있습니다. /about/:path에서 /about/a , /about/b는 매칭되지만 /about/a/c는 매칭되지 않습니다.

3. 명명된 매개변수에 modifiers를 사용할 수 있습니다(starting with): /about/path* 는 *가 0 이상이기 때문에 /about/a/b/c와 매칭됩니다. 추가로 ?는 0이거나 1 이고 +는 1이거나 1 이상입니다.

4. 괄호로 묶인 정규식을 사용할 수 있습니다. /about/(.*) 은 /about/:path*와 동일합니다.

경로 정규식 문서에서 자세한 내용을 참고해보세요

 

## Note

이전 버전과의 호환성을 위해서 next.js는 항상 /public을 /public/index로 간주합니다.

따라서 /public/:는 매칭이 됩니다.

# Conditional Statements

```ts
middleware.ts

import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
 
export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith('/about')) {
    return NextResponse.rewrite(new URL('/about-2', request.url));
  }
 
  if (request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.rewrite(new URL('/dashboard/user', request.url));
  }
}
```
# NextResponse

`NextResponse API`는 다음과 같은 일을 수행할 수 있습니다.

- 들어오는 요청을 다른 URL로 redirct 합니다.

- 지정된 URL을 표시하여 응답을 rewrite합니다.

- API Routes, getServerSideProps, 재작성 대상에 대한 요청 헤더 설정

- response cookies 설정

- response headers 설정

미들웨어에서 응답을 생성하려면 다음과 같은 방법을 선택할 수 있습니다.

1. 응답을 생성하는 경로를 rewrite 합니다.

2. NextResponse를 직접 반환합니다.

# Using Cookies

쿠키는 일반 헤더입니다(regular header) request 시에 cookie 헤더에 저장됩니다.

response에서는 set-Cookie header에 저장이 됩니다.

Next.js는 `NextRquest` , `NextResponse`의 cookies extension을 통해 쿠키를 access 하거나 조작할 수 있는 편리한 방법들을 제공해줍니다.

 
<br>

들어오는 요청의 경우(incoming request) 쿠키에는 다음과 같은 메서드가 제공됩니다.

**get, getAll , set, delete | Cookies** 

또한 당신은 has를 사용하여 쿠키의 존재 여부를 확인하거나 clear를 사용하여 모든 쿠키를 제거하는 것도 가능합니다.

<br>

발신 응답의 경우(outgoing responses) 쿠키에는 다음과 같은 메서드가 제공됩니다.

**get, getAll , set, delete**

```ts
middleware.ts

import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
 
export function middleware(request: NextRequest) {
  // Assume a "Cookie:nextjs=fast" header to be present on the incoming request
  // Getting cookies from the request using the `RequestCookies` API
  let cookie = request.cookies.get('nextjs')?.value;
  console.log(cookie); // => 'fast'
  const allCookies = request.cookies.getAll();
  console.log(allCookies); // => [{ name: 'nextjs', value: 'fast' }]
 
  request.cookies.has('nextjs'); // => true
  request.cookies.delete('nextjs');
  request.cookies.has('nextjs'); // => false
 
  // Setting cookies on the response using the `ResponseCookies` API
  const response = NextResponse.next();
  response.cookies.set('vercel', 'fast');
  response.cookies.set({
    name: 'vercel',
    value: 'fast',
    path: '/test',
  });
  cookie = response.cookies.get('vercel');
  console.log(cookie); // => { name: 'vercel', value: 'fast', Path: '/test' }
  // The outgoing response will have a `Set-Cookie:vercel=fast;path=/test` header.
 
  return response;
}
```

# Setting Headers

`NextResponse API`를 사용하여 request, response 헤더를 설정할 수 있습니다.

(reqeust header 설정은 next.js 버전 13.0.0부터 가능합니다.)

```ts
middleware.ts


import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
 
export function middleware(request: NextRequest) {
  // Clone the request headers and set a new header `x-hello-from-middleware1`
  const requestHeaders = new Headers(request.headers);
  requestHeaders.set('x-hello-from-middleware1', 'hello');
 
  // You can also set request headers in NextResponse.rewrite
  const response = NextResponse.next({
    request: {
      // New request headers
      headers: requestHeaders,
    },
  });
 
  // Set a new response header `x-hello-from-middleware2`
  response.headers.set('x-hello-from-middleware2', 'hello');
  return response;
}
```

## Note
백엔드 웹 서버 구성에 따라 431 요청 헤더 필드가 너무 클 수 있습니다. 

오류가 발생할 수 있으니 큰 헤더를 설정하지 마십시오

# Producing a Response
 

미들웨어에서 `Response` 또는 `NextResponse` 인스턴스를 반환하여 직접 응답할 수 있습니다.

(이 기능은 Next.js v 13.1.0부터 사용 가능합니다.)

 
```ts
middleware.ts

import { NextRequest, NextResponse } from 'next/server';
import { isAuthenticated } from '@lib/auth';
 
// Limit the middleware to paths starting with `/api/`
export const config = {
  matcher: '/api/:function*',
};
 
export function middleware(request: NextRequest) {
  // Call our authentication function to check the request
  if (!isAuthenticated(request)) {
    // Respond with JSON indicating an error message
    return new NextResponse(
      JSON.stringify({ success: false, message: 'authentication failed' }),
      { status: 401, headers: { 'content-type': 'application/json' } },
    );
  }
}
```
# Advanced Middleware Flags
Next.js v13.1에서는 고급 사용 사례를 처리하기 위해 미들웨어에 대한 두 가지 추가 플러그인인

`skipMiddlewareUrlNormalize` 및 `skipTrailingSlashRedirect`가 도입되었습니다.

`skipTrailingSlashRedirect`를 사용하면 후행 슬래시를 추가하거나 제거하기 위한 Next.js 기본 리디렉션을 비활성화하여 미들웨어 내부에서 사용자 지정 처리를 할 수 있으므로 일부 경로에는 후행 슬래시를 유지하지만 다른 경로에는 유지하지 않아 incremental migrations을 쉽게 할 수 있습니다.

```js
next.config.js

module.exports = {
  skipTrailingSlashRedirect: true,
};
middleware.js

const legacyPrefixes = ['/docs', '/blog'];
 
export default async function middleware(req) {
  const { pathname } = req.nextUrl;
 
  if (legacyPrefixes.some((prefix) => pathname.startsWith(prefix))) {
    return NextResponse.next();
  }
 
  // apply trailing slash handling
  if (
    !pathname.endsWith('/') &&
    !pathname.match(/((?!\.well-known(?:\/.*)?)(?:[^/]+\/)*[^/]+\.\w+)/)
  ) {
    req.nextUrl.pathname += '/';
    return NextResponse.redirect(req.nextUrl);
  }
}
```
`skipMiddlewareUrlNormalize`를 사용하면 Next.js가 수행하는 URL 정규화를 비활성화하여 직접 방문과 클라이언트 전환을 동일하게 처리할 수 있습니다.

원본 URL을 사용하여 완전한 제어가 필요한 고급 사례가 있는데, 이 기능을 사용하면 잠금이 해제됩니다.

```js
next.config.js

module.exports = {
  skipMiddlewareUrlNormalize: true,
};
middleware.js

export default async function middleware(req) {
  const { pathname } = req.nextUrl;
 
  // GET /_next/data/build-id/hello.json
 
  console.log(pathname);
  // with the flag this now /_next/data/build-id/hello.json
  // without the flag this would be normalized to /hello
}
```