원본 링크: [https://nextjs.org/docs/app/building-your-application/optimizing/open-telemetry](https://nextjs.org/docs/app/building-your-application/optimizing/open-telemetry)

# OpenTelemetry

참고: 이 기능은 실험적인 기능이므로 `next.config.js` 파일에서 `experimental.instrumentationHook = true`; 를 제공하여 명시적으로 선택할 필요가 있습니다.

관찰성(Observability)은 Next.js 앱의 동작과 성능을 이해하고 최적화하기 위해 중요합니다.

애플리케이션들이 복잡해질수록, 발생할 수 있는 문제를 식별하고 진단하는 것이 점점 어려워집니다.
로그 및 지표와 같은 관찰성 도구를 활용하여 개발자는 애플리케이션의 동작을 파악하고 최적화할 수 있는 영역을 식별할 수 있습니다.
관찰성을 통해 개발자는 문제가 주요한 문제가 되기 전에 예방적으로 대응하고 더 나은 사용자 경험을 제공할 수 있습니다.
따라서 Next.js애플리케이션에서 관찰성을 사용하여 성능을 향상시키고 리소스를 최적화하며 사용자 경험을 향상시키는 것을 강력히 권장합니다.

앱에 계측 기능을 적용하기 위해 OpenTelemetry를 사용하는 것을 권장합니다.
OpenTelemetry는 플랫폼에 구애받지 않는 방식으로 앱을 계측할 수 있으며, 코드를 변경하지 않고 관찰성 공급자를 변경할 수 있는 기능을 제공합니다.
OpenTelemetry에 대한 자세한 정보와 작동 방식에 대해서는 공식 OpenTelemetry 문서([Official OpenTelemetry docs](https://opentelemetry.io/docs/))를 참조하십시오.

이 문서에서는 Span, Trace 또는 Exporter와 같은 용어를 사용하며, 이들에 대한 자세한 내용은 [the OpenTelemetry Observability Primer](https://opentelemetry.io/docs/concepts/observability-primer/)에서 찾을 수 있습니다.

Next.js는 기본적으로 OpenTelemetry 계측을 지원하므로, 이미 Next.js 자체가 계측되어 있습니다.
OpenTelemetry를 활성화하면 `getStaticProps`와 같은 코드를 자동으로 span으로 래핑하고 유용한 속성과 함께 처리합니다.

참고: 현재 우리는 OpenTelemetry 바인딩을 서버리스 함수에서만 지원합니다. `엣지(Edge)`나 클라이언트 측 코드에 대해서는 지원하지 않습니다.

---

### Getting Started

OpenTelemetry는 확장 가능하지만, 올바르게 설정하기 위해서는 상당히 많은 설정이 필요할 수 있습니다.
그래서 우리는 빠르게 시작할 수 있도록 도와주는 `@vercel/otel` 패키지를 준비했습니다.
이 패키지는 확장이 불가능하며, OpenTelemetry를 수동으로 구성해야 하며, 설정을 직접 사용자화해야 합니다.

#### Using `@vercel/otel`

시작하려면, `@vercel/otel` 를 설치해야 됩니다.

`_ Terminal`

```
npm install @vercel/otel
```

다음으로, 프로젝트의 루트에 custom `[instrumentation.ts](./Instrumentation.md)`  파일을 생성하세요.

`instrumentation.ts`

```ts
import { registerOTel } from "@vercel/otel";

export function register() {
  registerOTel("next-app");
}
```

참고: [with-opentelemetry](https://github.com/vercel/next.js/tree/canary/examples/with-opentelemetry)  예제를 준비했으니 사용하실 수 있습니다.

#### [Manual OpenTelemetry configuration](#manual-opentelemetry-configuration)

만약 `@vercel/otel` 래퍼가 요구사항에 맞지 않는다면, OpenTelemetry를 수동으로 구성할 수 있습니다.

먼저 OpenTelemetry 패키지를 설치해야 합니다.
`_ Terminal`

```
npm install @opentelemetry/sdk-node @opentelemetry/resources @opentelemetry/semantic-conventions @opentelemetry/sdk-trace-base @opentelemetry/exporter-trace-otlp-http
```

이제 `instrumentation.ts`에서 `NodeSDK`를 초기화할 수 있습니다.
OpenTelemetry API는 엣지 런타임과 호환되지 않으므로, `process.env.NEXT_RUNTIME === 'nodejs'` 일 때에만 해당 API를 가져오도록 확인해야 합니다.
node를 사용할 때만 조건부로 가져오는 새 파일 `instrumentation.node.ts` 을 생성하는 것을 권장합니다.

`instrumentation.ts`

```ts
export async function register() {
  if (process.env.NEXT_RUNTIME === "nodejs") {
    await import("./instrumentation.node.ts");
  }
}
```

`instrumentation.node.ts`

```ts
import { trace, context } from "@opentelemetry/api";
import { NodeSDK } from "@opentelemetry/sdk-node";
import { OTLPTraceExporter } from "@opentelemetry/exporter-trace-otlp-http";
import { Resource } from "@opentelemetry/resources";
import { SemanticResourceAttributes } from "@opentelemetry/semantic-conventions";
import { SimpleSpanProcessor } from "@opentelemetry/sdk-trace-node";

const sdk = new NodeSDK({
  resource: new Resource({
    [SemanticResourceAttributes.SERVICE_NAME]: "next-app",
  }),
  spanProcessor: new SimpleSpanProcessor(new OTLPTraceExporter()),
});
sdk.start();
```

이렇게 하면 `@vercel/otel` 을 사용하는 것과 동일하지만 수정하고 확장할 수 있습니다.
예를 들어, `@opentelemetry/exporter-trace-otlp-http` 대신 `@opentelemetry/exporter-trace-otlp-grpc` 를 사용하거나 추가 리소스 속성을 지정할 수 있습니다.

---

### Testing your instrumentation

OpenTelemetry 추적을 로컬에서 테스트하려면, 호환되는 백엔드를 가진 OpenTelemetry 수집기가 필요합니다.
우리는 [OpenTelemetry dev environment](https://github.com/vercel/opentelemetry-collector-dev-setup)를 사용하는 것을 권장합니다.
만약 모든 것이 잘 작동한다면, `GET /requested/pathname` 으로 표시된 루트 서버 span을 볼 수 있어야 합니다.
해당 추적(trace)의 다른 모든 span은 그 하위에 중첩됩니다.
Next.js는 기본적으로 출력되는 span보다 더 많은 span을 추적합니다. 더 많은 span을 보려면 `NEXT_OTEL_VERBOSE=1` 로 설정해야 합니다.

---

### Deployment

#### Using OpenTelemetry Collector

OpenTelemetry Collector를 사용하여 배포할 때는 `@vercel/otel` 을 사용할 수 있습니다.
Vercel에서도 동작하며 자체 호스팅 환경에서도 동작합니다.

##### Deploying on Vercel

Vercel에서 OpenTelemetry가 기본 설정으로 작동하도록 확인했습니다.
프로젝트를 관찰성 공급자에 연결하기 위해 [Vercel 문서](https://vercel.com/docs/concepts/observability/otel-overview/quickstart)를 따르세요.

##### Self-hosting

다른 플랫폼에 배포하는 것도 간단합니다. Next.js 앱에서 생성된 텔레메트리 데이터를 수신하고 처리하기 위해 직접 OpenTelemetry Collector를 실행해야 합니다.
이를 위해 OpenTelemetry Collector 시작 가이드([OpenTelemetry Collector Getting Started guide](https://opentelemetry.io/docs/collector/getting-started/))를 따르세요. 이 가이드에서는 수집기를 설정하고 Next.js 앱에서 데이터를 수신하기 위한 구성 방법에 대해 안내합니다.
수집기를 실행한 후, 해당 플랫폼의 배포 가이드를 따라 선택한 플랫폼에 Next.js 앱을 배포할 수 있습니다.

#### Custom Exporters

OpenTelemetry Collector를 사용하는 것을 권장합니다. 그러나 플랫폼에서 사용할 수 없는 경우 [manual OpenTelemetry configuration](#manual-opentelemetry-configuration) 과 함께 사용자 정의 OpenTelemetry exporter를 사용할 수 있습니다.

---

### Custom Spans

[OpenTelemetry APIs](https://opentelemetry.io/docs/instrumentation/js/instrumentation/)를 사용하여 사용자 정의 span을 추가할 수 있습니다.

`_ Terminal`

```
npm install @opentelemetry/api
```

다음 예제는 GitHub 스타를 가져오는 함수이며, fetch 요청 결과를 추적하기 위해 사용자 정의 `fetchGithubStars` span을 추가하는 방법을 보여줍니다.

```js
import { trace } from "@opentelemetry/api";

export async function fetchGithubStars() {
  return await trace
    .getTracer("nextjs-example")
    .startActiveSpan("fetchGithubStars", async (span) => {
      try {
        return await getValue();
      } finally {
        span.end();
      }
    });
}
```

`register` 함수는 코드가 새로운 환경에서 실행되기 전에 실행됩니다. 새로운 span을 생성할 수 있으며, 이러한 span은 내보낸 추적(exported trace)에 정확하게 추가될 것으로 예상됩니다.

---

### Default Spans in Next.js

Next.js는 자동으로 여러 개의 span을 설하여 애플리케이션의 성능에 대한 유용한 통찰력을 제공합니다.

Span의 속성은 [OpenTelemetry semantic conventions](https://opentelemetry.io/docs/specs/otel/trace/semantic_conventions/)를 따릅니다. 또한 `next` 네임스페이스 아래에 몇 가지 사용자 정의 속성을 추가합니다.

- `next.span_name`
  - duplicates span name(중복 span 이름)
- `next.span_type`
  - each span type has a unique identifier(각 span에는 고유 식별자가 있습니다.)
- `next.route`
  - The route pattern of the request (각 요청의 경로 패턴)(e.g., `/[param]/user`).
- `next.page`
  - This is an internal value used by an app router.
    (이는 앱 라우터에서 사용하는 내부 값입니다.)
  - You can think about it as a route to a special file (like `page.ts`, `layout.ts`, `loading.ts` and others)
    (이를 특정 파일로의 경로로 생각할 수 있습니다.)
  - It can be used as a unique identifier only when paired with `next.route` because `/layout` can be used to identify both `/(groupA)/layout.ts` and `/(groupB)/layout.ts`
    (`next.route`와 결합될 때만 고유 식별자로 사용할 수 있습니다. 왜냐하면 `/layout`은 `/(groupA)/layout.ts`와 `/(groupB)/layout.ts`를 모두 식별하는 데 사용될 수 있기 때문입니다.)

### **`[[http.method] [next.route]](#httpmethod-nextroute)`**

- `next.span_type`: `BaseServer.handleRequest`

이 span은 Next.js 애플리케이션으로 들어오는 각 요청의 루트 span을 나타냅니다. 이 span은 요청의 HTTP 메서드, 라우트, 대상 및 상태 코드를 추적합니다.

Attributes:

- [Common HTTP attributes](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/http/#common-attributes)
  - `http.method`
  - `http.status_code`
- [Server HTTP attributes](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/http/#http-server-semantic-conventions)
  - `http.route`
  - `http.target`
- `next.span_name`
- `next.span_type`
- `next.route`

### **`[render route (app) [next.route]](#render-route-app-nextroute)`**

- `next.span_type`: `AppRender.getBodyResult`.

이 span은 앱 라우터에서 라우트를 렌더링하는 과정을 나타냅니다.

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.route`

### **`[fetch [http.method] [http.url]](#fetch-httpmethod-httpurl)`**

- `next.span_type`: `AppRender.fetch`

이 span은 코드에서 실행된 fetch 요청을 나타냅니다.

속성(Attributes):

- [Common HTTP attributes](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/http/#common-attributes)
  - `http.method`
- [Client HTTP attributes](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/http/#http-client)
  - `http.url`
  - `net.peer.name`
  - `net.peer.port` (only if specified)
- `next.span_name`
- `next.span_type`

### **`[executing api route (app) [next.route]](#executing-api-route-app-nextroute)`**

- `next.span_type`: `AppRouteRouteHandlers.runHandler`.

이 span은 앱 라우터에서 API 라우트 핸들러의 실행을 나타냅니다.

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.route`

### **`[getServerSideProps [next.route]](#getserversideprops-nextroute)`**

- `next.span_type`: `Render.getServerSideProps`.

이 span은 특정 라우트의 `getServerSideProps` 의 실행을 나타냅니다.

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.route`

### **`[getStaticProps [next.route]](#getstaticprops-nextroute)`**

- `next.span_type`: `Render.getStaticProps`.

이 span은 특정 라우트의 getStaticProps의 실행을 나타냅니다.

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.route`

### **`[render route (pages) [next.route]](#render-route-pages-nextroute)`**

- `next.span_type`: `Render.renderDocument`.

이 span은 특정 라우트의 문서 렌더링 과정을 나타냅니다.

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.route`

### **`[generateMetadata [next.page]](#generatemetadata-nextpage)`**

- `next.span_type`: `ResolveMetadata.generateMetadata`.

이 span은 특정 페이지의 메타데이터 생성 과정을 나타냅니다. (단일 라우트에는 여러 개의 이러한 span이 있을 수 있습니다.)

속성(Attributes):

- `next.span_name`
- `next.span_type`
- `next.page`
