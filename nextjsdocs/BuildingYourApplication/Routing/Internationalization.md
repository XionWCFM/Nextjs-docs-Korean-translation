https://nextjs.org/docs/app/building-your-application/routing/internationalization

위 문서에 대한 번역을 진행합니다.

번역시점은 05-09로 공식문서의 추가적인 업데이트가 있을 수 있습니다.

번역기와 챗지피티에 의존해서 번역하고있습니다.

번역체를 자연스러운 어투로 옮기는 과정에서 오역이 발생할 수 있는 점 미리 알립니다.

# Internationalization
Next.js를 사용하면 여러 언어를 지원하도록 콘텐츠 라우팅 및 렌더링을 구성할 수 있습니다.

사이트를 다양한 locales에 맞게 조정하려면 번역된 콘텐츠(localization)과 internationalized routes가 필요합니다.

## Terminology
**locale** : 언어 및 서식 기본 설정 집합에 대한 식별자 입니다.

여기에는 일반적으로 사용자가 선호하는 언어와 해당 지역이 포함됩니다.

- en-US : 미국에서 사용되는 영어

- nl-NL : 네덜란드에서 사용되는 네덜란드 어

- nl : 네덜란드어, 특정 지역 없음

# Routing Overview
브라우저에서 사용자의 언어 기본 설정을 사용하여 사용할 locale을 선택하는 것이 좋습니다.

기본 설정 언어를 변경하면 애플리케이션으로 수신되는 `Accept-Language` 헤더가 수정되게 됩니다.

예컨대 다음 라이브러리를 사용하면 수신 요청을 살펴보고 헤더, 지원하려는 locale 및 default locale에 따라 선택할 locale을 결정할 수 있습니다.

```js
import { match } from '@formatjs/intl-localematcher';
import Negotiator from 'negotiator';
 
let headers = { 'Accept-Language': 'en-US,en;q=0.5' };
let languages = new Negotiator(headers).languages();
let locales = ['en-US', 'nl-NL', 'nl'];
let defaultLocale = 'en-US';
 
match(languages, locales, defaultLocale); // -> 'en-US'
```
라우팅은 하위 경로(/fr/products) 또는 도메인 (my-site.fr/products) 를 통해 internationalized(국제화)할 수 있습니다.

이제 이 정보를 이용하여 미들웨어 내부의 locale을 따라 사용자를 리디렉션할 수 있습니다.


``` js
middleware.js

import { NextResponse } from 'next/server'
 
let locales = ['en-US', 'nl-NL', 'nl']
 
// Get the preferred locale, similar to above or using a library
function getLocale(request) { ... }
 
export function middleware(request) {
  // Check if there is any supported locale in the pathname
  const pathname = request.nextUrl.pathname
  const pathnameIsMissingLocale = locales.every(
    (locale) => !pathname.startsWith(`/${locale}/`) && pathname !== `/${locale}`
  )
 
  // Redirect if there is no locale
  if (pathnameIsMissingLocale) {
    const locale = getLocale(request)
 
    // e.g. incoming request is /products
    // The new URL is now /en-US/products
    return NextResponse.redirect(
      new URL(`/${locale}/${pathname}`, request.url)
    )
  }
}
 
export const config = {
  matcher: [
    // Skip all internal paths (_next)
    '/((?!_next).*)',
    // Optional: only run on root (/) URL
    // '/'
  ],
}
```

마지막으로 app/ 디렉토리 내의 모든 special files 들은 app/[lang] 아래에 중첩되도록 해야 합니다.

이렇게 하면 Next.js 라우터가 경로에서 다양한 locale 들을 동적으로 처리하고 모든 레이아웃과 페이지에 lang 매개 변수를 전달할 수 있습니다.

예컨대

```js
app/[lang]/page.js

// You now have access to the current locale
// e.g. /en-US/products -> `lang` is "en-US"
export default async function Page({ params: { lang } }) {
  return ...
}
```

루트 레이아웃은 새 폴더(ex : app/[lang]/layout.js)에 중첩할 수도 있습니다.

# Localization

 

사용자가 선호하는 locale에 따라 표시되는 콘텐츠를 변경하는 것을 말합니다.

즉 **localization** 은 Next.js에만 국한된 개념이 아닙니다.

아래에 설명된 패턴들은 모든 웹 애플리케이션에서 동일하게 작동합니다.

 

애플리케이션 내에서 영어와 네덜란드어 콘텐츠를 모두 지원하고자 한다고 가정해보겠습니다.

일부 키에서 현지화된 문자열로의 매핑을 제공하는 객체인 두개의 서로 다른 **dictionareies**를 유지할 수 있습니다.

예컨대

```json
dictionaries/en.json
{
  "products": {
    "cart": "Add to Cart"
  }
}
```
```json
dictionaries/nl.json
{
  "products": {
    "cart": "Toevoegen aan Winkelwagen"
  }
}
```
그런 다음 우리는 요청된 locale에 대한 번역을 로드하는 `getDictionary` 함수를 만들 수 있습니다.

```js
app/[lang]/dictionaries.js

import 'server-only';
 
const dictionaries = {
  en: () => import('./dictionaries/en.json').then((module) => module.default),
  nl: () => import('./dictionaries/nl.json').then((module) => module.default),
};
 
export const getDictionary = async (locale) => dictionaries[locale]();
현재 선택된 언어가 주어지면 레이아웃이나 페이지 내에서 dictionary를 가져올 수 있습니다.

app/[lang]/page.js

import { getDictionary } from './dictionaries';
 
export default async function Page({ params: { lang } }) {
  const dict = await getDictionary(lang); // en
  return <button>{dict.products.cart}</button>; // Add to Cart
}
```

app/ 디렉토리의 모든 레이아웃과 페이지는 기본적으로 서버 컴포넌트를 사용합니다.

따라서 번역 파일의 크기가 클라이언트 측의 javascript 번들 크기에 영향을 미치는 것을 걱정하지 않아도 됩니다.

이 코드는 서버에서만 실행되며 결과 HTML만 브라우저로 전송되기 때문입니다.

# Static Generation
특정한 locale의 집합에 대한 정적 경로를 생성하기 위해 페이지 또는 레이아웃에 `generateStaticParams`를 사용할 수 있습니다.

예를 들어서 루트 레이아웃에서 전역적으로 사용할 수 있습니다.

```js
app/[lang]/layout.js

export async function generateStaticParams() {
  return [{ lang: 'en-US' }, { lang: 'de' }];
}
 
export default function Root({ children, params }) {
  return (
    <html lang={params.lang}>
      <body>{children}</body>
    </html>
  );
}
```