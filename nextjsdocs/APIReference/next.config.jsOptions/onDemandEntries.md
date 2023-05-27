공유 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/onDemandEntries

<br>

# **onDemandEntries**

Next.js에는 서버가 개발 중인 빌드된 페이지를 폐기하거나 메모리에 보관하는 방법을 제어할 수 있는 몇 가지 옵션이 있습니다.

<br>

기본값을 변경하려면 `next.config.js`를 열고 `onDemandEntries` 구성을 추가합니다:

next.config.js

```jsx
module.exports = {
  onDemandEntries: {
    // 서버가 페이지를 버퍼에 보관하는 기간(ms)
    maxInactiveAge: 25 * 1000,
    // 폐기하지 않고 동시에 보관해야 하는 페이지 수
    pagesBufferLength: 2,
  },
};
```
