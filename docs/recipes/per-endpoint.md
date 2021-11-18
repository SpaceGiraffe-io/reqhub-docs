
# Add ReqHub per-endpoint

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

----

## Quick start

ReqHub can be configured for a single endpoint or group of endpoints.
This is useful if you want to include multiple APIs in a single project, or if you don't want to convert your entire project to a ReqHub API.

See the per-endpoint section in the README for your language to set it up!

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet#per-endpoint-configuration)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode#per-endpoint-configuration)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython#per-endpoint-configuration)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby#per-endpoint-configuration)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava#per-endpoint-configuration)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP#per-endpoint-configuration)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo#per-endpoint-configuration)

## Multiple APIs in a single project

You can include multiple ReqHub APIs in a single project. Each API will still have its own separate ReqHub page, API keys, and subscriptions, but exists in a codebase alongside other APIs.

![Multiple APIs in a single project](https://reqhubprod.blob.core.windows.net/public/docs/multiple-services-single-project.png)

Including multiple APIs in a single web service can be a way to reduce hosting costs. It may reduce the number of servers required, or allow you to switch to a cheaper hosting plan.

We're currently using this approach ourselves in order to reduce hosting costs for some of our smaller services. For example, our [public test API](https://reqhub.io/SpaceGiraffe/Public-test-API), [email API](https://reqhub.io/SpaceGiraffe/email), [SMS API](https://reqhub.io/SpaceGiraffe/sms), and a handful of other APIs use per-endpoint configuration within a single project.
Since each API is pretty small, it's not economical or practical to have them all deployed separately since the resource usage of each service instance would add up quickly!

#### Considerations

**Scalability** - This approach works best when you have many services with small workloads and relatively low traffic. Otherwise, scalability may become a concern.
If one of the APIs becomes extremely popular, you may want to consider breaking it out into its own deployment instance.
Alternatively, you can scale the deployment horizontally or vertically like any other application.

**Project architecture** - You may want to consider structuring your code such that an individual service can be easily extracted and deployed on its own.
For example, you could write your APIs as libraries and include them in host projects. Then extracting a service would consist of removing the import from the combined project
and creating a new project that includes only the service being isolated.

**Deployment** - Another consideration is that all the services in the project will be deployed together. You may have to be careful not to ship in-progress work for one API while intending to deploy another.
Therefore this approach is also best suited to services that are updated infrequently.
Also note that if you version each of your services separately, each version won't necessarily correspond to a single deployment.

**Domains** - If you like having domains or subdomains tailored to each of your APIs, you can simply point multiple domains at the same service. Using our own APIs as an example,
[public-test-api.spacegiraffe.io](https://public-test-api.spacegiraffe.io), [email.spacegiraffe.io](https://email.spacegiraffe.io), and [sms.spacegiraffe.io](https://sms.spacegiraffe.io) are all pointed at the same web service.
This may seem odd from a technical standpoint, but it makes for nice-looking URLs!

## Adding an API within a project

If you have an API that you're using as an app backend and want to distribute some aspect of it,
you can do so without creating additional deployments or converting the entire API to use ReqHub.
For example, you may want to provide API access to your application without duplicating code into a separate project.

![Multiple APIs in a single project](https://reqhubprod.blob.core.windows.net/public/docs/multiple-services-existing-project.png)

There are a couple ways to go about doing this.

**Create separate API endpoints** - Set up endpoints that will only be used for the API portion of your application.
This is probably the most straightforward and flexible approach, but could result in the most work.

**Using existing endpoints** - Tie ReqHub authentication into to your existing authentication code. We expose most of the objects in our middleware libraries specifically so this can be an option.

The logic can be simple:
```
  1. If the request contains a token for the existing authentication mechanism, use that.
  2. Otherwise, see if it contains any ReqHub headers and try to authenticate the request with ReqHub.
```

See our recipe for [adding ReqHub to an existing API](/recipes/existing-api.md) for more details on both of these approaches.

#### Considerations

This is still a multiple-services-in-one solution, so considerations for this approach are similar to those above.

**Scalability** - If the ReqHub or non-ReqHub portion of your service starts receiving too much traffic, it could negatively impact the other half of your service.
Just like the multiple APIs approach, you may want to break the ReqHub API into its own deployment instance so it can be scaled separately.
Alternatively, you can scale the existing deployment horizontally or vertically as necessary.

**Project architecture** - You may want to consider structuring your code such that your ReqHub API can be easily extracted and deployed on its own.
Writing your API as a library might be overkill, but you could organize your project such that all the code for the ReqHub portion is contained in its own directory.
Then you can simply move this code to its own project as necessary.

**Deployment** - Another consideration is that the ReqHub API will be deployed together with the non-ReqHub code.
You may have to be careful not to ship in-progress work for one part while intending to deploy the other.

**Domains** - In our opinion, while you can set up multiple domains that point to the same service, it should be fine to use the same domain for both portions of your application.
We think something like `api.myapp.com` would make sense for both the ReqHub API and non-ReqHub app backend portions of an application. However, if the non-ReqHub portion
is a more traditional web application that primarily renders and serves HTML pages, you may want to set up a separate domain for the API.

#### That's it!
Now go cut down your hosting costs &#x1f4aa;

## Next steps
Check out our tutorials
Set up Postman
Set up Stripe
Marketing
Making user-friendly docs

