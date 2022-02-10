
# Troubleshooting

This page contains details about error codes you may receive when attempting to communicate with ReqHub.

Our goal is that you never have to end up on this page. Since you're here, we hope to get you off of it as soon as possible and back on your way.

* [headers_missing](#headers_missing)
* [url_mismatch](#url_mismatch)
* [api_not_found](#api_not_found)
* [merchant_signature_invalid](#merchant_signature_invalid)
* [client_not_found](#client_not_found)
* [product_mismatch](#product_mismatch)
* [client_signature_invalid](#client_signature_invalid)
* [subscription_required](#subscription_required)

---
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

---
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

---
#### api_not_found

ReqHub uses the public key in the `MerchantKey` header to match a request to an API. This error occurs if ReqHub was unable to match an API to the API provider's public key.

This is most likely caused by a misconfiguration in the API, but can also be caused by an incorrect middleware implementation.

Check the following configuration items:

* Verify the keys are for the correct API
* Ensure the keys are present in the configuration
* Check that the publisher keys are used, not the client keys
* Check that the public and private keys aren't mixed up
* Check that no characters were lost while copy-pasting

If you are implementing a custom middleware, check the following items:

* Ensure the correct value is being used to set the `MerchantKey` header
* Check that the `MerchantKey` header is being set with the correct key (publisher public key)
* Check that the private key isn't being used to set the `MerchantKey` header
* Check that all characters are present in the `MerchantKey` header and none have been added or lost

For more information, see our [quickstart guide](getting-started/quickstart) or our guide on [writing your own middleware](guides/writing-your-own-middleware).

---
#### merchant_signature_invalid

Indicates a problem with the middleware. We don't expect you to receive this error unless you are [writing your own middleware](/guides/writing-your-own-middleware.md).

This error occurs when ReqHub is unable to rebuild the token hash to match the value from the `MerchantToken` header, or if the `Merchant` headers are invalid for any of the following reasons:

* The `Merchant` headers are duplicates from a previous request
* Unable to parse the `MerchantTimestamp` header into a number
* The timestamp indicates the request is too old (can happen if the timestamp is in seconds rather than milliseconds)

It can also indicate that there are issues with the other headers.
For example, if a valid token is reused with a different timestamp in the `MerchantTimestamp` header, ReqHub will be unable to rebuild the token because a different timestamp was used to create the original.
The same is true of other headers, such as `MerchantNonce` or `MerchantKey`, since they are also used to generate the token.
See the [token hash](/guides/writing-your-own-middleware.md?id=token-hash) section of our guide on [writing your own middleware](/guides/writing-your-own-middleware.md).

To fix this error, try the following items:

* Verify that the token [is being generated correctly](/guides/writing-your-own-middleware.md?id=token-hash) - the order of the concatenated items matters!
* Check that each header is correct as described in the guide on [writing your own middleware](/guides/writing-your-own-middleware.md?id=generating-the-headers)
* Check that no characters are being lost (or added) between generating and setting the headers
* Check that `MerchantTimestamp` is in milliseconds and not seconds (it should be 13 digits)
* Ensure that the network connection between your API and ReqHub isn't causing timeouts

For more information, see our guide on [writing your own middleware](guides/writing-your-own-middleware).

---
#### client_not_found

ReqHub uses the public key in the `ClientKey` header to match a request to an API user. This error occurs if ReqHub was unable to match a user to the client public key.

This is most likely caused by a misconfigured API client, but can also be caused by an incorrect middleware implementation.

Check the following configuration items:

* Verify the keys are for the correct API
* Ensure the keys are present in the configuration
* Check that the client keys are used, not the publisher keys (if this is your own API)
* Check that the public and private keys aren't mixed up
* Check that no characters were lost while copy-pasting

If you are implementing an API client, check the following items:

* Ensure all headers on the outgoing request are being set correctly according to our guide on [writing your own middleware](guides/writing-your-own-middleware)
* Check that the `ClientKey` header is present on the outgoing request
* Check that all characters are present in the `ClientKey` header and none have been added or lost

If you are implementing a custom middleware, check the following items:

* Ensure the correct value is being used to set the `ClientKey` header
* Check that the `ClientKey` header is being set correctly&mdash;your middleware should simply forward the value from the incoming request
* Check that all characters are present in the `ClientKey` header and none have been added or lost

For more information, see our [quickstart guide](getting-started/quickstart) or our guide on [writing your own middleware](guides/writing-your-own-middleware).

---
#### product_mismatch

This error occurs if the client and publisher keys are both valid, but for different APIs. This is an easy mistake if you are managing multiple APIs or client connections.

* If it's your own API, ensure the publisher keys were copied from the correct API page
* If calling an API, ensure the client keys were copied from the correct API page

---
#### client_signature_invalid

Indicates an issue with generating the ReqHub headers while sending a request to an API.

This error occurs when ReqHub is unable to rebuild the token hash to match the value from the `ClientToken` header, or if the `Client` headers are invalid for any of the following reasons:

* The `Client` headers are duplicates from a previous request
* Unable to parse the `ClientTimestamp` header into a number
* The timestamp indicates the request is too old (can happen if the timestamp is in seconds rather than milliseconds)

It can also indicate that there are issues with the other headers.
For example, if a valid token is reused with a different timestamp in the `ClientTimestamp` header, ReqHub will be unable to rebuild the token because a different timestamp was used to create the original.
The same is true of other headers, such as `ClientNonce` or `ClientKey`, since they are also used to generate the token.
See the [token hash](/guides/writing-your-own-middleware.md?id=token-hash) section of our guide on [writing your own middleware](/guides/writing-your-own-middleware.md).

If you are implementing an API client, check the following items:

* Verify that the token [is being generated correctly](/guides/writing-your-own-middleware.md?id=token-hash) - the order of the concatenated items matters!
* Check that each header is correct as described in the guide on [writing your own middleware](/guides/writing-your-own-middleware.md?id=generating-the-headers)
* Check that no characters are being lost (or added) between generating and setting the headers
* Check that `ClientTimestamp` is in milliseconds and not seconds (it should be 13 digits)
* Ensure that the network connection isn't causing timeouts

If you are implementing a custom middleware, check the following items:

* Ensure that all client header values are being set correctly&mdash;your middleware should simply forward them from the incoming request

For more information, see our guide on [writing your own middleware](guides/writing-your-own-middleware).

---
#### subscription_required

This error occurs when a client without a paid subscription calls an API that only has paid plans.

This can occur if the client cancels their subscription to the API and attempts to call it again, or if the API owner has changed to only paid plans after the client had already obtained API keys for free.
This error never occurs for the user that created the API.

The primary way to fix this error is:

* Purchase one of the subscription offerings

Alternatively, you could try the following:

* Convince the owner to offer a free plan
* Find a free alternative
* Write your own!

---
#### That's it!

Hopefully this helped! &#x1f64f;

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

