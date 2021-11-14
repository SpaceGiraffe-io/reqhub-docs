
# Add ReqHub per-endpoint

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

----

## Quick start

ReqHub can be configured for a single endpoint or group of endpoints. See the README for your language for implementation details.

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet#per-endpoint-configuration)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode#per-endpoint-configuration)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython#per-endpoint-configuration)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby#per-endpoint-configuration)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava#per-endpoint-configuration)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP#per-endpoint-configuration)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo#per-endpoint-configuration)

## Nanoservices

You may have heard of microservices, which are single-purpose web services. We're taking that concept one step further
by including multiple APIs in a single service.

## Use cases

Writing your APIs as nanoservices works best when you have many services with small workloads.

#### Reducing hosting costs

Including multiple services in a single project can be a way to reduce hosting costs.
Consolidating multiple APIs into a single application reduces the number of services that need hosting,
or can reduce resource usage if hosting multiple applications on a single server.
This can lead to lower costs by reducing the number of servers required, or by allowing you to
use cheaper hosting plans.

#### Monetizing app backends

If you have an API that you're using as an app backend and want to monetize a portion of it,
you can do so without converting the entire API to use ReqHub or creating additional web services.

#### asldjfsldjasldjfsldjasldjfsldjasldjfsldjasldjfsldj something else?

## Considerations

#### Architecture

You may wish to consider structuring your code such that an individual nanoservice can be extracted and deployed on its own.
For example, you could write your APIs as libraries and include them in host projects.

#### Deployment

Including multiple APIs in a single service instance means all services must be deployed together.

#### Scalability

A service containing several nanoservices can be scaled vertically or horizontally like any other service.

#### That's it!

## Next steps
Check out our tutorials
Set up Postman
Set up Stripe
Marketing
Making user-friendly docs

