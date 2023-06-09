# Contributing

Next.js beta docs 번역 프로젝트에 기여해주셔서 감사합니다.

기여를 원하시는 경우 아래 가이드라인을 따라주세요

먼저 깃허브 이슈 페이지의 

`번역 테이블입니다. 기여를 원하시는 경우 확인해주세요`

게시물을 확인하고 댓글을 통해 참여하고싶은 문서를 명시해주세요


<br>
<br>

## Fork & create a branch

먼저 레포지토리를 fork한 뒤 main 브랜치에서 작업하시거나

작업을 수행할 branch를 생성한 뒤 작업해주세요

만약 브랜치를 생성하여 작업하고 싶다면 명령어는 다음과 같습니다.

```
git checkout -b my-branch
```


branch 이름은 의미있는 이름일수록 좋습니다!

Next.js Docs의 대부분의 카테고리에 대하여 이미 md 파일이 만들어져있습니다.

자신이 번역 하고자 하는 문서에 매칭되는 md 파일에서 번역을 진행해주시면 됩니다.


<br>
<br>


## Upstream & PR

레포지토리특성상 충돌이 일어날 가능성은 적습니다.

하지만 기본적으로는 업스트림 뒤 PR을 권장드립니다.

업스트림에 대한 자료는 아래 링크에서 확인하실 수 있습니다.

[upstream이 뭔가요?](https://pers0n4.io/github-remote-repository-and-upstream/)

[upstream을 하는 방법](https://coding-groot.tistory.com/80)


<br>
<br>


## PullRequest

번역을 마쳤다면 push 한 뒤 pull request 요청을 보내세요

각자 자신이 맡은 파트만 번역하는 레포지토리의 특성상

충돌이 발생할 우려는 적지만

기본적으로 upstream후 PR을 권장드립니다.


<br>

# Translation rules


번역 컨벤션 및 용어는 다음 링크에서 확인해주세요!

다만 너무 엄격하게 지켜주시지는 않아도 좋습니다.


컨벤션 제작에 힘써주신 [이선아](https://github.com/Doyu-Lee)님 감사합니다.

[Next.js Docs 번역 용어 및 기타 컨벤션 정리](https://docs.google.com/spreadsheets/d/1_ZozdC6EU0cd2BJ5UdT6hboaRQzdq1IQ8Gr9FA28MrU/edit?usp=sharing)



# 번역에 유용한 마크다운 문법

```

"```ts"

"```"
백틱을 3번 반복하는것을 통해 코드를 작성할 수 있습니다.

백틱 3번을 반복하는것을 통해 닫을 수 있고 백틱 사이에서 코드를 작성해주면 됩니다.

[]()

하이퍼링크를 거는 문법입니다.
대괄호 사이에 시각적으로 보여질 부분을 작성하고
소괄호 사이에 링크를 걸어주시면 됩니다.

```

그 이외에도 마크다운 문법 등의 키워드로 검색을 하시면

많은 유용한 문법들을 찾으실 수 있습니다.