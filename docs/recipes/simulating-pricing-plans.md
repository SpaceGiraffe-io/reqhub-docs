
# Testing pricing plans

#### Prerequisites

The following steps require that you have connected your account with Stripe. Please see our guide on [setting up Stripe](/guides/setting-up-stripe).

This recipe also assumes you have created at least one pricing plan. For more information on creating plans, check out our recipe for [setting up monthly pricing](/recipes/monthly-pricing).

----

## Simulating a plan

You can test plans you've created using the `Simulate` button. Your requests will appear as if you are currently subscribed to the simulated plan, but without incurring any subscription charges.

![Simulating a plan](https://reqhubprod.blob.core.windows.net/public/docs/simulate-plan.png)

## Plan context

Each of the ReqHub middleware packages provide the following context information:

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

You can use this information to [adjust or restrict functionality](/recipes/functionality-by-plan).
This data is available for both live and simulated subscriptions.

For specific usage information, see the README for your language:

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet#plan-information)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode#plan-information)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython#plan-information)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby#plan-information)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava#plan-information)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP#plan-information)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo#plan-information)

## Next steps
Marketing
Making user-friendly docs

