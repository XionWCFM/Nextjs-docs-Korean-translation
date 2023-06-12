원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/poweredByHeader
위 문서에 대한 번역을 진행합니다.

번역시점은 2023-06-12로 공식문서의 추가적인 업데이트가 있을 수 있습니다.

번역기와 챗지피티에 의존해서 번역하고있습니다.

번역체를 자연스러운 어투로 옮기는 과정에서 오역이 발생할 수 있는 점 미리 알립니다.


# poweredByHeader

기본적으로 Next.js는 x-powered-by 헤더를 추가합니다. 이를 해제하려면, next.config.js 파일을 열고 poweredByHeader 구성을 비활성화하세요.

```jsx
module.exports = {
  poweredByHeader: false,
};
```