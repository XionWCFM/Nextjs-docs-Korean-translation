- 공식 링크 : https://nextjs.org/docs/getting-started/react-essentials#thinking-in-server-components
# React Essentials 
Next.js로 어플리케이션을 만들때 React의 Server Component와 같은 새로운 기능에 익숙하다면, 많은 도움이 됩니다. 
이 페이지에서는 Server Component와 Client Components의 차이점, 언제 사용해야 하는지, 추천 패턴 등에 대해서 소개합니다.

React를 처음 사용하시는 분이라면, [React 공식문서](https://react.dev/learn)를 참조하는 것을 추천합니다.
- [React Tutorial](https://react.dev/learn/tutorial-tic-tac-toe)
- [Thinking in React](https://react.dev/learn/thinking-in-react)
- [Learn React](https://react.dev/learn/describing-the-ui)

# Server Components
Server Components 와 Client Components를 사용하면 개발자는 클라이언트 측 앱의 풍부한 상호 작용과 기존 서버 렌더링의 향상된
성능을 결합하여 서버와 클라이언트를 아우르는 애플리케이션을 구축할 수 있습니다.

## Thinking in Server Components
React가 UI를 구축하는 방식을 바꾼 것과 유사하게 React Server Components는 서버와 클라이언트를 활용하는 hybrid 어플리케이션을 구축하는 새로운 사고 모델을 제시합니다.

React는 client-side에서(SPA Single Page Applications) 전체 어플리케이션을 렌더링하는 방식을 사용하는 대신,
React는 이제 컴포넌트의 목적에 따라 서버와 클라이언트 중 어디서 렌더링 할지 유연하게 선택할 수 있습니다.

예를 들어 애플리케이션의 한 페이지를 생각해 보겠습니다.
![image](https://github.com/kd02109/Nextjs-docs-Korean-translation/assets/57277708/5e3edd5a-cafe-47fa-a092-9115783cda3c)

페이지를 더 작은 컴포넌트로 분할해서 대부분의 컴포넌트가 비반응형(non-interactive)이며 서버 컴포넌트로서 서버에서 렌더링할 수 있음을 알 수 있습니다.
더 작은 반응형 UI의 경우 Client Component로 렌더링할 수 있습니다. 이는 Next.js의 서버 우전 접근 방식과 일치합니다.

## Why Server Components?
왜 Server Components를 사용할까요? Client Component보다 Server Component를 사용했을 때의 이점은 무엇일까요?

Server Components를 통해 개발자는 서버 인프라를 더 잘 활용할 수 있습니다. 예를 들어 데이터 fetching을 DB 와 더 가까운 서버로 옮기고 클라이언트 JavaScript 번들 크기에 영향을 미친
대규모 종속성을 서버에 유지하여 성능을 개선할 수 있습니다. Server Components를 사용하면 React 사용을 마치 PHP나 Ruby on Rails와 비슷한 느낌을 받을 수 있지만, React의 강력한 성능과 유연성,
UI 템플릿을 위한 Component model을 사용할 수 있습니다.

Server Components를 사용하면 초기 페이지 로딩이 빨라지고 클라이언트 측 JavaScript 번들 크기가 줄어듭니다. 
기본 클라이언트 측 런타임은 캐싱이 가능하고 크기 예측이 가능하며 애플리케이션이 커져도 증가하지 않습니다.
추가적인 JavaScript는 오직 [Client Component](./React_Essentials.md#client-components)를 통해 client-side 반응이 있을 때만 추가됩니다. 

Next.j로 경로가 로드 될 때, 초기 HTML은 서버에서 렌더링됩니다. 이후에 비동기 적으로 Next.js 및 React 클라이언트 측 런타임을 로드해서 브라우저의 HTML이 client와 상호작용할 수 있도록
점진적으로 향상됩니다.

Server Components로 쉽게 전환할 수 있도록 특수 파일([special files](../BuildingYourApplication/Routing/Routing.md#file-conventions))과 [colocate components](../BuildingYourApplication/Routing/Routing.md#colocation)를 포함한 [App Router](../BuildingYourApplication/Routing/Routing.md#the-app-directory) 
내부의 모든 컴포넌트들은 기본적으로 Server Components입니다. 따라서 추가 작업 없이 자동으로 적용하여 즉시 뛰어난 성능을 얻을 수 있습니다. 또한 선택적으로 [`use client` 선언](./#React-Essentials.md#the-use-client-directive)을 사용하여 Client Component를 옵트인(opt-in)할 수 있습니다.

# Client Components
Client Components를 사용하면 client-side 반응형을 추가할 수 있습니다. Next.js에서는 서버에서 미리 렌더링([pre-rendered](../BuildingYourApplication/Routing/Routing.md))되고 클라이언트에서 hydrated(수화) 됩니다. Client Components는 항상 [Pages Router](../BuildingYourApplication/Routing/Routing.md)의 Component가 작동하는 방식입니다. 

## The "use Client" directive
[`"use client"`](https://github.com/reactjs/rfcs/pull/227) 선언은  서버와 클라이언트 컴포넌트 모듈 그래프 사이의 경계를 선언하는 규칙입니다.
```jsx
// app/counter.tsx
'use client'
 
import { useState } from 'react'
 
export default function Counter() {
  const [count, setCount] = useState(0)
 
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}

```
![image](https://github.com/kd02109/Nextjs-docs-Korean-translation/assets/57277708/d1fdca16-0b0c-4dd2-9223-ca2a8df6042f)

`"use client"`는 서버 전용 코드와 클라이언트 코드 사이에 위치합니다. 파일 상단의 import 문 위에 위치하여 서버 전용에서 클라이언트 부분으로 경계를 넘나드는 cut-off point를 정의합니다.
`"use client"`가 파일에 정의 되면, 하위 컴포넌트를 포함하여 가져온 다른 모든 모듈은 클라이언트 번들의 일부로 간주됩니다.

서버 컴포넌트가 기본값이므로 `"use client"` 선언으로 시작하는 모듈에서 정의하거나 import 하지 않는 한 모든 컴포넌트는 서버 컴포넌트 모듈 그래프의 일부가 됩니다.
> **Good to Know:**
>  - Server Component의 components는 서버에서만 렌더링되도록 보장됩니다.
>  - Client Component module graph의 Components는 기본적으로 클라이언트 측에서 렌더링 되지만, Next.js를 사용하면 서버에서 미리 렌더링한 후 클라이언트에서 하이드레이션(hydraterd)할 수도 있습니다.
>  - `"use client"`는 import 전에 **파일의 최 상단**에서 선언되어야 합니다.
>  - `"use client"`는 모든 파일에서 정의되어야 할 필요는 없습니다. 클라이언트 모듈 경계는 'entry point'(진입점)에서 한 번만 정의하면 되며, 이 경계로 import한 모든 모듈은 클라이언트 컴포넌트로 간주됩니다.

# When to use Sercer and Client Components?
서버 컴포넌트와 클라이언트 컴포넌트 간의 결정을 단순화하기 위해 클라이언트 컴포넌트에 대한 사용 사례가 있을 때까지는 서버 컴포넌트(`app` 디렉토리의 기본값)를 사용하는 것이 좋습니다.

이 표에는 서버 및 클라이언트 컴포넌트의 다양한 사용 사례가 요약되어 있습니다:

| What do you need to do? | Server Component | Client Component |
| --- | --- | --- |
| Fetch data (데이터 가져오기) | ✅ | ❌ |
| Keep sensitive information on the server(access tokens, API keys, etc) <br> 서버에 민감한 정보 보관  | ✅ | ❌ |
| Access backend resources (directly) <br>백엔드 자원에 접근(직접적으로) | ✅ | ❌ |
| Keep large dependencies on the server / Reduce client-side JavaScript <br> 서버에 대한 대규모 종속성 유지 / 클라이언트 측 자바스크립트 감소 | ✅ | ❌ |
| Add interactivity and event listeners (onClick(), onChange(), etc) <br> 반응형, 이벤트 리스너 추가하기 | ❌ | ✅ |
| Use State and Lifecycle Effects (useState(), useReducer(), useEffect(), etc) <br> useState 등 사용 | ❌ | ✅ |
| Use browser-only APIs <br> Browser API 사용 | ❌ | ✅ |
| Use custom hooks that depend on state, effects, or browser-only APIs <br> useState, useEffect 혹은 브라우저 API에 의존하는 custom hook 사용 | ❌ | ✅ |
| Use [React Class components](https://react.dev/reference/react/Component) <br> 리엑트 클래스 컴포넌트 사용 | ❌ | ✅ |

# Patterns

## Moving Client Components to the Leaves
애플리케이션의 성능을 행상시키려면 가능한 경우 클라이언트 컴포넌트를 컴포넌트 트리에서 잎이 위치한 곳에 두는 것이 좋습니다.

예를 들어 정적 요소(예: 로고, 링크 등)가 있는 레이아웃과 상태를 사용하는 대화형 검색창이 있을 수 있습니다.

전체 레이아웃을 클라이언트 컴포넌트로 만드는 대신 인터랙티브 로직을 클라이언트 컴포넌트(예: `<SearchBar />`)로 이동하고 레이아웃을 서버 컴포넌트로 유지하세요. 즉, 레이아웃의 모든 컴포넌트 자바스크립트를 클라이언트로 보낼 필요가 없습니다.
```jsx
// app/layout.tsx

// SearchBar is a Client Component
import SearchBar from './searchbar'
// Logo is a Server Component
import Logo from './logo'
 
// Layout is a Server Component by default
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <nav>
        <Logo />
        <SearchBar />
      </nav>
      <main>{children}</main>
    </>
  )
}
```

## Composing Client and Server Components
서버 컴포넌트와 클라이언트 컴포넌트는 동일한 컴포넌트 트리에서 결합할 수 있습니다.

백그라운드에서 React는 다음과 같이 렌더링을 처리합니다:
- 서버에서 React는 결과물을 클라이언트로 보내기 전에 모든 서버 컴포넌트를 렌더링합니다.
   -  여기에는 클라이언트 컴포넌트 안에 중첩된 서버 컴포넌트가 포함됩니다.
   -  이 단계에서 만나는 클라이언트 컴포넌트는 건너뜁니다.
-  클라이언트에서 React는 클라이언트 컴포넌트를 렌더링하고 서버 컴포넌트의 렌더링 결과에 슬롯을 생성하여 서버와 클라이언트에서 수행한 작업을 병합합니다.
    -  서버 컴포넌트가 클라이언트 컴포넌트 안에 중첩된 경우 렌더링된 콘텐츠가 클라이언트 컴포넌트 내에 올바르게 배치됩니다.
> Good to Know: Next.js에서 초기 페이지 로딩 중 서버 컴포넌트와 클라이언트 컴포넌트의 렌더링 결과가 모두 서버에서 HTML로 미리 렌더링 ([ pre-rendered on the server as HTML](../BuildingYourApplication/Rendering/))되어 초기 페이지 로드 속도가 빨라집니다.


## Nesting Server Components inside Client Components
위에서 설명한 렌더링 흐름을 고려할 때 서버 컴포넌트를 클라이언트 컴포넌트로 import하는 데는 제한이 있는데, 이 접근 방식은 추가 서버 왕복이 필요하기 때문입니다.

### Unsupported Pattern: Importing Server Components into Client Components
다음 패턴은 지원되지 않습니다. 서버 컴포넌트를 클라이언트 컴포넌트로 임포트할 수 없습니다:
```jsx
// app/example-client-component.txt
'use client'
 
// This pattern will **not** work!
// You cannot import a Server Component into a Client Component.
import ExampleServerComponent from './example-server-component'
 
export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
      <ExampleServerComponent />
    </>
  )
}
```
### Recommended Pattern: Passing Server Components to Client Components as Props
대신 클라이언트 컴포넌트를 디자인할 때 React prop을 사용하여 서버 컴포넌트의 *"slots"*을 표시할 수 있습니다.

서버 컴포넌트는 서버에서 렌더링되고 클라이언트 컴포넌트가 클라이언트에서 렌더링될 때 *"slot"은 서버 컴포넌트의 렌더링된 결과로 채워집니다.

일반적인 패턴은 React `children` prop를 사용해 "slot"을 만드는 것입니다. `children` prop을 허용하도록 <ExampleClientComponent>를 리팩토링하고 <ExampleClientComponent>의 import 및 명시적 중첩(explicit nesting)을 부모 컴포넌트로 이동할 수 있습니다.
```jsx
// app/example-client-component.tsx
'use client'
 
import { useState } from 'react'
 
export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
      {children}
    </>
  )
}
```
이제 <ExampleClientComponent>는 `children`이 무엇인지에 대한 지식이 없습니다. `children`이 결국 서버 컴포넌트의 결과로 채워질 것이라는 것을 알지 못합니다.

ExampleClientComponent가 가진 유일한 책임은 어떤 `children` 최종적으로 배치할지 결정하는 것입니다.

부모 서버 컴포넌트에서, `<ExampleClientComponent>`  와 `<ExampleServerComponent>` 를 import하고 `<ExampleClientComponent>` 에게 자식 요소로 `<ExampleServerComponent>`를 전달합니다.
```jsx
// This pattern works:
// You can pass a Server Component as a child or prop of a
// Client Component.
import ExampleClientComponent from './example-client-component'
import ExampleServerComponent from './example-server-component'
 
// Pages in Next.js are Server Components by default
export default function Page() {
  return (
    <ExampleClientComponent>
      <ExampleServerComponent />
    </ExampleClientComponent>
  )
}
```
이 접근 방식을 사용하면 <ExampleClientComponent>와 <ExampleServerComponent>의 렌더링이 분리되어 독립적으로 렌더링될 수 있으며, 클라이언트 컴포넌트보다 먼저 서버에서 렌더링되는 서버 컴포넌트에 맞춰 조정됩니다.
> Good to Know : 
> - 해당 패턴은 이미 [layouts 와 pages](../BuildingYourApplication/Routing/Pages_and_Layouts.md)에 `children` prop으로 적용되어 있습니다. 따라서 추가적인 wrapper component를 생성할 필요가 없습니다.
> - React 컴포넌트(JSX)를 다른 컴포넌트에 전달하는 것은 새로운 개념이 아니며 항상 React 컴포넌트 구성 모델의 일부로 사용되어 왔습니다.
> - 해당 composition 전략은 prop을 받는 컴포넌트가 prop이 무엇인지 알지 못하기 때문에 서버와 클라이언트 컴포넌트에서 모두 작동합니다. 전달받은 prop이 어디에 배치되어야 하는지만 책입집니다.
>    -  이러한 경우 클라이언트 컴포넌트가 클라이언트에서 렌더링 되기 전에 서버에서 전달된 prop이 독립적으로 렌더링 되는 것을 허용합니다.
>    -  중첩된 자식 컴포넌트를 다시 렌더링하는 부모 컴포넌트의 상태 변경을 피하기 위해 "상태 끌어올리기"라는 동일한 전략이 사용되었습니다.
> -  `children` prop에만 국한되지 않습니다. 어껀 prop이든 JSX를 전달하는 것에 사용할 수 있습니다.

## Passing props from Server to Client Components (Serialization 직렬화)
서버에서 클라이언트 컴포넌트로 전달되는 prop은 직렬화([Serialization](https://developer.mozilla.org/en-US/docs/Glossary/Serialization))가 가능해야 합니다. 즉, 함수, 날짜 등과 같은 값은 클라이언트 컴포넌트에 직접 전달할 수 없습니다.
> Where is the Network Boundary?
> App router에서 네트워크 경계는 서버 컴포넌트와 클라이언트 컴포넌 사이입니다. 이는 getStaticProps/getServerSideProps와 페이지 컴포넌트 사이에 경계가 있는 페이지와는 다릅니다. 서버 컴포넌트 내부에서 가져온 데이터는 클라이언트 컴포넌트로 전달되지 않는 한 네트워크 경계를 넘지 않으므로 직렬화할 필요가 없습니다. 서버 컴포넌트를 사용한 [data detching](../BuildingYourApplication/DataFetching/Fetching.md)에 대해 자세히 알아보세요.


## Keeping Server-Only Code out of Client Components (Poisoning)
자바스크립트 모듈은 서버와 클라이언트 컴포넌트 모두에서 공유할 수 있기 때문에 서버에서만 실행되어야 할 코드가 클라이언트에 슬쩍 들어올 수 있습니다.

예를 들어 다음 데이터 가져오기(data fetch) 함수를 살펴보겠습니다:
```jsx
// ilb/data.ts
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })
 
  return res.json()
}
```

언뜻 보기에는 getData가 서버와 클라이언트 모두에서 작동하는 것처럼 보입니다. 하지만 환경 변수 `API_KEY`의 접두사 앞에 `NEXT_PUBLIC`이 붙지 않기 때문에 서버에서만 액세스할 수 있는 비공개 변수입니다. Next.js는 클라이언트 코드에서 비공개 환경 변수를 빈 문자열로 대체하여 보안 정보 유출을 방지합니다.

결과적으로 getData()를 가져와서 클라이언트에서 실행할 수 있지만 예상대로 작동하지 않습니다. 또한 변수를 공개하면 함수가 클라이언트에서 작동하지만 민감한 정보가 유출될 수 있습니다.

따라서 이 함수는 서버에서만 실행되도록 의도하여 작성되었습니다.

## The "server only" 
이러한 종류의 의도치 않은 클라이언트 서버 코드 사용을 방지하기 위해 서버 전용(`server-only`) 패키지를 사용하면 다른 개발자가 실수로 이러한 모듈 중 하나를 클라이언트 컴포넌트로 임포트하는 경우 빌드 시에 오류를 발생시킬 수 있습니다.

서버 전용(`server-only`)을 사용하려면 먼저 패키지를 설치하세요:
```bash
npm install server-only
```
그런 다음 패키지를 서버 전용(server-only) 코드가 포함된 모듈로 가져옵니다:
```jsx
import 'server-only'
 
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })
 
  return res.json()
}
```
이제 getData()를 임포트하는 모든 클라이언트 컴포넌트는 이 모듈을 서버에서만 사용할 수 있다는 빌드 타임 오류를 받게 됩니다.

해당 패키지 클라이언트 전용(`client-only`)은 클라이언트 전용 코드가 포함된 모듈(예: `window` 개체에 액세스하는 코드)을 표시하는 데 사용할 수 있습니다.

## Data Fetching
클라이언트 컴포넌트에서 데이터를 가져올 수 있지만 클라이언트에서 데이터를 가져와야 하는 특별한 이유가 없는 한 서버 컴포넌트에서 데이터를 가져오는 것이 좋습니다. 데이터 가져오기를 서버로 옮기면 성능과 사용자 경험이 향상됩니다.
[Data detching에 대해서 학습하기](../BuildingYourApplication/DataFetching/Fetching.md)

## Third-party packages
서버 컴포넌트가 새롭게 출시된 기능이기 때문에 서트파티 패키지는 이제 막 useState, useEffecut 및 createContext 와 같은 클라이언트 전용 기능을 사용하는 컴포넌트에 `"use client"` 선언을 추가하고 있습니다.

오늘날 클라이언트 전용 기능을 사용하는 npm 패키지의 많은 컴포넌트에는 아직 이 지시어가 없습니다. 이러한 서드파티 컴포넌트에는 "use client" 지시어가 있기 때문에 자체 클라이언트 컴포넌트에서는 정상적으로 작동하지만 서버 컴포넌트에서는 작동하지 않습니다.

예를 들어, `<Carousel />` 컴포넌트가 있는 가상의 `acme-carousel` 패키지를 설치했다고 가정해 보겠습니다. 이 컴포넌트는 `useState`를 사용하지만 아직 `"use client"` 지시어가 없습니다.

클라이언트 컴포넌트 내에서 `<Carousel />`을 사용하는 경우 예상대로 작동합니다:
```jsx
'use client'
 
import { useState } from 'react'
import { Carousel } from 'acme-carousel'
 
export default function Gallery() {
  let [isOpen, setIsOpen] = useState(false)
 
  return (
    <div>
      <button onClick={() => setIsOpen(true)}>View pictures</button>
 
      {/* Works, since Carousel is used within a Client Component */}
      {isOpen && <Carousel />}
    </div>
  )
}
```

그러나 서버 컴포넌트 내에서 직접 사용하려고 하면 오류가 표시됩니다:

```jsx
import { Carousel } from 'acme-carousel'
 
export default function Page() {
  return (
    <div>
      <p>View pictures</p>
 
      {/* Error: `useState` can not be used within Server Components */}
      <Carousel />
    </div>
  )
}
```
이는 Next.js가 <Carousel />이 클라이언트 전용 기능을 사용하고 있다는 것을 알지 못하기 때문입니다. 

이 문제를 해결하려면 클라이언트 전용 기능에 의존하는 타사 컴포넌트를 자체 클라이언트 컴포넌트로 래핑할 수 있습니다:

```jsx
'use client'
 
import { Carousel } from 'acme-carousel'
 
export default Carousel
```
이제 서버 컴포넌트 내에서 <Carousel />을 직접 사용할 수 있습니다:
```jsx
import Carousel from './carousel'
 
export default function Page() {
  return (
    <div>
      <p>View pictures</p>
 
      {/*  Works, since Carousel is a Client Component */}
      <Carousel />
    </div>
  )
}
```
대부분의 서드파티 컴포넌트는 클라이언트 컴포넌트 내에서 사용할 가능성이 높으므로 래핑할 필요가 없을 것으로 예상됩니다. 그러나 한 가지 예외는 provider 컴포넌트인데, 이는 React 상태와 context에 의존하며 일반적으로 애플리케이션의 루트에 필요하기 때문입니다. [아래에서 context provider에 대해 자세히 알아보세요.]()

### Library Authors
- 비슷한 방식으로 다른 개발자가 사용할 패키지를 만드는 라이브러리 작성자는 `"use client"` 지시문을 사용하여 패키지의 클라이언트 진입점을 표시할 수 있습니다. 이를 통해 패키지 사용자는 래핑 경계를 만들지 않고도 패키지 컴포넌트를 서버 컴포넌트로 직접 가져올 수 있습니다.
- [트리의 더 깊은 곳에서  `"use client"`을 사용](../GettingStarted/React_Essentials.md#moving-client-components-to-the-leaves)하여 가져온 모듈이 서버 컴포넌트 모듈 그래프의 일부가 될 수 있도록 하여 패키지를 최적화할 수 있습니다.
- 일부 번들러는 `"use client"` 지시문을 제거할 수 있다는 점에 유의할 필요가 있습니다. [React Wrap Balancer](https://github.com/shuding/react-wrap-balancer/blob/main/tsup.config.ts#L10-L13) 및 [Vercel Analytics](https://github.com/vercel/analytics/blob/main/packages/web/tsup.config.js#L26-L30) 리포지토리에서 `"use client"` 지시문을 포함하도록 esbuild를 구성하는 방법에 대한 예제를 찾을 수 있습니다.

# Context
대부분의 React 애플리케이션은 [context](https://react.dev/reference/react/useContext)에 의존해 컴포넌트 간에 데이터를 공유하는데, 이는 [createContext](https://react.dev/reference/react/useContext)를 통해 직접적으로 또는 타사 라이브러리에서 가져온 provider 컴포넌트를 통해 간접적으로 이루어집니다.

Next.js 13에서는 context가 클라이언트 컴포넌트 내에서 완벽하게 지원되지만 서버 컴포넌트 내에서 직접 생성하거나 사용할 수는 없습니다. 서버 컴포넌트에는 (반응형이 아닙니다.) React state가 없고, context는 주로 일부 React state가 업데이트된 후 트리 깊숙한 곳에 있는 반응형 컴포넌트를 다시 렌더링하는 데 사용되기 때문입니다.

서버 컴포넌트 간 데이터 공유에 대한 대안은 나중에 논의하겠지만, 먼저 클라이언트 컴포넌트 내에서 컨텍스트를 사용하는 방법을 살펴봅시다.

## Using context in Client Components
모든 context API는 클라이언트 컴포넌트 내에서 완벽하게 지원됩니다:
```jsx
// app/sidebar.tsx
'use client'
 
import { createContext, useContext, useState } from 'react'
 
const SidebarContext = createContext()
 
export function Sidebar() {
  const [isOpen, setIsOpen] = useState()
 
  return (
    <SidebarContext.Provider value={{ isOpen }}>
      <SidebarNav />
    </SidebarContext.Provider>
  )
}
 
function SidebarNav() {
  let { isOpen } = useContext(SidebarContext)
 
  return (
    <div>
      <p>Home</p>
 
      {isOpen && <Subnav />}
    </div>
  )
}
```
그러나 context provider는 일반적으로 현재 테마와 같은 글로벌 관심사를 공유하기 위해 애플리케이션의 루트 근처에 렌더링됩니다. 서버 컴포넌트에서는 컨텍스트가 지원되지 않으므로 애플리케이션의 루트에서 컨텍스트를 만들려고 하면 오류가 발생합니다:
```jsx
// app/layout.tsx
import { createContext } from 'react'
 
//  createContext is not supported in Server Components
export const ThemeContext = createContext({})
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
      </body>
    </html>
  )
}
```
이 문제를 해결하려면 컨텍스트를 생성하고 클라이언트 컴포넌트 내에서 해당 provider를 렌더링하세요:
```jsx
// app/theme-provider.tsx
'use client'
 
import { createContext } from 'react'
 
export const ThemeContext = createContext({})
 
export default function ThemeProvider({ children }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
}
```
이제 서버 컴포넌트가 클라이언트 컴포넌트로 표시되었으므로 공급자를 직접 렌더링할 수 있습니다:
```jsx
// app/layout.tsx
import ThemeProvider from './theme-provider'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```
provider가 루트에서 렌더링되면 앱의 다른 모든 클라이언트 컴포넌트가 이 컨텍스트를 사용할 수 있습니다.
> Goot to Know: provider를 트리에서 가능한 한 깊숙이 렌더링해야 합니다. `ThemeProvider`가 전체 <html> 문서 대신 {children}만 래핑하는 방식에 주목하세요. 이렇게 하면 Next.js가 서버 컴포넌트의 정적 부분을 더 쉽게 최적화할 수 있습니다.

## Rendering third-party context providers in Server Components
타사 npm 패키지에는 애플리케이션의 루트 근처에서 렌더링해야 하는 Provider가 포함되어 있는 경우가 많습니다. 이러한 공급자에 `"use client"` 지시어가 포함되어 있으면 서버 컴포넌트 내부에서 직접 렌더링할 수 있습니다. 하지만 서버 컴포넌트는 매우 새롭기 때문에 많은 서드파티 Provider가 아직 이 지시문을 추가하지 않았을 것입니다.

`'use-client'`이 없는 타사 Provider를 렌더링하려고 하면 오류가 발생합니다:
```jsx
// app/layout.tsx
import { ThemeProvider } from 'acme-theme'
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {/*  Error: `createContext` can't be used in Server Components */}
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```
이 문제를 해결하려면 third-party providers를 자체 클라이언트 컴포넌트로 래핑하세요:
```jsx
// app/providers.js
'use client'
 
import { ThemeProvider } from 'acme-theme'
import { AuthProvider } from 'acme-auth'
 
export function Providers({ children }) {
  return (
    <ThemeProvider>
      <AuthProvider>{children}</AuthProvider>
    </ThemeProvider>
  )
}
```
이제 루트 레이아웃 내에서 <Provider />를 직접 가져와 렌더링할 수 있습니다.
```jsx
// app/layout.js
import { Providers } from './providers'
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  )
}
```
루트에서 렌더링된 providers를 사용하면 이러한 라이브러리의 모든 컴포넌트와 hook이 자체 클라이언트 컴포넌트 내에서 예상대로 작동합니다.

타사 라이브러리가 클라이언트 코드에 `"use-client"`을 추가하면 래퍼 클라이언트 컴포넌트를 제거할 수 있습니다.

## Sharing data between Server Components
서버 컴포넌트는 반응향이 아니기 때문에 React state를 사용하지 않습니다. 따라서 데이터를 공유하기 위해 React context가 필요하지 않습니다. 대신 여러 서버 컴포넌트가 액세스해야 하는 공통 데이터에 native JavaScript 패턴을 사용할 수 있습니다. 예를 들어 모듈을 사용하여 여러 컴포넌트에서 데이터베이스 연결을 공유할 수 있습니다:
```ts
// utils/database.ts
export const db = new DatabaseConnection()
```
```ts
// app/users/layout.tsx
import { db } from '@utils/database'
 
export async function UsersLayout() {
  let users = await db.query()
  // ...
}
```
```ts
// app/users/[id]/page.tsx
import { db } from '@utils/database'
 
export async function DashboardPage() {
  let user = await db.query()
  // ...
}
```
위의 예제에서는 레이아웃과 페이지 모두 데이터베이스 쿼리를 수행해야 합니다. 이러한 각 컴포넌트는 @utils/database 모듈을 가져와 데이터베이스에 대한 액세스를 공유합니다. 이 자바스크립트 패턴을 글로벌 싱글톤(global singletons)이라고 합니다.

## Sharing fetch requests between Server Components
데이터를 가져올 때 `page` 또는 `layout`과 일부 하위 컴포넌트 간에 `fetch` 결과를 공유할 수 있습니다. 이는 구성 요소 간에 불필요한 결합을 유발하며 구성 요소 간에 prop이 양방향으로 전달될 수 있습니다.

대신 데이터를 사용하는 컴포넌트와  데이터 fetch를 함께 배치하는 것이 좋습니다. [`fetch` 요청은 서버 컴포넌트에 자동으로 메모리화됩니다](../BuildingYourApplication/DataFetching/Caching.md), 따라서 각 route segment는 중복 요청에 대한 걱정 없이 필요한 데이터를 정확하게 요청할 수 있습니다.  Next.js는 `fetch` 캐시에서 동일한 값을 읽습니다.
