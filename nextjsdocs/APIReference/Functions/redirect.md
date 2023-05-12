원본링크 : [https://nextjs.org/docs/app/api-reference/functions/redirect](https://nextjs.org/docs/app/api-reference/functions/redirect)

# **redirect**

`redirect` 함수를 사용하면 사용자를 다른 URL로 리디렉션할 수 있습니다. 404로 리디렉션해야 하는 경우[notFound](https://nextjs.org/docs/app/api-reference/functions/not-found) 함수를 사용할 수 있습니다.

---

## **[redirect()](https://nextjs.org/docs/app/api-reference/functions/redirect#redirect)**

`redirect()` 함수를 호출하면 `NEXT_REDIRECT` 에러가 발생하고 에러가 발생한 경로 세그먼트의 렌더링이 종료됩니다.`redirect()` 함수는 상대 또는 절대 URL을 사용하여 호출할 수 있습니다.

app/team/[id]/page.js

```jsx
import { redirect } from 'next/navigation';
 
async function fetchTeam(id) {
  const res = await fetch('https://...');
  if (!res.ok) return undefined;
  return res.json();
}
 
export default async function Profile({ params }) {
  const team = await fetchTeam(params.id);
  if (!team) {
    redirect('/login');
  }
 
  // ...
}
```

> Note: `redirect()` 는 타입스크립트 [never](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)`type을 사용하기 때문에 반환` redirect()` 를 사용할 필요가 없습니다.
>