# DIDWW Verification API OpenAPI Specification

This repository contains the [OpenAPI specification][openapi] for the DIDWW Verification API — the
REST interface to DIDWW's phone-number verification product: create a verification for a phone
number, deliver a one-time code by SMS or by a phone call, and report the code the user received.

The document is not written by hand. It is generated from an integration test suite that drives the
live endpoints: every request body, response body and status code it describes is exercised against
a running API on every build, and a response that stops matching its documented schema fails that
build. What you read here is what the API answered.

## Directory structure

| Path | Description |
|------|-------------|
| [`/openapi/`](./openapi/) | The OpenAPI 3.0 document, in JSON and YAML |
| [`VERSION`](./VERSION) | The API version the current document describes |
| [`RELEASING.md`](./RELEASING.md) | How a new document is published here |

## Files

| File | Description |
|------|-------------|
| [`openapi/verification_api.json`](./openapi/verification_api.json) | OpenAPI 3.0.3 document, JSON |
| [`openapi/verification_api.yaml`](./openapi/verification_api.yaml) | The same document, YAML |

Both files are generated from the same source and describe exactly the same API; CI fails if they
ever diverge. Pick whichever format your tooling prefers.

The paths above are stable — every release updates these files in place, so a tool can point at a
raw URL once and keep getting the current document:

```
https://raw.githubusercontent.com/didww/didww-verification-openapi/main/openapi/verification_api.json
https://raw.githubusercontent.com/didww/didww-verification-openapi/main/openapi/verification_api.yaml
```

## Authentication and environments

Every request is authenticated with HTTP Basic: the OTP application key as the username and its
secret as the password, both issued in the DIDWW user panel.

| Environment | Base URL |
|-------------|----------|
| Production | `https://verification.didww.com` |
| Sandbox | `https://verification-sandbox.didww.com` |

Both are declared as `servers` in the document, so a client generated from it can be pointed at the
sandbox without editing the base URL by hand.

## What the document covers

The full verification flow:

| Operation | Path |
|-----------|------|
| Create a verification | `POST /api/v1/verifications` |
| Fetch a verification | `GET /api/v1/verifications/{id}` |
| Fetch a verification by number | `GET /api/v1/verifications/by_number/{number}` |
| Report the received code | `PATCH /api/v1/verifications/{id}` |
| Report the received code by number | `PATCH /api/v1/verifications/by_number/{number}` |

The schemas are strict: the create payload is documented per delivery method (`sms`, `callout`) as
mutually exclusive `oneOf` branches, every error the API can answer is enumerated in a structured
error-code catalog generated from the API's own error registry, and an undocumented key appearing
in a request or response is treated as a defect rather than tolerated.

## Using the specification

The document is plain OpenAPI 3.0.3 with no vendor extensions, so standard tooling works as-is:

- render it — [Redocly][redocly], Swagger UI, Scalar, or your IDE's OpenAPI viewer;
- generate a client — [openapi-generator][openapi-generator] and similar (see the ready-made SDKs
  below before generating your own);
- import it into Postman, Insomnia or Bruno as a request collection;
- run a mock server against it — [Prism][prism];
- lint or diff it in CI to catch breaking changes before they reach you.

## Related

SDKs: [Ruby](https://github.com/didww/didww-verification-ruby-sdk) ·
[JS/TypeScript](https://github.com/didww/didww-verification-js-sdk) ·
[Python](https://github.com/didww/didww-verification-python-sdk) ·
[Dart/Flutter](https://github.com/didww/didww-verification-dart-sdk) ·
[iOS](https://github.com/didww/didww-verification-ios-sdk) ·
[Android](https://github.com/didww/didww-verification-android-sdk)

SDK guides: [doc.didww.com/otp-verification/sdks](https://doc.didww.com/otp-verification/sdks/index.html)

## Feedback

The document is generated, so it is not edited here: a mistake in it is a mistake in the generator
or in the API itself. Open an issue describing what you expected and what the API actually answered,
or contact [support@didww.com](mailto:support@didww.com).

## License

[MIT](./LICENSE)

[openapi]: https://spec.openapis.org/oas/v3.0.3
[redocly]: https://redocly.com/docs/cli/
[openapi-generator]: https://openapi-generator.tech/
[prism]: https://stoplight.io/open-source/prism
