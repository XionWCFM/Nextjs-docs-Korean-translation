# Draft_Mode

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/draft-mode](https://nextjs.org/docs/app/building-your-application/configuring/draft-mode)

정적 렌더링은 페이지가 헤드리스 CMS에서 데이터를 가져오는 경우 유용합니다. 그러나 헤드리스 CMS에서 초안을 작성하고 해당 초안을 즉시 페이지에서 확인하려는 경우에는 이상적이지 않습니다. 이러한 경우에는 Next.js가 페이지를 빌드 시간이 아닌 **요청 시간**에 렌더링하고 게시된 내용 대신 초안 내용을 가져오도록 해야 합니다. 특정한 경우에만 Next.js가 [동적 렌더링](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic#dynamic-rendering)으로 전환되도록 하고 싶을 것입니다.

Next.js에는 이러한 문제를 해결하는 "**Draft Mode(초안 모드)**"라는 기능이 있습니다. 이를 사용하는 방법에 대한 지침은 다음과 같습니다.

## **1단계: Route Handler 생성 및 접근하기**

먼저, [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)를 생성하세요. 이 Route Handler는 어떤 이름으로든 생성할 수 있습니다. 예를 들어 `app/api/draft/route.ts`와 같이 생성할 수 있습니다.

그런 다음, `next/headers`에서 `draftMode`를 import하고 `enable()` 메서드를 호출하세요.

app/api/draft/route.ts

```tsx
// route handler enabling draft mode
import { draftMode } from 'next/headers'
 
export async function GET(request: Request) {
  draftMode().enable()
  return new Response('Draft mode is enabled')
}
```

이렇게 하면 Draft Mode를 활성화하는 **쿠키**가 설정됩니다. 이 쿠키를 포함한 후속 요청은 정적으로 생성된 페이지의 동작을 변경하여 **Draft Mode**를 트리거합니다 (이에 대한 자세한 내용은 이후에 설명하겠습니다).

브라우저의 개발자 도구를 사용하여 수동으로 테스트할 수 있습니다. `/api/draft`로 이동하여 브라우저의 개발자 도구를 확인하면, `__prerender_bypass`라는 이름의 쿠키를 포함한 `Set-Cookie` 응답 헤더가 보이게 됩니다.

### **헤드리스 CMS에서 안전하게 접근하기**

실제로는 이 Route Handler를 헤드리스 CMS에서 *안전하게* 호출하려고 할 것입니다. 구체적인 단계는 사용하는 헤드리스 CMS에 따라 다르지만, 다음은 일반적인 절차 중 일부입니다.

이 단계들은 사용하는 헤드리스 CMS가 **사용자 정의 초안 URL**을 설정하는 것을 지원하는 것으로 가정합니다. 그렇지 않은 경우에도 이 방법을 사용하여 초안 URL을 안전하게 보호할 수 있지만, 초안 URL을 직접 생성하고 액세스해야 할 수도 있습니다.

**첫째**, 선택한 토큰 생성기를 사용하여 **비밀 토큰 문자열**을 생성해야 합니다. 이 비밀은 Next.js 앱과 헤드리스 CMS만이 알고 있는 비밀입니다. 이 비밀은 CMS에 액세스 권한이 없는 사람들이 초안 URL에 접근하는 것을 방지합니다.

**둘째**, 헤드리스 CMS가 사용자 정의 초안 URL을 설정하는 것을 지원한다면, 다음과 같이 초안 URL로 지정합니다. 이는 Route Handler가 `app/api/draft/route.ts`에 위치한다고 가정합니다.

Terminal

```
https://<your-site>/api/draft?secret=<token>&slug=<path>
```

- `<your-site>`는 배포 도메인을 나타내야 합니다.
- `<token>`은 생성한 비밀 토큰으로 대체되어야 합니다.
- `<path>`는 보고자 하는 페이지의 경로여야 합니다. 만약 `/posts/foo`를 보려면 `&slug=/posts/foo`와 같이 사용해야 합니다.

당신의 헤드리스 CMS는 `<path>`를 CMS 데이터를 기반으로 동적으로 설정할 수 있는 변수를 포함할 수 있도록 허용할 수 있습니다. 예를 들어 다음과 같이 사용할 수 있습니다: `&slug=/posts/{entry.fields.slug}`

**마지막으로**, Route Handler에서:

- 비밀 토큰이 일치하는지와 `slug` 매개변수가 있는지 확인합니다 (없는 경우 요청이 실패해야 함).
- `draftMode.enable()`를 호출하여 쿠키를 설정합니다.
- 그런 다음 브라우저를 `slug`로 지정된 경로로 리디렉션합니다.

app/api/draft/route.ts

```tsx
// route handler with secret and slug
import { draftMode } from 'next/headers'
import { redirect } from 'next/navigation'
 
export async function GET(request: Request) {
  // Parse query string parameters
  const { searchParams } = new URL(request.url)
  const secret = searchParams.get('secret')
  const slug = searchParams.get('slug')
 
  // Check the secret and next parameters
  // This secret should only be known to this route handler and the CMS
  if (secret !== 'MY_SECRET_TOKEN' || !slug) {
    return new Response('Invalid token', { status: 401 })
  }
 
  // Fetch the headless CMS to check if the provided `slug` exists
  // getPostBySlug would implement the required fetching logic to the headless CMS
  const post = await getPostBySlug(slug)
 
  // If the slug doesn't exist prevent draft mode from being enabled
  if (!post) {
    return new Response('Invalid slug', { status: 401 })
  }
 
  // Enable Draft Mode by setting the cookie
  draftMode().enable()
 
  // Redirect to the path from the fetched post
  // We don't redirect to searchParams.slug as that might lead to open redirect vulnerabilities
  redirect(post.slug)
}
```

성공하면 브라우저는 초안 모드 쿠키와 함께 보고자 하는 경로로 리디렉션될 것입니다.

## **2단계: 페이지 업데이트**

다음 단계는 페이지를 업데이트하여 `draftMode().isEnabled`의 값을 확인하는 것입니다.

쿠키가 설정된 페이지를 요청하면 데이터가 **빌드 시간이 아닌 요청 시간**에 가져올 것입니다.

게다가, `isEnabled`의 값은 `true`가 될 것입니다.

app/page.tsx

```tsx
// page that fetches data
import { draftMode } from 'next/headers'
 
async function getData() {
  const { isEnabled } = draftMode()
 
  const url = isEnabled
    ? 'https://draft.example.com'
    : 'https://production.example.com'
 
  const res = await fetch(url)
 
  return res.json()
}
 
export default async function Page() {
  const { title, desc } = await getData()
 
  return (
    <main>
      <h1>{title}</h1>
      <p>{desc}</p>
    </main>
  )
}
```

이게 전부입니다! 헤드리스 CMS에서 또는 수동으로 draft Route Handler(`secret` 그리고 `slug` 와 함께)에 접근하면 이제 초안 내용을 볼 수 있을 것입니다. 그리고 초안을 업데이트하고 게시하지 않았다면 초안을 확인할 수 있을 것입니다.

이 내용을 헤드리스 CMS에서 초안 URL로 설정하거나 수동으로 액세스하면 초안을 확인할 수 있을 것입니다.

Terminal

```
https://<your-site>/api/draft?secret=<token>&slug=<path>
```

## **자세한 내용**

### **Draft Mode 쿠키 지우기**

기본적으로 브라우저가 닫히면 Draft Mode 세션이 종료됩니다.

Draft Mode 쿠키를 수동으로 지우려면, `draftMode().disable()`을 호출하는 Route Handler를 생성하세요:

app/api/disable-draft/route.ts

```tsx
import { draftMode } from 'next/headers'
 
export async function GET(request: Request) {
  draftMode().disable()
  return new Response('Draft mode is disabled')
}
```

그런 다음 `/api/disable-draft`로 요청을 보내어 Route Handler를 호출하세요. 만약 이 라우트를 `[next/link](https://nextjs.org/docs/app/api-reference/components/link)`를 사용하여 호출하는 경우, 쿠키가 무작위로 삭제되는 것을 방지하기 위해 `prefetch={false}`를 전달해야 합니다.

### **`next build`마다 고유한 값**

`next build`를 실행할 때마다 새로운 우회 쿠키 값이 생성됩니다.

이렇게 하면 우회 쿠키를 추측할 수 없게 됩니다.

> 알아두면 좋은 정보: 로컬에서 HTTP를 통해 Draft Mode를 테스트하려면 브라우저에서 제3자 쿠키와 로컬 스토리지 액세스를 허용해야 합니다.
>