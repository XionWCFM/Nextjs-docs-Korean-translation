# Next.js Project Structure
현재 페이지는 개괄적인 Next.js의 file, folder 구조에 대해 알려줍니다. `app`, `pages` 폴더 내의 최상이 파일 및 폴더, 구성 파일, 라우팅 규칙을 다룹니다.

# Top-level folders
| |  |
|---|:---:|
| [app](../BuildingYourApplication/Routing/Routing.md) | App Router |
| [pages](../BuildingYourApplication/Routing/Routing.md) | Pages Router |
| [public](../BuildingYourApplication/Optimizing/Static_Assets.md) | 정적 파일 제공 |
| [src](../BuildingYourApplication/Configuring/src_Directory.md) | 조건부 웹 어플리케이션 source folder |

# Top-level files
| Next.js |  |
|---|:---:|
| [next.config.js](../APIReference/next.config.jsOptions/) | Next.js 설정 파일 |
| package.json | 프로젝트 dependencies와 scripts |
| [instumentation.ts](../BuildingYourApplication/Optimizing/Instrumentation.md) | 원격 분석 및 계측 파일  |
| [middleware.ts](../BuildingYourApplication/Routing/Middleware.md) | Next.js 요청 미들웨어 (request middleware) |
| [.env](../BuildingYourApplication/Configuring/Environment_Variables.md) | 환경 변수 |
| [.env.local](../BuildingYourApplication/Configuring/Environment_Variables.md) | 지역 환경 변수 |
| [.env.production](../BuildingYourApplication/Configuring/Environment_Variables.md) | 제품 환경 변수 |
| [.env.development](../BuildingYourApplication/Configuring/Environment_Variables.md) | 개발 환경 변수 |
| [.eslintrc.json](../BuildingYourApplication/Configuring/ESLint.md) | ESLint 설정 파일 |
|.gitignore | Git이 무시할 파일과 폴더 |
| .next-env.d.ts | Next.js TypeScript 선언 파일 |
| tsconfig.json | TypeScript 설정 파일 |
| jsconfig.json |  JavaScript 설정 파일 |
| postcss.config.js | Tailwind CSS 설정 파일 |


# `app` Routing Conventions

## Routing Files
| | | |
|---|---| --- |
| [layout](../APIReference/FileConventions/layout.js.md) |`.js` ,  `.jsx` , `.tsx` | Layout |
| [page](../APIReference/FileConventions/page.js.md) | `.js` ,  `.jsx` , `.tsx`  | Page |
| [loading](../APIReference/FileConventions/loading.js.md) | `.js` ,  `.jsx` , `.tsx`  | Loadgin UI |
| [not-found](../APIReference/FileConventions/not-found.js.md) | `.js` ,  `.jsx` , `.tsx`  | Not fount UI |
| [error](../APIReference/FileConventions/error.js.md) | `.js` ,  `.jsx` , `.tsx`  | Error UI |
| [global-error](../APIReference/FileConventions/error.js.md) | `.js` ,  `.jsx` , `.tsx`  | Global error UI |
| [route](../APIReference/FileConventions/route.js.md) | `.js` ,   `.ts`  | API endPoint |
| [template](../APIReference/FileConventions/template.js.md) | `.js` ,  `.jsx` , `.tsx`  | Re-rendered layout |
| [default](../APIReference/FileConventions/default.js.md) | `.js` ,  `.jsx` , `.tsx`  | 병렬 경로 fallback page|

