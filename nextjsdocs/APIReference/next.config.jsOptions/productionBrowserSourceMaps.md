원본 링크 : https://nextjs.org/docs/app/api-reference/next-config-js/productionBrowserSourceMaps
위 문서에 대한 번역을 진행합니다.

번역시점은 2023-06-12로 공식문서의 추가적인 업데이트가 있을 수 있습니다.

번역기와 챗지피티에 의존해서 번역하고있습니다.

번역체를 자연스러운 어투로 옮기는 과정에서 오역이 발생할 수 있는 점 미리 알립니다.


# productionBrowserSourceMaps

Next.js는 개발 중에는 자동으로 소스 맵을 활성화 하여 디버깅을 쉽게 할 수 있게 합니다. 하지만 프로덕션 빌드 중에는 보안상의 이유로 소스 맵을 비활성화 합니다. 만약 특정 구성 플래그로 활성화하지 않는 한, 클라이언트에서 소스 코드가 노출되는 것을 막기 위해서입니다.

그러나 Next.js는 구성 플래그를 제공하여 프로덕션 빌드 중에 브라우저 소스 맵 생성을 활성화할 수 있습니다. 이렇게 하면 보안 문제를 해결하면서도 디버깅이 가능합니다.

```jsx
module.exports = {
  productionBrowserSourceMaps: true,
};
```

productionBrowserSourceMaps 옵션이 활성화되면, 소스 맵은 JavaScript 파일과 동일한 디렉토리에 출력됩니다. Next.js는 요청 시 자동으로 이러한 파일을 제공합니다.

- 소스 맵을 추가 하면 next 빌드 시간이 증가할 수 있습니다.
- next 빌드 중 메모리 사용량이 증가할 수 있습니다.