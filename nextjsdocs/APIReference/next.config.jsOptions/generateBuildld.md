원본링크 : https://nextjs.org/docs/app/api-reference/next-config-js/generateBuildId

<br>

# **generateBuildId**

Next.js는 빌드 시 생성된 상수 ID를 사용하여 제공 중인 애플리케이션의 버전을 식별합니다. 이로 인해 다중 서버 배포에서 `next build`가 모든 서버에서 실행될 때 문제가 발생할 수 있습니다. 빌드 간에 일관된 빌드 ID를 유지하려면 고유한 빌드 ID를 제공할 수 있습니다.

<br>

`next.config.js`를 열고 `generateBuildId` 함수를 추가합니다:
next.config.js

```jsx
module.exports = {
  generateBuildId: async () => {
    // You can, for example, get the latest git commit hash here
    return "my-build-id";
  },
};
```
