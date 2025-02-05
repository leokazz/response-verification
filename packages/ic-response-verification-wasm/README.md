# Response Verification

[Response verification](https://internetcomputer.org/docs/current/references/http-gateway-protocol-spec#response-verification) on the [Internet Computer](https://dfinity.org) is the process of verifying that a canister's response to an HTTP [query call](https://internetcomputer.org/docs/current/references/ic-interface-spec#http-query) has gone through consensus with other replicas hosting the same canister.

This package encapsulates the protocol for such verification. It is used by the [Service Worker](https://github.com/dfinity/ic/tree/master/typescript/service-worker), [ICX Proxy](https://github.com/dfinity/ic/tree/master/rs/boundary_node/icx_proxy), [HTTP Proxy](https://github.com/dfinity/http-proxy), and may be used by other implementations of the [HTTP Gateway Protocol](https://internetcomputer.org/docs/current/references/http-gateway-protocol-spec) in the future. These projects can be used as references when working on new integrations.

## Usage

```javascript
import initResponseVerification, {
  verifyRequestResponsePair,
  ResponseVerificationError,
  ResponseVerificationErrorCode,
} from '@dfinity/response-verification';

// this is necessary for web, but not for NodeJS consumers
await initResponseVerification();

try {
  const result = verifyRequestResponsePair(
    request,
    response,
    canister_id,
    current_time_ns,
    max_cert_time_offset_ns,
    fromHex(IC_ROOT_KEY),
  );

  // do something with the result
  // `result.passed` will be true if verification succeeds, false otherwise, and
  // `result.response` will contain the certified response object if verification was successful.
} catch (error) {
  if (error instanceof ResponseVerificationError) {
    switch (error.code) {
      case ResponseVerificationErrorCode.MalformedCbor:
        // the cbor returned from the replica was malformed.
        // ...
        break;

      case ResponseVerificationErrorCode.MalformedCertificate:
        // the certificate returned from the replica was malformed.
        // ...
        break;

      // Other error cases...
    }
  }
}
```

## Examples

See the following for working examples:

- [Web](https://github.com/dfinity/response-verification/tree/main/examples/web)
- [Service Worker](https://github.com/dfinity/response-verification/tree/main/examples/service-worker)
- [NodeJS](https://github.com/dfinity/response-verification/tree/main/examples/nodejs)

Note that when bundling for a service worker with Webpack. The `target` property must be set to `webworker`.
