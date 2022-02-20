
# Subscriptions in depth

#### Prerequisites

While not required, it may be helpful to understand how to set up pricing plans in ReqHub.
Check out our recipes for [setting up monthly pricing](/recipes/monthly-pricing) and [setting up usage pricing](/recipes/usage-pricing).

You may also want to [set up Stripe](/guides/setting-up-stripe) if you haven't already.

----

## Basics

Subscriptions are handled through [Stripe](https://stripe.com), ReqHub's payment processor.
ReqHub uses Stripe's APIs to create and manage subscriptions, and Stripe handles the recurring billing, payouts, and all the rest!

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

We are very grateful for Stripe's PCI compliance solutions&mdash;we don't ever want to know any of your credit card details.
We absolutely do not want the responsibility of handling that kind of sensitive data!

## Initial purchase

A subscription starts when a user purchases a pricing plan for the first time, or if they subscribe again after canceling.

A credit card **is required** for all paid plans.
This is true even if there is no initial charge, such as when subscribing to a usage-only plan or starting a trial.
Then if the user incurs usage costs or allows a trial to upgrade to a full subscription, the card is present to pay for those services.

Unpaid plans **do not** require a credit card.
For this reason, we generally recommend that you provide some aspect of your service under an unpaid plan.
It provides a better user experience&mdash;people hate entering their credit card information into websites!
Removing the barrier of providing card information makes it friendlier for new users to interact both with your API and ReqHub as a platform.

We know this because we always grumble when websites make us pay for stuff, especially when we have to enter card details and there's no PayPal button.
On that note, unfortunately we can't offer PayPal as an alternative payment method because it isn't supported by Stripe (as PayPal is Stripe's main competitor).
So please try to provide a free plan!

<!--vvvv this kind of needs its own page or it's own section-->
<!--We realize that's not practical in all situations, but here are some ways it can be done without giving everything away:-->

<!--* Provide mock data-->
<!--* Limit the number of results-->
<!--* Scramble the results-->
<!--* Limit the number of actions-->

If an initial charge is required to start a subscription, the API vendor will receive a payout from Stripe. Congratulations &#x1f389;!
This can be viewed from the Stripe dashboard, accessible from the [account page](https://reqhub.io/account) (requires [setting up Stripe](/guides/setting-up-stripe)).
The funds will appear in the API vendor's bank account after a few days.

## Trials

A trial allows a user to access a paid plan free of charge for a brief period.
When the end date is reached, the trial upgrades to a full subscription.

A credit card is required to start a trial, even though there is no immediate payment.
The card will be charged when the trial upgrades if the plan includes a recurring base price, and will start accumulating usage if the plan includes usage pricing.
During the trial, usage does not count towards the upgrade cost.
The user can cancel at any time, which will allow access to the trial until the end date, but will **not** charge the user's card.

When a user begins a trial, they can access it only until the initial end date.
For the most part that doesn't mean anything special, except in certain scenarios.
For example, if a user changes plans while under a trial, the previous plan will retain its trial end date.
If the user changes back to that plan, their trial will upgrade to a full subscription on the original end date.

The user receives several notifications around trials.
First is an email receipt upon starting the trial, then a notification 7 days before the end date, and again when it finally upgrades.
If the user cancels their trial or changes to a non-trial plan, they will receive a notification that their trial has ended.

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

