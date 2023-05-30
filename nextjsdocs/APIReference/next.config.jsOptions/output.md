공유 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/output

<br>

# **output**

빌드하는 동안 Next.js는 각 페이지와 해당 종속성을 자동으로 추적하여 애플리케이션의 프로덕션 버전을 배포하는 데 필요한 모든 파일을 확인합니다.

이 기능을 사용하면 배포 크기를 크게 줄일 수 있습니다. 이전에는 Docker로 배포할 때 `next start`을 실행하려면 패키지의 `dependencies` 파일이 모두 설치되어 있어야 했습니다. Next.js 12부터는 `.next/` 디렉터리의 출력 파일 추적을 활용하여 필요한 파일만 포함할 수 있습니다.

또한 다양한 문제를 일으킬 수 있고 불필요한 중복을 유발할 수 있는 더 이상 사용되지 않는 `serverless` 타겟이 필요하지 않습니다.

<br>

---

<br>

## **[How it Works](https://nextjs.org/docs/app/api-reference/next-config-js/output#how-it-works)**

`next build` 중에 Next.js는 [`@vercel/nft`](https://github.com/vercel/nft)를 사용하여 `import`, `require` 및 `fs` 사용량을 정적으로 분석하여 페이지가 로드할 수 있는 모든 파일을 결정합니다.

또한 Next.js의 프로덕션 서버는 필요한 파일을 추적하여 프로덕션에서 활용할 수 있는 `.next/next-server.js.nft.json`에서 출력합니다.

`.next` 출력 디렉터리로 전송된 `.nft.json` 파일을 활용하려면 각 추적에서 `.nft.json` 파일과 관련된 파일 목록을 읽은 다음 배포 위치로 복사하면 됩니다.

<br>

---

<br>

## **[Automatically Copying Traced Files](https://nextjs.org/docs/app/api-reference/next-config-js/output#automatically-copying-traced-files)**

Next.js는 `node_modules`의 일부 파일을 포함하여 프로덕션 배포에 필요한 파일만 복사하는 `standalone` 폴더를 자동으로 생성할 수 있습니다.

이 자동 복사를 활용하려면 `next.config.js`에서 이 기능을 활성화하면 됩니다:

```jsx
module.exports = {
  output: "standalone",
};
```

이렇게 하면 `.next/standalone`에 폴더가 생성되며, 이 폴더는 `node_modules`를 설치하지 않고도 자체적으로 배포할 수 있습니다.

또한 `next start` 대신 사용할 수 있는 최소 `server.js` 파일도 출력됩니다. 이 최소 서버는 기본적으로 `public` 또는 `.next/static` 폴더를 복사하지 않습니다. 이러한 폴더는 CDN에서 처리하는 것이 이상적이지만, 이러한 폴더를 `standalone/public` 및 `standalone/.next/static` 폴더에 수동으로 복사할 수 있으며, 그 후에는 `server.js` 파일이 자동으로 해당 폴더를 서비스합니다.

<br>

> Note: `next.config.js`는 `next build` 중에 읽혀서 `server.js` 출력 파일에 직렬화됩니다. 레거시 [`serverRuntimeConfig` 또는 `publicRuntimeConfig` 옵션](https://nextjs.org/docs/pages/api-reference/next-config-js/runtime-configuration)을 사용하는 경우, 해당 값은 빌드 시점의 값으로 지정됩니다.

<br>

> Note: 프로젝트에서 기본 `loader`와 함께 이미지 최적화를 사용하는 경우 `sharp`를 종속 요소로 설치해야 합니다:

<br>

Terminal

```bash
npm i sharp
```

<br>

Terminal

```bash
yarn add sharp
```

<br>

Terminal

```bash
pnpm add sharp
```

<br>

---

<br>

## **[Caveats](https://nextjs.org/docs/app/api-reference/next-config-js/output#caveats)**

- 모노레포 설정에서 추적하는 동안 기본적으로 프로젝트 디렉터리가 추적에 사용됩니다. `next build packages/web-app`의 경우 `packages/web-app`이 추적 루트가 되며 해당 폴더 외부의 모든 파일은 포함되지 않습니다. 이 폴더 외부의 파일을 포함하려면 `next.config.js`에서 `experimental.outputFileTracingRoot`를 설정하면 됩니다.

packages/web-app/next.config.js

```jsx
module.exports = {
  experimental: {
    // this includes files from the monorepo base two directories up
    outputFileTracingRoot: path.join(__dirname, "../../"),
  },
};
```

- Next.js가 필수 파일을 포함하지 않거나 사용하지 않는 파일을 잘못 포함할 수 있습니다. 이러한 경우, `next.config.js`에서 각각 `experimental.outputFileTracingExcludes` 및 `experimental.outputFileTracingIncludes`를 활용할 수 있습니다. 각 구성은 특정 페이지와 일치하는 키에 대한 [minimatch globs](https://www.npmjs.com/package/minimatch)가 있는 객체와 추적에 포함하거나 제외할 프로젝트의 루트에 상대적인 글로브가 있는 배열의 값을 허용합니다.

next.config.js

```jsx
module.exports = {
  experimental: {
    outputFileTracingExcludes: {
      "/api/hello": ["./un-necessary-folder/**/*"],
    },
    outputFileTracingIncludes: {
      "/api/another": ["./necessary-folder/**/*"],
    },
  },
};
```

현재 Next.js는 방출된 `.nft.json` 파일에 대해 아무 작업도 수행하지 않습니다. 최소한의 배포를 생성하려면 배포 플랫폼(예: [Vercel](https://vercel.com/))에서 이 파일을 읽어야 합니다. 향후 릴리스에서는 이러한 `.nft.json` 파일을 활용하는 새로운 명령이 추가될 예정입니다.

<br>

---

<br>

## **[Experimental `turbotrace`](https://nextjs.org/docs/app/api-reference/next-config-js/output#experimental-turbotrace)**

종속성 추적은 매우 복잡한 계산과 분석이 필요하기 때문에 속도가 느릴 수 있습니다. 저희는 자바스크립트 구현에 대한 더 빠르고 스마트한 대안으로 Rust에서 `turbotrace`를 만들었습니다.

터보트레이스를 활성화하려면 다음 구성을 `next.config.js`에 추가하면 됩니다:

next.config.js

```jsx
module.exports = {
  experimental: {
    turbotrace: {
      // turbotrace의 로그 레벨을 제어하며, 기본값은 '오류'입니다.
      logLevel?:
      | 'bug'
      | 'fatal'
      | 'error'
      | 'warning'
      | 'hint'
      | 'note'
      | 'suggestions'
      | 'info',
      // turbotrace 로그에 분석 세부 정보를 포함할지 여부를 제어하며, 기본값은 `false`입니다.
      logDetail?: boolean
      // 제한 없이 모든 로그 메시지 표시
      // turbotrace는 기본적으로 각 카테고리에 대해 1개의 로그 메시지만 표시합니다.
      logAll?: boolean
      // turbotrace의 context directory를 제어합니다.
      // context directory 외부의 파일은 추적되지 않습니다.
      // `experimental.outputFileTracingRoot`를 설정해도 동일한 효과가 있습니다.
      // `experimental.outputFileTracingRoot`와 이 옵션이 모두 설정되어 있으면 `experimental.turbotrace.contextDirectory`가 사용됩니다.
      contextDirectory?: string
      // 코드에 'process.cwd' 식이 있는 경우 추적하는 동안 'process.cwd'의 값을 'process.cwd'로 알려주도록 이 옵션을 설정할 수 있습니다.
      // 예를 들어 require(process.cwd) + '/package'가 있습니다.json')은 required('/path/to/cwd/package로 추적됩니다.json')
      processCwd?: string
      // Turbottrace의 최대 메모리 사용량을 제어합니다. MB에서 기본값은 6000입니다.
      memoryLimit?: number
    },
  },
}
```
