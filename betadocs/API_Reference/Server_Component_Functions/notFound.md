원본내용 : [https://beta.nextjs.org/docs/api-reference/notfound](https://beta.nextjs.org/docs/api-reference/notfound)

# **Not Found**

`notFound` 함수는 라우트 세그먼트 내에서 찾을 수 없는 파일을 렌더링하고, `<meta name="robots" content="noindex" />` 태그를 삽입하는 것을 가능하게 합니다.

## **`[notFound()](https://beta.nextjs.org/docs/api-reference/notfound#notfound)`**

`notFound()` 함수를 호출하면 `NEXT_NOT_FOUND` 오류가 발생하며 해당 오류는 호출된 라우트  세그먼트의 렌더링을 중단시킵니다. [찾을 수 없는 파일](https://beta.nextjs.org/docs/api-reference/file-conventions/not-found)을 지정하면 해당 오류를 우아하게 처리할 수 있으며, 해당 세그먼트 내에서 Not Found UI를 렌더링할 수 있습니다.

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

**참고 :** `notFound()` 는 타입스크립트의 `[never](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)` 타입을 사용하기 때문에 `return notFound()` 를 사용할 필요가 없습니다.