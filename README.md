# Origoid TypeScript Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Forigoid%2Fsdk-node)
[![npm shield](https://img.shields.io/npm/v/@origoid/sdk)](https://www.npmjs.com/package/@origoid/sdk)

Official SDK for OrigoID, a REST API for identity verification in Mexico.
Validate a CURP against RENAPO, check an INE voter credential against the
voter list (Lista Nominal), extract data from the INE card, validate RFC,
tax status certificates (CSF) and CFDI invoices at SAT, look up an NSS at
IMSS, screen against OFAC, PEPs and SAT 69 / 69-B lists, and compare faces.
Every response uses the same envelope, so integration is a switch on `type`.
Zero data retention: each request is validated, answered and discarded.

**En español** — SDK oficial de OrigoID para verificación de identidad en México:
validar CURP en RENAPO, vigencia de la credencial de elector en la Lista Nominal del
INE, OCR de INE, RFC, Constancia de Situación Fiscal y CFDI en el SAT, NSS en el IMSS,
búsqueda en OFAC, PEPs y listas SAT 69 y 69-B, y comparación facial. Cero retención
de datos. Guías: [Validar CURP](https://origoid.com/validar-curp.html) ·
[Validar INE / Lista Nominal](https://origoid.com/validar-ine.html) ·
[OCR de INE](https://origoid.com/ocr-ine.html) · [Validar RFC](https://origoid.com/validar-rfc.html) ·
[Comparación facial](https://origoid.com/comparacion-facial.html) ·
[Documentación](https://docs.origoid.com) · [Estatus](https://status.origoid.com)

## Table of Contents

- [Documentation](#documentation)
- [Installation](#installation)
- [Reference](#reference)
- [Usage](#usage)
- [Request and Response Types](#request-and-response-types)
- [Exception Handling](#exception-handling)
- [Advanced](#advanced)
    - [Additional Headers](#additional-headers)
    - [Retries](#retries)
    - [Timeouts](#timeouts)
    - [Aborting Requests](#aborting-requests)
    - [Access Raw Response Data](#access-raw-response-data)
    - [Runtime Compatibility](#runtime-compatibility)
- [Contributing](#contributing)

## Documentation

API reference documentation is available [here](https://docs.origoid.com).

## Installation

```sh
npm i -s @origoid/sdk
```

## Reference

A full reference for this library is available [here](https://github.com/origoid/sdk-node/blob/HEAD/./reference.md).

## Usage

Instantiate and use the client with the following:

```typescript
import { OrigoidApiClient } from "@origoid/sdk";

const client = new OrigoidApiClient({ apiKey: "YOUR_API_KEY" });
await client.renapo.validateCurp({
    curp: "TEST900101HDFRRN09",
});
```

## Request and Response Types

The SDK exports all request and response types as TypeScript interfaces. Simply import them with the
following namespace:

```typescript
import { OrigoidApi } from "@origoid/sdk";

const request: OrigoidApi.IssueTokenRequest = {
    ...
};
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), a subclass of the following error
will be thrown.

```typescript
import { OrigoidApiError } from "@origoid/sdk";

try {
    await client.renapo.validateCurp(...);
} catch (err) {
    if (err instanceof OrigoidApiError) {
        console.log(err.statusCode);
        console.log(err.message);
        console.log(err.body);
        console.log(err.rawResponse);
    }
}
```

## Advanced

### Additional Headers

If you would like to send additional headers as part of the request, use the `headers` request option.

```typescript
const response = await client.renapo.validateCurp(..., {
    headers: {
        'X-Custom-Header': 'custom value'
    }
});
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2).

A request is deemed retryable when any of the following HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

Use the `maxRetries` request option to configure this behavior.

```typescript
const response = await client.renapo.validateCurp(..., {
    maxRetries: 0 // override maxRetries at the request level
});
```

### Timeouts

The SDK defaults to a 60 second timeout. Use the `timeoutInSeconds` option to configure this behavior.

```typescript
const response = await client.renapo.validateCurp(..., {
    timeoutInSeconds: 30 // override timeout to 30s
});
```

### Aborting Requests

The SDK allows users to abort requests at any point by passing in an abort signal.

```typescript
const controller = new AbortController();
const response = await client.renapo.validateCurp(..., {
    abortSignal: controller.signal
});
controller.abort(); // aborts the request
```

### Access Raw Response Data

The SDK provides access to raw response data, including headers, through the `.withRawResponse()` method.
The `.withRawResponse()` method returns a promise that results to an object with a `data` and a `rawResponse` property.

```typescript
const { data, rawResponse } = await client.renapo.validateCurp(...).withRawResponse();

console.log(data);
console.log(rawResponse.headers['X-My-Header']);
```

### Runtime Compatibility

The SDK defaults to `node-fetch` but will use the global fetch client if present. The SDK works in the following
runtimes:

- Node.js 18+
- Vercel
- Cloudflare Workers
- Deno v1.25+
- Bun 1.0+
- React Native

### Customizing Fetch Client

The SDK provides a way for you to customize the underlying HTTP client / Fetch function. If you're running in an
unsupported environment, this provides a way for you to break glass and ensure the SDK works.

```typescript
import { OrigoidApiClient } from "@origoid/sdk";

const client = new OrigoidApiClient({
    ...
    fetcher: // provide your implementation here
});
```

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically.
Additions made directly to this library would have to be moved over to our generation code,
otherwise they would be overwritten upon the next generated release. Feel free to open a PR as
a proof of concept, but know that we will not be able to merge it as-is. We suggest opening
an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!
