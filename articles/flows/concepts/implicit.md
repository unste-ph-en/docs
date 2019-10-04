---
title: Implicit Flow
description: Learn how the Implicit flow works and why you should use it for single-page apps (SPAs).
topics:
  - authorization-code
  - implicit
  - hybrid
  - api-authorization
  - grants
  - authentication
  - SPA
  - single-page apps
contentType: concept
useCase:
  - secure-api
  - call-api
  - add-login
---
# Implicit Flow

During authentication, single-page applications (SPAs) have some special requirements. Since the SPA is a public client, it is unable to securely store information such as a Client Secret. As such, traditional guidance suggests using a special authentication flow exists called the OAuth 2.0 Implicit Flow (defined in [OAuth 2.0 RFC 6749, section 4.2](https://tools.ietf.org/html/rfc6749#section-4.2)). 

Using the Implicit Flow streamlines authentication by returning tokens without introducing any unnecessary additional steps, but often returns tokens in the URL, which can be problematic in certain cases. As such, new the [Authorization Code Flow with PKCE](/flows/concepts/auth-code-pkce) instead. For a more detailed explanation, see our blog post [OAuth2 Implicit Grant and SPA: Everything you always wanted to know (but were afraid to ask)](https://auth0.com/blog/oauth2-implicit-grant-and-spa/).

::: panel Should I use Authorization Code Flow with PKCE or Implicit Flow for my SPA?
Use Authorization Code Flow with PKCE if:
* you are building a brand new SPA; why not use the latest recommendation when starting from the beginning?
* you are passing Access Tokens through the URL and have no steps in place to mitigate the known issues introduced by this approach.
* you are passing Access Tokens through the URL and it's worth it to you to update your code to adhere to the new recommendations.

Use Implicit Flow if:
* your pre-built SPA retrieves ID Tokens and directly consumes them.
* your pre-built SPA sends tokens to a server via a POST.
* your pre-built SPA already includes steps to mitigate the known issues with passing Access Tokens via the URL.
:::

## How it works

For SPAs, you should use the Implicit Flow in which issued tokens are short-lived. <dfn data-key="refresh-token">Refresh Tokens</dfn> are not available in this flow.

![Implicit Flow Authentication Sequence](/media/articles/flows/concepts/auth-sequence-implicit.png)

1. The user clicks **Login** within the SPA.
2. Auth0's SDK redirects the user to the Auth0 Authorization Server (**/authorize** endpoint) passing along a `response_type` parameter that indicates the type of requested credential.
3. Your Auth0 Authorization Server redirects the user to the login and authorization prompt.
4. The user authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the SPA.
5. Your Auth0 Authorization Server redirects the user back to the SPA with any of the following, depending on the provided `response_type` parameter (step 2):
* An ID Token;
* An Access Token;
* An ID Token and an Access Token.
6. Your SPA can use the Access Token to call an API.
7. The API responds with requested data.

## How to implement it

The easiest way to implement the Implicit Flow is to follow our [Single-Page App Quickstarts](/quickstart/spa).

You can also use our [Javascript SDKs](/libraries). Please ensure you are implementing mitigations that are appropriate for your SPA architecture.

::: note
The Auth0 Single-Page App SDK adheres to the new recommendations and uses the [Authorization Code Flow with PKCE](/flows/concepts/auth-code-pkce). Avoid this SDK if you want to implement traditional guidance using the Implicit Flow.
:::

Finally, you can follow our tutorials to use our API endpoints to [Add Login Using the Implicit Flow](/flows/guides/implicit/add-login-implicit) or [Call Your API Using the Implicit Flow](/flows/guides/implicit/call-api-implicit).

:::

## Keep reading

- Auth0 offers many ways to personalize your user's login experience using [rules](/rules) and [hooks](/hooks).
- [Tokens used by Auth0](/tokens)
