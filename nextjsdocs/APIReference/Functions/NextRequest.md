원본 링크: [https://nextjs.org/docs/app/api-reference/functions/next-request](https://nextjs.org/docs/app/api-reference/functions/next-request)

# **NextRequest**

NextRequest는 추가적인 편의 메서드를 통해 [Web Request API](https://developer.mozilla.org/en-US/docs/Web/API/Request) 를 확장합니다.

---

## **`[cookies](https://nextjs.org/docs/app/api-reference/functions/next-request#cookies)`**

요청의 `[Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)` 헤더를 읽거나 변경합니다.

### **`[set(name, value)](https://nextjs.org/docs/app/api-reference/functions/next-request#setname-value)`**

이름이 지정되면 요청에 지정된 값으로 쿠키를 설정합니다.

```jsx
// Given incoming request /home
// Set a cookie to hide the banner
// request will have a `Set-Cookie:show-banner=false;path=/home` header
request.cookies.set('show-banner', 'false');
```

### **`[get(name)](https://nextjs.org/docs/app/api-reference/functions/next-request#getname)`**

쿠키 이름이 주어지면 쿠키의 값을 반환합니다. 쿠키를 찾을 수 없으면 `undefined` 가 반환됩니다. 여러개의 쿠키가 발견되면 첫번째 쿠키가 반환됩니다.

```jsx
// Given incoming request /home
// { name: 'show-banner', value: 'false', Path: '/home' }
request.cookies.get('show-banner');
```

### **`[getAll()](https://nextjs.org/docs/app/api-reference/functions/next-request#getall)`**

쿠키 이름이 주어지면 쿠키의 값을 반환합니다. 이름이 지정되지 않으면 요청에 대한 모든 쿠키를 반환합니다.

```jsx
// Given incoming request /home
// [
//   { name: 'experiments', value: 'new-pricing-page', Path: '/home' },
//   { name: 'experiments', value: 'winter-launch', Path: '/home' },
// ]
request.cookies.getAll('experiments');
// Alternatively, get all cookies for the request
request.cookies.getAll();
```

### **`[delete(name)](https://nextjs.org/docs/app/api-reference/functions/next-request#deletename)`**

쿠키 이름이 주어지면 요청에서 해당 쿠키를 삭제합니다.

```jsx
// Returns true for deleted, false is nothing is deleted
request.cookies.delete('experiments');
```

### **`[has(name)](https://nextjs.org/docs/app/api-reference/functions/next-request#hasname)`**

쿠키 이름이 주어지고 요청에 쿠키가 존재하면 `true`를 반환합니다.

```jsx
// Returns true if cookie exists, false if it does not
request.cookies.has('experiments');
```

### **`[clear()](https://nextjs.org/docs/app/api-reference/functions/next-request#clear)`**

요청에서 `Set-Cookie` 헤더를 제거합니다.

```jsx
request.cookies.clear();
```

---

## **`[nextUrl](https://nextjs.org/docs/app/api-reference/functions/next-request#nexturl)`**

Next.js 특정 속성을 포함한 추가 편의 메서드로 네이티브 `[URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)` API를 확장합니다.

```jsx
// Given a request to /home, pathname is /home
request.nextUrl.pathname;
// Given a request to /home?name=lee, searchParams is { 'name': 'lee' }
request.nextUrl.searchParams;
```