원본 링크: [https://beta.nextjs.org/docs/styling/global-styles](https://beta.nextjs.org/docs/styling/global-styles)

# \***\*Global Styles\*\***

전역 스타일은 app 디렉토리 내의 모든 레이아웃, 패이지 또는 컴포넌트로 가져올 수 있습니다.

<aside>
💡 **알아두면 좋은 사항:** 이 디렉토리는 _app.js 파일 내에서 전역 스타일만 가져올 수 있는  pages 디렉토리와는 다릅니다.

</aside>

app/global.css라는 스타일시트를 예로 들어보겠습니다:

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

[Tailwind CSS](./Tailwind_CSS.md)

[CSS-in-JS](./CSS_in_JS.md)
