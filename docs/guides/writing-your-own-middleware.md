
# Writing your own middleware

#### Prerequisites

The following steps require a ReqHub account. Make sure you're logged in by [signing in](https://reqhub.io/login) or [signing up](https://reqhub.io/create-account).

You should probably also have a good understanding of the request flow from our guide on [how ReqHub works](/getting-started/how-it-works?id=request-flow).

----

## Introduction

In any language, a ReqHub middleware takes a set of headers that identify the client, combines them with headers that identify the API, and sends them to ReqHub for verification.
ReqHub provides a success response if the client is allowed access, or an error otherwise.

The request flow looks like this, per our guide on [how ReqHub works](/getting-started/how-it-works?id=request-flow).

![ReqHub request flow](https://reqhubprod.blob.core.windows.net/public/docs/flow-diagram-raw.png)

Let's break it down into more detail.

#### Request 1 - can I has dis?

This is the initial request from a client appliation to your API.

It should contain the following headers:

* ClientToken - base64-encoded HMACSHA256 hash (more detail below)
* ClientKey - the client's public key from ReqHub
* ClientTimestamp - a UNIX timestamp (in milliseconds) of when the request was created
* ClientNonce - 20 random bytes as a base64-encoded string
* ClientUrl - the URL the client is requesting, used in generating the token

#### Request 2 - hey can they has dat?

This is the request from your API to ReqHub for verification.

#### Response 2 - uhhh yup.

This is where ReqHub provides a success or error response according to whether the client is allowed to access your API.

These are the reasons ReqHub will deny your request:

* asdf
* asdf
* asdf
* asdf

#### Response 1 - okay here you go.

## Generating the token hash

