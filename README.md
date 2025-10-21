# bun-is-mobile

The fastest, lightweight, dependency‑free mobile device detection for Bun and the browser.

It's not much, but it works… 🫠…

## Overview

`bun-is-mobile` exposes a single function that tells you whether a request (or the current runtime) is coming from a mobile device. It prefers modern User‑Agent Client Hints when available and falls back to well‑tuned user‑agent pattern matching.

## Features

- Robust regex fallback for popular mobile user‑agents
- Runs in Bun (server) and the browser
- TypeScript‑first with a simple, boolean result
- Uses UA‑CH (`navigator.userAgentData?.mobile`) when available
- Works with input types:
  - Bun `Request` objects
  - Node `http.IncomingMessage` objects
  - Raw User‑Agent strings
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
// Browser…
import isMobile from 'bun-is-mobile';

const onMobile = isMobile();

if (onMobile) {
  // Do stuff…
}
```

```ts
// Bun server…
import isMobile from 'bun-is-mobile';

Bun.serve({
  fetch(request: Request) {
    const onMobile = isMobile(request);

    return new Response(onMobile ? 'Hello, mobile!' : 'Hello, desktop!');
  },
});
```

```ts
// Node server…
import { createServer } from 'http';
import isMobile from 'bun-is-mobile';

createServer((request, response) => {
  const onMobile = isMobile(request);

  response.statusCode = 200;
  response.end(onMobile ? 'Hello, mobile!' : 'Hello, desktop!');

  return;
}).listen(3000);
```

## API Highlights

- Default export: `(input?: Request | http.IncomingMessage | string) => boolean`
  - `Request` (Bun/Fetch) – reads the `user-agent` header
  - `http.IncomingMessage` (Node/Bun Node-compat) – reads the `user-agent` header
  - `string` – pass any User-Agent string directly
  - `undefined` – uses `navigator.userAgentData?.mobile` and `navigator.userAgent` (browser‑only)

## Notes

- Regex patterns adapted from [Detect Mobile Browsers](http://detectmobilebrowsers.com/).

- User-Agent detection is heuristic by nature; prefer responsive design when possible. UA‑CH is used when present to reduce false positives.
- When calling without an argument, use it only in environments where `navigator` exists (browsers). On the server, pass a `Request`, `http.IncomingMessage`, or a User-Agent string.

---

MIT © Stev Peifer
