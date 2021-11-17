
# Adding ReqHub to an existing API

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

If you're interested in monetizing your API, you can follow our guide on [setting up Stripe](guides/setting-up-stripe.md).

----

## Quickstart

For the most part, adding ReqHub to an existing API follows the same steps outlined in our [quickstart guide](/getting-started/quickstart?id=distributing-an-api),
but your specific codebase or use case may require a different setup.

ReqHub is primarily intended for standalone backend-only web services, but there's no reason it can't be used to monetize portions of an app backend or web application.
See our guide on [per-endpoint configuration](/recipes/per-endpoint.md) for more information.

This guide is intended to help you determine an approach for implementing ReqHub. If you're looking for detailed setup instructions, please refer to the [quickstart](/getting-started/quickstart?id=distributing-an-api) or one of our tutorials.

#### Set up ReqHub

If you want your entire API to authenticate with ReqHub, you can simply remove any existing authentication and follow the steps from the [quickstart guide](/getting-started/quickstart?id=distributing-an-api).

This is the quickest and easiest approach, but is best suited for APIs that are already designed as microservices, or which don't communicate with a UI.

#### Wrapping existing authentication

If you're able to customize authentication in your application, you could add logic to use ReqHub authentication in addition to standard token authentication.
We specifically expose the internals of our middleware packages so you're able to create your own logic.

For example, in pseudocode:

```js
if (userIsAuthenticated(request)) {
  // allow the request
} else if (isReqHub(request)) {
  // forward ReqHub headers to authenticate the request
  let response = reqhubMerchantClient.track(request);
  if (response.isSuccess) {
    // allow the request
  }
}

// deny the request
return notAuthorized;
```

For specific implementation details, see the middleware source code for your language. The following links show the authentication logic used in our middleware.

* [.Net middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubDotNet/blob/master/ReqHubDotNet/Middleware/ReqHubMerchantMiddleware.cs)
* [NodeJs middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubNode/blob/master/src/middleware/merchant-middleware.js)
* [Python middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go middleware implementation](https://github.com/SpaceGiraffe-io/ReqHubGo)

You can also check out our guide on [writing your own middleware](/guides/middleware.md).

#### Creating separate endpoints

ReqHub can be configured on a per-endpoint basis (see our recipe for [adding ReqHub per-endpoint](/recipes/per-endpoint.md)). This can be useful if you want to separate the ReqHub portion of your API,
such as to provide a different interface for your data, separate ReqHub authentication from the rest of your app, or keep server resource usage
to a minimum by bundling ReqHub code with an existing service.

For example, in pseudocode:

```js
// (recommended approach)

[route('/my-endpoint')]
[reqhub({ publicKey: 'your_public_key', privateKey: 'your_private_key' })] // add ReqHub middleware to the endpoint
function myEndpoint(param1, param2) {
  // secured by ReqHub middleware
  let data = someService.getData(param1, param2);
  return success(data);
}
```

Per-endpoint configuration is supported in all our middleware packages. For specific implementation details, see the README for your language or read our recipe for [adding ReqHub per-endpoint](/recipes/per-endpoint.md).

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet#per-endpoint-configuration)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode#per-endpoint-configuration)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython#per-endpoint-configuration)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby#per-endpoint-configuration)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava#per-endpoint-configuration)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP#per-endpoint-configuration)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo#per-endpoint-configuration)

#### That's it!

We hope this helps you find a solution that will work for your app.

If you found another solution not shown here, help us improve our docs by [creating an issue in GitHub](https://github.com/SpaceGiraffe-io/reqhub-docs/issues/new) and telling us what you did!

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

