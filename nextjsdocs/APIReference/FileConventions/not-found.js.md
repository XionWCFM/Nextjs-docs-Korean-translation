# not-found.js

<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/not-found</p>

**not-found** 파일은 `notFound` 함수가 경로 세그먼트 내에서 발생(throw) 될 때 UI를 랜더링하는데 사용되곤 합니다. 사용자 정의 UI를 제공하는 동시에, Next.js는 `404` HTTP 상태 코드도 반환합니다.

```jsx
//app/blog/not-found.js
import Link from 'next/link'
 
export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
      <p>
        View <Link href="/blog">all posts</Link>
      </p>
    </div>
  )
}
```

참고 **:** 예상되는 **notFound()** 에러를 처리하는 것 외에도, **`app/not-found.js`** 경로 파일은 애플리케이션 전체에서 처리되지 않은 URL에 대해서도 처리합니다. 이는 사용자가 앱에서 처리하지 않는 URL을 방문할 경우  **`app/not-found.js`** 파일에서 내보낸 UI가 표시된 다는 것을 의미합니다.

---

### Props

 **`not-found.js`** 컴포넌트는 어떠한 props 도 허용하지 않습니다.(받지 않습니다.)