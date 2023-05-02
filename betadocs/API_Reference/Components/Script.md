# next/script

<details>
  <summary><b>Version History</b></summary>
<div style="overflow-x: auto;"><table><thead><tr><th>Version</th><th>Changes</th></tr></thead><tbody><tr><td><code >v13.0.0</code></td><td><code >beforeInteractive</code> and <code >afterInteractive</code> is modified to support <code >app</code></td></tr><tr><td><code >v12.2.4</code></td><td><code >onReady</code> prop added.</td></tr><tr><td><code >v12.2.2</code></td><td>Allow <code >next/script</code> with <code >beforeInteractive</code> to be placed in <code >_document</code>.</td></tr><tr><td><code >v11.0.0</code></td><td><code >next/script</code> introduced.</td></tr></tbody></table></div>
</details>

이 API Reference는 `Script` 컴포넌트에서 사용할 수 있는 [props](https://beta.nextjs.org/docs/api-reference/components/script#props)의 사용 방법을 이해하는데 도움이 됩니다. 기능 및 사용법에 대해서는 [스크립트 최적화](https://beta.nextjs.org/docs/optimizing/scripts) 페이지를 참조하십시오.

```tsx
// app/dashboard/page.tsx
import script from 'next/script';

export default function Dashboard() {
  return (
    <>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

<br/>

## Props

`Script` 컴포넌트에서 사용할 수 있는 props의 요약은 다음과 같습니다.

<table><thead><tr><th>Prop</th><th>Example</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td><a href="#src" class="relative"><code class="inline">src</code></a></td><td><code class="inline">src="http://example.com/script"</code></td><td>String</td><td>Required unless inline script is used</td></tr><tr><td><a href="#strategy" class="relative"><code class="inline">strategy</code></a></td><td><code class="inline">strategy="lazyOnload"</code></td><td>String</td><td>-</td></tr><tr><td><a href="#onload" class="relative"><code class="inline">onLoad</code></a></td><td><code class="inline">onLoad={onLoadFunc}</code></td><td>Function</td><td>-</td></tr><tr><td><a href="#onready" class="relative"><code class="inline">onReady</code></a></td><td><code class="inline">onReady={onReadyFunc}</code></td><td>Function</td><td>-</td></tr><tr><td><a href="#onerror" class="relative"><code class="inline">onError</code></a></td><td><code class="inline">onError={onErrorFunc}</code></td><td>Function</td><td>-</td></tr></tbody></table>
<br />

## Required Props

`<Script />` 컴포넌트는 다음과 같은 속성들을 필요합니다.
<br/>

### `src`

외부 스크립트의 URL을 지정하는 문자열로된 경로입니다. 경로의 문자열 값은 절대 외부 URL(absolute external URL)이거나 내부 경로(internal path)가 될 수 있습니다. `src` 속성은 인라인 스크립트를 사용하지 않는 한 반드시 존재해야하는 속성입니다.
<br />

### Optional Props

`<Script />` 컴포넌트는 필요한 속성 외에도 여러 가지 추가 속성을 허용합니다.

<br />

### `strategy`

스크립트의 로드 전략입니다. 사용할 수 있는 4가지 전략은 다음과 같습니다:

- `beforeInteractive`: Next.js 코드가 로드 되기 전이나 하이드레이션(hydration)이 발생하기 전에 로드합니다.
- `afterInteractive`: (기본값) 빨리 로드(early load)하지만 페이지에서 몇몇 하이드레이션이 발생한 이후에 로드합니다.
- `lazyOnload`: 브라우저 유휴 시간(browser idle time) 동안에 로드합니다.
- `worker`: (실험) 웹 워커에 로드합니다.

<br />

### `beforeInteractive`

`beforeInteractive` 전략 속성을 가지고 로드하는 스크립트들은 서버에서 초기 HTML로 주입되고, Next.js 모듈 이전에 다운로드되며, 페이지에서 하이드레이션(hydration)이 발생하기 전에 배치된 순서대로 실행됩니다.

이 전략으로 표시된 스크립트는 퍼스트-파티(first-party) 코드보다 먼저 사전로드되고 가져오지만, 해당 스크립트의 실행은 페이지 하이드레잇ㄴ이 발생하는 것을 차단하지 않습니다.

`beforeInteractive` 스크립트들은 루트 레이아웃 안에 배치해야 하며 전체 사이트에서 필요로 하는 스크립트를 로드하도록 설계되어야 합니다(즉, 애플리케이션의 페이지가 서버 측에 로드되면 스크립트가 로드됩니다).

**이 전략은 페이지의 일부가 대화형(interactive)이 되기 전에 가져와야 하는 중요한 스크립트에만 사용해야 합니다.**

```tsx
// app/layout.tsx
import Script from 'next/script';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script
        src="https://example.com/script.js"
        strategy="beforeInteractive"
      />
    </html>
  );
}
```

> **알아두면 좋아요**: `beforeInteractive` 전략을 가진 스크립트는 컴포넌트가 어디에 있는지 상관 없이 항상 HTML의 `head` 안에 삽입됩니다.

`beforeInteractive`를 사용하여 가능한 빨리 로드해야 하는 스크립트의 몇 가지 예는 다음과 같습니다:

- 봇 탐지기 (Bot detectors)
- 쿠키 동의 관리자 (Cookie consent managers)

<br />

### `afterInteractive`

`afterInteractive` 전략을 사용하는 스크립트는 HTML 클라이언트 측에 주입되며 페이지에서 일부(또는 모든) 하이드레이션(hydration)이 발생한 후 로드됩니다. 이는 Script 컴포넌트의 기본 전략이며 가능한 빨리 로드해야 하지만 퍼스트-파티(first-party) Next.js 코드 이전에는 로드되지 않는 모든 스크립트에 사용해야 합니다.

`afterInteractive` 스크립트는 페이지 또는 레이아웃 내부에 배치할 수 있으며, 해당 페이지(또는 페이지 그룹)가 브라우저에서 열릴 때만 로드 및 실행됩니다.

```jsx
// app/page.js
import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="afterInteractive" />
    </>
  );
}
```

afterInteractive에 적합한 스크립트의 예는 다음과 같습니다:

- 태그 관리자 (Tag managers)
- 분석 (Analytics)

<br />

### `lazyload`

`lazyOnload` 전략을 사용하는 스크립트는 브라우저 유휴 시간 동안 HTML 클라이언트 측에 주입되며 페이지의 모든 리소스를 가져온 후 로드됩니다. 이 전략은 초기에 로드할 필요가 없는 백그라운드 스크립트나 낮은 우선순위 스크립트에 사용해야 합니다.

`lazyOnload` 스크립트는 페이지 또는 레이아웃 내부에 배치할 수 있으며, 해당 페이지(또는 페이지 그룹)가 브라우저에서 열릴 때만 로드 및 실행됩니다.

```jsx
// app/page.js
import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="lazyOnload" />
    </>
  );
}
```

즉시 로드할 필요가 없고 lazyOnload로 가져올 수 있는 스크립트의 예는 다음과 같습니다:

- 채팅 지원 플러그인 (Chat support plugins)
- 소셜 미디어 위젯 (Social media widgets)

<br />

### `worker`

> **경고**: worker 전략은 아직 안정적이지 않으며 [app](https://beta.nextjs.org/docs/routing/defining-routes) 디렉토리에서 아직 작동하지 않습니다. 주의하여 사용하십시오.

`worker` 전략을 사용하는 스크립트는 메인 스레드를 해제하고 중요한 타사 리소스만 처리되도록 하기 위해 웹 작업자에게 오프로드됩니다. 이 전략은 모든 스크립트에 사용할 수 있지만 모든 타사 스크립트를 지원하는 것은 보장되지 않는 고급 사용 사례입니다.

`worker를` 전략으로 사용하려면 `next.config.js`에서 `nextScriptWorkers` 플래그를 사용하도록 설정해야 합니다:

```js
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
};
```

`worker` 스크립트는 현재 다음 페이지/디렉토리에서만 사용할 수 있습니다:

```tsx
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

