
# Multiple environments

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

----

## Multiple APIs

If you want separate sets of API keys for your production and non-prod environments,
you can simply create multiple APIs. Since you can have unlimited private and public APIs, you can create as many as you need!

For example, you could have the following setup:
* Public API - Production environment *(example-api)*
* Private API - All non-prod environments *(example-api-dev or example-api-nonprod, etc.)*

![Multiple environments simple](https://reqhubprod.blob.core.windows.net/public/docs/multiple-environments-simple.png)

Alternatively, you could have one API per environment:
* Public API - Production environment *(example-api)*
* Private API 1 - Test environment *(example-api-test)*
* Private API 2 - Dev environment *(example-api-dev)*
* Private API 3 - Local environment *(example-api-local)*

![Multiple environments complex](https://reqhubprod.blob.core.windows.net/public/docs/multiple-environments-complex.png)

However, this approach can make key management more difficult, so we would generally recommend the first approach if it works for your project's needs.

For more information on creating APIs, see our [quickstart guide](/getting-started/quickstart?id=distributing-an-api) and our recipe for [creating a private API](/recipes/create-a-private-api).

## Why?

The main reason to use an approach like this is for security.
You probably don't want your production users interacting with lower environments, but there wouldn't be anything stopping them if both services use the same set of keys.

Private APIs prevent users from obtaining keys to your other environments.

#### That's it!

That's the recommend approach for using ReqHub with multiple environments!

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

