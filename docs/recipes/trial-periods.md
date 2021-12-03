
# Set up a trial period

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

Additionally, your account needs to be connected with Stripe in order to create pricing plans. Check out our guide on [setting up Stripe](guides/setting-up-stripe.md).

----

## Trial periods

You can add trial periods to your pricing plans, which lets your customers try an API before paying for it.

For example, you can configure a 2 week free trial on one of your pricing plans.

#### How trials work

When a user subscribes to a plan that includes a trial, they will still need to enter their credit card details,
but will not be charged until the trial ends. If the user decides to end their trial before the trial period is over,
their card will not be charged when the trial ends.

Users are allowed one trial period per plan. Once they begin a trial, they will have access to the trial until the end date from when they first started the trial.
During this time, users can cancel and re-subscribe as many times as they want without being charged.
However, once the original trial period is over, the user's card will be charged when they subscribe to the plan.

Customers are notified via email when their trial starts, is about to end, and when it has changed to a full subscription.

## Make sure Stripe is set up

If you haven't already, [set up Stripe](guides/setting-up-stripe.md). You should see a button like this on your API pages:

![Set up Stripe button](https://reqhubprod.blob.core.windows.net/public/docs/set-up-stripe.png)

You can also find a button in the [account section](https://dev.reqhub.io/account).

## Add a pricing plan

On your API pages, you should see a button to add a pricing plan under the &#x1F4B3; **Plans & pricing** section.

![Add a pricing plan](https://reqhubprod.blob.core.windows.net/public/docs/add-pricing-plan.png)

Clicking that button will take you to the pricing plan form.

In this recipe we'll be focusing on the usage options specifically. For more general information about setting up plans, check out our recipe for [setting up monthly pricing](recipes/monthly-pricing.md).

## Set up a trial period

Trial periods are only available for paid plans. Once you've entered a price for your API (either `Base price` or `Usage price`),
you will be able to select a trial duration using the `Trial` field.

![Trial selection](https://reqhubprod.blob.core.windows.net/public/docs/trial-selection.png)

The supported trial durations are

* 1 week
* 2 weeks
* 1 month (30 days)
* 3 months (91 days)
* 1 year

Simply choose the duration you want and you're all set!

#### That's it!

Once you've created some plans with trial periods, they'll look something like this to other users.

![Trial pricing plans](https://reqhubprod.blob.core.windows.net/public/docs/pricing-plans-trial.png)

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

