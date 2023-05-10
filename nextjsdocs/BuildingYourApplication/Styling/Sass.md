원본 링크: [https://nextjs.org/docs/app/building-your-application/styling/sass](https://nextjs.org/docs/app/building-your-application/styling/sass)

# \***\*Sass\*\***

Next.js는 .scss 및 .sass 확장자를 모두 사용하여 내장된 Sass 지원 기능을 제공합니다. CSS 모듈을 통해 컴포넌트 수준의 Sass를 사용할 수 있으며, .module.scss 또는 .module.sass 확장자를 사용할 수 있습니다.

먼저 [sass](https://github.com/sass/sass)를 설치합니다:

```bash
npm install --save-dev sass
```

<aside>
💡 **알아두면 좋은 사항:**

Sass는 각각 고유한 확장자를 가진 [두 가지 구문](https://sass-lang.com/documentation/syntax)을 지원합니다. .scss 확장자는 [SCSS 구문](https://sass-lang.com/documentation/syntax#scss)을 사용해야하며, .sass 확장자는 [들여쓰기 구문("Sass")](https://sass-lang.com/documentation/syntax#the-indented-syntax)을 사용해야 합니다.

어떤 것을 선택해야 할지 잘 모르겠다면, CSS의 상위 집합이며 들여쓰기 구문("Sass)을 배울 필요가 없는  .scss 확장자부터 시작하세요.

</aside>

## \***\*Sass 옵션 사용자 정의 (Customizing Sass Options)\*\***

Sass 컴파일러를 구성하려면 next.config.js에서 sassOptions를 사용하세요.

```jsx
const path = require("path");

module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, "styles")],
  },
};
```

## \***\*Sass 변수 (Sass Variables)\*\***

Next.js는 CSS 모듈 파일에서 내보낸 Sass 변수를 지원합니다.

예를 들어, 내보낸 primaryColor Sass 변수를 사용하는 방법:

```jsx
app/variables.module.scss

$primary-color: #64ff00;

:export {
  primaryColor: $primary-color;
}
```

```jsx
app / page.js;

// maps to root `/` URL

import variables from "./variables.module.scss";

export default function Page() {
  return <h1 style={{ color: variables.primaryColor }}>Hello, Next.js!</h1>;
}
```
