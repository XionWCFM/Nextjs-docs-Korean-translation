# ESLint

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/eslint](https://nextjs.org/docs/app/building-your-application/configuring/eslint)

Next.js는 기본적으로 통합된 [ESLint](https://eslint.org/) 경험을 제공합니다. `package.json`에 `next lint`를 스크립트로 추가하세요:

```js
"scripts": {
  "lint": "next lint"
}
```

그런 다음 `npm run lint` 또는 `yarn lint`를 실행하세요:

```bash
yarn lint
```

이미 응용 프로그램에서 ESLint를 구성하지 않은 경우 설치 및 구성 프로세스를 안내받게 될 것입니다.

```bash
yarn lint
```

> 다음과 같은 프롬프트가 표시됩니다:
<br>? How would you like to configure ESLint?
<br>❯ Strict (recommended) Base Cancel

다음 세 가지 옵션 중 하나를 선택할 수 있습니다:

- **Strict** : Next.js의 기본 ESLint 구성과 더 엄격한 [Core Web Vitals 규칙 세트](https://nextjs.org/docs/pages/building-your-application/configuring/eslint#core-web-vitals)가 포함됩니다. ESLint를 처음 설정하는 개발자에게 권장되는 구성입니다.

```js
{
  "extends": "next/core-web-vitals"
}
```

- **BASE** : Next.js의 기본 ESLint 구성이 포함됩니다.

```js
{
  "extends": "next"
}
```

- **Cancel** : 어떠한 ESLint 구성도 포함되지 않습니다. 사용자 고유의 사용자 정의 ESLint 구성을 설정할 계획인 경우에만이 옵션을 선택하세요.

두 가지 구성 옵션 중 하나가 선택되면, Next.js는 자동으로 응용 프로그램에 개발 종속성으로 `eslint`와 `eslint-config-next`를 설치하고 선택한 구성을 포함하는 프로젝트의 루트에 `.eslintrc.json` 파일을 생성합니다.

이제 ESLint를 실행하여 오류를 검출하는 모든 시간에 `next lint`를 실행할 수 있습니다. ESLint가 설정되면 빌드(`next build`) 중에 자동으로 실행됩니다. 오류는 빌드를 실패시키고 경고는 실패하지 않습니다.

> ESLint를 next build 중에 실행하지 않으려면 [Ignoring ESLint](https://nextjs.org/docs/pages/api-reference/next-config-js/eslint)에 대한 문서를 참조하십시오.

개발 중에 코드 편집기에서 직접 경고 및 오류를 보려면 적절한 [통합](https://eslint.org/docs/latest/use/integrations#editors) 기능을 사용하는 것이 좋습니다.

<hr>

## [ESLint Config](https://nextjs.org/docs/app/building-your-application/configuring/eslint#eslint-config)

기본 구성 (`eslint-config-next`)에는 Next.js에서 최적의 즉시 사용 가능한 린팅 경험을 위해 필요한 모든 구성이 포함되어 있습니다. 이미 응용 프로그램에서 ESLint를 구성하지 않았다면, 이 구성과 함께 ESLint를 설정하기 위해 `next lint`를 사용하는 것을 권장합니다.

>다른 ESLint 구성과 함께 `eslint-config-next`를 사용하려는 경우, 충돌이 발생하지 않도록하는 방법에 대해서는 [추가 구성](https://nextjs.org/docs/app/building-your-application/configuring/eslint#additional-configurations) 섹션을 참조하세요.

다음 ESLint 플러그인의 권장 규칙 세트는 모두 `eslint-config-next`에서 사용됩니다:

- [eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react)
- [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks)
- [eslint-plugin-next](https://www.npmjs.com/package/@next/eslint-plugin-next)

이는 `next.config.js`의 구성보다 우선합니다.

<hr>

## [ESLint Plugin](https://nextjs.org/docs/app/building-your-application/configuring/eslint#eslint-plugin)

Next.js는 기본 구성에 이미 번들된 [`eslint-plugin-next`](https://www.npmjs.com/package/@next/eslint-plugin-next)라는 ESLint 플러그인을 제공하여 Next.js 응용 프로그램에서 일반적인 문제와 문제를 감지할 수 있게 합니다. 전체 규칙 세트는 다음과 같습니다:

✓ 권장 구성에서 활성화됨

| 규칙 | 설명 |
| --- | --- |
| [@next/next/google-font-display](https://nextjs.org/docs/messages/google-font-display) | Google Fonts에서 font-display 동작을 강제합니다. | 
| [@next/next/google-font-preconnect](https://nextjs.org/docs/messages/google-font-preconnect) | Google Fonts에 `preconnect`를 사용하도록 합니다. |
| [@next/next/inline-script-id](https://nextjs.org/docs/messages/inline-script-id) | `next/script` 컴포넌트의 인라인 콘텐츠에 `id` 속성을 강제합니다. |
| [@next/next/next-script-for-ga](https://nextjs.org/docs/messages/next-script-for-ga) | Google Analytics의 인라인 스크립트를 사용할 때 `next/script` 컴포넌트를 선호합니다. |
| [@next/next/no-assign-module-variable](https://nextjs.org/docs/messages/no-assign-module-variable) | `모듈` 변수에 대한 할당을 방지합니다. |
| [@next/next/no-before-interactive-script-outside-document](https://nextjs.org/docs/messages/no-before-interactive-script-outside-document) | `pages/_document.js` 외부에서 `next/script`의 `beforeInteractive `전략 사용을 방지합니다. |
| [@next/next/no-css-tags](https://nextjs.org/docs/messages/no-css-tags) | 수동으로 스타일시트 태그를 사용하지 못하도록 합니다. |
| [@next/next/no-document-import-in-page](https://nextjs.org/docs/messages/no-document-import-in-page) | `pages/_document.js` 외부에서 `next/document`를 가져오는 것을 방지합니다. |
| [@next/next/no-duplicate-head](https://nextjs.org/docs/messages/no-duplicate-head) | `pages/_document.js`에서 `<Head>`의 중복 사용을 방지합니다. |
| [@next/next/no-head-element](https://nextjs.org/docs/messages/no-head-element) | `<head>` 요소 사용을 방지합니다. |
| [@next/next/no-head-import-in-document](https://nextjs.org/docs/messages/no-head-import-in-document) | `pages/_document.js`에서 `next/head` 사용을 방지합니다. |
| [@next/next/no-html-link-for-pages](https://nextjs.org/docs/messages/no-html-link-for-pages) | 내부 Next.js 페이지로 이동하기 위해 `<a>` 요소 사용을 방지합니다. |
| [@next/next/no-img-element](https://nextjs.org/docs/messages/no-img-element) | 더 느린 LCP와 더 높은 대역폭 때문에 `<img>` 요소 사용을 방지합니다. |
| [@next/next/no-page-custom-font](https://nextjs.org/docs/messages/no-page-custom-font) | 페이지 전용 사용자 정의 글꼴을 방지합니다. |
| [@next/next/no-script-component-in-head](https://nextjs.org/docs/messages/no-script-component-in-head) | `next/head` 컴포넌트에서 `next/script` 사용을 방지합니다. |
| [@next/next/no-styled-jsx-in-document](https://nextjs.org/docs/messages/no-styled-jsx-in-document) | `pages/_document.js`에서 `styled-jsx` 사용을 방지합니다. |
| [@next/next/no-sync-scripts](https://nextjs.org/docs/messages/no-sync-scripts) | 동기 스크립트 사용을 방지합니다. |
| [@next/next/no-title-in-document-head](https://nextjs.org/docs/messages/no-title-in-document-head) | `next/document`의 `Head` 컴포넌트에서 `<title>` 사용을 방지합니다. |
| @next/next/no-typos | [Next.js의 데이터 가져오기 함수](https://nextjs.org/docs/pages/building-your-application/data-fetching)에서 흔한 오타를 방지합니다. |
| [@next/next/no-unwanted-polyfillio](https://nextjs.org/docs/messages/no-unwanted-polyfillio) | Polyfill.io에서 중복된 폴리필을 방지합니다. |


애플리케이션에 ESLint가 이미 구성되어 있는 경우 몇 가지 조건이 충족되지 않는 한 `eslint-config-next`를 포함하지 않고 이 플러그인에서 직접 확장하는 것이 좋습니다. 자세한 내용은 [권장 플러그인 규칙 집합](https://nextjs.org/docs/pages/building-your-application/configuring/eslint#recommended-plugin-ruleset)을 참조하세요.

<br>

### [Custom Settings](https://nextjs.org/docs/app/building-your-application/configuring/eslint#custom-settings)

[`rootDir`](https://nextjs.org/docs/app/building-your-application/configuring/eslint#rootdir)

루트 디렉터리에 Next.js가 설치되지 않은 프로젝트(예: 모노레포)에서 `eslint-plugin-next`를 사용하는 경우, `.eslintrc`의 `설정` 속성을 사용하여 `eslint-plugin-next`에 Next.js 애플리케이션을 찾을 위치를 알려주면 됩니다:

```js
{
  "extends": "next",
  "settings": {
    "next": {
      "rootDir": "packages/my-app/"
    }
  }
}
```

`rootDir`은 경로(상대 또는 절대), 글로브(예: `"packages/*/"`) 또는 경로 및/또는 글로브의 배열일 수 있습니다.

<hr>

## [Linting Custom Directories and Files(사용자 지정 디렉터리 및 파일 린팅)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#linting-custom-directories-and-files)

기본적으로 Next.js는 `pages/`, `app`(실험적인 `appDir` 기능이 활성화된 경우에만), `components/`, `lib/`, `src/` 디렉터리의 모든 파일에 대해 ESLint를 실행합니다. 그러나 프로덕션 빌드의 경우 `next.config.js`의 `eslint` 구성에서 `dirs` 옵션을 사용하여 실행할 디렉터리를 지정할 수 있습니다:

```js
module.exports = {
  eslint: {
    dirs: ['pages', 'utils'], // Only run ESLint on the 'pages' and 'utils' directories during production builds (next build)
  },
}
```

마찬가지로 `--dir` 및 `--file` 플래그를 사용하여 다음 린트에 특정 디렉터리 및 파일을 린트할 수 있습니다:

```bash
next lint --dir pages --dir utils --file bar.js
```


<hr>

## [Caching(캐싱)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#caching)

성능 향상을 위해 ESLint가 처리하는 파일 정보는 기본적으로 캐시됩니다. 이 정보는 `.next/cache` 또는 정의한 [빌드 디렉터리](https://nextjs.org/docs/pages/api-reference/next-config-js/distDir)에 저장됩니다. 단일 소스 파일의 내용보다 더 많은 것에 의존하는 ESLint 규칙을 포함하고 캐시를 비활성화해야 하는 경우, `next lint`와 함께 `--no-cache` 플래그를 사용하세요.

```bash
next lint --no-cache
```

<hr>

## [Disabling Rules(규칙 비활성화하기)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#disabling-rules)

지원되는 플러그인(`react`, `react-hooks`, `next`)에서 제공하는 규칙을 수정하거나 비활성화하려면 `.eslintrc` 파일의 `rules` 속성을 사용하여 직접 변경할 수 있습니다:

```js
{
  "extends": "next",
  "rules": {
    "react/no-unescaped-entities": "off",
    "@next/next/no-page-custom-font": "off"
  }
}
```

## [Core Web Vitals(핵심 웹 바이탈)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#core-web-vitals)

`next lint`를 처음 실행하고 **strict** 옵션을 선택할 경우 `next/core-web-vitals` 규칙 세트가 활성화됩니다.

```js
{
  "extends": "next/core-web-vitals"
}
```
`next/core-web-vitals`은 [Core Web Vitals](https://web.dev/vitals/)에 영향을 미치는 규칙이 기본적으로 경고로 설정되어 있으면 에러로 업데이트하는 `eslint-plugin-next`을 포함합니다.

> [Create Next App](https://nextjs.org/docs/app/api-reference/create-next-app)으로 생성된 새로운 애플리케이션에는 자동으로 `next/core-web-vitals` 엔트리 포인트가 포함됩니다.


## [Usage With Other Tools(Prettier와 함께 사용하기)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#usage-with-other-tools)

### [Prettier](https://nextjs.org/docs/app/building-your-application/configuring/eslint#prettier)

ESLint에는 코드 포맷팅 규칙도 포함되어 있으며, 기존의 [Prettier](https://prettier.io/) 설정과 충돌할 수 있습니다. ESLint와 Prettier가 함께 작동할 수 있도록 [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)를 ESLint 구성에 포함하는 것을 권장합니다.

먼저 의존성을 설치하세요:

```bash
npm install --save-dev eslint-config-prettier
 
yarn add --dev eslint-config-prettier
```

그런 다음 기존의 ESLint 구성에 `prettier`를 추가하세요:

```js
{
  "extends": ["next", "prettier"]
}
```

### [lint-staged](https://nextjs.org/docs/app/building-your-application/configuring/eslint#lint-staged)

[lint-staged](https://github.com/okonet/lint-staged)를 사용하여 git의 staged 파일에서 린터를 실행하려면, 프로젝트 루트에 있는 `.lintstagedrc.js` 파일에 다음을 추가해야 합니다. 이렇게 하면 `--file` 플래그를 사용하도록 지정할 수 있습니다.


```js
const path = require('path')
 
const buildEslintCommand = (filenames) =>
  `next lint --fix --file ${filenames
    .map((f) => path.relative(process.cwd(), f))
    .join(' --file ')}`
 
module.exports = {
  '*.{js,jsx,ts,tsx}': [buildEslintCommand],
}

```

## [Migrating Existing Config(기존 구성 이전하기)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#migrating-existing-config)

### [Recommended Plugin Rulese(권장 플러그인 규칙 세트)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#recommended-plugin-ruleset)

이미 응용 프로그램에서 ESLint를 구성했으며 다음 중 하나 이상의 조건이 해당되는 경우:

- 다음 플러그인 중 하나 이상이 이미 설치되어 있음(별도로 또는 `airbnb` 또는 `react-app`과 같은 다른 구성을 통해):

  - react
  - react-hooks
  - jsx-a11y
  - import

- Next.js 내에서 Babel이 구성된 방식과 다른 특정 `parserOptions`를 정의했음([사용자 정의 Babel 구성](https://nextjs.org/docs/pages/building-your-application/configuring/babel)을 변경하지 않은 경우에는 권장하지 않음)
- Node.js 및/또는 TypeScript [resolver](https://github.com/import-js/eslint-plugin-import#resolvers)가 정의된 `eslint-plugin-import`가 설치되어 있음

이러한 설정을 유지하려는 경우, 이러한 속성이 [`eslint-config-next`](https://github.com/vercel/next.js/blob/canary/packages/eslint-config-next/index.js) 내에서 어떻게 구성되었는지에 대해 선호하는 경우에는 해당 설정을 제거하거나 Next.js ESLint 플러그인을 직접 확장하는 것을 권장합니다:

```js
module.exports = {
  extends: [
    //...
    'plugin:@next/next/recommended',
  ],
}
```

플러그인은 `next lint`를 실행하지 않고 프로젝트에 일반적으로 설치될 수 있습니다:

```bash
npm install --save-dev @next/eslint-plugin-next
 
yarn add --dev @next/eslint-plugin-next
```
이렇게 하면 여러 구성에서 동일한 플러그인이나 구문 분석기를 가져올 때 발생할 수 있는 충돌이나 오류의 위험이 사라집니다.

## [Additional Configurations(추가 구성)](https://nextjs.org/docs/app/building-your-application/configuring/eslint#additional-configurations)

이미 별도의 ESLint 구성을 사용 중이고 `eslint-config-next`를 포함하려는 경우, 다른 구성 다음에 마지막으로 확장해야 합니다. 예를 들어

```js
{
  "extends": ["eslint:recommended", "next"]
}
```

`next` 구성은 이미 `parser`, `plugins` 및 `settings` 속성에 대한 기본값을 설정합니다. 다른 구성을 사용하지 않는 한 이러한 속성을 수동으로 다시 선언할 필요는 없습니다. 사용 사례에 따라 다른 구성이 필요한 경우에만 이러한 속성을 변경해야 합니다.

다른 공유 가능한 구성을 포함하는 경우, **이러한 속성이 덮어쓰이거나 수정되지 않도록 확인해야 합니다.** 그렇지 않으면 next 구성과 동일한 동작을 공유하는 구성을 제거하거나 앞에서 언급한대로 Next.js ESLint 플러그인을 직접 확장하는 것을 권장합니다.



