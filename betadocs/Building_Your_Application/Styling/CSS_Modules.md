원본 링크: [https://beta.nextjs.org/docs/styling/css-modules](https://beta.nextjs.org/docs/styling/css-modules)

# **CSS Modules**

---

Next.js는 .module.css 확장자를 사용하여 CSS 모듈을 기본적으로 지원합니다.

CSS 모듈은 고유한 클래스 이름을 자동으로 생성하여 CSS 범위를 로컬로 지정합니다.

따라서 충돌에 대한 걱정 없이 다른 파일에서 동일한 클래스 이름을 사용할 수 있습니다.

## **사용법 (Usage)**

CSS 모듈은 앱 디렉토리 내의 모든 파일로 가져올 수 있습니다:

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

## **추가 기능 (Additional Features)**

Next.js에는 스타일 추가 작성 환경을 개선하기 위한 추가 기능이 포함되어 있습니다:

- next dev를 실행할 때, 로컬 스타일시트(글로벌 또는 CSS Modules)는 [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh) 를 활용하여 편집 내용이 저장됨과 동시에 즉시 변경 사항이 반영됩니다.
- next build로 프로덕션용 빌드를 만들 때, CSS 파일은 더 적은 수의 최소화된 .css 파일로 번들링 되어 스타일을 검색하는데 필요한 네트워크 요청 수를 줄입니다.
- JavaScript를 비활성화해도 프로덕션 빌드 (next start)에서 여전히 스타일이 로드됩니다. 그러나  [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh) 를 사용하려면 여전히 JavaScript가 필요하며, next dev를 실행해야 합니다.

[Data Fetching: API Routes](../Data_Fetching/API_Routes.md)

[Tailwind CSS](./Tailwind_CSS.md)