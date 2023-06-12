# loading.js

<p>공식문서 : https://nextjs.org/docs/app/api-reference/file-conventions/loading </p>

**loading** 파일은 [**Suspense**](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/BuildingYourApplication/Routing/Loading_UI_and_Streaming.md)를 기반으로 한 즉시 로딩 상태를 만들 수 있습니다. 

기본적으로, 이 파일은 [서버 컴포넌트](https://github.com/XionWCFM/Nextjs-docs-Korean-translation/blob/main/nextjsdocs/GettingStarted/React_Essentials.md) 입니다. - 하지만 **“use client”** 지시어를 통해 클라이언트 컴포넌트로도 사용할 수 있습니다. 

```jsx
//app/feed/loading.tsx
export default function Loading(){
	//Or a custom loading skeleton component
	return 'Loading...';
}
```

**Loading** UI 컴포넌트는 어떠한 매개변수도 허용하지 않습니다. (받지 않습니다.)

---

##### 알아두면 좋은 정보
- 로딩 UI를 디자인하는 동안 [React Developer Tools](https://react.dev/learn/react-developer-tools)를 사용하여 수동으로 Suspense 경계를 전환하는 것이 도움이 될 수 있습니다.