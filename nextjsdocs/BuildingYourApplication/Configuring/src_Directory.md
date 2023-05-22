# src Directory

Next.js는 프로젝트의 루트에 특별한 Next.js `app` 또는 `pages` 디렉토리를 갖는 것 외에도, 일반적인 패턴인 애플리케이션 코드를 `src` 디렉토리 아래에 배치하는 것을 지원합니다.

`src` 디렉토리를 사용하려면, 앱 라우터 폴더 또는 페이지 폴더를 각각 `src/app` 또는 `src/pages`로 이동시키면 됩니다.

> 알아두면 좋은 것
>
> - /public 디렉터리는 프로젝트의 루트에 유지되어야 합니다.
> - package.json, next.config.js, tsconfig.json과 같은 구성 파일은 프로젝트의 루트에 유지되어야 합니다.
> - 만약 루트 디렉터리에 app 또는 pages가 존재한다면, src/app 또는 src/pages는 무시됩니다.
> - src를 사용하는 경우, /components나 /lib와 같은 다른 애플리케이션 폴더도 함께 이동시키는 것이 일반적입니다.
