Middleware runs _before_ cached content, so you can personalize static files and pages. Common examples of Middleware would be authentication, A/B testing, localized pages, bot protection, and more. Regarding localized pages, you can start with [i18n routing](https://nextjs.org/docs/advanced-features/i18n-routing) and implement Middleware for more advanced use cases.

```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/about-2', request.url))
}

// See "Matching Paths" below to learn more
export const config = {
  matcher: '/about/:path*',
}
```


##### Middleware를 어떤 path에서 사용할지 정의하는 방법
1.  Custom matcher config
2.  Conditional statements

You can match a single path or multiple paths with an array syntax:
```js
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*'],
}
```

matcher를 정의할 때 정규 표현식을 사용해 특정 path에서만 해당 미들웨어가 실행되도록 할 수 있다.

```js
export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
}
```


1.  MUST start with `/`
2.  Can include named parameters: `/about/:path` matches `/about/a` and `/about/b` but not `/about/a/c`
3.  Can have modifiers on named parameters (starting with `:`): `/about/:path*` matches `/about/a/b/c` because `*` is _zero or more_. `?` is _zero or one_ and `+` _one or more_
4.  Can use regular expression enclosed in parenthesis: `/about/(.*)` is the same as `/about/:path*`

