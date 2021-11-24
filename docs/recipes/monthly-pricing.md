
# Set up monthly pricing

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

Clicking that button will take you to the pricing plan form, where you'll see a bunch of options for setting up your pricing plan.

![Pricing plan form](https://reqhubprod.blob.core.windows.net/public/docs/pricing-plan-form.png)

Continue reading for a look at some of the finer points to the options available.

#### Plan names

You can call your plans whatever you want, but try to keep your plan names short and sweet.
If you need inspiration, below are some common plan names found around the Internet.

*By audience*
* Hobbyist
* Dev (or "Developer")
* Pro (or "Professional")
* Elite
* Enterprise

*By Metal tier*
* Bronze
* Silver
* Gold
* Platinum

*By renewal period*
* Monthly
* Quarterly
* Annual

*By level*
* Basic
* Advanced
* Super
* Premium
* Plus (or "+"; this can also be a suffix, like "Premium plus")

*Absurd (nobody really does this ...yet)*
* Potato
* Horse
* Giraffe (obviously Giraffe would be highest)

In case those aren't good enough, you can also mix and match for gems like "Platinum Hobbyist Plus".

#### Pricing & billing options

You can set up pricing for recurring billing, per-request usage-based pricing, or both.

**Base price** - The base price is the fixed subscription cost.
This amount is charged when a user subscribes for the first time, and then every time the subscription renews.
For example, if you set up your plan with a base price of `$10.00` for a subscription interval of `monthly`,
your users would be charged $10 when they subscribe, then $10 each month thereafter on the day they started the subscription.

**Usage price** - The usage price is a variable component that is calculated according to the number of requests the user makes to the API.
For example, you can use this field to charge $5 per 1000 requests made to your API.
Setting the usage price will enable the **Usage billing options** fields, which control the cost per request for your API.
See our recipe on [setting up usage pricing](/recipes/usage-pricing) for more information.

**Combined pricing** - There's no reason you have to use only one pricing method or the other&mdash;you can use both!
For example, you can create a plan offering your API at a base price of $10 per month with an additional charge of $5 per 1000 requests.
This is useful if you want to offer a plan for low-volume customers featuring a reduced monthly cost and added usage charges.

**No pricing!** - If you leave the base price and usage price both at `$0.00`, the plan will allow users to access your API for free.
Card details are not required for users subscribing to a free plan. Creating a free plan can be a good way to allow entry-level access to
an API product. See our recipe for [restricing functionality by plan](/recipes/functionality-by-plan). Note that if you're planning on
offering your API for free with no paid plans, you don't need to create any pricing plans.

For an in-depth look at subscriptions in ReqHub, check out our guide on [how subscriptions work](/guides/how-subscriptions-work).

#### Biweekly, quarterly, and annual plans

It doesn't have to be monthly! ReqHub also supports subscription intervals of 2 weeks, 3 months, or 1 year.
For example, to complement a plan of $10 per month, you could also offer a discounted annual plan for $99.

![Subscription intervals](https://reqhubprod.blob.core.windows.net/public/docs/pricing-subscription-intervals.png)

#### That's it!

Once you've created some plans, they'll look something like this to other users.

![Pricing plans](https://reqhubprod.blob.core.windows.net/public/docs/pricing-plans.png)

## Next steps

* [Testing pricing plans](/recipes/simulating-pricing-plans)
* Marketing
* Making user-friendly docs

