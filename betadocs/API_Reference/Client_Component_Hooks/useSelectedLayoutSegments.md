# `useSelectedLayoutSegments`

`useSelectedLayoutSegments`는 호출된 레이아웃 **하위의** 활성 경로 세그먼트를 읽을 수 있게 해주는 **클라이언트 컴포넌트** 훅입니다. 이는 브레드크럼과 같이 활성 자식 세그먼트를 알아야 하는 상위 레이아웃에서 UI를 생성하는 데 유용합니다.

```jsx
//app/example-client-component.tsx
"use client";

import { useSelectedLayoutSegments } from "next/navigation";

export default function ExampleClientComponent() {
  const segments = useSelectedLayoutSegments();

  return (
    <ul>
      {segments.map((segment, index) => (
        <li key={index}>{segment}</li>
      ))}
    </ul>
  );
}
```

<br>

> ### Good to know
>
> - `useSelectedLayoutSegments`는 [클라이언트 컴포넌트](../../Building_Your_Application/Rendering/Server_and_Client_Components.md) 훅이고, 레이아웃은 기본적으로 [서버 컴포넌트](../../Building_Your_Application/Rendering/Server_and_Client_Components.md)이기 때문에, `useSelectedLayoutSegments`는 일반적으로 레이아웃으로 가져온 클라이언트 컴포넌트를 통해 호출됩니다.
> - 반환된 세그먼트에는 [라우트 그룹](../../Building_Your_Application/Routing/Defining_Routes.md)이 포함되는데, 이것은 UI에 포함되지 않는 것이 좋으므로, 대괄호([])로 시작하는 항목을 필터링하도록 배열 메서드인 `filter()`를 사용할 수 있습니다.

<br>

## API 참조

```jsx
const segments = useSelectedLayoutSegments();
```

<br>

### 매개변수

`useSelectedLayoutSegments`는 매개변수를 받지 않습니다.

<br>

### 반환값

`useSelectedLayoutSegments`는 호출된 훅이 있는 레이아웃에서 한 단계 아래에 있는 활성 세그먼트를 포함하는 문자열 배열을 반환합니다. 세그먼트가 없는 경우 빈 배열을 반환합니다.

예를 들어, 아래와 같은 레이아웃과 URL이 있다면, 반환된 세그먼트는 다음과 같을 것입니다.

| 레이아웃                  | 방문한 URL            | 반환된 세그먼트             |
| ------------------------- | --------------------- | --------------------------- |
| `app/layout.js`           | `/`                   | `[]`                        |
| `app/layout.js`           | `/dashboard`          | `['dashboard']`             |
| `app/layout.js`           | `/dashboard/settings` | `['dashboard', 'settings']` |
| `app/dashboard/layout.js` | `/dashboard`          | `[]`                        |
| `app/dashboard/layout.js` | `/dashboard/settings` | `['settings']`              |

[useSelectedLayoutSegment](./useSelectedLayoutSegments.md)

[usePathname](./usePathname.md)
