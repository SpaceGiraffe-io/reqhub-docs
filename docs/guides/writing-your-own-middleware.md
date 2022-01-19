
# Writing your own middleware

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

You should probably also have a good understanding of the request flow from our guide on [how ReqHub works](/getting-started/how-it-works?id=request-flow).

----

## Introduction

In any language, a ReqHub middleware takes a set of headers that identify the client, combines them with headers that identify the API, and sends them to ReqHub for verification.
ReqHub provides a success response if the client is allowed access, or an error otherwise.

The request flow looks like this, per our guide on [how ReqHub works](/getting-started/how-it-works?id=request-flow).

![ReqHub request flow](https://reqhubprod.blob.core.windows.net/public/docs/flow-diagram-raw.png)

Let's break it down into more detail.

#### Request 1 - can I has dis?

This is the initial request from a client appliation to your API.

It should contain the following headers:

* `ClientToken` - base64-encoded HMAC-SHA256 hash (more on that below)
* `ClientKey` - the client's public key from ReqHub
* `ClientTimestamp` - a UNIX timestamp (in milliseconds) of when the request was created
* `ClientNonce` - 20 random bytes as a base64-encoded string
* `ClientUrl` - the requested URL

#### Request 2 - hey can they has dat?

This is the request from your API to ReqHub for verification.

This should be a POST request to `https://api.reqhub.io/req`.

The request must include the client headers, and should add the following merchant headers:

* `MerchantToken` - base64-encoded HMACSHA256 hash (more on that later)
* `MerchantKey` - your publisher public key from ReqHub
* `MerchantTimestamp` - a UNIX timestamp (in milliseconds) of when the request was created
* `MerchantNonce` - 20 random bytes as a base64-encoded string
* `MerchantUrl` - the requested URL

Then the complete request would include all the headers above:
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

Keep reading or [jump to the section](/guides/writing-your-own-middleware?id=generating-the-headers) for more information on creating each of the headers.

#### Response 2 - uhhh yup.

If all the headers line up correctly, ReqHub will provide a success response&mdash;in which case congratulations, your middleware is working! &#x1f389;

Otherwise, these are the reasons ReqHub will deny your request:

* `headers_missing` - any of the headers are missing
* `url_mismatch` - ClientUrl and MerchantUrl headers don't match exactly
* `api_not_found` - unable to match an API to MerchantKey
* `merchant_signature_invalid` - unable to reconstruct the MerchantToken hash, the request is a duplicate, or the timestamp is too old
* `client_not_found` - unable to match a client to ClientKey
* `product_mismatch` - ClientKey is for a different API
* `client_signature_invalid` - unable to reconstruct the ClientToken hash
* `subscription_required` - for paid APIs with no free plans, the client must have an active subscription

In each of these cases, ReqHub will respond with a `400 Bad Request` HTTP status code. For more help with error codes, check out our [troubleshooting guide](/guides/troubleshooting.md).

#### Response 1 - okay here you go.

If the response from ReqHub was successful, your API should allow the request to continue normally.
Otherwise you should return an error response, preferably including the ReqHub error code and helpful diagnostic information.

Our official Middlewares set ReqHub response data somewhere on the request context so it can be accessed within the API.
For error responses they return `403 Forbidden` HTTP status codes with the error message from ReqHub.

You can handle the response with something like this:

```js
// pseudocode

let reqhubResponse = http.post('https://api.reqhub.io/req', headers)

if (reqhubResponse.statusCode == http.OK) {
  setContext(reqhubResponse.data)
  handleRequest()
} else {
  error()
}
```

For more examples, the following are links to the official implementations:

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet/blob/master/ReqHubDotNet/Middleware/ReqHubMerchantMiddleware.cs)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode/blob/master/src/middleware/merchant-middleware.js)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo)

## Generating the headers

The following are specifics on generating each of the headers.

#### Key

The `MerchantKey` and `ClientKey` headers are the public keys for the API.

`MerchantKey` should be configured in the API and registered with your middleware somehow.
It is the **Public key** value from the **Publisher keys** section of the API page.

`ClientKey` will be provided by the client in their request.
It is the **Public key** value from the **Client keys** section of the API page.
The official middleware packages include an API client that allows the user to configure their client keys.
If you are creating a library, you should consider providing an API client.

#### URL

The URL value must match exactly between the client and your API.
Generally this is the request `path` (as in `https://domain.com/path`).
We find this easier to replicate across environments because it doesn't require knowledge of the domain or any other aspects of the URL,
and is enough to indicate that the client and API both agree that they're making the same request.

#### Timestamp

This header is the UNIX timestamp in milliseconds. It should be 13 digits.

#### Nonce

Generate 20 random bytes, then encode in base64. The resulting string is the nonce.

For example, from our [JavaScript implementation](https://github.com/SpaceGiraffe-io/ReqHubNode/blob/master/src/utility/hashing-utility.js#L8-L9):

```js
const nonceData = CryptoJS.lib.WordArray.random(20);
const nonce = CryptoJS.enc.Base64.stringify(nonceData);
```

#### Token hash

The token should be an HMAC-SHA256 hash encoded with base64.
In generating the HMAC-SHA256 value, hash the public key using the concatenation of `privateKey + timestamp + nonce + requestUrl` (in that order!) as the secret.

That sounds more complicated than it is. For example, our [JavaScript implementation](https://github.com/SpaceGiraffe-io/ReqHubNode/blob/master/src/utility/hashing-utility.js#L13-L16) is only 3 lines:
```js
const secret = `${privateKey}${timestamp}${nonce}${requestUrl}`;
const hmacsha256 = CryptoJS.HmacSHA256(publicKey, secret);
const token = CryptoJS.enc.Base64.stringify(hmacsha256);
```

## Examples

All of our official middleware libraries are available in GitHub!

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo)

#### That's it!

Hopefully this makes writing your middleware significantly easier! &#x1f91e;

If you run into any issues, check out our [troubleshooting guide](/guides/troubleshooting.md) for more information on error codes.

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

