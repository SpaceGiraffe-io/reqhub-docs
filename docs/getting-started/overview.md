
# How it works

Learn what ReqHub does, what it doesn't do, and how it works.

----

## What is ReqHub?

ReqHub is a platform that allows web services to communicate using API keys.

&#x261d; That's the stuffy formal definition we came up with. But yes, it's a way of securing your APIs with API keys.
Built on top of that are some monetization features, where ReqHub can do things like verify that a user has an active subscription
before approving access to an API.

## What is it not?

ReqHub is not a hosting provider, which can be a bit confusing.

You can host your APIs anywhere, and you only need to
configure some middleware in your API to make it work with ReqHub. So when you create an API through the website,
you're mostly creating a new set of API keys, and it has nothing to do with the hosting or the API itself.

## Request flow

ReqHub is just an external service that verifies API keys. A client backend uses a ReqHub client library to call an API,
which uses a middleware package to forward the request to ReqHub.

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

