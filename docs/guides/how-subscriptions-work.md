
# How subscriptions work

#### Prerequisites

While not required, it may be helpful to understand how to set up pricing plans in ReqHub.
Check out our recipes for [setting up monthly pricing](/recipes/monthly-pricing) and [setting up usage pricing](/recipes/usage-pricing).

It may also be useful to [set up Stripe](/guides/setting-up-stripe) if you haven't already.

----

## Basics

Subscriptions are handled through Stripe.
ReqHub is PCI compliant and does not have any interaction with your credit card details.
Credit card form fields are provided by Stripe via `iframe` elements.
The field values never reach our servers or interact with our code.
Upon submitting a credit card form, the details are sent to Stripe only.
Stripe then provides a token that ReqHub can interact with.

API vendors are onboarded as sub-merchants of ReqHub, transactions go through ReqHub.

Anyone who wants to sell an API can become a sub-merchant. See our guide on [setting up Stripe](/guides/setting-up-stripe)!

All subscriptions are managed by ReqHub and not the API vendor.

Payment breakdown -- who gets how much + example with $10

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

