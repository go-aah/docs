Title: RESTful API Versioning
Desc: Learn about RESTful API versioning support of aah web framework
Keywords: restful api, rest api, api, api versioning, api version
---
# RESTful API Versioning

This page gives to you an insights into API versioning strategies supported by aah web framework. API versioning allows you to alter behavior or decided the application logic flow between different clients.

There are a number of valid strategies to approach versioning. Typically API versioning is determined by the incoming client request, and may either be based on the request URL, request URL query parameter or request HTTP headers.

  * [Strategy: URL Path](#strategy-url-path)
  * [Strategy: Accept Header Versioning](#strategy-accept-header-versioning)
  * Strategy: Accept Header with Vendor Media Types **`upcoming`**
  * [Strategy: URL Query Parameter](#strategy-url-query-parameter)
  * [Strategy: HostName/Subdomain](#strategy-hostname)

_**My thoughts: As of now, aah framework user have to fetch `version` info differently based on strategy they choose. Not sure, is it worth have one Method return version number based on chosen strategy. Let me know your thoughts.**_

## Strategy: URL Path

This strategy requires the client to specify the version as part of the URL path.

**For example:**
```
GET /v1/users/
```

Your `routes.conf` include [Named parameter](/routes-config.html#section-routes) with key as a `version`. So that version info available to you in the controller via `ctx.Req.PathValue("version")`.

```
routes {
  path = "/:version"

  routes {
    path = "/users"
    ...
    ...
  }
}
```

_Tip: URL Path based versioning strategy can be particularly useful if you have requirements to route incoming requests to different servers based on the version, as you can configure Request routing for different API versions._

## Strategy: Accept Header Versioning

This strategy requires the client to specify the version as part of the media type in the HTTP `Accept` header. The version is included as a media type parameter, that supplements the main media type.

**For example:** HTTP request using the accept header versioning style.
```
GET /v1/users/
Accept: application/json; version=2.0
```

You can access `version` info via `ctx.Req.AcceptContentType.Version()`.

## Strategy: URL Query Parameter

This strategy is a simple style that includes the version as a URL query parameter.

**For example:**
```
GET /users/?version=1.1
```

You can access `version` info via `ctx.Req.QueryValue("version")`.

## Strategy: HostName

This strategy requires the client to specify the requested version as part of the `hostname` in the URL.

**For example:**
```
# The following is an HTTP request to the URL: http://v1.sample.com/users/

GET /users/
Host: v1.sample.com
```

You can access `version` info via `ctx.Subdomain()`.

_**Note:**_ The HostName strategy may need additional attention on developer machine to use in development/debug mode. Since you may have to configure local DNS mapping in you `host` file. This [rackspace article](https://support.rackspace.com/how-to/modify-your-hosts-file/) covers the steps to modify-your-hosts-file for Mac, Linux and Windows.

For example:
```
127.0.0.1 sample.com v1.sample.com
```

_Tip: HostName based versioning strategy can be particularly useful if you have requirements to route incoming requests to different servers based on the version, as you can configure different DNS records for different API versions._


**Happy Coding :)**
