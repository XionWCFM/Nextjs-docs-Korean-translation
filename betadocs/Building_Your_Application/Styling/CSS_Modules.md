원본 링크: [https://beta.nextjs.org/docs/styling/css-modules](https://beta.nextjs.org/docs/styling/css-modules)

# **CSS Modules**

---

Next.js는 .module.css 확장자를 사용하여 CSS 모듈을 기본적으로 지원합니다.

CSS 모듈은 고유한 클래스 이름을 자동으로 생성하여 CSS 범위를 로컬로 지정합니다. 따라서 충돌에 대한 걱정 없이 다른 파일에서 동일한 클래스 이름을 사용할 수 있습니다. 이러한 동작으로 인해 CSS 모듈은 컴포넌트 수준 CSS를 포함하기에 이상적인 방법입니다.

## 예제 **(Example)**

CSS 모듈은 app 디렉토리 내의 모든 파일로 가져올 수 있습니다:

```jsx
app / dashboard / layout.tsx;

import styles from "./styles.module.css";

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return <section className={styles.dashboard}>{children}</section>;
}
```

```css
app/dashboard/styles.module.css .dashboard {
  padding: 24px;
}
```

CSS 모듈은 선택적 기능이며 **확장자가 .module.css인 파일에 대해서만 활성화됩니다**. 일반 <link> 스타일시트와 전역 CSS파일은 계속 지원됩니다.

프로덕션 환경에서 모든 CSS 모듈 파일은 자동으로 **많은 최소화 및 코드 분할된** .css 파일로 연결됩니다. 이러한 .css 파일은 애플리케이션의 핫 실행 경로를 나타내므로 애플리케이션이 페인팅하는데 필요한 최소한의 CSS만 로드됩니다.

## 전역 **스타일 (Global Styles)**

전역 스타일은 app 디렉토리 내의 모든 레이아웃, 페이지 또는 컴포넌트로 가져올 수 있습니다.

<aside>
💡 **알아두면 좋은 사항:** 이 디렉토리는 _app.js 파일 내에서 전역 스타일만 가져올 수 있는 pages 디렉토리와는 다릅니다.

</aside>

app/global.css 라는 스타일시트를 예로 들어보겠습니다:

```css
body {
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}
```

루트 레이아웃(app/layout.js) 내에서, global.css 스타일시트를 가져와 애플리케이션의 모든 경로에 스타일을 적용합니다:

```jsx
app / layout.tsx;

// These styles apply to every route in the application
import "./global.css";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

## 외부 **스타일시트 (External Stylesheets)**

외부 패키지에 게시한 스타일시트는 함께 배치된 컴포넌트를 포함하여 app 디렉토리 어디에서나 가져올 수 있습니다:

```jsx
import "bootstrap/dist/css/bootstrap.css";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body className="container">{children}</body>
    </html>
  );
}
```

## **추가 기능 (Additional Features)**

Next.js에는 스타일 추가 작성 환경을 개선하기 위한 추가 기능이 포함되어 있습니다:

- next dev를 실행할 때, 로컬 스타일시트(글로벌 또는 CSS Modules)는 [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh) 를 활용하여 편집 내용이 저장됨과 동시에 즉시 변경 사항이 반영됩니다.
- next build로 프로덕션용 빌드를 만들 때, CSS 파일은 더 적은 수의 최소화된 .css 파일로 번들링 되어 스타일을 검색하는데 필요한 네트워크 요청 수를 줄입니다.
- JavaScript를 비활성화해도 프로덕션 빌드 (next start)에서 여전히 스타일이 로드됩니다. 그러나  [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh) 를 사용하려면 여전히 JavaScript가 필요하며, next dev를 실행해야 합니다.
