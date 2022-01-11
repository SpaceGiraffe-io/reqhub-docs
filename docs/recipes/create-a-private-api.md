
# Create a private API

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

This guide assumes you have created an API, or are in the process of creating one. See our [quickstart guide](getting-started/quickstart) for more information.

----

## The private checkbox

To make your API private, simply check the `Private` checkbox while creating or editing an API and you're good to go!

![Private API checkbox](https://reqhubprod.blob.core.windows.net/public/docs/private-api.png)

This will stop your API from appearing in search and [the Marketplace](https://reqhub.io/marketplace),
and prevents other users from obtaining API keys for your API.

These are some common reasons to make your API private:
* You have internal APIs that aren't meant for public consumption
* You're still preparing your API for public use
* To prevent public access to a non-production instance (see our recipe for [multiple environments](/recipes/multiple-environments.md))

#### A note on changing from public to private

If you change your API from public to private, users with existing API keys will still be able to access your API and API page on ReqHub,
but new users will be prevented from obtaining keys or viewing the page.

A warning will appear if you try to do this:

![Visibility warning](https://reqhubprod.blob.core.windows.net/public/docs/confirm-visibility-change.png)

#### That's it!

That's how you can create a private API! &#x1f511;

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

