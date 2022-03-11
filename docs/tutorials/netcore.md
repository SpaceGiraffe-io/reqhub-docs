
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
We won't be concerned about the actual content of the API, so we'll be building a ridiculous web service that replaces certain words from an input string.
Then when you're done, you can either use this project as an example, or as a starting point for your real project!

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

#### Testing the deployment

## Set up pricing (optional)

#### Recurring pricing

#### Usage pricing

## Go public (optional)

#### That's it!

Congratulations, you have created an API with ReqHub &#x1f389;


## Next steps
Check out our tutorials
Set up Postman
Set up Stripe
Marketing
Making user-friendly docs

