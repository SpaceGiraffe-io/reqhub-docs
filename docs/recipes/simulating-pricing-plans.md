
# Testing pricing plans

#### Prerequisites

The following steps require that you have connected your account with Stripe. Please see our guide on [setting up Stripe](/guides/setting-up-stripe).

----

## Simulating a plan

You can test plans you've created using the `Simulate` button. Your requests will appear as if you are currently subscribed to the simulated plan, but without incurring any subscription charges.
This is useful for debugging purposes, such as if you restrict functionality in your API by plan.

![Simulating a plan](https://reqhubprod.blob.core.windows.net/public/docs/simulate-plan.png)

## Plan context

While simulating a plan, you will have access to the following context information in your API:

```js
{
  "clientId": string,           // A clientId unique to the user
  "planName": string,           // The plan name as entered, like "Extra awesome"
  "normalizedPlanName": string, // The plan name whitespace and special characters removed, like "Extra-awesome"
  "planSku": string,            // The plan SKU as entered, like "Extra awesome SKU!!!"
  "normalizedPlanSku": string,  // The plan SKU with whitespace and special characters removed, like "Extra-awesome-SKU"
  "isTrial": boolean            // Indicates whether the user is currently in a trial period
}
```

ReqHub provides the same data for live subscriptions. You can use this data to adjust or restrict functionality.

For specific usage information, see the README for your language:

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo)

## Next steps
Marketing
Making user-friendly docs

