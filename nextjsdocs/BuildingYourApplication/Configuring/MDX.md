# MDX

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/mdx](https://nextjs.org/docs/app/building-your-application/configuring/mdx)

[마크다운](https://daringfireball.net/projects/markdown/syntax)은 텍스트 서식을 지정하는 가벼운 마크업 언어입니다. 이를 사용하여 일반 텍스트 구문을 작성하고 이를 구조적으로 유효한 HTML로 변환할 수 있습니다. 웹 사이트와 블로그에서 콘텐츠를 작성하는 데 자주 사용됩니다.

당신이 작성하는 내용...

```html
I **love** using [Next.js](https://nextjs.org/)
```

결과:

```html
<p>I <strong>love</strong> using <a href="https://nextjs.org/">Next.js</a></p>
```

[MDX](https://mdxjs.com/)는 마크다운의 확장으로, 마크다운 파일 내에서 직접 [JSX](https://react.dev/learn/writing-markup-with-jsx)를 작성할 수 있는 기능을 제공합니다. 이는 동적 상호작용을 추가하고 컨텐츠 내에서 React 컴포넌트를 포함시키는 강력한 방법입니다.

Next.js는 응용 프로그램 내부에서 로컬 MDX 콘텐츠뿐만 아니라 서버에서 동적으로 가져온 원격 MDX 파일을 모두 지원할 수 있습니다. Next.js 플러그인은 마크다운과 React 컴포넌트를 HTML로 변환하며, 서버 컴포넌트(Server Components)에서도 사용할 수 있습니다(`app`의 기본 설정).

---

## **`[@next/mdx](https://nextjs.org/docs/app/building-your-application/configuring/mdx#nextmdx)`**

`@next/mdx` 패키지는 프로젝트의 루트에 있는 `next.config.js` 파일에서 구성됩니다. **이 패키지는 로컬 파일에서 데이터를 가져와** `/pages` 또는 `/app` 디렉터리에서 직접 `.mdx` 확장자를 가진 페이지를 생성할 수 있게 해줍니다.

---

## **[Getting Started](https://nextjs.org/docs/app/building-your-application/configuring/mdx#getting-started)**

Install the `@next/mdx` package:

```
npm install @next/mdx @mdx-js/loader @mdx-js/react @types/mdx
```

응용 프로그램의 루트(즉, `app/` 또는 `src/`의 부모 폴더)에 `mdx-components.tsx` 파일을 생성하세요:

mdx-components.tsx

```tsx
tsxCopy code
import type { MDXComponents } from 'mdx/types';

// 이 파일을 사용하면 MDX 파일에서 사용할 사용자 정의 React 컴포넌트를 제공할 수 있습니다.
// 다른 라이브러리의 컴포넌트를 가져와 사용할 수 있습니다.

// MDX를 `app` 디렉터리에서 사용하려면 이 파일이 필요합니다.
export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    // 내장된 컴포넌트를 사용자 정의할 수 있습니다. 예: 스타일 추가
    // h1: ({ children }) => <h1 style={{ fontSize: "100px" }}>{children}</h1>,
    ...components,
  };
}

```

`next.config.js` 파일을 업데이트하여 `mdxRs`를 사용하도록 설정하세요:

next.config.js

```tsx
jsCopy code
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    mdxRs: true,
  },
};

const withMDX = require('@next/mdx')();
module.exports = withMDX(nextConfig);
```

`app` 디렉터리에 MDX 내용을 담은 새 파일을 추가하세요:

app/hello.mdx

```markdown
Hello, Next.js!
 
You can import and use React components in MDX files.
```

MDX 파일을 `page` 내에서 가져와 내용을 표시하려면:

app/page.tsx

```tsx
tsxCopy code
import HelloWorld from './hello.mdx';

export default function Page() {
  return <HelloWorld />;
}
```

---

## **[Remote MDX](https://nextjs.org/docs/app/building-your-application/configuring/mdx#remote-mdx)**

앱의 마크다운 또는 MDX 파일이 응용 프로그램 내부에 없는 경우 서버에서 동적으로 가져올 수 있습니다. 이는 CMS나 기타 데이터 소스에서 콘텐츠를 가져올 때 유용합니다.

MDX 콘텐츠를 가져오기 위해 두 가지 인기 있는 커뮤니티 패키지가 있습니다: `next-mdx-remote`와 `contentlayer`. 아래 예시에서는 `next-mdx-remote`를 사용하는 방법을 보여줍니다:

**알아두면 좋은 정보**: 신중하게 진행해야 합니다. MDX는 JavaScript로 컴파일되며 서버에서 실행됩니다. 신뢰할 수 있는 소스에서만 MDX 콘텐츠를 가져와야 하며, 그렇지 않으면 원격 코드 실행(RCE) 문제가 발생할 수 있습니다.

app/page.tsx

```tsx
tsxCopy code
import { MDXRemote } from 'next-mdx-remote/rsc';

export default async function Home() {
  const res = await fetch('https://...');
  const markdown = await res.text();
  return <MDXRemote source={markdown} />;
}
```

---

## **[Layouts](https://nextjs.org/docs/app/building-your-application/configuring/mdx#layouts)**

MDX 콘텐츠 주변에 레이아웃을 공유하려면 [내장된 레이아웃 지원](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#layouts)과 App Router를 사용할 수 있습니다.

---

## **[Remark and Rehype Plugins](https://nextjs.org/docs/app/building-your-application/configuring/mdx#remark-and-rehype-plugins)**

MDX 콘텐츠를 변환하기 위해 `remark` 및 `rehype` 플러그인을 선택적으로 제공할 수 있습니다. 예를 들어, GitHub Flavored Markdown을 지원하려면 `remark-gfm`을 사용할 수 있습니다.

`remark` 및 `rehype` 생태계는 ESM(EcmaScript Modules)만 지원하기 때문에 구성 파일로 `next.config.mjs`를 사용해야 합니다.

next.config.mjs

```jsx
jsCopy code
import remarkGfm from 'remark-gfm';
import createMDX from '@next/mdx';

/** @type {import('next').NextConfig} */
const nextConfig = {};

const withMDX = createMDX({
  options: {
    extension: /\.mdx?$/,
    remarkPlugins: [remarkGfm],
    rehypePlugins: [],
    // `MDXProvider`를 사용하는 경우 다음 줄의 주석을 해제하세요.
    // providerImportSource: "@mdx-js/react",
  },
});
export default withMDX(nextConfig);
```

---

## **[Frontmatter](https://nextjs.org/docs/app/building-your-application/configuring/mdx#frontmatter)**

Frontmatter는 페이지에 관한 데이터를 저장하는 YAML과 유사한 키/값 쌍입니다. 그러나 `@next/mdx`는 기본적으로 frontmatter를 지원하지 **않습니다**. 그럼에도 불구하고, [gray-matter](https://github.com/jonschlinkert/gray-matter)와 같은 방법을 사용하여 MDX 콘텐츠에 frontmatter를 추가하는 방법이 있습니다.

`@next/mdx`를 사용하여 페이지 메타데이터에 액세스하려면 `.mdx` 파일 내에서 meta 객체를 내보내세요:

```
jsCopy code
export const meta = {
  author: 'Rich Haines',
};

# My MDX page

```

---

## **[Custom Elements](https://nextjs.org/docs/app/building-your-application/configuring/mdx#custom-elements)**

마크다운을 사용하는 것의 장점 중 하나는 원시 HTML 요소에 매핑되므로 쓰기가 빠르고 직관적이라는 점입니다:

```markdown
This is a list in markdown:

- One
- Two
- Three
```

위 코드는 다음과 같은 HTML을 생성합니다:

```html
<p>This is a list in markdown:</p>

<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

사용자 고유의 요소에 스타일을 적용하여 웹 사이트나 애플리케이션에 맞춤 느낌을 줄 때는 단축 코드를 전달할 수 있습니다. 이렇게 하려면 MDXProvider를 사용하고 컴포넌트 객체를 prop으로 전달하면 됩니다. 컴포넌트 객체 내의 각 객체 키는 HTML 요소 이름에 매핑됩니다.

`next.config.js`에서 `providerImportSource: "@mdx-js/react"`를 지정해야 활성화됩니다.

next.config.js

```jsx
const withMDX = require('@next/mdx')({
  // ...
  options: {
    providerImportSource: '@mdx-js/react',
  },
});
```

그런 다음 페이지에서 provider를 설정하세요:

pages/index.js

```jsx
import { MDXProvider } from '@mdx-js/react';
import Image from 'next/image';
import { Heading, InlineCode, Pre, Table, Text } from 'my-components';

const ResponsiveImage = (props) => (
  <Image
    alt={props.alt}
    sizes="100vw"
    style={{ width: '100%', height: 'auto' }}
    {...props}
  />
);

const components = {
  img: ResponsiveImage,
  h1: Heading.H1,
  h2: Heading.H2,
  p: Text,
  pre: Pre,
  code: InlineCode,
};

export default function Post(props) {
  return (
    <MDXProvider components={components}>
      <main {...props} />
    </MDXProvider>
  );
}
```

사이트 전체에서 사용하는 경우 `_app.js`에 제공자를 추가하여 모든 MDX 페이지가 사용자 정의 요소 구성을 가져올 수 있도록 할 수 있습니다.

---

## [심층 분석: 마크다운을 HTML로 어떻게 변환하나요?](https://nextjs.org/docs/app/building-your-application/configuring/mdx#deep-dive-how-do-you-transform-markdown-into-html)

React는 기본적으로 마크다운을 이해하지 않습니다. 마크다운 텍스트는 먼저 HTML로 변환되어야 합니다. 이를 위해 `remark`와 `rehype`를 사용하여 변환할 수 있습니다.

`remark`는 마크다운 주변의 도구 생태계입니다. `rehype`도 마찬가지로 HTML에 대한 것입니다. 예를 들어, 다음 코드 조각은 마크다운을 HTML로 변환합니다:

```jsx
import { unified } from 'unified';
import remarkParse from 'remark-parse';
import remarkRehype from 'remark-rehype';
import rehypeSanitize from 'rehype-sanitize';
import rehypeStringify from 'rehype-stringify';

main();

async function main() {
  const file = await unified()
    .use(remarkParse) // 마크다운 AST로 변환
    .use(remarkRehype) // HTML AST로 변환
    .use(rehypeSanitize) // HTML 입력을 정제
    .use(rehypeStringify) // AST를 직렬화된 HTML로 변환
    .process('Hello, Next.js!');

  console.log(String(file)); // <p>Hello, Next.js!</p>
}
```

`remark`와 `rehype` 생태계에는 [문법 강조 표시](https://github.com/atomiks/rehype-pretty-code), [제목 링크](https://github.com/rehypejs/rehype-autolink-headings), [목차 생성](https://github.com/remarkjs/remark-toc) 등을 위한 플러그인이 포함되어 있습니다.

아래에 표시된 대로 `@next/mdx`를 사용하는 경우 직접적으로 `remark` 또는 `rehype`를 사용할 필요가 **없습니다**. 이 작업은 자동으로 처리됩니다.

---

## [Rust 기반 MDX 컴파일러 사용(실험 중)](https://nextjs.org/docs/app/building-your-application/configuring/mdx#using-the-rust-based-mdx-compiler-experimental)

Next.js는 Rust로 작성된 새로운 MDX 컴파일러를 지원합니다. 이 컴파일러는 여전히 실험적이며 제품 환경에서 사용하는 것을 권장하지 않습니다. 새로운 컴파일러를 사용하려면 `withMDX`에 전달할 때 `next.config.js`를 구성해야 합니다:

next.config.js

```jsx
module.exports = withMDX({
  experimental: {
    mdxRs: true,
  },
});
```