원본 링크: [https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation](https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation)

# Instrumentation

참고: 이 기능은 실험적인 기능입니다. 사용하려면 `next.config.js`에서 `experimental.instrumentationHook = true;`를 명시적으로 정의해야 합니다.

이 파일에서 `register`라는 이름의 함수를 내보내면, 새로운 Next.js 서버 인스턴스가 부트스트랩될 때마다 해당 함수를 호출합니다. `register` 함수가 배포될 때마다 각각의 환경에서 한 번씩 (하지만 각 환경에서 정확히 한 번씩) 호출됩니다.

때로는 코드 내에서 파일을 가져오는 것이 유용할 수 있습니다. 이는 해당 파일이 일으킬 부작용 때문입니다. 예를 들어, 전역 변수 집합을 정의하는 파일을 가져올 수 있지만 코드에서 해당 가져온 파일을 명시적으로 사용하지 않을 수 있습니다. 그럼에도 불구하고 해당 패키지에서 선언한 전역 변수에 접근할 수 있습니다.

다음 예시에서처럼, 부작용을 일으키는 파일을 `instrumentation.ts` 에서 가져올 수 있습니다. 이를 `register`함수에서 사용하고자 할 때 다음과 같이 사용할 수 있습니다:

`/instrumentation.ts`

```ts
import { init } from "package-init";

export function register() {
  init();
}
```

하지만, 우리는 부작용이 있는 파일을 가져올 때는 `register` 함수 내에서 `import` 를 사용하는 것을 권장합니다. 다음 예시는 `register` 함수에서 `import` 를 사용한 기본적인 사용법을 보여줍니다:

`/instrumentation.ts`

```ts
export async function register() {
  await import("package-with-side-effect");
}
```

이렇게 함으로써 코드 내에서 모든 부작용을 한 곳에 모을 수 있으며, 파일을 가져올 때 발생할 수 있는 의도하지 않은 부작용을 피할 수 있습니다. 이렇게 하면 코드를 더욱 안정적으로 유지하고 예기치 않은 동작을 방지할 수 있습니다.

`register` 함수는 모든 환경에서 호출되므로, `edge` 와 `nodejs`를 모두 지원하지 않는 코드를 조건부로 가져와야 합니다. 현재 환경을 가져오기 위해 환경 변수 `NEXT_RUNTIME` 을 사용할 수 있습니다. 환경별 코드를 가져오는 예시는 다음과 같습니다:

`instrumentation.ts`

```ts
export async function register() {
  if (process.env.NEXT_RUNTIME === "nodejs") {
    await import("./instrumentation-node");
  }

  if (process.env.NEXT_RUNTIME === "edge") {
    await import("./instrumentation-edge");
  }
}
```
