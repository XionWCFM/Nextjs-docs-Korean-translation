# 터보팩(알파)

[터보팩](https://turbo.build/pack)은 JavaScript와 TypeScript를 최적화한 증분 번들러로, Rust로 작성되었으며 Next.js 13에 내장되어 있습니다.

대규모 애플리케이션에서 터보팩은 Webpack보다 700배 더 빠르게 업데이트됩니다.

<br>

## 사용 방법

터보팩은 `페이지`와 ``앱` 디렉토리 모두에서 Next.js 13에서 사용할 수 있습니다.

1.터보팩이 포함된 Next.js 13 프로젝트 생성

```jsx
npx create-next-app@latest -e with-turbopack
```

2.(터보팩으로) Next.js 개발 서버 시작

```jsx
next dev --turbo
```

<br>

## 지원되는 기능

현재 터보팩에서 지원하는 기능에 대해 자세히 알아보려면 [문서](https://turbo.build/pack/docs/features)를 참조하십시오.

[Typescript](./TypeScript.md)

[ESLint](./ESLint.md)
