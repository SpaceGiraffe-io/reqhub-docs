
# How subscriptions work

#### Prerequisites

While not required, it may be helpful to understand how to set up pricing plans in ReqHub.
Check out our recipes for [setting up monthly pricing](/recipes/monthly-pricing) and [setting up usage pricing](/recipes/usage-pricing).

It may also be useful to [set up Stripe](/guides/setting-up-stripe) if you haven't already.

----

## Basics

Subscriptions are handled through Stripe.

Users are onboarded as sub-merchants of ReqHub, transactions go through ReqHub.
All subscriptions are managed by ReqHub and not the API vendor.

Payment breakdown -- who gets how much + example with $10

## Initial purchase

A subscription starts when a user purchases a pricing plan for the first time.

A credit card is required for all paid plans, even if there is no initial charge.

A free plan, with monthly and usage prices of $0.00, will not require a credit card.

#### Trials

A credit card is required to start a trial, so it can be charged when it ends and upgrades to a full subscription.
It is not charged upon starting the trial.

The user will be notified before the trial upgrades.

If the user cancels the trial, their card will not be charged at the time when it would have ugpraded.

## Renewal

## Subscription changes

#### Proration

## Cancellation

## Payouts

#### That's it!

That's an in-depth look at how subscriptions work in ReqHub.

If you would like to make your own subscription platform, check out [our subscription engine]().

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

