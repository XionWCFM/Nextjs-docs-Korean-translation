# \***\*Next.js CLI\*\***

Next.js CLI를 사용하면 애플리케이션을 시작하고 빌드하며 내보낼 수 있습니다.

사용 가능한 CLI 명령어 목록을 얻으려면 프로젝트 디렉토리 내에서 다음 명령어를 실행하세요.

```bash
npx next -h
```

(*[npx](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b)*는 npm 5.2 이상 버전에서 제공됩니다.)

출력은 다음과 같아야 합니다:

```bash
사용법:
  $ next <command>

사용 가능한 명령어:
  build, start, export, dev, lint, telemetry, info

옵션:
  --version, -v   버전 번호
  --help, -h      이 메시지를 표시합니다.

더 많은 정보를 얻으려면 --help 플래그와 함께 명령어를 실행하세요.
  $ next build --help
```

next 명령어에는 어떤 [노드 인수](https://nodejs.org/api/cli.html#cli_node_options_options)든 전달할 수 있습니다.

```bash
NODE_OPTIONS='--throw-deprecation' next
NODE_OPTIONS='-r esm' next
NODE_OPTIONS='--inspect' next
```

<aside>
💡 참고: 명령어 없이 `next`를 실행하는 것은 `next dev`를 실행하는 것과 동일합니다.

</aside>

---

## Build

`next build`는 애플리케이션의 최적화된 프로덕션 빌드를 생성합니다. 출력에는 각 라우트에 대한 정보가 표시됩니다.

- ‘Size’는 페이지를 클라이언트 측에서 탐색할 때 다운로드되는 에셋의 수입니다. 각 라우트의 크기에는 종속성만 포함됩니다.
- "First-Load-JS"는 서버에서 페이지를 방문할 때 다운로드되는 에셋의 수를 의미합니다. 모든 페이지에서 공유되는 JavaScript의 양은 별도의 측정 항목으로 표시됩니다.

이러한 값은 gzip으로 압축됩니다. 첫 로드는 초록, 노랑, 빨강으로 표시됩니다. 성능이 좋은 애플리케이션을 위해 초록을 목표로 하세요.

next build에서 --profile 플래그를 사용하여 React의 프로덕션 프로파일링을 활성화할 수 있습니다. 이는 [Next.js 9.5](https://nextjs.org/blog/next-9-5) 이상을 요구합니다.

```bash
next build --profile
```

이후에는 개발 환경에서와 동일한 방식으로 프로파일러를 사용할 수 있습니다.

next build에서 --debug 플래그를 사용하여 더 자세한 빌드 출력을 활성화할 수 있습니다. 이는 Next.js 9.5.3 이상을 요구합니다.

```bash
next build --debug
```

이 플래그를 활성화하면 추가적인 빌드 출력인 리라이트(rewrites), 리다이렉트(redirects) 및 헤더(headers)가 표시됩니다.

---

## Development

`next dev`는 hot-code-reloading, 오류 보고 등을 포함한 개발 모드로 애플리케이션을 시작합니다:

기본적으로 애플리케이션은 **[http://localhost:3000에서](http://localhost:3000%EC%97%90%EC%84%9C/)** 시작됩니다. 기본 포트는 -p 옵션을 사용하여 변경할 수 있습니다. 예를 들어, 다음과 같이 사용합니다:

```bash
npx next dev -p 4000
```

또는 PORT 환경 변수를 사용할 수도 있습니다:

```bash
PORT=4000 npx next dev
```

<aside>
💡 참고: `.env`에 `PORT`를 설정할 수 없습니다. HTTP 서버의 부팅은 다른 모든 코드가 초기화되기 전에 진행되기 때문입니다.

</aside>

또한 기본값인 0.0.0.0과 다른 호스트 이름을 설정할 수도 있습니다. 이는 네트워크의 다른 기기에서 애플리케이션을 사용할 수 있도록 도움이 될 수 있습니다. 기본 호스트 이름은 -H 옵션을 사용하여 변경할 수 있습니다. 예를 들어, 다음과 같이 사용합니다:

```bash
npx next dev -H 192.168.1.2
```

---

## Production

`next start`는 애플리케이션을 프로덕션 모드로 시작합니다. 애플리케이션은 먼저 `next build`로 컴파일되어야 합니다.

기본적으로 애플리케이션은 **`http://localhost:3000`에서** 시작됩니다. 기본 포트는 `-p` 옵션을 사용하여 변경할 수 있습니다. 예를 들어, 다음과 같이 사용합니다:

```bash
npx next start -p 4000
```

또는 PORT 환경 변수를 사용할 수도 있습니다:

```bash
PORT=4000 npx next start
```

<aside>
💡 참고: .env에 PORT를 설정할 수 없습니다. HTTP 서버의 부팅은 다른 모든 코드가 초기화되기 전에 진행되기 때문입니다.

</aside>

<aside>
💡 참고: next start는 output: 'standalone' 또는 output: 'export'와 함께 사용할 수 없습니다.

</aside>

### Keep Alive Timeout

Next.js를 하위 프록시(예: AWS ELB/ALB와 같은 로드 밸런서) 뒤에 배포할 때는 Next의 내부 HTTP 서버를 설정해야 합니다. 이때 하위 프록시의 타임아웃보다 큰 [keep-alive timeouts](https://nodejs.org/api/http.html#http_server_keepalivetimeout)을 설정하는 것이 중요합니다. 그렇지 않으면 TCP 연결에 대한 keep-alive 타임아웃이 도달하면 Node.js는 하위 프록시에 알리지 않고 해당 연결을 즉시 종료합니다. 이로 인해 Node.js가 이미 종료한 연결을 하위 프록시가 재사용하려고 할 때 프록시 오류가 발생합니다.

프로덕션 Next.js 서버의 타임아웃 값을 설정하려면 next start에 --keepAliveTimeout (밀리초 단위)를 전달하면 됩니다. 예를 들어 다음과 같이 사용합니다:

```bash
npx next start --keepAliveTimeout 70000
```

---

## Lint

`next lint`는 `pages/`, `app` (실험적인 `appDir` 기능이 활성화된 경우에만), `components/`, `lib/`, `src/` 디렉토리에 있는 모든 파일에 대해 ESLint를 실행합니다. 또한, 이미 애플리케이션에서 ESLint가 구성되지 않은 경우 필요한 종속성을 설치하기 위한 안내 설정을 제공합니다.

Lint를 수행하고 싶은 다른 디렉토리가 있다면 `--dir` 플래그를 사용하여 지정할 수 있습니다:

```bash
next lint --dir utils
```

---

## Telemetry

Next.js는 일반적인 사용에 관한 **완전히 익명의** Telemetry 데이터를 수집합니다. 이 익명의 프로그램에 참여는 선택 사항이며, 어떤 정보도 공유하지 않으려면 참여를 거부할 수 있습니다.

텔레메트리에 대해 더 알아보려면 [이 문서를 읽어보세요.](https://nextjs.org/telemetry)

---

## Next Info

`next info`는 Next.js 버그를 보고할 때 사용할 수 있는 현재 시스템에 대한 관련 세부 정보를 출력합니다. 이 정보에는 운영 체제 플랫폼/아키텍처/버전, 이진 파일(Node.js, npm, Yarn, pnpm) 및 npm 패키지 버전(`next`, `react`, `react-dom`)이 포함됩니다.

프로젝트의 루트 디렉토리에서 다음을 실행하세요:

```bash
next info
```

다음과 같은 예시와 같은 정보를 제공할 것입니다:

```bash
Operating System:
      Platform: linux
      Arch: x64
      Version: #22-Ubuntu SMP Fri Nov 5 13:21:36 UTC 2021
    Binaries:
      Node: 16.13.0
      npm: 8.1.0
      Yarn: 1.22.17
      pnpm: 6.24.2
    Relevant packages:
      next: 12.0.8
      react: 17.0.2
      react-dom: 17.0.2
```

이 정보는 GitHub Issues에 붙여넣어야 합니다.
