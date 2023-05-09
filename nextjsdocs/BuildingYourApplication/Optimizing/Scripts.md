# 스크립트 최적화
## 레이아웃 스크립트
여러 라우트에서 써드파티 스크립트를 로드하려면 `next/script`를 가져와서 스크립트를 레이아웃 컴포넌트에 직접 포함시키면 됩니다.
```typescript
// app/dashboard/layout.tsx

import Script from 'next/script';
 
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <>
      <section>{children}</section>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```
써드파티 스크립트는 사용자가 폴더 라우트(예: `dashboard/page.js`) 또는 중첩된 라우트(예 : `dashboard/settings/page.js`)에 액세스할 때 가져옵니다. Next.js는 사용자가 동일한 레이아웃에서 여러 라우트 사이를 탐색하더라도 스크립트가 한 번만 로드 되도록 합니다.

## 애플리케이션 스크립트
모든 라우트에서 써드파티 스크립트를 로드하려면 `next/script`를 가져와서 스크립트를 루트 레이아웃에 직접 포함시키면 됩니다.
```typescript
app/layout.tsx

import Script from 'next/script';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script src="https://example.com/script.js" />
    </html>
  );
}
```
이 스크립트는 애플리케이션의 어떤 라우트에 접근하더라도 액세스할 때 로드되고 실행됩니다. Next.js는 사용자가 여러 페이지 사이를 탐색하더라도 스크립트가 한 번만 로드 되도록 합니다.

> __권장 사항 :__ 성능에 대한 불필요한 영향을 최소화하기 위해 특정 페이지나 레이아웃에만 써드파티 스크립트를 포함하는 것이 좋습니다.

## 전략
`next/script`는 기본적으로 모든 페이지 또는 레이아웃에서 써드파티 스크립트를 로드할 수 있지만, `strategy` 속성을 사용하여 로딩 동작을 세밀하게 조정할 수 있습니다.

* `beforeInteractive`: Next.js 코드와 페이지 수화(hydration) 이전에 스크립트를 로드합니다.
* `afterInteractive`: ( 기본값 ) 스크립트를 초기에 로드하지만 페이지에 약간의 수분 공급(hydration)이 발생한 후에 로드합니다.
* `lazyOnload`: 브라우저 유휴(idle) 시간에 스크립트를 로드합니다.
* `worker`: (실험적) web work에서 스크립트를 로드합니다.

각 전략과 사용 사례에 대해 자세히 알아보려면 [`next/script`]() API 참조 문서를 확인하세요.

## Web Worker로 스크립트 오프로드하기 (실험적)
> __경고 :__ `worker` 전략은 아직 안정화되지 않았으며 아직 `app` 디렉토리에서 작동하지 않습니다. 주의하여 사용하십시오.

`worker` 전략을 사용하는 스크립트는 Partytown을 사용하여 web worker에서 오프로드 및 실행됩니다. 이렇게 하면 메인 스레드를 애플리케이션 코드의 나머지 부분에 집중하여 사이트의 성능을 향상시킬 수 있습니다.

이 전략은 아직 실험적이며 `nextScriptWorkers` 플래그가 `next.config.js`에서 활성화된 경우에만 사용할 수 있습니다.

```
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
};
```

그런 다음 `next`(보통 `npm run dev`또는 `yarn dev`)을 실행하면, Next.js가 필요한 패키지를 설치하기 위한 안내를 제공합니다.

```
npm run dev
```

다음과 같은 안내가 표시됩니다: Please install Partytown by running `npm install @builder.io/partytown`
설치가 완료되고 `strategy="worker"`라고 정의하면, 애플리케이션에서 Partytown이 자동으로 인스턴스화되고 스크립트가 web worker로 오프로드됩니다.

```typescript
// pages/home.tsx

import Script from 'next/script';
 
export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="worker" />
    </>
  );
}
```
web worker에서 써드파티 스크립트를 로드할 때 고려해야 할 여러 장단점이 있습니다. 자세한 내용은 Partytown의 [장단점]() 설명서를 참조하십시오.

## 인라인 스크립트
인라인 스크립트 또는 외부 파일에서 로드되지 않은 스크립트도 스크립트 컴포넌트에서 지원됩니다. 중괄호 안에 JavaScript를 배치하여 작성할 수 있습니다.

```javascript
<Script id="show-banner">
  {`document.getElementById('banner').classList.remove('hidden')`}
</Script>
```

또는 `dangerouslySetInnerHTML` 속성을 사용합니다:

```javascript
<Script
  id="show-banner"
  dangerouslySetInnerHTML={{
    __html: `document.getElementById('banner').classList.remove('hidden')`,
  }}
/>
```
경고 : 인라인 스크립트의 경우 Next.js가 스크립트를 추적하고 최적화하려면 `id` 속성을 할당해야 합니다.

## 추가 코드 실행

특정 이벤트가 발생한 후 추가 코드를 실행하기 위해서 이벤트 핸들러를 스크립트 컴포넌트와 함께 사용할 수 있습니다.

`onLoad`: 스크립트 로딩이 완료된 후 코드를 실행합니다.
`onReady`: 스크립트 로드가 완료되고 컴포넌트가 마운트될 때마다 코드를 실행합니다.
`onError`: 스크립트가 로드되지 않으면 코드를 실행합니다.
이러한 핸들러는  `next/script`가 import되고, `"use client"`가 코드의 첫 번째 줄로 정의된 클라이언트 컴포넌트 내에서 사용될 때만 작동합니다. 

```typescript
// app/page.tsx

'use client';
 
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onLoad={() => {
          console.log('Script has loaded');
        }}
      />
    </>
  );
}
```

각 이벤트 핸들러에 대해 자세히 알아보고 예제를 보려면 `next/script` API 참조를 확인하십시오.

## 추가 속성
`nonce` 또는 사용자 정의 데이터 속성과 같이 스크립트 컴포넌트에서 사용하지 않는 `<script>` 엘리먼트에 할당할 수 있는 많은 DOM 속성이 있습니다.
추가 속성을 포함하면 자동으로 HTML에 포함된 최종 최적화된 `<script>` 엘리먼트로 전달됩니다.

```javascript
// app/page.tsx

import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        id="example-script"
        nonce="XUENAJFW"
        data-test="script"
      />
    </>
  );
}
```

# API 참조

`next/script` API에 대해 자세히 알아보세요.
App Router > ... > Components
<스크립트>
내장된 `next/script` 컴포넌트를 사용하여 Next.js 애플리케이션에서 써드파티 스크립트를 최적화합니다.