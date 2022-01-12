
# How it works

Learn what ReqHub does, what it doesn't do, and how it works.

----

## What is ReqHub?

ReqHub is a platform that allows web services to communicate using API keys.

&#x261d; That's the stuffy formal definition we came up with. But yes, it's a way of securing your APIs with API keys.
Built on top of that are some monetization features, where ReqHub can do things like verify that a user has an active subscription
before approving access to an API.

#### What is it not?

ReqHub is not a hosting provider, which can be a bit confusing.

You can host your APIs anywhere, and you only need to
configure some middleware in your API to make it work with ReqHub. So when you create an API through the website,
you're mostly creating a new set of API keys, and it has nothing to do with the hosting or the API itself.

## Selling your APIs

ReqHub makes it easy to sell your APIs. We provide a marketplace where both individuals and businesses
can sell subscriptions to web services.

We use [Stripe](https://stripe.com) as our payment processor. In order to sell your APIs through ReqHub, you need to set up a Stripe account. Check out our guide on [setting up Stripe](guides/setting-up-stripe).
A Stripe account is not required for purchasing a subscription to an API.

#### Payment flow

When a user makes a payment for one of your plans, the money goes to you, minus our small platform fee of 4% and [Stripe's processing fee](https://stripe.com/pricing) of 2.9% + $0.30.

![ReqHub payment flow](https://reqhubprod.blob.core.windows.net/public/docs/payment-diagram.png)

The money will appear in your account in 2-3 business days, after Stripe finishes processing the charge.

## Request flow

ReqHub is a service that verifies API keys. A client backend uses a ReqHub client library to call an API,
which uses a middleware package to forward the request to ReqHub to authenticate the request.

The request flow looks like this:

![ReqHub request flow](https://reqhubprod.blob.core.windows.net/public/docs/flow-diagram-raw.png)

1. The client backend requests a resource from the API and sends its client keys
2. The API forwards the client keys and its publisher keys to ReqHub
3. ReqHub verifies the client/publisher API key combination
4. The API processes the request and sends its response to the client

If something doesn't look good, like if the request was tampered with, or the API requires a subscription but the client isn't currently subscribed,
ReqHub won't authorize the request and the middleware will return an error response.

#### That's it!

Hopefully our platform makes more sense now. If it's still a little fuzzy, try one of our tutorials!

## Next steps

* Check out our tutorials
* Quickstart
* Troubleshooting

