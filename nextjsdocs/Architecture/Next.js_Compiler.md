# Next.js Compiler

<br>

[SWC](https://swc.rs/)를 사용하여 Rust로 작성된 Next.js 컴파일러는 프로덕션을 위해 JavaScript 코드를 변환하고 축소할 수 있습니다. 이 컴파일러는 개별 파일의 경우 Babel을, 출력 번들을 축소하는 경우 Terser를 대체합니다.

Next.js 컴파일러를 사용한 컴파일은 Babel보다 17배 빠르며, Next.js 버전 12부터 기본적으로 활성화되어 있습니다. 기존 Babel 구성이 있거나 [지원되지 않는 기능](./Next.js_Compiler.md#unsupported-features)을 사용 중인 경우 애플리케이션은 Next.js 컴파일러를 옵트아웃하고 계속 Babel을 사용합니다.

<br>

---

<br>

## Why SWC?

<br>

[SWC](https://swc.rs/)는 차세대 고속 개발자 도구를 위한 확장 가능한 Rust 기반 플랫폼입니다.

SWC는 컴파일, 축소, 번들링 등에 사용할 수 있으며 확장할 수 있도록 설계되었습니다. 코드 변환(기본 제공 또는 사용자 지정)을 수행하기 위해 호출할 수 있습니다. 이러한 변환을 실행하는 것은 Next.js와 같은 상위 수준의 도구를 통해 이루어집니다.

몇 가지 이유로 SWC를 기반으로 구축하기로 결정했습니다:

- 확장성: SWC는 라이브러리를 포크하거나 디자인 제약을 해결할 필요 없이 Next.js 내부에서 크레이트로 사용할 수 있습니다.
- 성능: SWC로 전환한 후 Next.js에서 최대 3배 빠른 빠른 새로고침과 최대 5배 빠른 빌드를 달성할 수 있었으며, 아직 최적화가 더 진행 중입니다.
- 웹어셈블리: 가능한 모든 플랫폼을 지원하고 어디서나 Next.js 개발을 수행하는 데 있어 Rust의 WASM 지원은 필수적입니다.
- 커뮤니티: Rust 커뮤니티와 에코시스템은 놀랍고 계속 성장하고 있습니다.

<br>

---

<br>

## Supported Features

<br>

### Styled Components

`babel 플러그인 스타일 컴포넌트`를 Next.js 컴파일러로 포팅하는 작업을 진행 중입니다.

먼저, 최신 버전의 Next.js로 업데이트합니다: `npm install next@latest`. 그런 다음 `next.config.js` 파일을 업데이트합니다:

```js
// next.config.js
module.exports = {
  compiler: {
    // 옵션에 대한 자세한 내용은 https://styled-components.com/docs/tooling#babel-plugin 를 참조하세요.
    styledComponents: boolean | {
      // 개발 환경에서는 기본적으로 활성화되며, 프로덕션 환경에서는 파일 크기를 줄이기 위해 비활성화됩니다,
      // 이 옵션을 설정하면 모든 환경에서 기본값이 재정의됩니다.
      displayName?: boolean,
      // 기본적으로 활성화되어 있습니다.
      ssr?: boolean,
      // 기본적으로 활성화되어 있습니다.
      fileName?: boolean,
      // 기본적으로 비어 있습니다.
      topLevelImportPaths?: string[],
      // 기본값은 ["index"]입니다.
      meaninglessFileNames?: string[],
      // 기본적으로 활성화되어 있습니다.
      cssProp?: boolean,
      // 기본적으로 비어 있습니다.
      namespace?: string,
      // 아직 지원되지 않습니다.
      minify?: boolean,
      // 아직 지원되지 않습니다.
      transpileTemplateLiterals?: boolean,
      // 아직 지원되지 않습니다.
      pure?: boolean,
    },
  },
}
```

`minify`, `transpileTemplateLiterals` 및 `pure`는 아직 구현되지 않았습니다. 진행 상황은 [여기](https://github.com/vercel/next.js/issues/30802)에서 확인할 수 있습니다. Next.js에서 `스타일 컴포넌트`를 사용하기 위한 주요 요구 사항은 `ssr` 및 `displayName` 트랜스폼입니다.

<br>

### Jest

Next.js 컴파일러는 테스트를 트랜스파일링하고 다음을 포함하여 Next.js와 함께 Jest 구성을 간소화합니다:

- `.css`, `.module.css`(및 해당 `.scss` 변형) 및 이미지 가져오기 자동 모킹
- SWC를 사용하여 `transform` 자동 설정
- `.env`(및 모든 변형)를 `process.env`에 로드
- 테스트 확인 및 변환에서 `node_modules` 무시
- 테스트 확인에서 `.next` 무시
- 실험적 SWC 트랜스폼을 활성화하는 플래그를 위해 `next.config.js`를 로드합니다.

먼저 Next.js의 최신 버전으로 업데이트합니다: `npm install next@latest`. 그런 다음 `jest.config.js` 파일을 업데이트합니다:

```js
// jest.config.js
const nextJest = require("next/jest");

// 다음.config.js 및 .env 파일을 로드할 수 있는 Next.js 앱의 경로를 제공합니다.
const createJestConfig = nextJest({ dir: "./" });

// Jest에 전달하려는 모든 사용자 지정 설정
const customJestConfig = {
  setupFilesAfterEnv: ["<rootDir>/jest.setup.js"],
};

// createJestConfig는 이러한 방식으로 내보내 다음/jest가 비동기식인 Next.js 구성을 로드할 수 있도록 합니다.
module.exports = createJestConfig(customJestConfig);
```

<br>

### Relay

[Relay](https://relay.dev/) 지원을 사용하려면:

```js
// next.config.js
module.exports = {
  compiler: {
    relay: {
      // 이것은 relay.config.js와 일치해야 합니다.
      src: "./",
      artifactDirectory: "./__generated__",
      language: "typescript",
      eagerEsModules: false,
    },
  },
};
```

> 참고: Next.js에서는 `pages` 디렉토리에 있는 모든 JavaScript 파일이 경로로 간주됩니다. 따라서 `relay-compiler`의 경우 `pages` 외부에서 `artifactDirectory` 구성 설정을 지정해야 하며, 그렇지 않으면 `relay-compiler`가 `__generated__` 디렉터리의 소스 파일 옆에 파일을 생성하고 이 파일이 경로로 간주되어 프로덕션 빌드가 중단될 수 있습니다.

<br>

### Remove React Properties

JSX 속성을 제거할 수 있습니다. 테스트에 자주 사용됩니다. `babel-plugin-react-remove-properties`와 유사합니다.

기본 정규식 `^data-test`와 일치하는 속성을 제거합니다:

```js
// next.config.js
module.exports = {
  compiler: {
    reactRemoveProperties: true,
  },
};
```

사용자 지정 속성을 제거합니다:

```js
// next.config.js
module.exports = {
  compiler: {
    // 여기에 정의된 정규식은 Rust에서 처리되므로 구문은 다음과 다릅니다.
    // 자바스크립트 `RegExp`와 다릅니다. https://docs.rs/regex 참조.
    reactRemoveProperties: { properties: ["^data-custom$"] },
  },
};
```

<br>

### Remove Console

이 트랜스폼을 사용하면 애플리케이션 코드에서 모든 `console.*` 호출을 제거할 수 있습니다(`node_modules`가 아님). `babel-plugin-transform-remove-console`과 유사합니다.

모든 `console.*` 호출을 제거합니다:

```js
// next.config.js
module.exports = {
  compiler: {
    removeConsole: true,
  },
};
```

`console.*` 오류를 제외한 `console.error` 출력을 제거합니다:

```js
// next.config.js
module.exports = {
  compiler: {
    removeConsole: {
      exclude: ["error"],
    },
  },
};
```

<br>

### Legacy Decorators

Next.js는 `jsconfig.json` 또는 `tsconfig.json`에서 `experimentalDecorators`를 자동으로 감지합니다. `Legacy Decorators`는 일반적으로 `mobx와` 같은 이전 버전의 라이브러리에서 사용됩니다.

이 플래그는 기존 애플리케이션과의 호환성을 위해서만 지원됩니다. 새 애플리케이션에서 `Legacy Decorators`를 사용하지 않는 것이 좋습니다.

먼저 Next.js의 최신 버전으로 업데이트하세요: `npm install next@latest`. 그런 다음 `jsconfig.json` 또는 `tsconfig.json` 파일을 업데이트합니다:

```js
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

<br>

### importSource

Next.js는 `jsconfig.json` 또는 `tsconfig.json`에서 `jsxImportSource`를 자동으로 감지하여 이를 적용합니다. 이는 [Theme UI](https://theme-ui.com/)와 같은 라이브러리에서 일반적으로 사용됩니다.

먼저 Next.js의 최신 버전으로 업데이트합니다: npm install next@latest. 그런 다음 jsconfig.json 또는 tsconfig.json 파일을 업데이트합니다:

```js
{
  "compilerOptions": {
    "jsxImportSource": "theme-ui"
  }
}
```

<br>

### Emotion

Next.js 컴파일러에 `@emotion/babel-plugin`을 포팅하는 작업을 진행 중입니다.

먼저 다음 버전의 Next.js를 최신 버전으로 업데이트합니다: `npm install next@latest`. 그런 다음 `next.config.js` 파일을 업데이트합니다:

```js
// next.config.js

module.exports = {
  compiler: {
    emotion: boolean | {
      // 기본값은 true입니다. 빌드 유형이 프로덕션인 경우 비활성화됩니다.
      sourceMap?: boolean,
      // 기본값은 'dev-only'입니다.
      autoLabel?: 'never' | 'dev-only' | 'always',
      // 기본값은 '[local]'입니다.
      // 허용되는 값: '[local]` `[filename]` 및 `[dirname]`
      // 이 옵션은 자동 레이블이 'dev-only' 또는 'always'로 설정된 경우에만 작동합니다.
      // 결과 레이블의 형식을 정의할 수 있습니다.
      // 형식은 문자열을 통해 정의되며, 변수 부분은 대괄호 []로 묶습니다.
      // 예를 들어 labelFormat: "my-classname--[local]", 여기서 [local]은 결과가 할당되는 변수의 이름으로 대체됩니다.
      labelFormat?: string,
      // 기본값은 정의되지 않았습니다.
      // 이 옵션을 사용하면 컴파일러가 어떤 임포트를 가져와야 하는지 알려줄 수 있습니다.
      // 무엇을 변환할지 결정하기 위해 컴파일러에 알려줄 수 있습니다.
      // 이모션을 익스포트해도, 트랜스폼을 계속 사용할 수 있습니다.
      importMap?: {
        [packageName: string]: {
          [exportName: string]: {
            canonicalImport?: [string, string],
            styledBaseImport?: [string, string],
          }
        }
      },
    },
  },
}
```

<br>

### Minification

Next.js의 swc 컴파일러는 v13부터 기본적으로 축소에 사용됩니다. 이는 Terser보다 7배 빠릅니다.

어떤 이유로든 Terser가 여전히 필요한 경우 이를 구성할 수 있습니다.

```js
// next.config.js
module.exports = {
  swcMinify: false,
};
```

<br>

### Module Transpilation

Next.js는 로컬 패키지(예: monorepos) 또는 외부 종속성(예: `node_modules`)에서 종속성을 자동으로 트랜스파일하고 번들링할 수 있습니다. 이렇게 하면 `next-transpile-modules` 패키지가 대체됩니다.

```js
// next.config.js
module.exports = {
  transpilePackages: ["@acme/ui", "lodash-es"],
};
```

<br>

### Modularize Imports

<br>

> Example - [modularize-imports](https://github.com/vercel/next.js/tree/canary/examples/modularize-imports)

[babel-plugin-transform-imports](https://www.npmjs.com/package/babel-plugin-transform-imports)와 유사하게 imports를 모듈화할 수 있습니다.

"barrel file"(다른 모듈을 다시 내보내는 단일 파일)을 사용하는 패키지의 멤버 스타일 imports를 변환합니다:

```js
import { Row, Grid as MyGrid } from "react-bootstrap";
import { merge } from "lodash";
```

...를 각 모듈의 기본 스타일 임포트에 추가합니다. 이렇게 하면 사용하지 않는 모듈의 컴파일을 방지할 수 있습니다:

```js
import Row from "react-bootstrap/Row";
import MyGrid from "react-bootstrap/Grid";
import merge from "lodash/merge";
```

위의 트랜스폼에 대한 구성입니다:

```js
// next.config.js
module.exports = {
  modularizeImports: {
    "react-bootstrap": {
      transform: "react-bootstrap/{{member}}",
    },
    lodash: {
      transform: "lodash/{{member}}",
    },
  },
};
```

<br>

#### Handlebars variables and helper functions

<br>

이 트랜스폼은 [handlebars](https://docs.rs/handlebars/latest/handlebars/)를 사용하여 `transform` 필드에서 대체 import 경로를 템플릿으로 만듭니다. 이러한 변수와 도우미 함수를 사용할 수 있습니다:

1. `member`: 문자열 유형입니다. member import의 이름입니다.
2. `lowerCase`, `upperCase`, `camelCase`, `kebabCase`: 문자열을 소문자, 대문자, 카멜 또는 케밥 케이스로 변환하는 도우미 함수입니다.
3. `matches`: `string[]` 타입을 가집니다. 정규식과 일치하는 모든 그룹입니다. `matches.[0]`은 전체 일치입니다.
   예를 들어 다음과 같이 kebabCase 헬퍼를 사용할 수 있습니다:

예를 들어 다음과 같이 `kebabCase` 헬퍼를 사용할 수 있습니다:

```js
// next.config.js
module.exports = {
  modularizeImports: {
    "my-library": {
      transform: "my-library/{{ kebabCase member }}",
    },
  },
};
```

위의 구성은 코드를 다음과 같이 변환합니다:

```js
// Before
import { MyModule } from "my-library";

// After (`MyModule` was converted to `my-module`)
import MyModule from "my-library/my-module";
```

Rust [regex](https://docs.rs/regex/latest/regex/) 상자의 구문을 사용하여 정규식을 사용할 수도 있습니다:

```js
// next.config.js
module.exports = {
  modularizeImports: {
    "my-library/?(((\\w*)?/?)*)": {
      transform: "my-library/{{ matches.[1] }}/{{member}}",
    },
  },
};
```

위의 구성은 코드를 다음과 같이 변환합니다:

```js
// Before
import { MyModule } from "my-library";
import { App } from "my-library/components";
import { Header, Footer } from "my-library/components/app";

// After
import MyModule from "my-library/my-module";
import App from "my-library/components/app";
import Header from "my-library/components/app/header";
import Footer from "my-library/components/app/footer";
```

<br>

#### Using named imports

<br>

기본적으로 `modularizeImports`는 각 모듈이 기본 내보내기를 사용한다고 가정합니다.

그러나 항상 그런 것은 아니며 명명된 내보내기를 사용할 수도 있습니다.

```js
// my-library/MyModule.ts

// Using named export instead of default export
export const MyModule = {};

// my-library/index.ts
// The “barrel file” that re-exports `MyModule`
export { MyModule } from "./MyModule";
```

이 경우 `skipDefaultConversion` 옵션을 사용하여 default imports 대신 named imports를 사용할 수 있습니다:

```js
// next.config.js
module.exports = {
  modularizeImports: {
    "my-library": {
      transform: "my-library/{{member}}",
      skipDefaultConversion: true,
    },
  },
};
```

위의 구성은 코드를 다음과 같이 변환합니다:

```js
// Before
import { MyModule } from "my-library";

// After (imports `MyModule` using named import)
import { MyModule } from "my-library/MyModule";
```

<br>

#### Preventing full import

<br>

`preventFullImport` 옵션을 사용하는 경우 default import를 사용하여 "barrel file"을 가져오면 컴파일러에서 오류가 발생합니다. 다음 구성을 사용하는 경우:

```js
// next.config.js
module.exports = {
  modularizeImports: {
    lodash: {
      transform: "lodash/{{member}}",
      preventFullImport: true,
    },
  },
};
```

(named imports를 사용하지 않고) 전체 `lodash` 라이브러리를 가져오려고 하면 컴파일러에서 오류가 발생합니다:

```js
// Compiler error
import lodash from "lodash";
```

<br>

---

<br>

## Experimental Features

<br>

### SWC Trace profiling

<br>

SWC의 internal transform traces를 chromium의 [trace event format](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview?mode=html#%21=)으로 생성할 수 있습니다.

```js
// next.config.js
module.exports = {
  experimental: {
    swcTraceProfiling: true,
  },
};
```

활성화하면 swc는 `.next/`에 `swc-trace-profile-${timestamp}.json`이라는 이름의 트레이스를 생성합니다. Chromium의 트레이스 뷰어(chrome://tracing/, [https://ui.perfetto.dev/](https://ui.perfetto.dev/)) 또는 호환되는 플레임그래프 뷰어([https://www.speedscope.app/](https://www.speedscope.app/))에서 생성된 트레이스의 로드 및 시각화를 수행할 수 있습니다.

<br>

### SWC Plugins (Experimental)

<br>

변환 동작을 사용자 정의하기 위해 wasm으로 작성된 SWC의 experimental plugin 지원을 사용하도록 swc의 변환을 구성할 수 있습니다.

```js
// next.config.js
module.exports = {
  experimental: {
    swcPlugins: [
      [
        "plugin",
        {
          ...pluginOptions,
        },
      ],
    ],
  },
};
```

`swcPlugins`는 플러그인 구성을 위한 튜플 배열을 받아들입니다. 플러그인 튜플에는 플러그인 경로와 플러그인 구성을 위한 객체가 포함됩니다. 플러그인 경로는 npm 모듈 패키지 이름 또는 `.wasm` 바이너리 자체의 절대 경로일 수 있습니다.

<br>

---

<br>

## Unsupported Features

<br>

애플리케이션에 `.babelrc` 파일이 있으면 Next.js는 개별 파일을 변환할 때 자동으로 Babel을 사용하도록 되돌아갑니다. 이렇게 하면 사용자 지정 Babel 플러그인을 활용하는 기존 애플리케이션과의 역호환성이 보장됩니다.

사용자 정의 Babel 설정을 사용 중이라면, [설정을 공유해 주세요](https://github.com/vercel/next.js/discussions/30174). 저희는 앞으로 플러그인을 지원할 뿐만 아니라 일반적으로 사용되는 바벨 변환을 최대한 많이 이식하기 위해 노력하고 있습니다.

<br>

---

<br>

## Version History

<br>

| Version   | Changes                                                                                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `v13.1.0` | [Module Transpilation](https://nextjs.org/blog/next-13-1#built-in-module-transpilation-stable) and [Modularize Imports](https://nextjs.org/blog/next-13-1#import-resolution-for-smaller-bundles) stable. |
| `v13.0.0` | SWC Minifier enabled by default.                                                                                                                                                                         |
| `v12.3.0` | SWC Minifier [stable](https://nextjs.org/blog/next-12-3#swc-minifier-stable).                                                                                                                            |
| `v12.2.0` | [SWC Plugins](./Next.js_Compiler.md#swc-plugins-experimental) experimental support added.                                                                                                                |
| `v12.1.0` | Added support for Styled Components, Jest, Relay, Remove React Properties, Legacy Decorators, Remove Console, and jsxImportSource.                                                                       |
| `v12.0.0` | Next.js Compiler [introduced](https://nextjs.org/blog/next-12).                                                                                                                                          |