## Nested Routes
| | | 
|---|---| 
| [folder](../BuildingYourApplication/Routing/Routing.md) | 라우트 세그먼트(Route segment) | 
| [folder/folder](../BuildingYourApplication/Routing/Routing.md#nested-routes) | 중첩 라우트 세그먼트(Nested route segment)| 


## Dynamic Routes
| | | 
|---|---|
| [[folder]](../BuildingYourApplication/Routing/Dynamic_Routes.md) | Dynamic route segment |
| [[...folder]](../BuildingYourApplication/Routing/Dynamic_Routes.md) | 모든 라우트 세그먼트 포착(catch-all route segment) |
| [[[...folder]]](../BuildingYourApplication/Routing/Dynamic_Routes.md) | 조건부 라우트 세그먼트 포착(optional catch-all route segment) |

## Route Groups and Private Folders
| | | 
|---|---|
| [(folder)](../BuildingYourApplication/Routing/Routes_Groups.md#convention) | 라우팅에 영향을 주지 않고 경로 그룹화 (Group routes without affecting routing) |
| [_folder](https://nextjs.org/docs/app/building-your-application/routing/colocation#private-folders) | 폴더 및 모든 하위 세그먼트를 라우팅에서 제외하도록 옵트인 (Opt folder and all child segments out of routing) |

## Parallel and Intercepted Routes (병렬 및 가로채기 경로)
| | | 
|---|---|
| [@folder] | Named slot |
| [(.)folder] | Intercept same level |
| [(..)folder] | Intercept one level above (한단계 상위에서 가로채기)  |
| [(..)(..)folder] | 	Intercept two levels above |
| [(...)folder] | Intercept from root(root에서 가로채기) |

## Metadata File Conventions
### App Icons
| | | |
|---|---| --- |
| [favicon](../APIReference/FileConventions/Metadata/favicon.ico_apple-icon.jpg_and_icon.jpg.md) | `.ico` | Favicon file |
|  [icon](../APIReference/FileConventions/Metadata/favicon.ico_apple-icon.jpg_and_icon.jpg.md) | `.ico` `.jpg` `.jpeg` `.png` `.svg` | 	App Icon file ( App 아이콘 파일)|
|  [icon](../APIReference/FileConventions/Metadata/favicon.ico_apple-icon.jpg_and_icon.jpg.md) | `.js` `.ts` `.tsx` | 	Generated App Icon ( App 아이콘 생성) |
|  [applen-icon](../APIReference/FileConventions/Metadata/favicon.ico_apple-icon.jpg_and_icon.jpg.md) | `.jpg` `.jpeg` `.png` | Apple App Icon file (apple App 아이콘 파일) |
|  [applen-icon](../APIReference/FileConventions/Metadata/favicon.ico_apple-icon.jpg_and_icon.jpg.md) | `.js` `.ts` `.tsx` | Generated Apple App Icon (apple App 아이콘 생성) |


### Open Graph and Twitter Images
| | | |
|---|---| --- |
| [opengraph-image](../APIReference/FileConventions/Metadata/opengraph-image.js_and_twitter-image.js.md) | `.jpg` `.jpeg` `.png` `.gif`  | Open Graph image file |
| [opengraph-image](../APIReference/FileConventions/Metadata/opengraph-image.js_and_twitter-image.js.md) | `.js`  `.ts` `.tsx` | 	Generated Open Graph image  |
| [twitter-image](../APIReference/FileConventions/Metadata/opengraph-image.js_and_twitter-image.js.md) | `.jpg` `.jpeg` `.png` `.gif`   | 	Twitter image file |
| [twitter-image](../APIReference/FileConventions/Metadata/opengraph-image.js_and_twitter-image.js.md) | `.js`  `.ts` `.tsx`  | 	Generated Twitter image |

### SEO
| | | |
|---|---| --- |
| [sitemap](../APIReference/FileConventions/Metadata/sitemap.xml.md)  | `.xml` | 	Sitemap file  |  
| [sitemap](../APIReference/FileConventions/Metadata/sitemap.xml.md)  | `.js` `.ts` | Generated Sitemap  |  
| [robots](../APIReference/FileConventions/Metadata/sitemap.xml.md) | `.txt` | 	Robots file  |  
| [robots](../APIReference/FileConventions/Metadata/sitemap.xml.md)  | `.js`  `ts` | 	Generated Robots file  |  

## `pages` Routing Conventions

### Special Files
| | | |
| --- | --- | --- |
| [_app](https://nextjs.org/docs/pages/building-your-application/routing/custom-app) | `.js` `.jsx` `.tsx` | Custom App | 
| [_document](https://nextjs.org/docs/pages/building-your-application/routing/custom-document) | `.js` `.jsx` `.tsx` | Custom Document | 
| [_error](https://nextjs.org/docs/pages/building-your-application/routing/custom-error#more-advanced-error-page-customizing) | `.js` `.jsx` `.tsx` | Custom Error Page | 
| [404](https://nextjs.org/docs/pages/building-your-application/routing/custom-error#404-page) | `.js` `.jsx` `.tsx` | 404 Error Page | 
| [500](https://nextjs.org/docs/pages/building-your-application/routing/custom-error#500-page) | `.js` `.jsx` `.tsx` | 500 Error Page | 


###  Routes
| Folder convention | | |
| --- | --- | --- |
| [index](../BuildingYourApplication/Routing/Pages_and_Layouts.md) | `.js` `.jsx` `.tsx` | Home page |
| [folder/index](../BuildingYourApplication/Routing/Pages_and_Layouts.md) | `.js` `.jsx` `.tsx` | Nested page |
| File convention| |
| [index](../BuildingYourApplication/Routing/Pages_and_Layouts.md)  | `.js` `.jsx` `.tsx`  | Home page |
| [file](../BuildingYourApplication/Routing/Pages_and_Layouts.md) | `.js` `.jsx` `.tsx`  | Nested page |


### Dynamic Routes
| Folder convention | | |
| --- | --- | --- |
| [[folder]/index](../BuildingYourApplication/Routing/Dynamic_Routes.md) | `.js` `.jsx` `.tsx` | Dynamic route segment |
| [[...folder]/index](../BuildingYourApplication/Routing/Dynamic_Routes.md) | `.js` `.jsx` `.tsx` | Catch-all route segment(모든 라우트 세그먼트 포착) |
| [[[...folder]]/index](../BuildingYourApplication/Routing/Dynamic_Routes.md) | `.js` `.jsx` `.tsx` |Optional catch-all route segment |
| File convention| |
| [[file]](../BuildingYourApplication/Routing/Dynamic_Routes.md)  | `.js` `.jsx` `.tsx`  | 	Dynamic route segment |
| [[...file]](../BuildingYourApplication/Routing/Dynamic_Routes.md) | `.js` `.jsx` `.tsx`  | Catch-all route segment |
| [[[...file]]](../BuildingYourApplication/Routing/Dynamic_Routes.md) | `.js` `.jsx` `.tsx`  | 	Optional catch-all route segment |