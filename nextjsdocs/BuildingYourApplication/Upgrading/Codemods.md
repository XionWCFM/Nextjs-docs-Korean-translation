# **Codemods**

Codemods는 코드베이스에서 프로그램적으로 실행되는 변환입니다. 이를 통해 모든 파일을 수동으로 검토하지 않아도 대규모 변경사항을 자동으로 적용할 수 있습니다.

Next.js는 API가 업데이트되거나 더 이상 사용되지 않을 때 Next.js 코드베이스를 업그레이드하는 데 도움이 되는 Codemod 변환을 제공합니다.

<br><hr><br>

## **사용법**

터미널에서 프로젝트 폴더로 이동(`cd`)한 다음, 다음을 실행하세요.

```bash
npx @next/codemod <transform> <path>
```

`<transform>`과 `<path>`를 적절한 값으로 대체합니다.

- `transform` - 변환의 이름
- `path` - 변환할 파일 또는 디렉토리
- `--dry` 드라이 런(dry-run)을 실행합니다. 코드는 변경되지 않습니다.
- `--print` 변경된 결과를 출력하여 비교합니다.

<br><hr><br>

## **Next.js Codemods**

### **13.2**

#### **내장된 폰트 사용**

```bash
npx @next/codemod@latest built-in-next-font
```

`@next/font` 패키지를 제거하고 `@next/font` import를 내장된 `next-font`로 변환합니다.

예시 :

```JSX
import { Inter } from '@next/font/google';
```

다음과 같이 변환합니다:

```JSX
import { Inter } from 'next/font/google';
```

### **13.0**

#### **Next Image Import 이름 변경**

```bash
npx @next/codemod@latest next-image-to-legacy-image ./pages
```

기존 Next.js 10, 11 또는 12 애플리케이션의 `next/image` import를 Next.js 13의 `next/legacy/image`로 안전하게 변경합니다. 또한 `next/future/image`를 `next/image`로 변경합니다.

예시 :

```JSX
// pages/index.js
import Image1 from 'next/image';
import Image2 from 'next/future/image';

export default function Home() {
  return (
    <div>
      <Image1 src="/test.jpg" width="200" height="300" />
      <Image2 src="/test.png" width="500" height="400" />
    </div>
  );
}
```

다음과 같이 변환합니다 :

```JSX
// pages/index.js
// 'next/image' -> 'next/legacy/image'
import Image1 from 'next/legacy/image';
// 'next/future/image' -> 'next/image'
import Image2 from 'next/image';

export default function Home() {
  return (
    <div>
      <Image1 src="/test.jpg" width="200" height="300" />
      <Image2 src="/test.png" width="500" height="400" />
    </div>
  );
}
```

#### **새로운 이미지 컴포넌트로 이전**

```bash
npx @next/codemod@latest next-image-experimental ./pages
```

`next/legacy/image`에서 인라인 스타일을 추가하고 사용하지 않는 props를 제거하여 새로운 `next/image`로 위험하게 이전합니다.

- `layout` prop을 제거하고 `style`을 추가합니다.
- `objectFit` prop을 제거하고 `style`을 추가합니다.
- `objectPosition` prop을 제거하고 `style`을 추가합니다.
- `lazyBoundary` prop을 제거합니다.
- `lazyRoot` prop을 제거합니다.

#### **링크 컴포넌트로부터 `<a>` 태그 제거**

```bash
npx @next/codemod@latest new-link ./pages
```

