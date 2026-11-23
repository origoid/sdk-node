# Origoid TypeScript Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Forigoid%2Fsdk-node)
[![npm shield](https://img.shields.io/npm/v/@origoid/sdk)](https://www.npmjs.com/package/@origoid/sdk)

Official SDK for OrigoID, a REST API for identity verification in Mexico:
CURP (RENAPO), INE, RFC, CSF and CFDI (SAT), employment, contribution weeks
and NSS (IMSS), ISSSTE, sanctions and tax lists, face match and liveness,
proof of address, SPEI receipts (CEP) and email validation.
Every response uses the same envelope, so integration is a switch on `type`.
Zero data retention: each request is validated, answered and discarded.

### What you can verify

| Source     | What it does                                                                                                                                              | Endpoint                                                                                                                                                                   | Guide                                                                                    |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| RENAPO     | Validate a CURP                                                                                                                                           | `POST /mex/renapo/v1/curp-validations`                                                                                                                                     | [CURP validation](https://origoid.com/en/curp-validation.html)                           |
| RENAPO     | Find a CURP from a person's name, date of birth, gender and state of birth                                                                                | `POST /mex/renapo/v1/curp-lookups`                                                                                                                                         | [Documentation](https://docs.origoid.com)                                                |
| RENAPO     | Get the official CURP document (Constancia de la CURP) as a PDF                                                                                           | `POST /mex/renapo/v1/curp-documents`                                                                                                                                       | [Documentation](https://docs.origoid.com)                                                |
| INE        | Check a voter credential against the voter list (Lista Nominal)                                                                                           | `POST /mex/id/v1/voter-list-validations`                                                                                                                                   | [INE voter ID validation](https://origoid.com/en/ine-validation.html)                    |
| INE        | Extract the data from an INE card (OCR)                                                                                                                   | `POST /mex/id/v1/voter-id-extractions`                                                                                                                                     | [INE OCR](https://origoid.com/en/ine-ocr.html)                                           |
| INE        | Read the QR codes on the back of an INE card                                                                                                              | `POST /mex/id/v1/qr-extractions`                                                                                                                                           | [Documentation](https://docs.origoid.com)                                                |
| SAT        | Validate an RFC                                                                                                                                           | `POST /mex/fiscal/v1/rfc-validations`                                                                                                                                      | [RFC validation](https://origoid.com/en/rfc-validation.html)                             |
| SAT        | Extract and validate a tax status certificate (CSF)                                                                                                       | `POST /mex/fiscal/v1/csf-extractions`                                                                                                                                      | [Documentation](https://docs.origoid.com)                                                |
| SAT        | Validate a CFDI invoice                                                                                                                                   | `POST /mex/fiscal/v1/cfdi-validations`                                                                                                                                     | [Documentation](https://docs.origoid.com)                                                |
| IMSS       | Employment verification from a CURP (verificación de empleo). The result is delivered to your webhook                                                     | `POST /mex/social-security/v1/imss-employment-status-async`                                                                                                                | [IMSS employment verification](https://origoid.com/en/imss-employment-verification.html) |
| IMSS       | Contribution weeks (semanas cotizadas). The result is delivered to your webhook; the second endpoint is the fallback if your webhook could not receive it | `POST /mex/social-security/v1/imss-contribution-weeks`, `POST /mex/social-security/v1/imss-contribution-weeks-results`                                                     | [IMSS contribution weeks](https://origoid.com/en/imss-contribution-weeks.html)           |
| IMSS       | NSS lookup from a CURP. The result is delivered to your webhook                                                                                           | `POST /mex/social-security/v1/imss-nss-lookup-async`                                                                                                                       | [IMSS NSS lookup](https://origoid.com/en/imss-nss-lookup.html)                           |
| ISSSTE     | ISSSTE record of a government worker from a CURP                                                                                                          | `POST /mex/social-security/v1/issste-records`                                                                                                                              | [Documentation](https://docs.origoid.com)                                                |
| Lists      | SAT Article 69 and 69-B, OFAC and PEPs screening                                                                                                          | `POST /mex/compliance/v1/sat-69-searches`, `POST /mex/compliance/v1/sat-69b-searches`, `POST /global/compliance/v1/ofac-searches`, `POST /mex/compliance/v1/peps-searches` | [Documentation](https://docs.origoid.com)                                                |
| Biometrics | Compare a selfie against the photo on the ID (face match)                                                                                                 | `POST /global/biometrics/v1/face-matches`                                                                                                                                  | [Face match](https://origoid.com/en/face-match.html)                                     |
| Biometrics | Liveness detection on a selfie (prueba de vida)                                                                                                           | `POST /global/biometrics/v1/liveness-checks`                                                                                                                               | [Documentation](https://docs.origoid.com)                                                |
| Documents  | Extract the data from a proof of address: utility bills and bank statements (comprobante de domicilio)                                                    | `POST /mex/documents/v1/proof-of-address-extractions`                                                                                                                      | [Documentation](https://docs.origoid.com)                                                |
| Banking    | Validate a SPEI transfer (CEP, Comprobante Electrónico de Pago)                                                                                           | `POST /mex/banking/v1/cep-validations`                                                                                                                                     | [Documentation](https://docs.origoid.com)                                                |
| Email      | Check an email address for deliverability and risk                                                                                                        | `POST /global/email/v1/email-validations`                                                                                                                                  | [Documentation](https://docs.origoid.com)                                                |

More: [IMSS API overview](https://origoid.com/en/imss-api.html) ·
[Webhooks guide](https://origoid.com/en/webhooks.html) ·
[Documentation](https://docs.origoid.com) · [Status](https://status.origoid.com)

**En español** — SDK oficial de OrigoID para verificación de identidad en México:
validar CURP en RENAPO, vigencia de la credencial de elector en la Lista Nominal del
INE, OCR de INE, RFC, Constancia de Situación Fiscal y CFDI en el SAT, verificación de
empleo, semanas cotizadas y NSS en el IMSS, búsqueda en OFAC, PEPs y listas SAT 69 y
69-B, comparación facial y prueba de vida. También: búsqueda de CURP por datos
personales, constancia de la CURP en PDF, lectura del QR de la INE, comprobante de
domicilio, registro del ISSSTE, validación de CEP (SPEI) y validación de correo.
Cero retención de datos.
Guías: [Validar CURP](https://origoid.com/validar-curp.html) ·
[Validar INE / Lista Nominal](https://origoid.com/validar-ine.html) ·
[OCR de INE](https://origoid.com/ocr-ine.html) · [Validar RFC](https://origoid.com/validar-rfc.html) ·
[Comparación facial](https://origoid.com/comparacion-facial.html) ·
[Verificación de empleo IMSS](https://origoid.com/verificar-empleo-imss.html) ·
[Semanas cotizadas IMSS](https://origoid.com/semanas-cotizadas-imss.html) ·
[Consultar NSS con la CURP](https://origoid.com/consultar-nss-imss.html) ·
[API del IMSS](https://origoid.com/api-imss.html) · [Webhooks](https://origoid.com/webhooks.html) ·
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
