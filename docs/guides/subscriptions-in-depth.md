
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
ReqHub is a fully PCI compliant solution, and all transactions within the ReqHub platform are between the user and ReqHub, not the third-party API vendors.
This ensures that all payments are handled consistently and correctly.

Our PCI compliance is achieved by tools provided by Stripe.
All credit card form fields are provided by Stripe via `iframe` elements.
Those field values never reach our servers or interact with our code.
Upon submitting a credit card form, Stripe generates a randomized token that ReqHub can interact with to create and manage subscriptions.

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
For example, if a subscription has a monthly interval and a user subscribed on June 15, then the subscription will renew on July 15.

When the subscription renews, the user's card is charged if the plan includes a base price or if they have incurred any usage costs.
Regardless of whether or not there was a charge, the user is provided with a receipt via email.

Upon successful payment, the API vendor will receive a payout from Stripe. Congratulations &#x1f389;!

Finally, note that all subscription renewals are processed by Stripe.
ReqHub does not have any direct involvement in the automatic renewal stage of the subscription lifecycle.

## Subscription changes

If an API has multiple plans, subscribers can change to a different plan without any interruption in service.
Subscription changes are somewhat complex, since there are quite a few combinations.

![Plan transitions](https://reqhubprod.blob.core.windows.net/public/docs/plan-transitions.png)

In general, the transition behavior can be summarized with the following:

* **Free to paid**&mdash;The user pays for the price of the new plan.
* **Trial to paid**&mdash;The user pays for the price of the new plan. The trial end date remains the same.
* **Paid to paid**&mdash;The user receives a discount on the initial purchase of the new plan (see [Proration](#Proration) below).
* **Paid to trial**&mdash;The user receives a credit balance for the amount of time remaining on the current plan. The trial begins.
* **Paid to free**&mdash;The user receives a credit balance for the amount of time remaining on the current plan.

If the plans have different renewal intervals, the subscription will have a new billing cycle, starting from the date of the plan change.
The billing cycle does not change if the interval is the same.
For example, if a user subscribes to a monthly plan on June 15, then changes to an annual plan on June 20, the billing cycle for the new plan will start from June 20 and renew on June 20 of the following year.
If another user starts a subscribes to a monthly plan on June 15, then changes to another monthly plan on June 20, the billing cycle will remain the same and renew the following month on July 15.

#### Proration

ReqHub **prorates** subscription changes.
We apply a credit to the purchase of the new plan for remaining unused paid time on the current plan.
This usually results in a lower initial purchase price for the new plan.

For example, if a user subscribes to a plan that costs $5.00, and wants to switch to a plan that costs $10.00 exactly halfway through their current billing cycle,
they will receive $2.50 off the initial purchase price of the new plan, for a total of $7.50.
This is only for the initial purchase&mdash;subsequent renewals in this scenario would still cost the full $10.00.

This can look like you're getting a great discount!
Not exactly&mdash;it's just being fair.
Credits applied under proration have already been paid for, and the proration behavior is simply applying the remaining value of the current plan to the new plan.

When changing plans, all usage incurred on the old plan is carried over to the new plan.
Credit is not applied for usage costs because the billing paradigm is different.
With a recurring base price, the user is paying upfront for an amount of time to use the API, and credits are applied for the unused portion.
Usage costs are paid at the end of the billing cycle for having accessed a certain amount of value through the API,
but since the usage hasn't been paid yet, there is no credit to apply to the new plan.

If the credit is greater than the cost of the new plan, the remainder will be saved and applied to future subscription renewals.
Just be aware that credits are only good for the API they were issued for!
Credits do not transfer across APIs.

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

