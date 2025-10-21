# bun-is-mobile

Lightweight, dependency‑free mobile device detection for Bun and the browser.

## Overview

`bun-is-mobile` exposes a single function that tells you whether a request (or the current runtime) is coming from a mobile device. It prefers modern User‑Agent Client Hints when available and falls back to well‑tuned user‑agent pattern matching.

## Features

- Robust regex fallback for popular mobile user‑agents
- Runs in Bun (server) and the browser
- TypeScript‑first with a simple, boolean result
- Uses UA‑CH (`navigator.userAgentData?.mobile`) when available
- Works with `Request` objects or raw user‑agent strings
- Zero dependencies, tiny and fast (compiled once)

## Requirements

- Any modern browser for client‑side usage
- Bun runtime for server‑side usage

## Installation

```sh
bun add bun-is-mobile
```

## Quick Start

```ts
import isMobile from 'bun-is-mobile';

// Bun server: branch responses by device…
Bun.serve({
  fetch(request: Request) {
    return new Response(isMobile(request) ? 'Hello, mobile!' : 'Hello, desktop!');
  },
});
```

```ts
// Browser: detect the current device…
import isMobile from 'bun-is-mobile';

if (isMobile()) {
  // mobile‑specific UI…
}
```

## API Highlights

- Default export: `(input?: Request | string) => boolean`
  - `Request` (Bun/Fetch) – reads the `user-agent` header
  - `string` – pass any UA string directly
  - `undefined` – uses `navigator.userAgentData?.mobile` and `navigator.userAgent` (browser‑only)

## Examples

```ts
// Using a Request (recommended on the server)…
import isMobile from 'bun-is-mobile';

export function deviceClass(request: Request) {
  return isMobile(request) ? 'mobile' : 'desktop';
}
```

```ts
// Using a raw user‑agent string…
import isMobile from 'bun-is-mobile';

const userAgent = 'Mozilla/5.0 (iPhone; CPU iPhone OS 17_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Mobile/15E148 Safari/604.1';

console.log(isMobile(userAgent)); // true…
```

```ts
// Browser runtime (no args)…
import isMobile from 'bun-is-mobile';

const onMobile = isMobile();
```

## Notes

- Regex patterns adapted from [Detect Mobile Browsers](http://detectmobilebrowsers.com/).

- User-Agent detection is heuristic by nature; prefer responsive design when possible. UA‑CH is used when present to reduce false positives.
- When calling without an argument, use it only in environments where `navigator` exists (browsers). On the server, pass a `Request` or a User-Agent string.

---

MIT © Stev Peifer
