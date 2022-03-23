
# .Net Core tutorial

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

You will need a recent version of [Visual Studio](https://visualstudio.microsoft.com/downloads/). If you're unsure of what to download, Visual Studio Community 2022 will work just fine &#x1f44c;

You will need a recent version of [.Net Core](https://dotnet.microsoft.com/en-us/download), if not included with Visual Studio.

----

## Introduction

In this tutorial we will walk through creating an API project, configuring ReqHub, and testing it with [Postman](https://www.postman.com/downloads/).
We'll also cover deploying the API to Azure, setting up pricing on ReqHub, and taking it public.

The goal of this tutorial is to show you the steps involved in distributing your API on ReqHub, starting from scratch.
We won't be concerned about the actual content of the API, so we'll be building a ridiculous web service that replaces words from an input string.
When you're done, you can either use this project as an example, or as a starting point for your real project!

Let's get started! &#x1f680;

## Create a project

Fire up Visual Studio and create a new project.

Find the "ASP.NET Core Web API" project template, give it a name and click **Next**, then click **Create** with the default options.

![Creating a new project in Visual Studio](https://reqhubprod.blob.core.windows.net/public/docs/netcore-project-creation-sequence.png)

With the project created, delete `WeatherForecast.cs` and `Controllers/WeatherForecastController.cs`.

![Deleting default code files](https://reqhubprod.blob.core.windows.net/public/docs/netcore-delete-weatherforecast.png)

With the project ready to go, let's add some code &#x1f4aa;

## Make the endpoint

Now create a file called `TutorialController.cs` under the `Controllers/` folder and replace its contents with the following:

```js
using Microsoft.AspNetCore.Mvc;

namespace NetCoreTutorialApi.Controllers
{
    [ApiController]
    [Route("tutorial")]
    public class TutorialController : ControllerBase
    {
        [HttpGet("translate")]
        public IActionResult Get([FromQuery] string input)
        {
            var result = input?
                .Replace("the", "potato")
                .Replace("with", "bacon")
                .Replace("for", "pancakes")
                .Replace("because", "waffles");
            return this.Ok(new { message = result });
        }
    }
}
```

Great, run it &#x1f60e;

Using the "Try it out" button in the OpenApi UI, enter some input text like "the cat with the fluffy tail ran for cover because cat reasons".
Execute the request, and you should see a response like the following:
```js
{
  "message": "potato cat bacon potato fluffy tail ran pancakes cover waffles cat reasons"
}
```

![OpenApi response](https://reqhubprod.blob.core.windows.net/public/docs/openapi-tutorial-response.png)

Wonderful &#x1f44c;

You can also visit `https://<your_base_url>/tutorial/translate?input=whatever_you_want` in your browser, replacing `<your_base_url>` with your actual base URL.
If a browser window automatically opens when you start the API, you can copy the base URL from the address bar.
Otherwise you can find the possible base URLs within `Properties/launchSettings.json` in Visual Studio.

For example, ours ended up being `localhost:44328`. Our full URL would then be `https://localhost:44328/tutorial/translate`.

## Create a ReqHub API

Now that we have a project up and running, [sign in to ReqHub](https://reqhub.io/login) and create a new API.

![Create API button](https://reqhubprod.blob.core.windows.net/public/docs/new-api.png)

Enter a name for your API, like "Tutorial API" if you don't feel like being silly, or "SUPERBUCKETULTRACATS" if you do.
All you really need is the name, but feel free to edit any of the other options!

![Creating a new API](https://reqhubprod.blob.core.windows.net/public/docs/create-tutorial-api.png)

Click **Save** and you will see the page for your brand new API!
[Here's ours](https://reqhub.io/SpaceGiraffe/Tutorial-API) &#x1f60e;

## Add ReqHub

Back to the code, let's install and configure ReqHub.

Right-click the project and select **Manage NuGet Packages**.

![Manage NuGet packages button](https://reqhubprod.blob.core.windows.net/public/docs/netcore-manage-nuget-packages.png)

Search for ReqHub and install the latest version.

![Installing ReqHub](https://reqhubprod.blob.core.windows.net/public/docs/nuget-install-reqhub.png)

Now there are a couple different paths, depending on how your project is set up.

#### .Net 6.0/C# 10 (Visual Studio 2022)
If you're using Visual Studio 2022, your Program.cs file is probably using the [top-level statements](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/top-level-statements) feature of C#.
Edit Program.cs and add the following `using` statement to the top of the file:

```js
using ReqHub;
```

Add the following lines somewhere near the other calls to `builder.Services`:

```js
var publicKey = "<your_public_key>"; // this can be any string for now
var privateKey = "<your_private_key>"; // this can be any string for now
builder.Services.AddReqHub(publicKey, privateKey);
```

Next, add `app.UseReqHub()` after `app.UseHttpsRedirection()`:

```js
app.UseReqHub();
```

#### Conventional Startup.cs
Otherwise, if your project has a Startup.cs file, open that and add the following `using` statement to the top of the file:

```js
using ReqHub;
```

Add the following lines inside the `ConfigureServices` method:

```js
var publicKey = "<your_public_key>"; // this can be any string for now
var privateKey = "<your_private_key>"; // this can be any string for now
services.AddReqHub(publicKey, privateKey);
```

Go down to the `Configure` method and add the following line between `app.UseRouting()` and `app.UseEndpoints()` if present, otherwise somewhere towards the top of the method.

```js
app.UseReqHub();
```

## Get API keys

Back to your API page, scroll down to the **Publisher keys** section.

![Publisher keys](https://reqhubprod.blob.core.windows.net/public/docs/publisher-keys.png)

Copy the public key and paste over `<your_public_key>` in the code. Then copy the private key and paste over `<your_private_key>`.
Your code should look something like the following:

```js
var publicKey = "r4k4MD6tH82dNtphZ6MO65mcce6n5mc6gaswxEVwcat11AiuyIliQKeDUXIMUfiNoWlq8sGGUkI03rP7A0Q==";
var privateKey = "8RlZzCbRRexhRu3teza2FRIVWZwPXrb0zn0N6r4=";
builder.Services.AddReqHub(publicKey, privateKey);
```

That's it&mdash;your API is ready to go! &#x1f680;

## Testing with Postman

Now when you run the app, the `/tutorial/translate` endpoint will produce errors like `400 Bad Request. headers_missing`.
This is because your API now requires a set of client API keys to call it.
That's good, it's working!
If you have other errors, you may have to troubleshoot your setup.

[Postman](https://www.postman.com) is a tool for testing APIs.
You can [download it here](https://www.postman.com/downloads/).
*(Note: you should be able to skip creating an account if you don't want to, but they're becoming increasingly persistent about that!)*

Once that's set up, [download our Postman collection](https://reqhubprod.blob.core.windows.net/public/tools/postman/ReqHub%20Tutorial.postman_collection.json) for this tutorial.

Import the collection using the import button.

![Import button](https://reqhubprod.blob.core.windows.net/public/docs/postman-import-collection.png)

Then select/drag-and-drop the collection file and select `Import`.

Next, edit the collection and select the **Variables** tab. (Collection menu > Edit > Variables)

![Edit collection variables](https://reqhubprod.blob.core.windows.net/public/docs/postman-tutorial-edit-variables.png)

Copy and paste your client public and private keys into their respective variable slots, for both the initial value and current value.

When you're done, save with Ctrl+S or the save button.

![Client keys](https://reqhubprod.blob.core.windows.net/public/docs/client-keys.png)

Next, expand the collection and select the `/tutorial/translate` request.
Edit the URL to replace `<your_base_url>` with your actual base URL.

You can find your base URL a couple of ways.
If a browser window automatically opens when you start the API, you can copy the base URL from the address bar.
Otherwise you can find the possible base URLs within `Properties/launchSettings.json` in Visual Studio.

For example, ours ended up being `localhost:44328`. Our full URL would then be `https://localhost:44328/tutorial/translate`.

![Client keys](https://reqhubprod.blob.core.windows.net/public/docs/postman-tutorial-endpoint.png)

Make sure your API is running and click **Send**.

You should see a response like the following:

```js
{
    "message": "potato cat"
}
```

Congratulations &#x1f389;! You are now up and running, and ready to conquer the world &#x1f4aa;

Coming up in this tutorial we cover deploying your API, setting up pricing, and going public.
Feel free to stop here if you have an itch to get building your API, and come back when you're ready to put it into production.
Otherwise, let's ship it! &#x1f680;

## Deployment (optional)

Right now your API is only available on your local machine, or possibly within your home network.
In order to make your API accessible to the outside world, it needs to be deployed somewhere on the Internet.

There are many options for hosting, but we will be using [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/overview) for this tutorial.
It's free and integrates well with .Net Core and Visual Studio.

In Visual Studio, right-click the project and select Publish.

![Visual Studio publish project](https://reqhubprod.blob.core.windows.net/public/docs/netcore-publish-project.png)

You should appear on the publish page with a popup window that shows the ways you can publish your app.
Select Azure and click **Next**.

*Note: if the popup window doesn't appear, you can click "Add a publish profile" on the publish page.*

![Publish target](https://reqhubprod.blob.core.windows.net/public/docs/publish-target.png)

After that, select Azure App Service (Windows) and click **Next**.

![Publish platform](https://reqhubprod.blob.core.windows.net/public/docs/publish-target-platform.png)

Now you'll need to be signed in to Visual Studio and Azure with your Microsoft account.
Create or sign into accounts until it lets you proceed to the next step.

![Create account step](https://reqhubprod.blob.core.windows.net/public/docs/publish-create-account.png)

Once your accounts are all set up, you can create an App Service resource to publish to.
If a window to create a new App Service doesn't appear automatically, click the small green plus to the right of **App Service instances**.

1. Replace the ugly, auto-generated name with a nicer one.
2. For the resource group, click **New** and provide a name that makes sense, like "tutorials", or "tutorial resource group" or something. A resource group is kind of like a folder for services in Azure.
3. For the hosting plan, click **New** and fill out the form as shown below. Use any name you want, and try to select a region that makes sense for you (for the purposes of this tutorial it's not important). Be sure to select **Free** for the size!

![Hosting plan setup](https://reqhubprod.blob.core.windows.net/public/docs/publish-hosting-plan.png)

When everything looks good, click **Create**.

![Completed app service form](https://reqhubprod.blob.core.windows.net/public/docs/publish-new-appservice.png)

Now that the app service is created, expand your resource group and select it under **App Service instances**, then click **Next**.

![Select the app service](https://reqhubprod.blob.core.windows.net/public/docs/publish-resource-created.png)

If it asks you about API Management, click **Skip this step** and **Finish**.

![Skipping API management](https://reqhubprod.blob.core.windows.net/public/docs/publish-skip-api-management.png)

If it asks you how you'd like to deploy, stick with the Publish option and click **Finish**.

![Deploy method](https://reqhubprod.blob.core.windows.net/public/docs/publish-method.png)

Finally, hit **Publish**.

![Final deployment](https://reqhubprod.blob.core.windows.net/public/docs/publish-deploy.png)

After a few seconds your browser will open the published project URL.
You should see the `400 Bad Request. headers_missing` error message.
It's a little confusing to be glad to see an error message, but congratulations&mdash;your API is deployed! &#x1f389;

*Note: we know that seemed like a long and complicated process, but it's easy once you're used to it. For subsequent deployments you can just click Publish like in the final step, but we encourage you to create a new publish profile and click through it on your own. If you mess something up, you can always log into Azure and delete it!*

Now let's test it with Postman &#x1f4aa;

#### Testing the deployment

Open up Postman and find the request you created earlier.
We're just going to replace the localhost base URL with the published URL, and send it.

*Note: you can either duplicate the request and modify the URL so you have requests for both the local and the published instances, or you can keep working with the current request&mdash;totally up to you!*

For example, in our case we'll be replacing `localhost:44328` with `reqhub-netcore-tutorial-api.azurewebsites.net`.
After hitting **Send**, you should get a successful response.

![Published API response](https://reqhubprod.blob.core.windows.net/public/docs/postman-published-api.png)

## Set up pricing (optional)

With your API deployed and ready to go, let's add pricing.

To make money selling your APIs, you will need to set up a Stripe account.
If you haven't set up Stripe yet, you should see a button on your dashboard, on the page for an API, or you can find it in the [account section](https://dev.reqhub.io/account).

![Set up Stripe button](https://reqhubprod.blob.core.windows.net/public/docs/set-up-stripe.png)

Follow the screens to complete Stripe's onboarding process.

*Note: Stripe may ask for sensitive information, but that information stays with Stripe and is not visible to ReqHub or SpaceGiraffe, LLC. We don't want it anyway &#x2764;*

Once that's completed, you can set up pricing!

#### Creating a pricing plan

On your API pages, you should now see a button to add pricing plans under the &#x1F4B3; **Plans & pricing** section.

![Add a pricing plan](https://reqhubprod.blob.core.windows.net/public/docs/add-pricing-plan.png)

Fill out the form with the pricing details you want.

We've made ours so it costs just under one million dollars per month &#x1f4aa;

![Creating a pricing plan](https://reqhubprod.blob.core.windows.net/public/docs/tutorial-create-plan.png)

Since this isn't exactly a useful API, the absurdly high cost will hopefully discourage most people from trying to buy it, even though we're pretty sure the transaction would be declined anyway.

Cool. Now hit **Save** at the bottom of the page and check out your new pricing plan!

![Pricing plan](https://reqhubprod.blob.core.windows.net/public/docs/tutorial-pricing-plan.png)

#### Recurring and usage pricing

You probably noticed some of the different pricing options while you were creating your plan.
These allow you to control billing frequency (monthly, quarterly, annually, etc.), amount billed, and per-request price for usage.
You can learn more about this with our recipes for [setting up monthly pricing](/recipes/monthly-pricing) and [setting up usage pricing](/recipes/usage-pricing).

## Go public (optional)

Now we're ready to make our API appear in the ReqHub API marketplace&mdash;let's ship it! &#x1f680;

Edit your API and make sure the **Private** checkbox is un-checked. Hit **Save** and you're done!

![Publishing to the marketplace](https://reqhubprod.blob.core.windows.net/public/docs/tutorial-public-api.png)

#### That's it!

Congratulations, you have created an API from scratch and shipped it with ReqHub &#x1f680;

Now go build something awesome! &#x1f4aa;


## Next steps
Check out our tutorials
Set up Postman
Set up Stripe
Marketing
Making user-friendly docs

