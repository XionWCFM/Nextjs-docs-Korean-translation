- [원본 링크](https://beta.nextjs.org/docs/getting-started)

# Getting Started

> ### Next.js 문서는 두 개의 사이트로 나뉩니다: <br>
>
> <strong> [Next 13 App Router (beta) docs](https://beta.nextjs.org/docs/getting-started)<strong> (당신이 현재 읽고 있는 페이지입니다.): Try out React 서버, Components(컴포넌트), Streaming(스트리밍), new data fetching(새로운 데이터 패칭) 등을 소개합니다. <br> > <strong>[Next 13 (stable) docs](https://nextjs.org/docs)<strong> : 폰트 최적화, 이미지 업데이트, 링크(LINK) 와 스크립트 컴포넌트 등을 소개합니다. <br>

## App Router 소개

- 지난 몇 달 동안 Next.js 팀은 Next.js와 [React 서버 컴포넌트](../Building_Your_Application/Rendering/Server_and_Client_Components.md) 및 [React 18 기능](https://react.dev/blog/2022/03/29/react-v18)을 통합하기 위해 노력해왔습니다. 이제 새로운 앱 디렉토리에서 이러한 새로운 기능을 사용해 볼 수 있습니다.<br><br>
  > 🚧앱 라우터는 현재 베타 버전으로 운영 환경에서는 사용하지 않는 것이 좋습니다.

## 특징 개요(Features Overview)

다음은 [App Router](../Building_Your_Application/Routing/Fundamentals.md)의 새로운 기능에 대한 요약 입니다. :

<table><thead><tr><th>Features</th><th>What's New?</th></tr></thead><tbody><tr><td><a href="../Building_Your_Application/routing/fundamentals.md" class="relative">Routing</a></td><td>레이아웃, 중첩 라우팅, 로딩 상태, 오류 처리 등을 지원하는 서버 컴포넌트 위에 구축된 새로운 파일 시스템 기반 라우터입니다.</td></tr><tr><td><a href="/docs/rendering/fundamentals" class="relative">Rendering</a></td><td> Client 컴포넌트와 Server 컴포넌트를 활용한 Client-side 와 Server-side Rendering. 서버의 정적 렌더링과 동적 렌더링은 Next.js를 통해 더욱 최적화됩니다. Edge 및 Node.js runtime의 스트리밍.</td></tr><tr><td><a href="/docs/data-fetching/fundamentals" class="relative">Data Fetching</a></td><td>React 컴포넌트에서 <code class="inline">async</code>/<code class="inline">await</code>  지원을 통한 간소화된 데이터 불러오기 및 React 및 웹 플랫폼에 부합하는  <code class="inline">fetch()</code> API. </td></tr><tr><td>Caching</td><td>Server Components 와 client-side navigation에 최적화된 새로운 <a href="/docs/data-fetching/fundamentals#caching-data" class="relative">Next.js HTTP Cache</a> 와 <a href="/docs/routing/linking-and-navigating#client-side-caching-of-rendered-server-components" class="relative">client-side cache</a> </td></tr><tr><td>Optimizations</td><td>Improved <a href="/docs/optimizing/images" class="relative">Image Component</a> with native browser lazy loading. New <a href="/docs/optimizing/fonts" class="relative">Font Module</a> with automatic font optimization.</td></tr><tr><td><a href="/docs/api-reference/next-config#transpilepackages" class="relative">Transpilation</a></td><td>Automatic transpilation and bundling of dependencies from local packages (like monorepos) or from external dependencies (<code class="inline">node_modules</code>).</td></tr><tr><td>API</td><td>Updates to the API design throughout Next.js. Please refer to the API Reference Section for new APIs.</td></tr><tr><td>Tooling</td><td>Introducing <a href="https://turbo.build/pack" class="absolute" target="_blank" rel="noopener noreferrer">Turbopack</a>, up to 700x faster Rust-based Webpack replacement.</td></tr></tbody></table>
