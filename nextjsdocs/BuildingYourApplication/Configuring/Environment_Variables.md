# Environment Variables

<details>
    <summary>Examples</summary>
    <div markdown="1">
    <a href="https://github.com/vercel/next.js/tree/canary/examples/environment-variables" target="_blank">- Environment Variables
    </a>
    </div>
</details>

</br>

Next.js에는 환경 변수 지원이 내장되어 있어 다음과 같은 작업을 수행할 수 있습니다:

- [`.env.local`을 사용하여 환경 변수를 로드합니다.](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables#loading-environment-variables)
- [`NEXT_PUBLIC_`로 접두사를 붙여 브라우저에서 환경 변수를 노출시킵니다.](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables#exposing-environment-variables-to-the-browser)

## Loading Environment Variables (환경 변수 불러오기)

Next.js는 `.env.local`에서 `process.env`로 환경 변수를 로드하는 기능을 기본적으로 지원합니다.

`.env.local`의 예시:

```js
DB_HOST = localhost;
DB_USER = myuser;
DB_PASS = mypassword;
```

이렇게 하면 `process.env.DB_HOST`, `process.env.DB_USER` 및 `process.env.DB_PASS`가 Node.js 환경으로 자동으로 로드되어 Route Handlers에서 사용할 수 있습니다.

> 참고: 서버 전용 비밀을 안전하게 유지하기 위해 환경 변수는 빌드 시간에 평가되며, 실제로 사용되는 환경 변수만 포함됩니다. 이는 `process.env`가 표준 JavaScript 객체가 아니기 때문에 [객체 구조 분해](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)를 사용할 수 없다는 것을 의미합니다. 환경 변수는 `process.env.PUBLISHABLE_KEY`와 같이 참조해야 하며, `const { PUBLISHABLE_KEY } = process.env`와 같이 사용할 수 없습니다.

> 참고: Next.js는 `.env*` 파일 내에서 변수(`$VAR`)를 자동으로 확장합니다. 이를 통해 다른 비밀을 참조할 수 있습니다. 예를 들어 다음과 같이 사용할 수 있습니다:

```js
# .env
HOSTNAME=localhost
PORT=8080
HOST=http://$HOSTNAME:$PORT
```

> 실제 값에 `$`가 있는 변수를 사용하려는 경우 `\$`와 같이 이스케이프 처리해야 합니다.

예를 들어:

```js
# .env
A=abc

# becomes "preabc"
WRONG=pre$A

# becomes "pre$A"
CORRECT=pre\$A
```

> 참고: `/src` 폴더를 사용하는 경우 Next.js는 `/src` 폴더가 아닌 상위 폴더에서만 `.env` 파일을 로드합니다.

## Exposing Environment Variables to the Browser (브라우저에 환경 변수 노출)

기본적으로 환경 변수는 Node.js 환경에서만 사용할 수 있으므로 브라우저에 노출되지 않습니다.

브라우저에 변수를 노출하려면 변수를 `NEXT_PUBLIC_`로 접두사를 붙여야 합니다.
예를 들어:

```js
NEXT_PUBLIC_ANALYTICS_ID = abcdefghijk;
```

이렇게 하면 `process.env.NEXT_PUBLIC_ANALYTICS_ID`가 Node.js 환경으로 자동으로 로드되어 코드의 어디에서든 사용할 수 있습니다. `NEXT_PUBLIC_` 접두사로 인해 해당 값은 브라우저로 전송되는 JavaScript에 인라인으로 포함됩니다. 이 인라인화는 빌드 시간에 발생하므로 프로젝트를 빌드할 때 해당 `NEXT_PUBLIC_` 환경 변수들을 설정해야 합니다.

```js
import setupAnalyticsService from "../lib/my-analytics-service";

// 'NEXT_PUBLIC_ANALYTICS_ID' can be used here as it's prefixed by 'NEXT_PUBLIC_'.
// It will be transformed at build time to `setupAnalyticsService('abcdefghijk')`.
setupAnalyticsService(process.env.NEXT_PUBLIC_ANALYTICS_ID);

function HomePage() {
  return <h1>Hello World</h1>;
}

export default HomePage;
```

다음과 같은 동적 조회는 인라인되지 않습니다.

```js
// This will NOT be inlined, because it uses a variable
const varName = "NEXT_PUBLIC_ANALYTICS_ID";
setupAnalyticsService(process.env[varName]);

// This will NOT be inlined, because it uses a variable
const env = process.env;
setupAnalyticsService(env.NEXT_PUBLIC_ANALYTICS_ID);
```

## Default Environment Variables (기본 환경 변수)

일반적으로 `.env.local` 파일 하나만 필요합니다. 그러나 때로는 개발 (next dev) 또는 프로덕션 (next start) 환경에 대한 일부 기본값을 추가하고 싶을 수 있습니다.

Next.js는 `.env` (모든 환경), `.env.development` (개발 환경) 및 `.env.production` (프로덕션 환경)에서 기본값을 설정할 수 있도록 합니다.

`.env.local`은 항상 설정된 기본값을 덮어씁니다.

> 참고: `.env`, `.env.development` 및 `.env.production` 파일은 기본값을 정의하기 때문에 저장소에 포함되어야 합니다. `.env*.local` 파일은 무시되어야 하므로 `.gitignore`에 추가되어야 합니다. `.env.local`은 비밀을 저장할 수 있는 공간입니다.

## Environment Variables on Vercel (Vercel에서의 환경 변수)

Next.js 애플리케이션을 [Vercel](https://vercel.com/)에 배포할 때, 환경 변수는 [프로젝트 설정](https://vercel.com/docs/concepts/projects/environment-variables?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)에서 구성할 수 있습니다.

모든 유형의 환경 변수를 여기에서 구성해야 합니다. 개발에 사용되는 환경 변수도 그곳에서 구성해야 합니다. 이후에 [로컬 장치로 다운로드하여 사용할 수 있습니다.](https://vercel.com/docs/concepts/projects/environment-variables#development-environment-variables?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)

[개발 환경 변수](https://vercel.com/docs/concepts/projects/environment-variables#development-environment-variables?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)를 구성했다면, 다음 명령을 사용하여 로컬 기기에서 사용하기 위해 `.env.local`로 가져올 수 있습니다:

```js
vercel env pull .env.local
```

## Test Environment Variables (테스트 환경 변수)

`개발`과 `프로덕션` 환경 외에도, 세 번째 옵션으로 테스트(test) 환경이 있습니다. 개발이나 프로덕션 환경에 기본값을 설정하는 것과 마찬가지로, `.env.test` 파일을 사용하여 테스트 환경에 대한 기본값을 설정할 수 있습니다 (이 옵션은 이전 두 옵션보다는 일반적이지 않습니다). Next.js는 테스트 환경에서 `.env.development`나 `.env.production` 파일로부터 환경 변수를 로드하지 않습니다.

이는 `jest`나 `cypress`와 같은 도구로 테스트를 실행할 때 특정 테스트 목적으로만 환경 변수를 설정해야 할 때 유용합니다. `NODE_ENV`가 `test`로 설정되면 테스트 기본값이 로드됩니다. 그러나 일반적으로 직접 이 작업을 수행할 필요는 없으며, 테스트 도구가 이를 처리해 줄 것입니다.

테스트 환경과 `개발` 및 `프로덕션` 사이에는 고려해야 할 작은 차이점이 있습니다. `.env.local`은 로드되지 않습니다. 왜냐하면 테스트가 모든 사람에게 동일한 결과를 생성하기를 기대하기 때문입니다. 따라서 `.env.local` 파일을 무시하고 다른 실행에서도 동일한 환경 기본값을 사용하도록 테스트 실행 시마다 설정됩니다. (`.env.local`은 기본 설정을 덮어쓰는 것을 의도한 파일입니다.)

> 참고: 기본 환경 변수와 마찬가지로 `.env.test` 파일은 저장소에 포함되어야 하지만 `.env.test.local`은 포함되지 않아야 합니다. `.env*.local` 파일은 `.gitignore`을 통해 무시되도록 의도되었기 때문입니다.

유닛 테스트를 실행할 때에도, `@next/env` 패키지의 `loadEnvConfig` 함수를 활용하여 Next.js와 동일한 방식으로 환경 변수를 로드할 수 있습니다.

```js
// The below can be used in a Jest global setup file or similar for your testing set-up
import { loadEnvConfig } from "@next/env";

export default async () => {
  const projectDir = process.cwd();
  loadEnvConfig(projectDir);
};
```

## Environment Variable Load Order (환경 변수 로드 순서)

환경 변수는 다음 위치에서 순서대로 조회되며 변수가 발견되면 중지됩니다.

환경 변수는 다음의 순서대로 검색되며, 변수가 발견되면 검색이 중단됩니다.

1. `process.env`
2. `.env.$(NODE_ENV).local`
3. `.env.local` (`NODE_ENV`가 `test`일 때는 체크되지 않습니다.)
4. `.env.$(NODE_ENV)`
5. `.env`

예를 들어, `NODE_ENV`가 `development`이고 `.env.development.local` 및 `.env`에 변수를 정의한 경우, `.env.development.local`에 있는 값이 사용됩니다.

> 참고: `NODE_ENV`에 허용되는 값은 `production`, `development`, `test`입니다.
