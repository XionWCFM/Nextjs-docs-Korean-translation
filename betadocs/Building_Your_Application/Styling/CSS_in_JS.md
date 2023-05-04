원본 링크: [https://beta.nextjs.org/docs/styling/css-in-js](https://beta.nextjs.org/docs/styling/css-in-js)

# \***\*CSS-in-JS\*\***

<aside>
📢 **경고:**

런타임 JavaScript가 필요한 CSS-in-JS 라이브러리는 현재 서버 컴포넌트에서 지원되지 않습니다. 서버 컴포넌트 및 스트리밍과 같은 최신 React 기능과 함께 CSS-in-JS를 사용하려면 라이브러리 작성자가 [동시 렌더링](https://react.dev/blog/2022/03/29/react-v18#what-is-concurrent-react)을 포함한 최신 버전의 React를 지원해야 합니다.

저희는 React 팀과 함께 서버 컴포넌트 및 스트리밍 아키텍처를 지원하는 CSS 및 JavaScript 자산을 처리하기 위해 upstream APIs를 개발중입니다.

</aside>

앱 디렉토리 내 클라이언트 컴포넌트에서 지원되는 라이브러리는 다음과 같습니다:

- [styled-jsx](https://beta.nextjs.org/docs/styling/css-in-js#styled-jsx)
- [styled-components](https://beta.nextjs.org/docs/styling/css-in-js#styled-components)
- [style9](https://github.com/johanholmerin/style9)

다음은 현재 지원을 위해 작업 중입니다:

- [emotion](https://github.com/emotion-js/emotion/issues/2928)
- [Material UI](https://github.com/mui/material-ui/issues/34905#issuecomment-1401306594)
- [Chakra UI](https://github.com/chakra-ui/chakra-ui/issues/7180)

<aside>
💡 **참고:** 다양한 CSS-in-JS 라이브러리를 테스트하고 있으며, React 18 기능과/또는 앱 디렉토리를 지원하는 라이브러리에 대한 더 많은 예제를 추가할 예정입니다.

</aside>

서버 컴포넌트의 스타일을 지정하려면 [CSS Modules](https://beta.nextjs.org/docs/styling/css-modules) 이나 PostCSS 또는 [Tailwind CSS](https://beta.nextjs.org/docs/styling/tailwind-css)와 같은 CSS 파일을 출력하는 다른 솔루션을 사용하는 것을 권장합니다.

## \***\*앱에서 CSS-in-JS 구성 (Configuring CSS-in-JS in app)\*\***

CSS-in-JS 구성은 다음과 같은 3단계 옵트인 프로세스로 이루어집니다:

1. 렌더링에서 모든 CSS 규칙을 수집하는 **스타일 레지스트리**.
2. 사용 가능한 모든 콘텐츠 이전에 규칙을 삽입하는 새로운 useServerInsertedHTML 훅.
3. 초기 서버 사이드 렌더링 중에 스타일 레지스트리로 앱을 래핑하는 클라이언트 컴포넌트.

## \***\*styled-jsx\*\***

클라이언트 컴포넌트에서 styled-jsx를 사용하려면 v5.1.0을 사용해야 합니다. 먼저 새 레지스트리를 만듭니다:

```jsx
app / registry.tsx;

("use client");

import React, { useState } from "react";
import { useServerInsertedHTML } from "next/navigation";
import { StyleRegistry, createStyleRegistry } from "styled-jsx";

export default function StyledJsxRegistry({
  children,
}: {
  children: React.ReactNode,
}) {
  // Only create stylesheet once with lazy initial state
  // x-ref: https://reactjs.org/docs/hooks-reference.html#lazy-initial-state
  const [jsxStyleRegistry] = useState(() => createStyleRegistry());

  useServerInsertedHTML(() => {
    const styles = jsxStyleRegistry.styles();
    jsxStyleRegistry.flush();
    return <>{styles}</>;
  });

  return <StyleRegistry registry={jsxStyleRegistry}>{children}</StyleRegistry>;
}
```

그런 다음 [루트 레이아웃](https://beta.nextjs.org/docs/routing/pages-and-layouts#root-layout-required)을 레지스트리로 래핑합니다:

```jsx
app / layout.tsx;

import StyledJsxRegistry from "./registry";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html>
      <body>
        <StyledJsxRegistry>{children}</StyledJsxRegistry>
      </body>
    </html>
  );
}
```

[여기에서 예시를 확인하세요.](https://github.com/vercel/app-playground/tree/main/app/styling/styled-jsx)

## \***\*스타일이 적용된 컴포넌트 (Styled Components)\*\***

다음은 styled-components를 구성하는 방법의 예시입니다:

1. 첫째로, styled-components API를 사용하여 렌더링 중에 생성된 모든 CSS 스타일 규칙을 수집하는 전역 레지스트리 컴포넌트와, 해당 규칙을 반환하는 함수를 만듭니다. 그런 다음 useServerInsertedHTML 훅을 사용하여 레지스트리에서 수집된 스타일을 루트 레이아웃의 <head> HTML 태그에 삽입합니다.

```jsx
lib / registry.tsx;

("use client");

import React, { useState } from "react";
import { useServerInsertedHTML } from "next/navigation";
import { ServerStyleSheet, StyleSheetManager } from "styled-components";

export default function StyledComponentsRegistry({
  children,
}: {
  children: React.ReactNode,
}) {
  // Only create stylesheet once with lazy initial state
  // x-ref: https://reactjs.org/docs/hooks-reference.html#lazy-initial-state
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet());

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement();
    styledComponentsStyleSheet.instance.clearTag();
    return <>{styles}</>;
  });

  if (typeof window !== "undefined") return <>{children}</>;

  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  );
}
```

2. 루트 레이아웃의 children을 스타일 레지스트리 컴포넌트로 래핑합니다:

```jsx
app / layout.tsx;

import StyledComponentsRegistry from "./lib/registry";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html>
      <body>
        <StyledComponentsRegistry>{children}</StyledComponentsRegistry>
      </body>
    </html>
  );
}
```

[여기에서 예시를 확인하세요.](https://github.com/vercel/app-playground/tree/main/app/styling/styled-components)

알아두면 좋습니다:

서버 렌더링 중에 스타일이 전역 레지스트리로 추출되고 HTML의 <head>로 플러시됩니다. 이렇게 하면 스타일 규칙을 사용할 수 있는 콘텐츠 앞에 배치됩니다. 향후에는 React의 업커밍 기능을 사용하여 스타일을 삽입할 위치를 결정할 수 있습니다.

스트리밍하는 동안 각 청크의 스타일이 수집되어 기존 스타일에 추가됩니다. 클라이언트 측 수화가 완료되면 styled-component가 평소대로 동작하여 추가적인 동적 스타일을 삽입합니다.

특히 스타일 레지스트리의 트리 최상위 레벨에 클라이언트 컴포넌트를 사용하는 이유는 이 방식으로 CSS 규칙을 추출하는 것이 더 효율적이기 때문입니다. 이렇게 하면 후속 서버 렌더링에서 스타일을 다시 생성하지 않아도 되고, 서버 컴포넌트 페이로드에 스타일이 전송되는 것을 방지할 수 있습니다.

[Global Styles](./Global_Styles.md)

[External Stylesheets](./External_Stylesheets.md)
