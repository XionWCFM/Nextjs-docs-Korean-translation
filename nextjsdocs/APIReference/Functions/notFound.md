원본 링크: [https://nextjs.org/docs/app/api-reference/functions/not-found](https://nextjs.org/docs/app/api-reference/functions/not-found)

# **notFound**

`notFound` 함수를 사용하면 경로 세그먼트 내에서 `[not-found file](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)` 을 렌더링할 수 있을 뿐만 아니라  `<meta name="robots" content="noindex" />` 태그를 삽입할 수도 있습니다.

---

## **`[notFound()](https://nextjs.org/docs/app/api-reference/functions/not-found#notfound)`**

`notFound()` 함수를 호출하면 `NEXT_NOT_FOUND` 에러가 발생하고 에러가 발생한 경로 세그먼트의 렌더링이 종료됩니다.  **[not-found** file](https://nextjs.org/docs/app/api-reference/file-conventions/not-found) 을 지정하면 세그먼트 내에서 Not Found UI를 렌더링하여 이러한 오류를 우아하게 처리할 수 있습니다.

app/user/[id]/page.js

```jsx
import { notFound } from 'next/navigation';
 
async function fetchUsers(id) {
  const res = await fetch('https://...');
  if (!res.ok) return undefined;
  return res.json();
}
 
export default async function Profile({ params }) {
  const user = await fetchUser(params.id);
 
  if (!user) {
    notFound();
  }
 
  // ...
}
```

> Note: `notFound()` 는 타입스크립트 [`never`](https://www.typescriptlang.org/docs/handbook/2/functions.html#never) 타입을 사용하기 때문에 `return notFound()`를 사용할 필요가 없습니다.
>