
# Troubleshooting

This page contains details about error codes you may receive when attempting to communicate with ReqHub.

Our goal is that you never have to end up on this page. Since you're here, we hope to get you off of it as soon as possible and back on your way.

#### headers_missing

Occurs if any of the required ReqHub headers are missing.

If you are using an official ReqHub package, ensure your API keys are configured correctly. You may receive this error if your API keys are missing. This can occur both when distributing an API and using a client to call an API.

If you are writing your own middleware, double-check to ensure none of the headers are missing while creating the request to `https://api.reqhub.io/req`.

These are the required headers:

* `ClientToken` - base64-encoded HMAC-SHA256 hash
* `ClientKey` - the client's public key from ReqHub
* `ClientTimestamp` - a UNIX timestamp (in milliseconds) of when the request was created
* `ClientNonce` - 20 random bytes as a base64-encoded string
* `ClientUrl` - the requested URL, must match `MerchantUrl` exactly
* `MerchantToken` - base64-encoded HMACSHA256 hash
* `MerchantKey` - the publisher public key from ReqHub
* `MerchantTimestamp` - a UNIX timestamp (in milliseconds) of when the request was created
* `MerchantNonce` - 20 random bytes as a base64-encoded string
* `MerchantUrl` - the requested URL, must match `ClientUrl` exactly

A complete request should include headers that look something like this:

```js
{
  ClientToken: "ayEzBx8+rJxLFkoi8XQwc8Ff28CMpz07a4bZpd84zc0=",
  ClientKey: "K+mE4RjP4ZqDgq7mxfydILlmXQe9CYFPCgkjYaeW6/e1/vyRUOD0/p7IQY1jNq3boD7HJlABUUdtOzydsCCrgw==",
  ClientTimestamp: "1642001473447",
  ClientNonce: "EuRF7LWuG5yDl0rqTmcX/WtmCIk=",
  ClientUrl: "/test",
  MerchantToken: "olU8rWJr28OKi1gvTRsvHIVTETQ2azn8S4WG9GTINbI=",
  MerchantKey: "2vRUJjV2lY88a1C4LRL7RPFC74vr0HJBP3D2TJCuR/OIM16UClIWJ4mw9pU4ftUFMG6LFAKEEDUk1bC/dJxCZg==",
  MerchantTimestamp: "1642001473468",
  MerchantNonce: "WS0dXv0hR45zxWTnqol8XEj9X2M=",
  MerchantUrl: "/test"
}
```

If any of the above headers are `null`, empty, or missing from the request, you will receive the `headers_missing` error.

Note that if a value is incorrect, but not `null` or empty, you may receive a different error such as `api_not_found`, `client_not_found`, `merchant_signature_invalid`, or `client_signature_invalid`.

For example, the string `"null"` in the `MerchantKey` header will produce an `api_not_found` error:
```js
{
  ...
  MerchantKey: "null", // the string of the word "null", not the value null!
  ...
}

// ---> results in api_not_found
```

Our guide on [writing your own middleware](/guides/writing-your-own-middleware.md) may be helpful.

#### url_mismatch

Occurs when the `ClientUrl` and `MerchantUrl` header values do not match **exactly**.

```js
// good
{
  ClientUrl: "/test",
  MerchantUrl: "/test"
}

// bad
// (slashes don't match -- doesn't matter if the slash is present or not, but it matters that they match)
{
  ClientUrl: "test",
  MerchantUrl: "/test"
}

// bad
// (domains don't match)
{
  ClientUrl: "http://localhost:3000/test",
  MerchantUrl: "https://example-api.spacegiraffe.io/test"
}
```

When requesting the base route for an API, you may need to include the `/`.

```js
// good
http.get(baseUrl + '/')

// could be bad
http.get(baseUrl);
```

If you are writing your own middleware, ensure that your code is producing the exact same values for these headers.

In the official middlewares, we use the `path` URL component for simplicity.
This is because the `path` is usually readily available and the middleware doesn't have to be aware of domains.
The trick is usually making sure the correct value is being accessed.

#### api_not_found

Occurs if ReqHub was unable to match an API to the publisher's public key from the `MerchantKey` header.

Possible causes:
* ReqHub middleware is not configured correctly in the API. Ensure the keys are present and you are using the correct publisher keys. Verify that you haven't mixed up the public and private key. Verify no characters were lost while copy-pasting.

see the quickstart, or the README for your language:

[readme links for the publisher header]

[probably want to work on wording/formatting for these ones]

#### merchant_signature_invalid

#### client_not_found

Occurs if ReqHub was unable to match a record to the client's public key from the `ClientKey` header.

Possible causes:
* HTTP client is not configured correctly when calling an API. Ensure the keys are present and you are using the correct client keys. Verify that you haven't mixed up the public and private key. Verify no characters were lost while copy-pasting.

see the quickstart, or the README for your language:

[readme links for calling an API header]

[probably want to work on wording/formatting for these ones]

#### product_mismatch

#### client_signature_invalid

#### subscription_required

