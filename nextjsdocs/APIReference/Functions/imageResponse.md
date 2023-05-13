공식문서 : https://nextjs.org/docs/app/api-reference/functions/image-response

# **imageResponse API 참조**
`ImageResponse` 생성자를 사용하면 JSX 및 CSS를 사용하여 동적 이미지를 생성할 수 있습니다. 이는 Open Graph 이미지, 트위터 카드 등의 소셜 미디어 이미지를 생성하는 데 유용합니다.

`ImageResponse`에는 다음과 같은 옵션이 있습니다 : 
```javascript
import { ImageResponse } from 'next/server'
 
new ImageResponse(
  element: ReactElement,
  options: {
    width?: number = 1200
    height?: number = 630
    emoji?: 'twemoji' | 'blobmoji' | 'noto' | 'openmoji' = 'twemoji',
    fonts?: {
      name: string,
      data: ArrayBuffer,
      weight: number,
      style: 'normal' | 'italic'
    }[]
    debug?: boolean = false
 
    // HTTP 응답에 전달될 옵션들
    status?: number = 200
    statusText?: string
    headers?: Record<string, string>
  },
)
```

---
### **지원되는 CSS 속성**
지원되는 HTML 및 CSS 기능 목록은 [Satori의 문서](https://github.com/vercel/satori#css)를 참조하세요.