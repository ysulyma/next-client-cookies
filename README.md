# Next.js Client Cookies

Client cookies with support for SSR on the newest Next.js 13 (app directory).

SSR are **client components** that can be rendered both on the client and server side. Server components are **server only** components that can only be rendered on the server side. This library can support all 3 cases and will give you everything you need to work with cookies on the client and server side.

Please note that Next.js currently [does NOT support](https://nextjs.org/docs/app/api-reference/functions/cookies) updating cookies on Server Components (only on Actions or Routes are allowed to make changes to cookies within the server). If you need to update cookies on the server, please switch to a Client Component. Client components can be rendered on the server side (via SSR) and will support also updating cookies via **special dehydration mechanism** implemented by this library.

Interface and client side implementation based on the [js-cookie](https://www.npmjs.com/package/js-cookie) package.

## Install

1. Install the package:

```
npm add next-client-cookies
```

2. On your `app/layout.tsx` file, add the `CookiesProvider`:

```jsx
import { CookiesProvider } from 'next-client-cookies/server';

export default function RootLayout({ children }) {
  return <CookiesProvider>{children}</CookiesProvider>;
}
```

Please note this will make the entire app to opted out of static generation. If you want to opt out only some pages, you can use the `CookiesProvider` on a per page basis and create a custom server-side `layout.tsx` file for those pages.

## Usage

### Within a client side component

This will work **BOTH** on the client and server side for SSR.

```jsx
'use client';

import { useCookies } from 'next-client-cookies';

const MyComponent = () => {
  const cookies = useCookies();

  return (
    <div>
      <p>My cookie value: {cookies.get('my-cookie')}</p>

      <button onClick={() => cookies.set('my-cookie', 'my-value')}>
        Set cookie
      </button>
      {' | '}
      <button onClick={() => cookies.remove('my-cookie')}>Delete cookie</button>
    </div>
  );
};
```

### Within a server only component

Will produce the same `Cookies` interfaces using Next.js's `cookies()` helper.

```jsx
import { getCookies } from 'next-client-cookies/server';

const MyComponent = async () => {
  const cookies = await getCookies();

  return (
    <div>
      <p>My cookie value: {cookies.get('my-cookie')}</p>
    </div>
  );
};
```

## Migration

### From `v1`

If you are migrating from `v1`, you will need to add `await` to the `getCookies` function. This is because Next.js's `cookies()` function is now async and will return a promise.

```diff
- const cookies = getCookies();
+ const cookies = await getCookies();
```

## License

MIT
