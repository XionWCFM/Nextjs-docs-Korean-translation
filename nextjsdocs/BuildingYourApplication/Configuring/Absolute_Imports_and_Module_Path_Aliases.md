# Absolute Imports and Module Path Aliases

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases](https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases)

**Absolute Imports and Module Path Aliases**

<details>
    <summary>Examples</summary>
    <div markdown="1">
    <a href="https://github.com/vercel/next.js/tree/canary/examples/with-absolute-imports" target="_blank">- Absolute Imports and Aliases
    </a>
    </div>
</details>

Next.js는 **`tsconfig.json`** 및 **`jsconfig.json`** 파일의 **`"paths"`** 및 **`"baseUrl"`** 옵션에 대한 내장 지원을 제공합니다.

이러한 옵션을 사용하면 프로젝트 디렉토리를 절대 경로로 별칭 지정할 수 있어 모듈을 더 쉽게 가져올 수 있습니다. 예를 들어:

```jsx
// before
import { Button } from '../../../components/button'
 
// after
import { Button } from '@/components/button'
```

> 참고: **`create-next-app`**은 이러한 옵션을 구성하도록 안내할 것입니다.
> 

## **[Absolute Imports](https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases#absolute-imports)**

**`baseUrl`** 구성 옵션을 사용하면 프로젝트의 루트에서 직접 가져올 수 있습니다.

이 구성의 예시:

tsconfig.json 또는 jsconfig.json

```json
{
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

components/button.tsx

```tsx
export default function Button() {
  return <button>Click me</button>
}
```

app/page.tsx

```tsx
import Button from 'components/button'
 
export default function HomePage() {
  return (
    <>
      <h1>Hello World</h1>
      <Button />
    </>
  )
}
```

## **[Module Aliases](https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases#module-aliases)**

**`baseUrl`** 경로를 구성하는 것 외에도, **`"paths"`** 옵션을 사용하여 모듈 경로를 "별칭" 할 수 있습니다.

예를 들어, 다음 구성은 **`@/components/*`**을 **`components/*`**로 매핑합니다:

tsconfig.json or jsconfig.json

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/components/*": ["components/*"]
    }
  }
}
```

components/button.tsx

```tsx
export default function Button() {
  return <button>Click me</button>;
}
```

app/page.tsx

```tsx
import Button from '@/components/button';
export default function HomePage() {
  return (
    <>
      <h1>Hello World</h1>
      <Button />
    </>
  );
}
```

각각의 **`"paths"`**는 **`baseUrl`** 위치를 기준으로 상대적입니다. 예를 들어:

```json
// tsconfig.json 또는 jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src/",
    "paths": {
      "@/styles/*": ["styles/*"],
      "@/components/*": ["components/*"]
    }
  }
}
```

```jsx
// pages/index.js
import Button from '@/components/button';
import '@/styles/styles.css';
import Helper from 'utils/helper';
export default function HomePage() {
  return (
    <Helper>
      <h1>Hello World</h1>
      <Button />
    </Helper>
  );
}
```