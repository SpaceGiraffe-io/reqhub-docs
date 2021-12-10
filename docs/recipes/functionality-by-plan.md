
# Restrict functionality by plan

#### Prerequisites

The following steps require that you have connected your account with Stripe. Please see our guide on [setting up Stripe](/guides/setting-up-stripe).

This recipe also assumes you have created at least one pricing plan. For more information on creating plans, check out our recipe for [setting up monthly pricing](/recipes/monthly-pricing).

----

## Writing plan-specific code

The ability to detect the current plan, trial status, or user is a powerful tool that gives you a lot of control over how you can sell your APIs.
There are many reasons you may want to use plan information alter the functionality of your API.
First we'll take a look at the plan context which provides this information, and then we'll look at a few common use cases.

## The plan context

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

For specific usage information, see the README for your language:

* [.Net](https://github.com/SpaceGiraffe-io/ReqHubDotNet#plan-information)
* [NodeJs](https://github.com/SpaceGiraffe-io/ReqHubNode#plan-information)
* [Python](https://github.com/SpaceGiraffe-io/ReqHubPython#plan-information)
* [Ruby](https://github.com/SpaceGiraffe-io/ReqHubRuby#plan-information)
* [Java](https://github.com/SpaceGiraffe-io/ReqHubJava#plan-information)
* [PHP](https://github.com/SpaceGiraffe-io/ReqHubPHP#plan-information)
* [Go](https://github.com/SpaceGiraffe-io/ReqHubGo#plan-information)

## Examples

Below are some examples of ways you can use the plan context information.
This is not an exhaustive list, but it should give you an idea of some things you can do with the plan context data.

#### Restricting features

Suppose you have several plans and only want to make certain features available for certain plans.

```js
// pseudocode

if (getNormalizedPlanName() !== 'PRO') {
  createErrorResponse(403, `You're on the ${getPlanName()} plan. Upgrade to Pro for access to this feature!`);
}
```

#### Detecting trials

You may want to alter the functionality of your endpoints for trial users.
For example, you may want to limit the number of rows returned in a data set:

```js
// pseudocode

let dataRows = getDataRows();

if (isTrial()) {
  let trialData = limit(dataRows, 50);
  createResponse(200, trialData);
} else {
  createResponse(200, dataRows);
}
```

#### Rate-limiting users

Since you have access to a unique client ID, you could perform user-specific operations like rate limiting.
The following example limits a user to 1 million requests in the last 30 seconds:

```js
// pseudocode

if (requestsInLast30Seconds(clientId) >= 1000000) {
  createErrorResponse(403, 'Stop it.');
}
```

#### That's it!

But make sure your free and basic tier users can still do **something!** &#x1f631;

## Next steps
Marketing
Making user-friendly docs

