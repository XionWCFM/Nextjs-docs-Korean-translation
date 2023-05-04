# `usePathname`

`usePathname`는 현재 URL의 **경로 이름**(pathname)을 읽을 수 있는 **클라이언트 컴포넌트** 훅입니다.

```jsx
//app/example-client-component.tsx
"use client";

import { usePathname } from "next/navigation";

export default function ExampleClientComponent() {
  const pathname = usePathname();
  return <>Current pathname: {pathname}</>;
}
```

<br>

> ### Good to know
>
> - `usePathname`은 [클라이언트 컴포넌트](../../Building_Your_Application//Rendering/Server_and_Client_Components.md) 훅으로, [서버 컴포넌트](../../Building_Your_Application//Rendering/Server_and_Client_Components.md)에서는 지원되지 않습니다.
> - [폴백 경로](../../../stabledocs/API_Reference/Data_Fetching/getStaticPaths.md)(fallback route)가 렌더링되거나 Next.js가 [자동으로 페이지 디렉토리 페이지를 정적으로 최적화](../../../stabledocs/Documentation/Advanced_Features/Automatic_Static_Optimization.md)하고 라우터가 준비되지 않은 경우 `usePathname`은 `null`을 반환할 수 있습니다.
> - Next.js는 프로젝트에 `앱`과 `페이지` 디렉토리 모두가 감지되면 자동으로 타입을 업데이트합니다.

<br>

## API 레퍼런스

```jsx
const pathname = usePathname();
```

<br>

### 매개변수

`usePathname`은 매개변수를 사용하지 않습니다.

<br>

### 반환값

`usePathname`은 현재 URL의 경로 이름을 나타내는 문자열을 반환합니다. 예를 들면 다음과 같습니다.

| URL                 | 반환값                |
| ------------------- | --------------------- |
| `/`                 | `'/'`                 |
| `/dashboard`        | `'/dashboard'`        |
| `/dashboard?v=2`    | `/dashboard'`         |
| `/blog/hello-world` | `'/blog/hello-world'` |

<br>

## 예제

<br>

### 라우트 변경에 대한 응답으로 뭔가 실행하기

```jsx
//app / example - client - component.tsx;
("use client");

import { usePathname, useSearchParams } from "next/navigation";

function ExampleClientComponent() {
  const pathname = usePathname();
  const searchParams = useSearchParams();
  useEffect(() => {
    // 여기서 뭔가를 실행합니다...
  }, [pathname, searchParams]);
}
```

[useSelectedSegments](./useSelectedLayoutSegments.md)

[Edge Runtime](../Edge_Runtime/Edge_Runtime.md)