[링크 컴포넌트](https://nextjs.org/docs/app/api-reference/components/link) 내부의 `<a>` 태그를 제거하거나 자동으로 수정할 수 없는 링크에 대해 legacyBehavior prop을 추가합니다.

예시 :

```JSX
<Link href="/about">
  <a>About</a>
</Link>
// transforms into
<Link href="/about">
  About
</Link>

<Link href="/about">
  <a onClick={() => console.log('clicked')}>About</a>
</Link>
// transforms into
<Link href="/about" onClick={() => console.log('clicked')}>
  About
</Link>
```

자동 수정이 적용될 수 없는 경우, legacyBehavior prop이 추가됩니다. 이를 통해 앱은 해당 특정 링크에 대해 이전 동작을 유지하면서 계속 작동할 수 있습니다.

```JSX
const Component = () => <a>About</a>

<Link href="/about">
  <Component />
</Link>
// becomes
<Link href="/about" legacyBehavior>
  <Component />
</Link>
```

### **11**

#### **CRA로부터 이전**

```bash
npx @next/codemod cra-to-next
```

Create React App 프로젝트를 Next.js로 이전하여 페이지 라우터(Pages Router) 및 필요한 구성을 생성합니다. 초기에는 클라이언트 측 단독 렌더링을 활용하여 SSR 중 `윈도우` 사용으로 인한 호환성 문제를 방지하며, Next.js 특정 기능의 점진적인 적용을 가능하게 하기 위해 원활하게 활성화될 수 있습니다.

이 변환에 관련된 의견을 [이 토론](https://github.com/vercel/next.js/discussions/25858)에 공유해주세요.

### **10**

#### **React import 추가**

```bash
npx @next/codemod add-missing-react-import
```

[새로운 JSX 변환](https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)이 작동하도록 하기 위해 `React`를 import하지 않는 파일에 해당 import를 추가하는 변환입니다.

예시 :

```JSX
// my-component.js
export default class Home extends React.Component {
  render() {
    return <div>Hello World</div>;
  }
}
```

다음과 같이 변환합니다 :

```JSX
// my-component.js
import React from 'react';
export default class Home extends React.Component {
  render() {
    return <div>Hello World</div>;
  }
}
```

### **9**

#### **익명 컴포넌트를 명명된 컴포넌트로 변환**

```bash
npx @next/codemod name-default-component
```

**버전 9 이상**

[Fast Refresh](https://nextjs.org/blog/next-9-4#fast-refresh)와 호환되도록 익명 컴포넌트를 명명된 컴포넌트로 변환합니다.

예시 :

```JSX
// my-component.js
export default function () {
  return <div>Hello World</div>;
}
```

다음과 같이 변환합니다 :

```JSX
// my-component.js
export default function MyComponent() {
  return <div>Hello World</div>;
}
```

컴포넌트는 파일 이름을 기반으로 카멜 케이스로 작성된 이름을 갖게 되며, 화살표 함수에 또한 적용됩니다.

### **8**

AMP HOC를 페이지 구성으로 변환합니다.

```bash
npx @next/codemod withamp-to-config
```

`withAMP` HOC를 Next.js 9 페이지 구성으로 변환합니다.

예시 :

```JSX
// Before
import { withAmp } from 'next/amp';

function Home() {
  return <h1>My AMP Page</h1>;
}

export default withAmp(Home);
```

```JSX
// After
export default function Home() {
  return <h1>My AMP Page</h1>;
}

export const config = {
  amp: true,
};
```

### **6**

#### **`withRouter` 사용**

```bash
npx @next/codemod url-to-withrouter
```

상위 페이지의 자동으로 삽입된 `url` 속성을 `withRouter` 및 해당 속성을 삽입하는 `router`로 변환합니다. 자세한 내용은 [https://nextjs.org/docs/messages/url-deprecated](https://nextjs.org/docs/messages/url-deprecated)를 참조하세요.

예시 :

```JSX
// From
import React from 'react';
export default class extends React.Component {
  render() {
    const { pathname } = this.props.url;
    return <div>Current pathname: {pathname}</div>;
  }
}
```

```JSX
// To
import React from 'react';
import { withRouter } from 'next/router';
export default withRouter(
  class extends React.Component {
    render() {
      const { pathname } = this.props.router;
      return <div>Current pathname: {pathname}</div>;
    }
  },
);
```

이는 하나의 예시입니다. 변환되고(테스트된) 모든 사례는 [\_\_testfixtures\_\_ 디렉토리](https://github.com/vercel/next.js/tree/canary/packages/next-codemod/transforms/__testfixtures__/url-to-withrouter)에서 찾을 수 있습니다.
