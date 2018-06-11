Title: aah OAuth2 Auth Scheme
Desc: aah provides OAuth2 3-legged authentication flow.
Keywords: oauth2, auth scheme, authentication, social login
---
# OAuth2 Auth Scheme

<span class="badge lb-sm">Since v0.11.0</span> aah supports [OAuth2](https://oauth.net/2/) auth scheme, library  `golang.org/x/oauth2` have been integrated with aah.

Three steps to make use of OAuth2 scheme -

  * Configure client credentials
  * Implement `authc.PrincipalProvider` to provide subject principals based on OAuth2 provider
  * Implement `authz.Authorizer` to provide subject's [roles and permissions](/security-permissions.html)

Refer [aah OAuth2 - Social Login]({{aah-examples}}/oauth2-social-login) example, it demonstrates Facebook, Google and GitHub social login.

### Table of Contents

  * [Configurations](#configuration)
      - [oauth_scheme_name { ... }](#section-oauth-scheme-key)
  * [Example Config](#example-config)
  * [Known Provider's Endpoints](#known-provider-endpoints)
      - [OAuth2 provider names](#oauth2-provider-names-just-alphabetically-ordered)
      - [Configuring OAuth2 Endpoint URLs](#configuring-oauth2-endpoint-urls)

## Configuration

Easy to use and flexible, supports one or more `OAuth2` auth scheme (For e.g.: configuring multiple social login, etc).


### Section: oauth_scheme_key { ... }

`oauth_scheme_key { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# OAuth2 auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/oauth2.html
# -----------------------------------------------------------------------------
oauth_scheme_key {
  # Auth scheme name.
  # Supported values are `form`, `oauth2`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "oauth2"

  # To provide subject's principal information, implement interface
  # `authc.PrincipalProvider` and configure here.
  #
  # It is required value, no default.
  principal = "security/SubjectPrincipalProvider"

  # To provide subject's Roles and Permissions, implement interface
  # `authz.Authorizer` and configure here.
  #
  # It is required value, no default.
  authorizer = "security/AuthorizationProvider"

  # -----------------------------------------------------------------------------
  # OAuth2 Provider's Client credentials
  # Doc: https://docs.aahframework.org/auth-schemes/oauth2.html#configuration
  # -----------------------------------------------------------------------------
  client {
    # Client ID
    id = "client id"

    # Client secret
    secret = "client secret"

    # Scope specifies the requested permissions.
    scopes = ["profile", "email"]

    provider {
      # aah is aware of few OAuth2 providers auth and token URLs via library.
      # So just configure the name here.
      # Refer https://docs.aahframework.org/auth-schemes/oauth2.html#known-provider-endpoints
      name = "facebook"

      # Provider's Auth and Token URL
      # It is required, when `client.provider.name` is not defined.
      #url {
      #  auth = "https://provider.com/o/oauth2/auth"
      #  token = "https://provider.com/o/oauth2/token"
      #}
    }
  }

  # Configuration to provide login, redirect/callback and success URL.
  url {
    # OAuth Login URL
    #
    # Default value is derived from user defined auth scheme key.
    # For e.g.: auth scheme key is `github_auth` then login would be
    # `/github-auth/login`.
    #login = "oauth2/login"

    # Redirect/callback URL is used to receive request from OAuth2 provider
    # after successful authentication.
    #
    # Default value is derived from user defined auth scheme key.
    # For e.g.: auth scheme key is `github_auth` then callback would be
    # `/github-auth/callback`.
    #redirect = "oauth2/callback"

    # Success URL is used to redirect after successful authentication
    # and authorization at application.
    # Default value is `/`.
    #success = "/success.html"
  }
}
```

## Example Config

This is one of the OAuth2 provider configuration from [aah OAuth2 Social login]({{aah-examples}}/oauth2-social-login) example.

```bash
# OAuth2 provider: Google
google_auth {
  scheme = "oauth2"  
  principal = "security/google/SubjectPrincipalProvider"
  authorizer = "security/AuthorizationProvider"

  client {
    id = "client id"
    secret = "client secret"
    scopes = ["profile", "email"]
    provider {
      name = "google"
    }
  }

  url {
    success = "/success.html"
  }
}
```

## Known Provider Endpoints

aah is aware of few OAuth2 providers auth and token URL via oauth2 library. So just mention the name in `client.provider.name`.

#### OAuth2 provider names, just alphabetically ordered -

```
amazon, bitbucket, cern, facebook, fitbit, foursquare, github, gitlab,
google, heroku, hipchat, kakao, linkedin, mailchimp, mailru, mediamath,
microsoft, odnoklassniki, paypal, slack, spotify, twitch, uber, vk,
yahoo, yandex
```


#### For Azure OAuth2 endpoints, name format it -

For more information see: https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-protocols#endpoints

```bash
provider {
  name = "azure.<tenant-name>"
}
```

#### Configuring OAuth2 Endpoint URLs

Configuring OAuth2 URLs instead of mentioning provider's name.

`url { ... }` configuration goes under section `client.provider { ... }`.

```bash
# Provider's Auth and Token URL
# It is required, when `client.provider.name` is not defined.
url {
  auth = "https://provider.com/o/oauth2/auth"
  token = "https://provider.com/o/oauth2/token"
}
```
