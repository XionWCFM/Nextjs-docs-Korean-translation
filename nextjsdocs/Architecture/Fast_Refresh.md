# Fast Refresh

> Example - [Fast Refresh Demo](https://github.com/vercel/next.js/tree/canary/examples/fast-refresh-demo)

Fast Refresh는 React 컴포넌트 편집에 대한 즉각적인 피드백을 제공하는 Next.js 기능입니다. Fast Refresh는 9.4 이상의 모든 Next.js 애플리케이션에서 기본적으로 활성화되어 있습니다. Next.js Fast Refresh를 활성화하면 컴포넌트 상태를 잃지 않고 대부분의 편집 내용을 1초 이내에 볼 수 있습니다.

<br>

---

<br>

## How It Works

<br>

- React 컴포넌트만 내보내는 파일( only exports React component(s) )을 편집하는 경우, Fast Refresh는 해당 파일에 대해서만 코드를 업데이트하고 컴포넌트를 다시 렌더링합니다. 스타일, 렌더링 로직, 이벤트 핸들러, 이펙트 등 해당 파일에 있는 모든 것을 편집할 수 있습니다.
- React 컴포넌트가 아닌 내보내기가 있는 파일을 편집하면 Fast Refresh는 해당 파일과 해당 파일을 가져오는 다른 파일을 모두 다시 실행합니다. 따라서 `Button.js`와 `Modal.js`가 모두 `theme.js`를 import하는 경우 `theme.js`를 편집하면 두 컴포넌트가 모두 업데이트됩니다.
- 마지막으로, React 트리 외부의 파일에서 가져온 파일을 편집하면 Fast Refresh는 전체 새로고침으로 되돌아갑니다. React 컴포넌트를 렌더링하지만 Non-React 컴포넌트에서 가져온 값을 내보내는 파일이 있을 수 있습니다. 예를 들어 컴포넌트가 상수를 내보내는데 Non-React 유틸리티 파일이 이를 가져올 수도 있습니다. 이 경우 상수를 별도의 파일로 마이그레이션하고 두 파일로 모두 가져오는 것을 고려해 보세요. 그러면 Fast Refresh가 다시 작동합니다. 다른 경우에도 일반적으로 비슷한 방법으로 해결할 수 있습니다.

<br>

---

<br>

## Error Resilience

<br>

Syntax Errors

개발 중에 구문 오류가 발생하면 이를 수정하고 파일을 다시 저장하면 됩니다. 오류는 자동으로 사라지므로 앱을 다시 로드할 필요가 없습니다. 컴포넌트 상태는 손실되지 않습니다.

<br>

Runtime Errors

컴포넌트 내에서 실수로 런타임 오류가 발생하는 경우 컨텍스트 오버레이가 표시됩니다. 오류를 수정하면 앱을 다시 로드하지 않고도 오버레이가 자동으로 해제됩니다.

렌더링 중에 오류가 발생하지 않은 경우 컴포넌트 상태가 유지됩니다. 렌더링 중에 오류가 발생한 경우 React는 업데이트된 코드를 사용하여 애플리케이션을 다시 마운트합니다.

앱에 [error boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)가 있는 경우(프로덕션 환경에서 정상적으로 실패할 때 좋은 아이디어) 렌더링 오류가 발생한 후 다음 편집에서 렌더링을 다시 시도합니다. 즉, error boundaries가 있으면 항상 루트 앱 상태로 초기화되는 것을 방지할 수 있습니다. 하지만 error boundaries가 너무 세분화되어서는 안 된다는 점을 명심하세요. error boundaries는 프로덕션 환경에서 React가 사용하며 항상 의도적으로 설계해야 합니다.

<br>

---

<br>

## Limitations

<br>

Fast Refresh는 편집 중인 컴포넌트의 로컬 React 상태를 유지하려고 시도하지만, 그렇게 해도 안전한 경우에만 가능합니다. 파일을 편집할 때마다 로컬 상태가 초기화되는 몇 가지 이유는 다음과 같습니다:

- 클래스 컴포넌트에는 로컬 상태가 보존되지 않습니다(함수 컴포넌트와 Hook만 상태를 보존합니다).
- 편집 중인 파일에 React 컴포넌트 외에 다른 내보내기가 있을 수 있습니다.
- 때때로 파일은 `HOC(WrappedComponent)`와 같은 상위 컴포넌트를 호출한 결과를 내보낼 수 있습니다. 반환된 컴포넌트가 클래스인 경우, 그 상태는 초기화됩니다.
- `export default () => <div />;` 와 같은 익명 화살표 함수는 Fast Refresh가 로컬 컴포넌트 상태를 보존하지 못하게 합니다. 대규모 코드베이스의 경우 [name-default-component codemod](https://nextjs.org/docs/pages/building-your-application/upgrading/codemods#name-default-component)를 사용할 수 있습니다.

코드베이스의 더 많은 부분이 함수 컴포넌트와 Hook으로 이동함에 따라 더 많은 경우에 상태가 보존될 것으로 기대할 수 있습니다.

<br>

---

<br>

## Tips

- Fast Refresh는 기본적으로 함수 컴포넌트(및 Hook)에서 React 로컬 상태를 보존합니다.
- 때로는 상태를 강제로 리셋하고 컴포넌트를 다시 마운트하고 싶을 때가 있습니다. 예를 들어 마운트할 때만 발생하는 애니메이션을 조정할 때 유용할 수 있습니다. 이렇게 하려면 편집 중인 파일 아무 곳에나 `// @refresh reset`을 추가하면 됩니다. 이 지시문은 파일에 로컬로 적용되며, 편집할 때마다 해당 파일에 정의된 컴포넌트를 다시 마운트하도록 빠른 새로고침에 지시합니다.
- 개발 중에 편집하는 컴포넌트에 `console.log` 또는 `debugger;` 를 넣을 수 있습니다.

<br>

---

<br>

## Fast Refresh and Hooks

<br>

가능하면 Fast Refresh는 편집 사이에 컴포넌트의 상태를 보존하려고 시도합니다. 특히, `useState`와 `useRef`는 인수나 Hook 호출 순서를 변경하지 않는 한 이전 값을 보존합니다.

`useEffect`, `useMemo`, `useCallback` 등의 종속성이 있는 Hooks은 Fast Refresh 중에 항상 업데이트됩니다. 빠른 새로 고침이 진행되는 동안 해당 종속성 목록은 무시됩니다.

예를 들어, `useMemo(() => x * 2, [x])`를 `useMemo(() => x * 10, [x])`로 편집하면 `x`(의존성)가 변경되지 않았음에도 불구하고 다시 실행됩니다. React가 그렇게 하지 않았다면 편집 내용이 화면에 반영되지 않았을 것입니다!

때때로 이로 인해 예기치 않은 결과가 발생할 수 있습니다. 예를 들어, 빈 종속성 배열이 있는 `useEffect`라도 Fast Refresh 중에 한 번 다시 실행될 수 있습니다.

하지만 가끔씩 `useEffect`를 재실행하는 것에 탄력적인 코드를 작성하는 것은 Fast Refresh 없이도 좋은 방법입니다. 나중에 새로운 종속성을 더 쉽게 도입할 수 있으며, [React Strict Mode](https://nextjs.org/docs/pages/api-reference/next-config-js/reactStrictMode)에 의해 강제 적용되므로 활성화할 것을 적극 권장합니다.
