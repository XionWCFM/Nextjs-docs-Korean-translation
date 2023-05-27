원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/env

<br>

# **env**

> [Next.js 9.4](https://nextjs.org/blog/next-9-4) 릴리스부터 [환경 변수를 추가](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)할 때 더욱 직관적이고 인체공학적인 환경을 제공합니다. 한 번 사용해 보세요!

<br>

### **Examples**

- [With env](https://github.com/vercel/next.js/tree/canary/examples/with-env-from-next-config-js)

> **Note**: 이러한 방식으로 지정된 환경 변수는 항상 자바스크립트 번들에 포함되며, 환경 변수 이름 앞에 `NEXT_PUBLIC_`을 붙이는 것은 [환경 또는 .env 파일을 통해](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables) 지정할 때만 효과가 있습니다.

<br>

JavaScript 번들에 환경 변수를 추가하려면 `next.config.js`를 열고 `env` 설정을 추가합니다:
next.config.js

```jsx
module.exports = {
  env: {
    customKey: "my-value",
  },
};
```

<br>

이제 코드에서 `process.env.customKey`에 액세스할 수 있습니다. 예:

```jsx
function Page() {
  return <h1>The value of customKey is: {process.env.customKey}</h1>;
}

export default Page;
```

Next.js는 빌드 시점에 `process.env.customKey`를 `'my-value’`으로 대체합니다. 웹팩 [DefinePlugin](https://webpack.js.org/plugins/define-plugin/)의 특성으로 인해 `process.env` 변수를 파괴하려고 하면 작동하지 않습니다.

<br>

예를 들어 다음 줄이 있습니다:

```jsx
return <h1>The value of customKey is: {process.env.customKey}</h1>;
```

<br>

다음과 같이 됩니다:

```jsx
return <h1>The value of customKey is: {"my-value"}</h1>;
```
