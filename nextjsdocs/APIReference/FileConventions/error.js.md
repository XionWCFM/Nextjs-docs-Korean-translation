# error.js
<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/error</p>

<br>

[**에러**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) 파일은 경로 세그먼트에 대한 오류 UI 경계를 정의합니다. 

```javascript
'use client'; // 에러 컴포넌트는 클라이언트 컴포넌트여야 합니다. 

import { useEffect } from 'react';

export default function Error({
error,
reset,
}: {
error: Error;
  reset: () => void;
}) {
  useEffect(() => {
  // 에러 보고 시스템에 에러를 기록합니다. 
  console.error(error);
}, [error]);

return (
 <div>
   <h2>Something went wrong!</h2>
   <button
      onClick={
      // 해당 세그먼트를 재 렌더링하여 복구를 시도합니다. 
        () => reset()
      }
    >
      Try again
   </button>
  </div>
);
}
```

---
### **Props**

<br>

### `error`

에러 객체의 인스턴스 입니다. 이 에러는 서버나 클라이언트에서 발생할 수 있습니다. 

<br>

### `reset`

에러 경계를 초기화하하고 응답을 반환하지 않는 메서드 입니다. 

<br>

##### 알아두면 좋은 정보
- `error.js`  의 경계는 반드시 [클라이언트 컴포넌트](https://nextjs.org/docs/getting-started/react-essentials)여야 합니다.
- 동일한 세그먼트의 `layout.js` 컴포넌트에서 발생한 에러는 `error.js`의 경계가 해당 레이아웃 컴포넌트 내부에 중첩되어 있기 때문에 처리되지 않습니다. 
    - 특별한 레이아웃을 위해 에러를 다루기 위해서는 `error.js` 파일을 레이아웃의 부모 세그먼트 안에 위치해야 합니다.
    - 에러를 root layout 이나 template 없이 다루기 위해서는 `app/global-error.js`라고 불리우는 `error.js` 의 변형을 사용해야 합니다.

---
### **global-error.js**
root `layout.js`에서 에러를 특별하게 처리하려면 `app` 디렉토리 안에 위치해 있는 `app/global-error.js`라고 불리우는 `error.js` 의 변형을 사용해야 합니다. 

```javascript
'use client';

export default function GlobalError({
	error,
	reset,
}: {
	error: Error;
	reset: () => boid;
}) {
	return (
		<html>
			<body>
				<h2>Something went wrong!</h2>
				<button onClick={() => reset()}>Try again</button>
			</body>
		</htm/>
	);
}
```

##### 알아두면 좋은 정보
- `global-error.js` 는 활성화 될 때 root `layout.js`을 대체하므로 자신의 <html>과 <body>태그를 정의해야 합니다. 
- 에러 UI를 디자인하는 동안 [React Developer Tools](https://react.dev/learn/react-developer-tools)를 사용하여 수동으로 에러 경계를 전환하는 것이 도움이 될 수 있습니다.
