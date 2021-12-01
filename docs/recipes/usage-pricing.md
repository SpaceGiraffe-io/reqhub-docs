
# Set up usage pricing

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

Additionally, your account needs to be connected with Stripe in order to create pricing plans. Check out our guide on [setting up Stripe](guides/setting-up-stripe.md).

----

## Make sure Stripe is set up

If you haven't already, [set up Stripe](guides/setting-up-stripe.md). You should see a button like this on your API pages:

![Set up Stripe button](https://reqhubprod.blob.core.windows.net/public/docs/set-up-stripe.png)

You can also find a button in the [account section](https://dev.reqhub.io/account).

## Add a pricing plan

On your API pages, you should see a button to add a pricing plan under the &#x1F4B3; **Plans & pricing** section.

![Add a pricing plan](https://reqhubprod.blob.core.windows.net/public/docs/add-pricing-plan.png)

Clicking that button will take you to the pricing plan form.

In this recipe we'll be focusing on the usage options specifically. For more general information about setting up plans, check out our recipe for [setting up monthly pricing](recipes/monthly-pricing.md).

#### Set up usage pricing

Usage pricing is a variable component to a subscription that is calculated according to the number of requests the user makes to the API.

In order to add usage pricing to your plan, under **Subscription price** enter a dollar amount in the `Usage price` field.
This will enable the **Usage billing options** fields, which control the cost per request for your API.

![Usage pricing fields](https://reqhubprod.blob.core.windows.net/public/docs/usage-pricing.png)

The **Usage billing options** section is highly specific to your traffic expectations and the billing experience you would like to provide.
You may want to increase the unit quantity if you expect extremely high traffic, or change the billing mode to `Integral`
if you would like to allow a certain number of requests for free.

#### Unit quantity

For the most part, you can leave the unit quantity at the default value of `1000 requests` and tune the per-request cost with the `Usage price` field.

This will depend on the pricing experience you would like to provide. $1 per 1000 requests is the same as $10 for 10,000 requests or even $100 for 100,000 requests,
but seeing a $100 price tag might result in sticker shock despite having the same per-request cost of only $0.001.

This is because the unit quantity is also a suggestion of the number of requests that will be reached through normal usage, so you should try to match this value with your actual usage expectations.
For example, it's fair to present $100 for 100,000 requests if you expect users to make between 80,000 and 300,000 requests, but not if you only expect between 200 and 5000 requests.
In that case you would most likely want to present the pricing as $1 per 1000 requests.

If you expect a lot of traffic where tuning the price value isn't practical, you should change the unit quantity.
For example, if you would like to charge $0.01 for 1 million requests, you should set the price at $0.01 and increase the unit quantity to 1,000,000.

#### Billing mode

You can choose between `Continuous` and `Integral` billing modes.

Before we say anything, here are the graphs:

![Continuous billing mode](https://reqhubprod.blob.core.windows.net/public/docs/continuous-billing-mode.png)

![Integral billing mode](https://reqhubprod.blob.core.windows.net/public/docs/integral-billing-mode.png)

Both charts are using a pricing scheme of $10 per 1000 requests, but the behavior is very different.

With **continuous billing mode**, the unit of measure is one request. 500 requests will cost $5.00.
Obviously this is a bit steep, but this is a pretty common pricing method, especially amont cloud service providers.

By contrast, **Integral billing mode**&mdash;that's integral as in "integer", consisting of a whole number; not related to integrals from calculus&mdash;
treats the `Unit quantity` as the unit of measure, so in this example the customer pays in units of 1000. Then 500 requests will cost $0.00
and the customer would only be charged for usage when they reached the threshold of 1000 requests.

#### That's it!

Once you've created some plans with usage pricing, they'll look something like this to other users.
The middle plan uses the `continuous` billing mode, and the other two use `integral`.

![Usage pricing plans](https://reqhubprod.blob.core.windows.net/public/docs/pricing-plans-usage.png)

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

