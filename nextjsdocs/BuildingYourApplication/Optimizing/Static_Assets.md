원본 링크: [https://nextjs.org/docs/app/building-your-application/optimizing/static-assets](https://nextjs.org/docs/app/building-your-application/optimizing/static-assets)

# Static Assets

Next.js는 루트 디렉터리의 public이라는 폴더에서 이미지와 같은 정적 파일을 제공할 수 있습니다.
`public` 폴더 내부의 파일은 기본 URL(’/’)에서 시작하는 코드로 참조할 수 있습니다.

예를 들어, `public` 폴더 내부에 `me.png`를 추가하면 다음 코드가 해당 이미지에 접근할 것이다.

```js
import Image from "next/image";

function Avatar() {
  return <Image src="/me.png" alt="me" width="64" height="64" />;
}

export default Avatar;
```

이 public 폴더는 `robots.txt`, `favicon.ico` , Google 사이트 검증 및 기타 정적 파일(`.html` 포함)에 유용합니다.

- 반드시 디렉토리 이름을 `public` 으로 지정하세요. 이름을 변경할 수 없으며 정적 자산을 제공하는 유일한 디렉토리입니다.
- `pages/` 디렉토리 안에 있는 파일과 이름이 같은 정적 파일이 없도록 주의하세요. 그렇지 않으면 오류가 발생합니다. 자세한 내용은 [링크](https://nextjs.org/docs/messages/conflicting-public-file-page)를 참조하세요.
- Next.js에서는 [빌드 시점](../../APIReference/Next.jsCLI.md)에 `public` 디렉토리 안에 있는 자산만 제공됩니다. 런타임에 추가된 파일은 사용할 수 없습니다. 지속적인 파일 저장을 [AWS S3](https://aws.amazon.com/ko/s3/)와 같은 타사 서비스를 사용하는 것이 좋습니다.
