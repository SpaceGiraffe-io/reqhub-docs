
## Testing pricing plans

You can test plans you've created using the `Simulate` button. Your requests will appear as if you are currently subscribed to the simulated plan, but without incurring any subscription charges.
This is useful for debugging purposes, such as if you restrict functionality in your API by plan.

![Simulating a plan](https://reqhubprod.blob.core.windows.net/public/docs/simulate-plan.png)

Just like for subscriptions, simulating a plan provides the following data:

```js
{
  "clientId": string,           // A clientId unique to the user
  "planName": string,           // The plan name as entered, like "Extra awesome"
  "normalizedPlanName": string, // The plan name normalized, like "EXTRA_AWESOME"
  "planSku": string,            // The plan SKU as entered, like "extra-awesome-sku"
  "normalizedPlanSku": string,  // The plan SKU normalized, like "EXTRA_AWESOME_SKU"
  "isTrial": boolean            // Indicates whether the user is currently in a trial period
}
```

## Next steps
Marketing
Making user-friendly docs