<br />

### `onLoad`

> **경고**: `onLoad`는 아직 서버 구성요소와 함께 작동하지 않으며 클라이언트 구성요소에서만 사용할 수 있습니다. 또한 `onLoad`는 `beforeInteractive`와 함께 사용할 수 없습니다. - 대신 `onReady`를 사용하는 것이 좋습니다.

일부 써드-파티 스크립트에서는 내용을 인스턴스화하거나 함수를 호출하기 위해 스크립트 로드가 완료된 후 JavaScript 코드를 실행해야 합니다. 다음 중 하나를 사용하여 스크립트를 로드하는 경우 `afterInteractive` 또는 `lazyOnload` 전략으로 onLoad 속성을 사용하여 로드한 후 코드를 실행할 수 있습니다.

다음은 라이브러리가 로드된 후에만 lodash 메서드를 실행하는 예입니다.

```tsx
'use client';

import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script
        src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"
        onLoad={() => {
          console.log(_.sample([1, 2, 3, 4]));
        }}
      />
    </>
  );
}
```

<br />

### `onReady`

> **경고**: `onReady`는 아직 서버 컴포넌트와 함께 작동하지 않으며 클라이언트 컴포넌트에서만 사용할 수 있습니다.

일부 third-party 스크립트는 스크립트 로드가 완료된 후 컴포넌트가 마운트될 때마다(예: 경로 탐색 후) JavaScript 코드를 실행해야 합니다. 스크립트가 처음 로드될 때 스크립트 로드 이벤트 후에 코드를 실행한 다음 `onReady` 속성을 사용하여 모든 후속 컴포넌트를 다시 마운트할 수 있습니다.

다음은 Google Maps JS가 내장된 컴포넌트가 마운트될 때마다 다시 인스턴스화하는 방법의 예입니다:

```tsx
// app/page.tsx
'use client';

import { useRef } from 'react';
import Script from 'next/script';

export default function Page() {
  const mapRef = useRef();

  return (
    <>
      <div ref={mapRef}></div>
      <Script
        id="google-maps"
        src="https://maps.googleapis.com/maps/api/js"
        onReady={() => {
          new google.maps.Map(mapRef.current, {
            center: { lat: -34.397, lng: 150.644 },
            zoom: 8,
          });
        }}
      />
    </>
  );
}
```

<br />

### `onError`

> **경고**: `onError`는 아직 서버 구성 요소와 함께 작동하지 않으며 클라이언트 구성 요소에서만 사용할 수 있습니다. `onError`는 `beforeInteractive` 로드 전략과 함께 사용할 수 없습니다.

스크립트가 로드되지 않을 때 발견하는 것이 도움이 될 수 있습니다. `onError` 속성을 사용하여 다음 오류를 처리할 수 있습니다:

```tsx
// app/plage.tsx
'use client';

import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onError={(e) => {
          console.error('Script failed to load', e);
        }}
      />
    </>
  );
}
```

# Next Steps

> **Optimizing Scripts**
> [Next.js 애플리케이션에서 써드-파티 스크립트들을 최적화하는 방법에 대해 학습합니다.](https://beta.nextjs.org/docs/optimizing/scripts)
