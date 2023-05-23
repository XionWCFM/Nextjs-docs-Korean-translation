원본 링크 : [https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)

<br>

# **Edge and Node.js Runtimes**

Next.js의 맥락에서 '런타임'은 코드가 실행되는 동안 사용할 수 있는 라이브러리, API 및 일반 기능의 집합을 의미합니다.

Next.js에는 애플리케이션 코드의 일부를 렌더링할 수 있는 두 개의 서버 런타임이 있습니다:

- [Node.js Runtime](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#nodejs-runtime)
- [Edge Runtime](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#edge-runtime)

각 런타임에는 고유한 API 세트가 있습니다. 사용 가능한 API의 전체 목록은 [Node.js 문서](https://nodejs.org/docs/latest/api/) 및 [Edge Docs](https://edge-runtime.vercel.app/features/available-apis)를 참조하세요. 두 런타임 모두 배포 인프라에 따라 스트리밍을 지원할 수도 있습니다.

기본적으로 `app` 디렉토리는 Node.js 런타임을 사용합니다. 하지만 경로별로 다른 런타임(예: Edge)을 선택할 수 있습니다.

<br>

---

<br>

## **[Runtime Differences](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#runtime-differences)**

런타임을 선택할 때 고려해야 할 사항이 많습니다. 이 표는 주요 차이점을 한눈에 보여줍니다. 차이점에 대한 보다 심층적인 분석이 필요하다면 아래 섹션을 참조하세요.

|                                                                                                                                                     | Node   | Serverless | Edge             |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ---------------- |
| [Cold Boot](https://vercel.com/docs/concepts/get-started/compute#cold-and-hot-boots?utm_source=next-site&utm_medium=docs&utm_campaign=next-website) | /      | ~250ms     | Instant          |
| [HTTP Streaming](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)                                            | Yes    | Yes        | Yes              |
| IO                                                                                                                                                  | All    | All        | fetch            |
| Scalability                                                                                                                                         | /      | High       | Highest          |
| Security                                                                                                                                            | Normal | High       | High             |
| Latency                                                                                                                                             | Normal | Low        | Lowest           |
| npm Packages                                                                                                                                        | All    | All        | A smaller subset |

<br>

### **[Edge Runtime](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#edge-runtime)**

Next.js에서 경량 엣지 런타임은 사용 가능한 Node.js API의 하위 집합입니다.

엣지 런타임은 작고 간단한 기능으로 짧은 지연 시간으로 동적이고 개인화된 콘텐츠를 제공해야 하는 경우에 이상적입니다. 엣지 런타임의 속도는 최소한의 리소스 사용에서 비롯되지만, 많은 시나리오에서 제한적일 수 있습니다.

예를 들어, Vercel의 엣지 런타임에서 실행되는 코드는 [1MB에서 4MB를 초과할 수 없으며](https://vercel.com/docs/concepts/limits/overview#edge-middleware-and-edge-functions-size), 이 제한에는 가져온 패키지, 글꼴 및 파일이 포함되며 배포 인프라에 따라 달라질 수 있습니다.

<br>

### **[Node.js Runtime](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#nodejs-runtime)**

Node.js 런타임을 사용하면 모든 Node.js API와 이에 의존하는 모든 npm 패키지에 액세스할 수 있습니다. 하지만 Edge 런타임을 사용하는 경로만큼 시작 속도가 빠르지는 않습니다.

Next.js 애플리케이션을 Node.js 서버에 배포하려면 인프라를 관리, 확장 및 구성해야 합니다. 또는 이 작업을 대신 처리해 주는 Vercel과 같은 서버리스 플랫폼에 Next.js 애플리케이션을 배포하는 것을 고려할 수 있습니다.

<br>

### **[Serverless Node.js](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#serverless-nodejs)**

서버리스는 엣지 런타임보다 더 복잡한 연산 부하를 처리할 수 있는 확장 가능한 솔루션이 필요한 경우 이상적입니다. 예를 들어 Vercel의 서버리스 함수를 사용하면 가져온 패키지, 글꼴, 파일을 포함하여 전체 코드 크기가 [50MB](https://vercel.com/docs/concepts/limits/overview#serverless-function-size)가 됩니다.

[Edge](https://vercel.com/docs/concepts/functions/edge-functions)를 사용하는 경로에 비해 단점은 서버리스 함수가 요청 처리를 시작하기 전에 부팅하는 데 수백 밀리초가 걸릴 수 있다는 것입니다. 사이트가 수신하는 트래픽의 양에 따라, 기능이 자주 "워밍업"되지 않기 때문에 이 문제가 자주 발생할 수 있습니다.

<br>

---

<br>

## **[Examples](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#examples)**

### **[Segment Runtime Option](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes#segment-runtime-option)**

Next.js 애플리케이션에서 개별 경로 세그먼트에 대한 런타임을 지정할 수 있습니다. 이렇게 하려면 [`runtime`이라는 변수를 선언하고 내보내세요](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config). 변수는 문자열이어야 하며, `'nodejs'` 또는 `'edge'` 런타임 중 하나의 값을 가져야 합니다.

다음 예제는 값이 `'edge'`인 런타임을 내보내는 페이지 경로 세그먼트를 보여줍니다:

```jsx
export const runtime = "edge"; // 'nodejs' (default) | 'edge'
```

세그먼트 런타임을 설정하지 않으면 기본 `nodejs` 런타임이 사용됩니다. Node.js 런타임에서 변경할 계획이 없는 경우 `runtime` 옵션을 사용할 필요가 없습니다.
