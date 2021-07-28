
# Quick start

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

----

## Distributing an API

Use the following steps to connect your API to ReqHub.

#### 1. Add your API through the website

Use the `New API` button at the top of the page to create a new API.

![New API button](https://reqhubprod.blob.core.windows.net/public/docs/new-api.png)

Fill out the form and click `Save`. All you really need is a name, but feel free to explore the other options.

![Creating an API](https://reqhubprod.blob.core.windows.net/public/docs/create-api.png)

#### 2. Grab your publisher keys

With the API created, you will have two sets of keys.
One set is for publishing your API, and the other is for consuming it.
Copy the `Publisher keys` and proceed to the next step.

![Publisher keys](https://reqhubprod.blob.core.windows.net/public/docs/publisher-keys.png)

#### 3. Follow the instructions for your language

Now you need to set up your API with ReqHub. Download the middleware package for your language and follow the README instructions to set it up.

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo)

Don't see your language? Check out our guide on [writing your own middleware](/guides/middleware).

#### 4. (Optional) Set up Stripe and add pricing

Looking to make money selling your APIs? Check out our guide on [setting up Stripe](/guides/setting-up-stripe).

#### That's it!

Congratulations, you have created an API with ReqHub &#x1f389;

----

## Consuming an API

Use the following steps to consume an API.

#### 1. Find an API

You can create an API using the steps above, find an API in [our marketplace](https://reqhub.io/marketplace), or use [our test API](https://reqhub.io/SpaceGiraffe/Public-test-API).

#### 2. Get your API keys

Once you've found an API, get your API keys by clicking `Get API keys` for a free API (like [our test API](https://reqhub.io/SpaceGiraffe/Public-test-API)) or by subscribing to a plan for a paid API.

![Get API keys](https://reqhubprod.blob.core.windows.net/public/docs/get-api-keys.png)

Or select a plan:

![Client keys](https://reqhubprod.blob.core.windows.net/public/docs/subscribe-to-api.png)

#### 3. Grab your client keys

Once you've acquired keys for the API,
copy the `Client keys` and proceed to the next step.

![Subscribing to an API](https://reqhubprod.blob.core.windows.net/public/docs/client-keys.png)

#### 4. Follow the instructions for your language

Now you need to set up a ReqHub client. Download the middleware package for your language and follow the README instructions to set it up.

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo)

We also support [Postman](/recipes/postman).

Don't see your language? Check out our guide on [writing your own middleware](/guides/middleware).

#### 5. Try it out

Use the documentation for your chosen API to find an endpoint. If you're using [our test API](https://reqhub.io/SpaceGiraffe/Public-test-API), you can use the `/test` endpoint.

Run your code and make sure it works!

![Postman example](https://reqhubprod.blob.core.windows.net/public/docs/postman-test.png)

#### That's it!

Congratulations, you called an API with ReqHub &#x1f389;


## Next steps
Check out our tutorials
Set up Postman
Set up Stripe
Marketing
Making user-friendly docs

