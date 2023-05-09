# Create-next-app

공식 문서 : [https://nextjs.org/docs/app/api-reference/create-next-app](https://nextjs.org/docs/app/api-reference/create-next-app)

Next.js를 시작하는 가장 쉬운 방법은 create-next-app을 사용하는 것입니다. 이 CLI 도구를 사용하면 모든 것이 설정된 새로운 Next.js 애플리케이션을 빠르게 만들 수 있습니다. 기본 Next.js 템플릿을 사용하거나 [공식 Next.js 예제](https://github.com/vercel/next.js/tree/canary/examples) 중 하나를 사용하여 새 앱을 만들 수 있습니다. 시작하려면 다음 명령을 사용하세요.

## Interactive

다음과 같이 상호작용 방식으로 새 프로젝트를 생성할 수 있습니다:

```bash
npx create-next-app@latest
# or
yarn create next-app
# or
pnpm create next-app
```

프로젝트의 이름과 TypeScript 프로젝트를 생성할지 여부를 묻는 메시지가 표시됩니다:

```bash
Would you like to use TypeScript with this project? … No / Yes

이 프로젝트에 TypeScript를 사용하시겠습니까? ... 아니요 / 예
```

필요한 타입/의존성을 설치하고 새로운 TS 프로젝트를 만들려면 Yes(예)를 선택하세요.

## Non-interactive

또한 명령줄 인수를 전달하여 대화형이 아닌 방식으로 새 프로젝트를 설정할 수도 있습니다.

또한, `--no-eslint`와 같이 접두사 `--no-`를 사용하여 기본 옵션을 비활성화할 수 있습니다.

`create-next-app --help`를 참조하세요.

```bash
사용법 : create-next-app <project-directory> [options]

Options:
  -V, --version                        output the version number
  --ts, --typescript

		TypeScript 프로젝트로 초기화합니다. (기본값)

  --js, --javascript

		JavaScript 프로젝트로 초기화합니다.

  --tailwind

		Tailwind CSS 구성으로 초기화합니다. (기본값)

  --eslint

		ESLint 구성으로 초기화합니다.

  --src-dir

		`src/` 디렉토리 내부에서 초기화합니다.

  --import-alias <alias-to-configure>

		사용할 import 별칭을 지정합니다 (기본값 "@/*").

  --use-npm

		CLI에게 npm을 사용하여 앱을 bootStrap하도록 명시적으로 지정합니다.

  --use-pnpm

    CLI에게 pnpm을 사용하여 앱을 bootStrap하도록 명시적으로 지정합니다.

  -e, --example [name]|[github-url]

    앱을 부트스트랩하는 예시입니다.
		공식 Next.js 저장소의 예시 이름이나 GitHub URL을 사용할 수 있습니다.
		URL에는 모든 브랜치 및/또는 하위 디렉토리를 사용할 수 있습니다.

  --example-path <path-to-example>

    드물게, GitHub URL에는 브랜치 이름과 (예: bug/fix-1)
		예제의 경로 (예: foo/bar)가 모두 포함될 수 있습니다.
		이 경우, 예제 경로를 별도로 지정해야합니다: --example-path foo/bar

  --reset-preferences

    CLI에게 저장된 모든 기본 설정을 재설정하도록 명시적으로 지정합니다.

  -h, --help                           사용법 정보를 출력합니다.
```

## 왜 Create Next App을 사용해야 할까요?

`create-next-app`은 몇 초 안에 새로운 Next.js 앱을 만들 수 있도록 해줍니다. 이것은 Next.js의 개발자들이 공식적으로 유지 관리하고 있으며 다음과 같은 이점이 있습니다:

- 상호작용 경험: `npx create-next-app@latest`(인자 없이)를 실행하면 프로젝트 설정을 안내하는 상호작용 경험을 제공합니다.
- Zero 종속성: 프로젝트를 초기화하는 것은 1초만에 가능합니다. Create Next App은 Zero 종속성을 가지고 있습니다.
- 오프라인 지원: Create Next App은 오프라인인지 자동으로 감지하고 로컬 패키지 캐시를 사용하여 프로젝트를 bootStrap합니다.
- 예제 지원: Create Next App은 Next.js 예제 컬렉션(예: `npx create-next-app --example api-routes`)에서 예제를 사용하여 애플리케이션을 부트스트랩 할 수 있습니다.
- 테스트 완료: 이 패키지는 Next.js 모노레포의 일부이며, Next.js 자체와 동일한 통합 테스트 스위트를 사용하여 테스트됩니다. 이는 모든 릴리스에서 기대와 같이 작동함을 보장합니다.
