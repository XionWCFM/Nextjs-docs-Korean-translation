# Accessibility

<br>

Next.js 팀은 모든 개발자(및 최종 사용자)가 Next.js에 액세스할 수 있도록 하기 위해 최선을 다하고 있습니다. 기본적으로 Next.js에 접근성 기능을 추가하여 모든 사람을 위한 웹을 만드는 것을 목표로 합니다.

<br>

---

<br>

## Route Announcements

<br>

서버에서 렌더링된 페이지 간에 전환할 때(예: `<a href>` 태그 사용) 화면 리더 및 기타 보조 기술은 페이지가 로드될 때 페이지 제목을 알려주어 사용자가 페이지가 변경되었음을 알 수 있도록 합니다.

<br>

기존 페이지 탐색 외에도 Next.js는 성능 향상을 위해 client-side transitions(`next/link`)도 지원합니다. client-side transitions을 보조 기술에도 알릴 수 있도록 Next.js에는 기본적으로 route announcer가 포함되어 있습니다.

<br>

Next.js route announcer는 먼저 `document.title`을 검사한 다음 `<h1>` 요소를 검사하고 마지막으로 URL 라우트명을 검사하여 알릴 페이지 이름을 찾습니다. 가장 접근하기 쉬운 사용자 경험을 위해 애플리케이션의 각 페이지에 고유하고 설명이 포함된 제목이 있는지 확인하세요.

<br>

---

<br>

## Linting

<br>

Next.js는 Next.js에 대한 사용자 지정 규칙을 포함하여 [통합 ESLint 환경](../BuildingYourApplication/Configuring/ESLint.md)을 즉시 제공합니다. 기본적으로 Next.js에는 `eslint-plugin-jsx-a11y`가 포함되어 있어 접근성 문제를 조기에 발견하는 데 도움이 됩니다(경고 켜짐 포함):

- [aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [aria-proptypes](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-proptypes.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [aria-unsupported-elements](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-unsupported-elements.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [role-has-required-aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/role-has-required-aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [role-supports-aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/role-supports-aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)

<br>

예를 들어, 이 플러그인을 사용하면 `img` 태그에 대체 텍스트를 추가하고, 올바른 `aria-*` 속성을 사용하고, 올바른 `role` 속성을 사용하는 등의 작업을 수행할 수 있습니다.

<br>

---

<br>

## Accessibility Resources

<br>

- [WebAIM WCAG checklist](https://webaim.org/standards/wcag/checklist)
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/)
- [The A11y Project](https://www.a11yproject.com/)
- Check [color contrast ratios](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Understanding_WCAG/Perceivable/Color_contrast) between foreground and background elements
- Use [`prefers-reduced-motion`](https://web.dev/prefers-reduced-motion/) when working with animations
