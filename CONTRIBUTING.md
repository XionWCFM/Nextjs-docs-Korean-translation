## Contributing

Next.js beta docs 번역 프로젝트에 기여해주셔서 감사합니다.

기여를 원하시는 경우 아래 가이드라인을 따라주세요

먼저 notion 페이지로 방문하여 자신이 작업할 파트를 명시해주세요

(추후 작업 파트에 대한 내용은 깃허브 이슈로 개설할 예정이지만 아직 실현되지 않았습니다.)



### Fork & create a branch

먼저 레포지토리를 fork한 뒤 작업을 수행할 branch를 생성해주세요

```
git checkout -b 325-add-japanese-translations
```

branch 이름은 의미있는 이름일수록 좋습니다!

Next.js Beta Docs의 대부분의 카테고리에 대하여 이미 md 파일이 만들어져있습니다.

자신이 번역 하고자 하는 문서에 매칭되는 md 파일에서 번역을 진행해주시면 됩니다.

  
### Translation rules

1. 공식문서에 서술되지않은 내용은 역주의 의견으로 취급합니다. 

> 역주의 의견은 이 markdown 문법을 이용하여 서술합니다. >를 이용해 사용할 수 있어요

2. 공식문서에 포함되어있는 이미지는 반드시 첨부하며 번역을 진행해주세요

이미지는 자신이 작업하고 있는 문서 이름으로 문서와 동일한 깊이에서 폴더를 생성하여 넣어주세요

이때 폴더이름은 작업하는문서+Images로 통일합니다.

ex : API_Reference/useParams.md를 작업하고있다면 API_Reference/useParamsImages/image1.png와 같은 형식

3. 모든 문서의 최상단에는 그에 대응하는 docs로 연결되는 link가 있어야합니다.

4. 각 이미지에 대해 

### PullRequest

번역을 마쳤다면 push 한 뒤 pull request 요청을 보내세요