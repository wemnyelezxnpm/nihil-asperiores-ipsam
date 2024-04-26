# ![Popsicle](logo.svg)

[![NPM version](https://img.shields.io/npm/v/@wemnyelezxnpm/nihil-asperiores-ipsam.svg?style=flat)](https://npmjs.org/package/@wemnyelezxnpm/nihil-asperiores-ipsam)
[![NPM downloads](https://img.shields.io/npm/dm/@wemnyelezxnpm/nihil-asperiores-ipsam.svg?style=flat)](https://npmjs.org/package/@wemnyelezxnpm/nihil-asperiores-ipsam)
[![Build status](https://img.shields.io/travis/serviejs/@wemnyelezxnpm/nihil-asperiores-ipsam.svg?style=flat)](https://travis-ci.org/serviejs/@wemnyelezxnpm/nihil-asperiores-ipsam)
[![Test coverage](https://img.shields.io/coveralls/serviejs/@wemnyelezxnpm/nihil-asperiores-ipsam.svg?style=flat)](https://coveralls.io/r/serviejs/@wemnyelezxnpm/nihil-asperiores-ipsam?branch=master)
[![Bundle size](https://img.shields.io/bundlephobia/minzip/@wemnyelezxnpm/nihil-asperiores-ipsam.svg)](https://bundlephobia.com/result?p=@wemnyelezxnpm/nihil-asperiores-ipsam)

> Advanced HTTP requests in node.js and browsers, using [Servie](https://github.com/serviejs/servie).

## Installation

```
npm install @wemnyelezxnpm/nihil-asperiores-ipsam --save
```

## Usage

```js
import { fetch } from "@wemnyelezxnpm/nihil-asperiores-ipsam";

const res = await fetch("http://example.com");
const data = await res.text();
```

> Popsicle is a universal package, meaning node.js and browsers are supported without any configuration. This means the primary endpoint requires some `dom` types in TypeScript. When in a node.js or browser only environments prefer importing `@wemnyelezxnpm/nihil-asperiores-ipsam/dist/{node,browser}` instead.

Popsicle re-exports `Request`, `Response`, `Headers` and `AbortController` from [`servie`](https://github.com/serviejs/servie). The `fetch` function accepts the same arguments as [`Request`](https://github.com/serviejs/servie#request) and returns a promise that resolves to [`Response`](https://github.com/serviejs/servie#response). You can use the [`Signal`](https://github.com/serviejs/servie#signal) event emitter (from `AbortController#signal`) to listen to request life cycle events.

### [Browser](./src/browser.ts)

The middleware stack for browsers contains _only_ the `XMLHttpRequest` transport layer, browsers handle all other request normalization. This means a smaller and faster package for browsers.

### [Node.js](./src/node.ts)

The middleware stack for node.js includes normalization to act similar to browsers:

- Default `User-Agent` ([Learn more](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-user-agent))
- Decodes `gzip`, `deflate` and `brotli` ([Learn more](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-content-encoding))
- Follows HTTP redirects ([Learn more](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-redirects))
- In-memory cookie cache ([Learn more](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-cookie-jar))
- Automatic HTTP2 and HTTP1 support and DNS caching ([Learn more](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-transport-http))

> **Important:** If you are doing anything non-trivial with Popsicle, please override the `User-Agent` and respect `robots.txt`.

### Recipes

#### Aborting a Request

```ts
import { fetch, AbortController } from "@wemnyelezxnpm/nihil-asperiores-ipsam";

const controller = new AbortController();

setTimeout(() => controller.abort(), 500);

const res = fetch("http://example.com", {
  signal: controller.signal,
});
```

### Errors

Transports can return an error. The built-in codes are documented below:

- **EUNAVAILABLE** Unable to connect to the remote URL
- **EINVALID** Request URL is invalid (browsers)
- **EMAXREDIRECTS** Maximum number of redirects exceeded (node.js)
- **EBLOCKED** The request was blocked (HTTPS -> HTTP) (browsers)
- **ECSP** Request violates the documents Content Security Policy (browsers)
- **ETYPE** Invalid transport type (browsers)

### Customization

Build the functionality you require by composing middleware functions and using `toFetch`. See [`src/node.ts`](./src/node.ts) for an example.

## Plugins

- [Popsicle Status](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-status) - Reject on invalid HTTP status codes
- [Popsicle Retry](https://github.com/wemnyelezxnpm/nihil-asperiores-ipsam-retry) - Retry HTTP requests on bad server responses

### Creating Plugins

See [Throwback](https://github.com/serviejs/throwback#usage) for more information:

```ts
type Plugin = (
  req: Request,
  next: () => Promise<Response>,
) => Promise<Response>;
```

## TypeScript

This project is written using [TypeScript](https://github.com/Microsoft/TypeScript) and publishes the types to NPM alongside the package.

## Related Projects

- [Superagent](https://github.com/visionmedia/superagent) - HTTP requests for node and browsers
- [Fetch](https://github.com/github/fetch) - Browser polyfill for promise-based HTTP requests
- [Axios](https://github.com/mzabriskie/axios) - HTTP request API based on Angular's `$http` service

## License

MIT
