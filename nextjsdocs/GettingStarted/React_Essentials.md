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
