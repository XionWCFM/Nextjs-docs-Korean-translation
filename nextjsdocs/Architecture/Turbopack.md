# 터보팩(알파)

[터보팩](https://turbo.build/pack)은 JavaScript와 TypeScript를 최적화한 증분 번들러로, Rust로 작성되었으며 Next.js에 내장되어 있습니다.

<br>

## 사용 방법

터보팩은 Next.js의 페이지 및 앱 디렉토리에서 더 빠른 로컬 개발을 위해 사용할 수 있습니다. 터보팩을 사용하려면 Next.js 개발 서버를 실행할 때 --turbo 플래그를 사용하면 됩니다.

```jsx
//package.json

{
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

<br>

## 지원되는 기능

현재 터보팩에서 지원하는 기능에 대해 자세히 알아보려면 [문서](https://turbo.build/pack/docs/features)를 참조하십시오.

## 지원되지 않는 기능

터보팩은 현재 next dev만 지원하며 next build는 지원하지 않습니다. 안정성에 가까워짐에 따라 빌드 지원에 대한 작업을 진행 중입니다.
