# TypeScript

공식 문서: [https://nextjs.org/docs/app/building-your-application/configuring/typescript](https://nextjs.org/docs/app/building-your-application/configuring/typescript)

Next.js는 React 애플리케이션을 만들기 위한 TypeScript 기반 개발 환경을 제공합니다.

내장된 TypeScript 지원으로 필요한 패키지를 자동으로 설치하고 적절한 설정을 구성할 수 있으며, 에디터용 [TypeScript 플러그인](https://nextjs.org/docs/app/building-your-application/configuring/typescript#using-the-typescript-plugin)도 제공됩니다.

> 🎥 **Watch**: Learn about the built-in TypeScript plugin → [YouTube (3 minutes)](https://www.youtube.com/watch?v=pqMqn9fKEf8)

## New Projects (새 프로젝트 생성)

`create-next-app` 이제 기본적으로 TypeScript와 함께 제공됩니다.

```bash
npx create-next-app@latest
```

## Existing Projects (기존 프로젝트에 추가)

기존 프로젝트에 TypeScript를 추가하려면 파일 확장자를 `.ts` 또는 `.tsx`로 변경하면 됩니다. 그 후` next dev`와 `next build`를 실행하면 필요한 종속성이 자동으로 설치되며 권장 구성 옵션을 사용한 `tsconfig.json` 파일이 추가됩니다.

## The TypeScript Plugin

Next.js에는 VSCode 및 다른 코드 에디터에서 사용할 수 있는 고급 타입 체크 및 자동 완성을 위한 사용자 정의 TypeScript 플러그인 및 타입 체커가 포함되어 있습니다.

TypeScript 파일을 열고 처음으로 `next dev`를 실행하면 플러그인을 활성화할 것인지 묻는 메시지가 표시됩니다.
