Title: aah Routes Configuration
Desc: Route configuration is to map controller and action with request URL. Supports path variables, domain and sub-domains.
Keywords: routes config, routes configuration, namespace routes, group routes, routes
---
# aah Routes Configuration

aah Routes configuration is flexible and effective. The configuration syntax is used by aah framework is very similar to HOCON syntax. To learn more about **[configuration syntax](configuration.html)**.

Reference to [App Config](app-config.html), [Security Config](security-config.html), [Log Config](log-config.html).

### Table of Contents

  * [domains { ... }](#section-domains)
    - [domain { ... }](#section-unique-keyname-user-defined)
      * [name](#name)
      * [host](#host)
      * [port](#port)
      * [subdomain](#subdomain)
      * [redirect_trailing_slash](#redirect-trailing-slash)
      * [method_not_allowed](#method-not-allowed)
      * [auto_options](#auto-options)
      * [default_auth](#default-auth) <span class="badge lb-xs">Since v0.7</span>
      * [not_found { ... }](#section-not-found) <span class="badge lb-xs lb-drop-color">On v0.8</span> removed, in-favor of [Centralized Error Handler](centralized-error-handler.html)
      * [static { ... }](static-files.html)
      * [routes { ... }](#section-routes)
          - [path](#path)
          - [method](#method)
          - [controller](#controller)
          - [action](#action)
          - [auth](#auth) <span class="badge lb-xs">Since v0.7</span>
          - [max_body_size](#max-body-size) <span class="badge lb-xs">Since v0.8</span>
          - [anti_csrf_check](#anti_csrf_check) <span class="badge lb-xs">Since v0.9</span>
          - [Namespace/Group routes { &hellip; }](#namespace-group-routes)

Have a look at [aahframework.org routes configuration](https://github.com/go-aah/website/blob/master/config/routes.conf). It is simple one, it gives an idea on how you can do it for your application.

## Section: domains { ... }
Domain and sub-domain configuration goes into section `domains { ... }`.

## Section: unique keyname - user defined
Pick your choice of an `unique keyname` to define your domain section in the routes configuration.

No default value.
```cfg
localhost {
  # domain config goes here
}

#OR

aahframework_org {
  # domain config goes here
}
```

### name
Used as free text for mentioning domain name.

Default value is `keyname`.
```cfg
name = "mysampleapp routes"
```

### host
aah framework supports multi-domain routes configuration out-of-the-box. `host` used to determine routes of domain and processing the incoming request.

<span class="badge lb-sm">Since v0.6</span> Wildcard subdomain is supported.

It is required, no default value.
```cfg
host = "localhost"

host = "aahframework.org"

# this is for wildcard Subdomain, don't forget mention `subdomain = true`
host = "*.aahframework.org"
```

### port

aah framework supports multi-domain routes configuration out-of-the-box. Port is used to determine domain routes for the incoming request. This is useful when you have proxy server in-front of aah application.

For port `80` and `443`, put empty string or actual value.

Default value is 8080.
```cfg
port = "80"
```

### subdomain
Indicates the current domain section is a sub-domain.

<span class="badge lb-sm">Since v0.6</span> Wildcard subdomain supported.

Default value is `false`.
```cfg
subdomain = true
```

### redirect_trailing_slash
Redirect trailing slash is to enable automatic redirection if the current route can't be matched but a `route` for the path with (without) the trailing slash exists.

**For e.g.:** if `/foo/` is requested but a route only exists for `/foo`, the client is redirected to `/foo` with http status code `301` for `GET` requests and `307` for all other request methods as per `RFC7231`.

Default value is `true`.
```cfg
redirect_trailing_slash = true
```

### method_not_allowed
`405 MethodNotAllowed` reply is supported out-of-the-box status with HTTP Header `Allow` as per `RFC7231`. Perfect for RESTful APIs.

The router checks if another method is allowed for the current route, if the current request can not be routed. If this is the case, the request is answered with `MethodNotAllowed` and HTTP status code `405`. If no other Method is allowed, the request is delegated to the `not_found` controller if defined otherwise default one.

Default value is `true`.
```cfg
method_not_allowed = true
```

### auto_options
`OPTIONS` request auto replies supported out-of-the-box. User defined `OPTIONS` routes take priority over the automatic replies. Perfect for RESTful APIs.

Default value is `true`.
```cfg
auto_options = true
```

### default_auth
<span class="badge lb-sm">Since v0.7</span> Default auth is used when route does not have attribute `auth` defined.

Default value is empty string.
```cfg
default_auth = "form_auth"
```

### Section: not_found { ... }
<span class="badge lb-sm lb-drop-color">On v0.8</span> configuration is removed in-favor of [Centralized Error Handler](centralized-error-handler.html).

<strike>Define your custom `NotFound` implementation. It is invoked when no matching route is found. If not defined default one is invoked. This is optional section.

Create your controller and action of your choice. Then register in the routes config. You may call `IsStaticRoute()` in the NotFound action to know whether the incoming request is `static or application route`.

`controller` and `action` is required value if `not_found` section is defined.
```cfg
not_found {
  controller = "App"
  action = "NotFound"
}
```
</strike>

---

## Section: routes { ... }
Routes section is used to define application routes. It is easy to Individual routes or namespace/group routes.

Each route definition has config attributes called `path`, `method` `controller`, and `action`. Some has default value if it's not set.

Pick your choice of `unique name` for each route definition. It is called as `route name` and used for Reverse URL generation.

Sample route definition:
```cfg
# Usages
register_user { # route name, it is used for reverse route
  # route config goes here
}

edit_user { # route name, it is used for reverse route
  # route config goes here
}
```

### path
Path config attribute value is used to match incoming request by router. It can contain two types of parameters:

  * `:name` - Named parameter : It is dynamic path segments. They match anything until the next `/` or the path end. For e.g.: `/blog/:category/:post`
  * `*name` - Catch-all parameter : It match anything until the path end, including the directory index (the `/` before the catch-all). Since they match anything until the end, catch-all parameters `must` always be the final path element. For e.g.: `/assets/*filepath`

<div class="alert alert-info-blue">
<p><strong>Note:</strong> <code>path</code> must begin with <code>/</code>.</p>
</div>

It is required, no default value.
```cfg
# Usages
path = "/users/:userId/profile"

path = "/users/:userId"

path = "/users"
```

### method
Method config attribute value is `HTTP` method. It can be multiple `HTTP` methods with comma separated. It can be lowercase or uppercase.

Default value is `GET`.
```cfg
# Usages
method = "POST"

method = "PUT,PATCH"
```

### controller
`controller` attribute is used to map controller to be called for the mapped URL definition in the `path`.

* `controller` attribute supports with or without package prefix. For e.g.: `v1/User` or `User`
    * Best Practices: choose one format definition style and stick to it.
* `controller` attribute supports both naming conventions. For e.g.: `User` or `UserController`

It is required, no default value.
```cfg
# Usages
# Best Practices: choose one format of definition style and stick to it.
controller = "User"

controller = "UserController"

controller = "v1/User"

controller = "v1/UserController"
```

### action
`action` attribute is used to map defined action method from the controller for the `path`.

Default values are mapped based on `HTTP` method. ***Note: for multiple HTTP method mapping no default value, you have to provide one***.

* `GET` - action is `Index`
* `POST` - action is `Create`
* `PUT` - action is `Update`
* `PATCH` - action is `Update`
* `DELETE` - action is `Delete`
* `OPTIONS` - action is `Options`
* `HEAD` - action is `Head`
* `TRACE` - action is `Trace`

```cfg
action = "EditUser"
```

### auth
<span class="badge lb-sm">Since v0.7</span> Auth config attribute is used to assign auth scheme for the route. If you do not set this attribute then framework acquire value as follows.

  * Inherits the parent route `auth` attribute if present.
  * Inherits the `default_auth` attribute config value if defined.
  * Otherwise it becomes not defined.

**Note:**

When routes `auth` attribute is not defined; two possible actions are taken:

  * If one or more auth schemes are defined in `security.auth_schemes { ... }` and routes `auth` attribute is not defined then framework treats that route as `403 Forbidden`.
  * Else framework treats that route as `anonymous`.

Default value is empty string.
```cfg
auth = "form_auth"
```

### max_body_size
<span class="badge lb-sm">Since v0.8</span> Max request body size for this particular route. This is override value of `request.max_body_size` from aah.conf.

Default value is `0mb`. Global default value in aah.conf is `5mb`.
```cfg
max_body_size = "100mb"
```

### anti_csrf_check
<span class="badge lb-sm">Since v0.9</span> Optionally you can disable Anti-CSRF check for particular route. There are cases you might need this option. In-general don't disable the check.

Default value is `true`.
```cfg
anti_csrf_check = false
```

---

## Namespace/Group routes { ... }

Configuring namespace/group routes is very easy to define. Simply define `routes` within route definition to make that as namespace/group routes.

If you're not interested in namespace/group, you can define every routes with full path as traditional approach.

#### Sample:
Defining route within route definition to make that as namespace/group routes.
```cfg
routes {
  v1_api {
    path = "/v1"

    routes {
      list_users {
        # /v1/users
        path = "/users"
        controller = "User"
        action = "List"

        # adding child routes
        # this routes section can go to create_user route too, doesn't matter
        routes {
          edit_user {
            # /v1/users/:id
            path = "/:id"
            method = "POST"
            controller = "User"
            action = "Edit"
          }

          disable_user {
            # /v1/users/:id/settings
            path = "/:id/settings"
            controller = "User"
            action = "Disable"
          }
        }
      }

      create_user {
        # /v1/users
        path = "/users"
        method = "POST"
        controller = "User"
        #action = "Create" # default value is Create
      }
    }
  }
}
```
