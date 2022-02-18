
# How subscriptions work

#### Prerequisites

While not required, it may be helpful to understand how to set up pricing plans in ReqHub.
Check out our recipes for [setting up monthly pricing](/recipes/monthly-pricing) and [setting up usage pricing](/recipes/usage-pricing).

You may also want to [set up Stripe](/guides/setting-up-stripe) if you haven't already.

----

## Basics

Subscriptions are handled through [Stripe](https://stripe.com), ReqHub's payment processor.
ReqHub uses Stripe's APIs to create and manage subscriptions, but they handle the rest!

API vendors are onboarded as sub-merchants of ReqHub through Stripe.
Anyone who wants to sell an API can become a sub-merchant. See our guide on [setting up Stripe](/guides/setting-up-stripe)!

When a payment is made, Stripe splits the purchase amount between the API vendor, ReqHub, and Stripe itself.
The payment breakdown for a subscription payment is as follows:

* Stripe takes [a fee](https://stripe.com/pricing) of 2.9% + 30&cent;
* ReqHub takes 4%
* The API vendor gets the rest!

The API vendor's payout can be calculated as `amount - (amount x 0.069) - 0.3`.

For example, a $10 payment would result in:

* Stripe receives $10 x 0.029 + $0.30 = `$0.59`
* ReqHub receives $10 x 0.04 = `$0.40`
* The API vendor receives $10 - $0.99 = `$9.01`!

Our 4% is kind of abritrary, but we're not that greedy &#x1f60e;
If we took 20%, the API vendor would only get $7.41 &#x1f44e;

We think ReqHub should be a good way for **you** to make money, more than we think it should be a good way for **us** to make money.

ReqHub aims to provide a secure and trustworthy marketplace for bying and selling subscriptions to APIs.
ReqHub is a fully PCI compliant solution (or Stripe would probably shut down our account), and all transactions within ReqHub are between the user and ReqHub, not the third-party API vendors.
This ensures that all payments are handled consistently and correctly.

Our PCI compliance is achieved by tools provided by Stripe.
All credit card form fields are provided by Stripe via `iframe` elements.
Those field values never reach our servers or interact with our code.
Upon submitting a credit card form, Stripe generates a randomized token that ReqHub can interact with to create and manage subscriptions.

We love PCI compliance&mdash;we don't ever want to know any of your credit card details. We absolutely do not want the responsibility of handling that kind of sensitive data!

## Initial purchase

A subscription starts when a user purchases a pricing plan for the first time.

A credit card is required for all paid plans, even if there is no initial charge.

A free plan, with monthly and usage prices of $0.00, will not require a credit card.

The API vendor will receive a payout from Stripe that will appear in their bank account after a few days.

#### Trials

When a user starts a trial, they can access it until the duration is reached, ctarting from the date the trial was started.
If a user cancels and resubscribes to the same trial, it will still end on the original end date.

A credit card is required to start a trial, so it can be charged when it ends and upgrades to a full subscription.
It is not charged upon starting the trial.

The user will be notified before the trial upgrades.

If the user cancels the trial, their card will not be charged at the time when it would have ugpraded to a full subscription.

## Renewal

Subscriptions renew every time the plan interval is reached, starting from the date of the initial purchase.
For example, if a subscription has a monthly interval (30 days) and a user subscribed to it on June 15, then the user will be charged 30 days later, on July 15.

Subscription renewals are processed by Stripe.

The user will be notified with an email receipt.

The API vendor will receive a payout from Stripe that will appear in their bank account after a few days.

## Subscription changes

If an API has multiple plans, subscribers can change to a different plan.

The date the subscription renews does not change if the interval is the same.
Otherwise, the subscription will renew when the new interval is reached, starting from the date the subscription changed.

Subscription changes are complex, since each combination of plans requires unique treatment.

[table of combinations and a description of how they're handled?]

#### Proration

When changing plans, ReqHub will issue a credit for the remaining time on the current plan.
This usually results in a lower initial purchase price for the new plan.
If the credit is greater than the cost of the new plan, the remainder will be saved and applied to future subscription renewals.

Credits are only good for the API they were issued for!

## Cancellation

Subscriptions can be canceled at any time.

Recurring billing will cease. The card will not be charged again for that subscription.

ReqHub provides credits for the API upon canceling the subscription, which will be applied if the user chooses to subscribe again.
We would like to allow the user to refund that amount back to their card, but haven't reached a solution for that yet. Stay tuned!

If a trial is canceled, the card will not be charged at the time when it would have upgraded to a full subscription.

## Payouts

Payouts are handled by Stripe.

They can be viewed from the Stripe dashboard in the account section.

You will receive an email notifying you of the payout.

Payouts are processed pretty quickly, and should end up in your bank account with a statement descriptor of REQHUB_PAYMENTS_OR_SOMETHING_LIKE_THAT

#### That's it!

That's an in-depth look at how subscriptions work in ReqHub.

If you would like to make your own subscription platform, check out [our subscription engine]().

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

