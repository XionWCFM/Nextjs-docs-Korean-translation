# <Script>

이 API 레퍼런스는 `Script` 컴포넌트에서 사용 가능한 `props`를 어떻게 사용하는지 이해하는데 도움이 될 것입니다. 기능과 사용법은 [Optimizing Scripts](https://nextjs.org/docs/app/building-your-application/optimizing/scripts) 페이지에서 참고해주세요.

```tsx
// app/dashboard/page.tsx
 
import Script from 'next/script'
 
export default function Dashboard() {
  return (
    <>
      <Script src="https://example.com/script.js" />
    </>
  )
}
```

---

## Props

`Script` 컴포넌트에서 사용 가능한 `props`의 요약입니다:

<table><thead><tr><th>Prop</th><th>Example</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td><a href="#src"><code>src</code></a></td><td><code>src="http://example.com/script"</code></td><td>String</td><td>Required unless inline script is used</td></tr><tr><td><a href="#strategy"><code>strategy</code></a></td><td><code>strategy="lazyOnload"</code></td><td>String</td><td>-</td></tr><tr><td><a href="#onload"><code>onLoad</code></a></td><td><code>onLoad={onLoadFunc}</code></td><td>Function</td><td>-</td></tr><tr><td><a href="#onready"><code>onReady</code></a></td><td><code>onReady={onReadyFunc}</code></td><td>Function</td><td>-</td></tr><tr><td><a href="#onerror"><code>onError</code></a></td><td><code>onError={onErrorFunc}</code></td><td>Function</td><td>-</td></tr></tbody></table>

---

## Required Props

`<Script />` 컴포넌트는 아래의 속성들이 필요합니다.

### `src`

외부 스크립트의 특정 문자열 경로입니다. ㅇ이것은 절대적인 외부 경로 또는 내부 경로일 수 있습니다. 인라인 스크립트를 사용하지 않는 한 `src` 속성은 필수입니다.

---

## Optional Props

`<Script />` 컴포넌트는 필수 속성 이외에도 여러 추가 소성을 허용합니다.

### `strategy`

스크립트의 로딩 전략입니다. 사용할 수 있는 네 가지 다른 전략이 있습니다.
- `beforeInteractive`: Next.js 코드와 페이지 하이드레이션 이전에 로드됩니다.
- `afterInteractive`: (default) 일부 하이드레이션 이후에 일찍 로드됩니다.
- `lazyOnload`: 브라우저의 유휴 시간에 로드됩니다.
- `worker`: (experimental) 웹 워커에서 로드됩니다.

### `beforeInteractive`

`beforeInteractive` 전략으로 로드되는 스크립트는 초기 HTML에 서버에서 주입되며, Next.js 모듈 이전에 다운로드되며, 페이지의 하이드레이션 이전에 배치된 순서대로 실행됩니다.

이 전략으로 표시된 스크립트는 첫 번째 파티 코드 이전에 사전로드되고 가져옵니다. 그러나 그 실행은 페이지 하이드레이션을 차단하지 않습니다.

`beforeInteractive` 스크립트는 루트 레이아웃(`app/layout.tsx`) 내부에 배치되어야 하며, 사이트 전체에서 필요한 스크립트를 로드하는 데 사용됩니다 (즉, 응용 프로그램의 어떤 페이지가 서버 측에서 로드된 경우 스크립트가 로드됩니다).

이 전략은 페이지의 일부가 상호 작용 가능해지기 전에 가져와야 하는 중요한 스크립트에만 사용해야 합니다.

```tsx
// app/layout.tsx
import Script from 'next/script'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script
        src="https://example.com/script.js"
        strategy="beforeInteractive"
      />
    </html>
  )
}
```

>알아두면 좋은 정보: `beforeInteractive`를 사용하는 스크립트는 컴포넌트 내에서 배치된 위치에 관계없이 항상 HTML 문서의 head 내에 주입됩니다.

`beforeInteractive`로 가능한 빨리 로드되어야 하는 스크립트의 몇 가지 예시는 다음과 같습니다:
- 봇 탐지기(Bot detectors)
- 쿠키 동의 관리자(Cookie consent managers)

### `afterInteractive`


`afterInteractive` 전략을 사용하는 스크립트는 HTML에 클라이언트 측에서 주입되며, 페이지의 일부(또는 전체) 하이드레이션이 발생한 후에 로드됩니다. 이는 Script 컴포넌트의 기본 전략이며, 가능한 빨리 로드되어야 하지만 첫 번째 파티 Next.js 코드보다 먼저 로드되지는 않아야 하는 모든 스크립트에 사용해야 합니다.

`afterInteractive` 스크립트는 페이지나 레이아웃 내부에 배치될 수 있으며, 해당 페이지(또는 그룹의 페이지)가 브라우저에서 열릴 때에만 로드되고 실행됩니다.

```jsx
// app/page.js
import Script from 'next/script'
 
export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="afterInteractive" />
    </>
  )
}
```


`afterInteractive`에 적합한 스크립트의 몇 가지 예시는 다음과 같습니다:

- 태그 관리자(Tag managers)
- 해석학(Analytics)

### `lazyOnload`


`lazyOnload` 전략을 사용하는 스크립트는 브라우저의 유휴 시간에 클라이언트 측 HTML에 주입되며, 페이지의 모든 리소스가 가져온 후에 로드됩니다. 이 전략은 조기에 로드할 필요가 없는 백그라운드 또는 우선순위가 낮은 스크립트에 사용되어야 합니다.

`lazyOnload` 스크립트는 페이지나 레이아웃 내부에 배치될 수 있으며, 해당 페이지(또는 그룹의 페이지)가 브라우저에서 열릴 때에만 로드되고 실행됩니다.

```jsx
// app/page.js
import Script from 'next/script'
 
export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="lazyOnload" />
    </>
  )
}
```

즉시 로드될 필요가 없고 `lazyOnload`로 가져올 수 있는 스크립트의 예시는 다음과 같습니다:

- 챗 서포트 플러그인(Chat support plugins)
- 소셜 미디어 위젯(Social media widgets)

### `worker`

>경고: worker 전략은 아직 안정화되지 않았으며, app 디렉토리와 함께 작동하지 않습니다. 주의해서 사용해주세요.

`worker` 전략을 사용하는 스크립트는 주요 스레드를 해제하고, 중요한 첫 번째 파티 리소스만 해당 스레드에서 처리되도록 웹 워커로 오프로드됩니다. 이 전략은 모든 스크립트에 사용될 수 있지만, 모든 서드 파티 스크립트를 지원하는 것은 보장되지 않는 고급 사용 사례입니다.

`worker` 전략을 사용하려면 `next.config.js`에서 `nextScriptWorkers` 플래그를 활성화해야 합니다:

```js
// next.config.js
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
}
```

현재 `worker` 스크립트는 `pages/` 디렉토리에서만 사용할 수 있습니다:

```tsx
// pages/home.tsx
import Script from 'next/script'
 
export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="worker" />
    </>
  )
}
```

### `onLoad`

>경고: `onLoad`는 현재 서버 컴포넌트에서 작동하지 않으며, 클라이언트 컴포넌트에서만 사용할 수 있습니다. 또한, `onLoad`는 `beforeInteractive`와 함께 사용할 수 없습니다. 대신 `onReady`를 사용하는 것을 고려해보세요.

일부 서드 파티 스크립트는 콘텐츠를 인스턴스화하거나 함수를 호출하기 위해 스크립트 로딩 후에 사용자가 JavaScript 코드를 한 번 실행해야 할 수 있습니다. afterInteractive 또는 lazyOnload로 스크립트를 로드하는 경우, onLoad 속성을 사용하여 로드된 후에 코드를 실행할 수 있습니다.

다음은 lodash 라이브러리가 로드된 후에 메소드를 실행하는 예시입니다:

```tsx
// app/page.tsx
'use client'
 
import Script from 'next/script'
 
export default function Page() {
  return (
    <>
      <Script
        src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"
        onLoad={() => {
          console.log(_.sample([1, 2, 3, 4]))
        }}
      />
    </>
  )
}
```

### `onReady`

>경고: `onReady`는 현재 서버 컴포넌트에서 작동하지 않으며, 클라이언트 컴포넌트에서만 사용할 수 있습니다.

일부 서드 파티 스크립트는 스크립트 로딩이 완료된 후에 그리고 컴포넌트가 마운트될 때마다(예: 경로 탐색 후) 사용자가 JavaScript 코드를 실행해야 할 수 있습니다. `onReady` 속성을 사용하여 스크립트의 로드 이벤트 이후에 코드를 실행하고, 이후에도 매 컴포넌트 재마운트 시에 실행할 수 있습니다.

다음은 컴포넌트가 마운트될 때마다 `Google Maps JS` 임베드를 다시 인스턴스화하는 예시입니다.

```tsx
// app/page.tsx
'use client'
 
import { useRef } from 'react'
import Script from 'next/script'
 
export default function Page() {
  const mapRef = useRef()
 
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
          })
        }}
      />
    </>
  )
}
```

### `onError`

>경고: `onError`는 현재 서버 컴포넌트에서 작동하지 않으며, 클라이언트 컴포넌트에서만 사용할 수 있습니다. `onError`는 `beforeInteractive` 로딩 전략과 함께 사용할 수 없습니다.


때로는 스크립트 로딩에 실패했을 때 이를 감지하는 것이 유용할 수 있습니다. 이러한 오류는 onError 속성을 사용하여 처리할 수 있습니다:

```tsx
// app/page.tsx
'use client'
 
import Script from 'next/script'
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onError={(e: Error) => {
          console.error('Script failed to load', e)
        }}
      />
    </>
  )
}
```