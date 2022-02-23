
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

## Make the endpoint

With the project created, delete `WeatherForecast.cs` and `Controllers/WeatherForecastController.cs`.

Then create `TutorialController.cs` under the `Controllers/` folder and replace its contents with the following:

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

Using the "Try it out" button in the OpenApi UI with some input text like "the cat with the fluffy tail ran for cover because cat reasons" should produce a response like:
```js
{
  "message": "potato cat bacon potato fluffy tail ran pancakes cover waffles cat reasons"
}
```
Perfection &#x1f44c;

You can also visit `https://<your base URL>/tutorial/translate?input=whatever_you_want` in your browser, replacing `<your base URL>` with your actual base URL.
If a browser window doesn't automatically open to the correct URL when you start the API, you can find the possible base URLs within `Properties/launchSettings.json`.

## Get API keys

## Add ReqHub

## Testing with Postman

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

