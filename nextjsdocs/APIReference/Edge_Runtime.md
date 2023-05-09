# **Edge Runtime**
Next.js Edge Runtime은 표준 Web API를 기반으로하며 다음 API를 지원합니다.

<br/>

## **Network APIs**
|API|설명|
|:--|:--|
|[`fetch`](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)|리소스를 가져옴
|[`Request `](https://developer.mozilla.org/ko/docs/Web/API/Request)|HTTP 요청을 나타냄
|[`Response`](https://developer.mozilla.org/ko/docs/Web/API/Response)|응답을 나타냄
|[`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers)|헤더를 나타냄
|[`FetchEvent`](https://developer.mozilla.org/ko/docs/Web/API/FetchEvent)|이벤트를 나타냄
|[`addEventListener`](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/addEventListener)|이벤트 리스너를 추가함
|[`FormData`](https://developer.mozilla.org/ko/docs/Web/API/FormData)|폼 데이터를 나타냄
|[`File`](https://developer.mozilla.org/ko/docs/Web/API/File)|파일을 나타냄
|[`Blob`](https://developer.mozilla.org/ko/docs/Web/API/Blob)|Blob을 나타냄
|[`URLSearchParams`](https://developer.mozilla.org/ko/docs/Web/API/URLSearchParams)|URL 검색 매개변수를 나타냄

## **Encoding APIs**
|API|설명|
|:--|:--|
|[`TextEncoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder)|문자열을 Uint8Array로 인코딩합니다.
|[`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder)|Uint8Array를 문자열로 디코딩합니다.
|[`atob`](https://developer.mozilla.org/en-US/docs/Web/API/atob)|base-64로 인코딩된 문자열을 디코딩합니다.
|[`btoa`](https://developer.mozilla.org/en-US/docs/Web/API/btoa)|문자열을 base-64로 인코딩합니다.

## **Stream APIs**
|API|설명|
|:--|:--|
|[`ReadableStream `](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)|읽을 수 있는 스트림을 나타냄
|[`WritableStream `](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream)|쓸 수 있는 스트림을 나타냄
|[`WritableStreamDefaultWriter `](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultWriterr)|쓸 수 있는 스트림의 작성자를 나타냄
|[`TransformStream `](https://developer.mozilla.org/en-US/docs/Web/API/TransformStream)|변환 스트림을 나타냄
|[`ReadableStreamDefaultReader `](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultReader)|읽을 수 있는 스트림의 리더를 나타냄
|[`ReadableStreamBYOBReader `](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBReader)|읽을 수 있는 스트림의 BYOB 리더를 나타냄

## **Crypto APIs**
|API|설명|
|:--|:--|
|[`crypto `](https://developer.mozilla.org/en-US/docs/Web/API/crypto_property)|플랫폼의 암호화 기능에 대한 액세스를 제공합니다.
|[`SubtleCrypto`](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)|해싱, 서명, 암호화 또는 복호화와 같은 일반적인 암호 기본 기능에 대한 액세스를 제공합니다.
|[`CryptoKey `](https://developer.mozilla.org/en-US/docs/Web/API/CryptoKey)|

## **Web Standard APIs**
|API|설명|
|:--|:--|
|[`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)|원하는 시점에 하나 이상의 DOM 요청을 중단할 수 있도록 합니다.
|[`DOMException`](https://developer.mozilla.org/en-US/docs/Web/API/DOMException)| DOM에서 발생하는 오류를 나타냅니다.
|[`structuredClone`](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm)|값을 깊은 복사하여 새로운 값으로 생성합니다.
|[`URLPattern`](https://developer.mozilla.org/en-US/docs/Web/API/URLPattern)|URL 패턴을 나타냅니다.
|[`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)| 값의 배열을 나타냅니다.
|[`ArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)|일반적인 고정 길이의 바이너리 데이터 버퍼를 나타냅니다.
|[`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)| 정적 메서드로 원자적 작업을 제공합니다.
|[`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)|임의 정밀도로 정수를 나타냅니다.
|[`BigInt64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt64Array)|64비트 부호 있는 정수의 타입화 배열을 나타냅니다.
|[`BigUint64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigUint64Array)|64비트 부호 없는 정수의 타입화 배열을 나타냅니다.
|[`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)|논리 개체를 나타내며 `true`와 `false`의 두 값 중 하나를 가집니다.
|[`clearInterval `](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval)|이전에 `setInterval()` 함수를 사용하여 설정한 시간 간격으로 반복적으로 호출되는 작업을 취소합니다.
|[`clearTimeout `](https://developer.mozilla.org/en-US/docs/Web/API/clearTimeout)|이전에 `setTimeout()` 함수를 사용하여 설정한 시간 간격으로 한 번 호출되는 작업을 취소합니다.
|[`console `](https://developer.mozilla.org/en-US/docs/Web/API/Console)|브라우저의 디버깅 콘솔에 액세스할 수 있는 기능을 제공합니다.
|[`DataView `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView)| `ArrayBuffer`의 일반적인 뷰를 나타냅니다.
|[`Date `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)|플랫폼 독립적인 형식으로 시간의 단일 순간을 나타냅니다.
|[`decodeURI `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)| `encodeURI` 또는 유사한 루틴에 의해 이전에 생성된 URI(Uniform Resource Identifier)를 디코딩합니다.
|[`decodeURIComponent `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)|`encodeURIComponent` 또는 유사한 루틴에 의해 이전에 생성된 URI 구성 요소를 디코딩합니다.
|[`encodeURI `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)|일부 문자열 인스턴스를 대체하여 문자의 UTF-8 인코딩을 나타내는 하나, 두 개, 세 개 또는 네 개의 이스케이프 시퀀스로 대체하여 URI(Uniform Resource Identifier)를 인코딩합니다.
|[`encodeURIComponent `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)|일부 문자열 인스턴스를 대체하여 문자의 UTF-8 인코딩을 나타내는 하나, 두 개, 세 개 또는 네 개의 이스케이프 시퀀스로 대체하여 URI 구성 요소를 인코딩합니다.
|[`Error `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)|문을 실행하거나 속성에 액세스할 때 오류가 발생하는 경우 오류를 나타냅니다.
|[`EvalError `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/EvalError)|전역 함수 `eval()`과 관련된 오류가 발생할 때 오류를 나타냅니다.
|[`Float32Array `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float32Array)|32비트 부동 소수점 숫자의 타입 배열을 나타냅니다.
|[`Float64Array `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array)| 64비트 부동 소수점 숫자의 타입 배열을 나타냅니다.
|[`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)|함수를 나타냅니다.
|[`Infinity `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity)|수학적인 무한대 값을 나타냅니다.
|[`Int8Array `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int8Array)|8비트 부호 있는 정수의 타입 배열을 나타냅니다.
|[`Int16Array `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int16Array)|16비트 부호 있는 정수의 타입 배열을 나타냅니다.
|[`Int32Array `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int32Array)|32비트 부호 있는 정수의 타입 배열을 나타냅니다.
|[`Intl `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)|국제화 및 지역화 기능에 액세스할 수 있습니다.
|[`isFinite `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isFinite)|값이 유한한 숫자인지 여부를 결정합니다.
|[`isNaN `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN)|값이 `NaN`인지 여부를 결정합니다.
|[`JSON `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)|JavaScript 값들을 JSON 형식으로 변환하거나 JSON 형식의 값을 JavaScript 값으로 변환하는 기능을 제공합니다.
|[`Map `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)|각 값이 한 번만 발생할 수 있는 값들의 컬렉션을 나타냅니다.
|[`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)|수학 함수와 상수에 대한 액세스를 제공합니다.
|[`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)|숫자 값으로 표현됩니다.
|[`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)|모든 JavaScript 객체의 기본이 되는 객체를 나타냅니다.
|[`parseFloat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)|문자열 인자를 구문 분석하고 부동 소수점 숫자를 반환합니다.
|[`parseInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)|문자열 인자를 구문 분석하고 지정된 진수의 정수를 반환합니다.
|[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)|비동기 작업의 최종 완료(또는 실패) 및 그 결과값을 나타냅니다.
|[`Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)|기본적인 연산(예: 속성 조회, 할당, 열거, 함수 호출 등)에 대한 사용자 정의 동작을 정의하는 데 사용되는 객체를 나타냅니다.
|[`RangeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RangeError)|값이 허용되는 값 집합이나 범위에 속하지 않을 때 발생하는 오류를 나타냅니다.
|[`ReferenceError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)|존재하지 않는 변수를 참조할 때 발생하는 오류를 나타냅니다.
|[`Reflect`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)|가로챌 수 있는 JavaScript 작업에 대한 메서드를 제공합니다.
|[`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)|문자열의 조합을 일치시키는 정규식을 나타내며 일치하는 문자열을 찾을 수 있습니다.
|[`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)|각 값이 한 번만 발생할 수 있는 값들의 컬렉션을 나타냅니다.
|[`setInterval`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/setInterval)| 일정한 시간 간격으로 함수를 반복 호출합니다.
|[`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/setTimeout)|지정된 밀리초 후 함수를 호출하거나 표현식을 평가합니다.
|[`SharedArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)|일반적인 고정 길이 이진 데이터 버퍼를 나타냅니다.
|[`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)|문자열의 시퀀스를 나타냅니다.
|[`Symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)|객체 속성의 키로 사용되는 고유하고 불변한 데이터 타입을 나타냅니다.
|[`SyntaxError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)| 구문적으로 잘못된 코드를 해석하려고 할 때 발생하는 오류를 나타냅니다.
|[`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError)|값이 예상되는 형식이 아닐 때 발생하는 오류를 나타냅니다.
|[`Uint8Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)|8비트 부호 없는 정수의 타입화된 배열을 나타냅니다.
|[`Uint8ClampedArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray)| 0에서 255로 제한된 8비트 부호 없는 정수의 타입화된 배열을 나타냅니다.
|[`Uint32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint32Array)|32비트 부호 없는 정수의 타입화된 배열을 나타냅니다.
|[`URIError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/URIError)|잘못된 방법으로 전역 URI 처리 함수를 사용했을 때 발생하는 오류를 나타냅니다.
|[`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL)|객체 URL을 생성하는 데 사용되는 정적 메서드를 제공하는 객체를 나타냅니다.
|[`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)|키/값 쌍의 컬렉션을 나타냅니다.
|[`WeakMap	`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)|키가 약한 참조로 지정된 키/값 쌍의 컬렉션을 나타냅니다.
|[`WeakSet	`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)| 각 객체가 한 번만 발생할 수 있는 객체의 컬렉션을 나타냅니다.
|[`WebAssembly`](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface)| WebAssembly에 액세스할 수 있게 해주는 기능을 제공합니다.


## **Next.js 특정 Polyfills**
- [`AsyncLocalStorage`](https://nodejs.org/api/async_context.html#class-asynclocalstorage)

## **환경 변수 (Environment variable)**
`next dev`와 `next build` 모두에서 환경 변수에 접근하기 위해 `process.env`를 사용할 수 있습니다.

`process.env`에 `console.log`을 실행하면 모든 환경 변수가 표시되지 않습니다. 아래와 같이 직접 변수에 액세스해야 합니다.
```javascript
console.log(process.env);
// { NEXT_RUNTIME: 'edge' }
console.log(process.env.TEST_VARIABLE);
// value
```

## **지원되지 않는 API들**
Edge 런타임에는 다음과 같은 제한 사항이 있습니다:
- **네이티브 Node.js API를 지원하지 않습니다.** 예를 들어, 파일 시스템에 읽거나 쓸 수 없습니다.
- `node_modules`는 ES Modules를 구현하고 네이티브 Node.js API를 사용하지 않는 한 사용할 수 있습니다.
- `require`을 직접 호출하는 것은 허용되지 않습니다. 대신 ES Modules을 사용하세요.

다음 JavaScript 언어 기능은 **비활성화되어 작동하지 않습니다**:
|API|설명|
|:--|:--|
|[`eval	`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)|문자열로 표현된 JavaScript 코드를 평가합니다.
|[`new Function(evalString)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)|인수로 제공된 코드로 새로운 함수를 만듭니다.
|[`WebAssembly.compile`](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/compile)|버퍼 소스에서 WebAssembly 모듈을 컴파일합니다.
|[`WebAssembly.instantiate`](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/instantiate)|버퍼 소스에서 WebAssembly 모듈을 컴파일하고 인스턴스화합니다.

드물게, 코드에 런타임에서 도달할 수 없고 트리쉐이킹(Tree shaking)으로 제거할 수 없는 동적 코드 평가 구문이 포함되거나 가져올 수 있습니다. 미들웨어나 Edge API 라우트 내보낸 구성으로 특정 파일을 허용하도록 검사를 완화할 수 있습니다. :
```javascript
export const config = {
  runtime: 'edge', // Edge API 라우트 전용
  unstable_allowDynamic: [
    // 특정 파일 허용
    '/lib/utilities.js',
    // function-bind 3rd party 모듈 내의 모든 파일을 허용
    '/node_modules/function-bind/**',
  ],
};
```

`unstable_allowDynamic`는 특정 파일에 대한 동적 코드 평가를 무시하는 glob 또는 glob 배열입니다. [glob](https://github.com/micromatch/micromatch#matching-features)는 응용 프로그램 루트 폴더를 기준으로 상대적입니다.

하지만 이러한 문이 Edge에서 실행되면 *런타임 오류를 발생시켜 예외를 던집니다.* 주의해야 합니다.





