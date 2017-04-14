# aah Routes Configuration

aah Routes configuration flexible and effective. The configuration syntax is used by aah framework is similar to HOCON syntax, not 100%. To learn more about **[configuration syntax](configuration.html)**.

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
      * [not_found { ... }](#section-not-found)
      * [static { ... }](static-files.html)
      * [routes { ... }](#section-routes)
          - [Namespace/Group routes { &hellip; }](#namespace-group-routes)

Have a look at [aahframework.org routes configuration](https://github.com/go-aah/website/blob/master/config/routes.conf). It is simple one, it gives an idea on how you can do it for your application.

## Section: domains { ... }
Domain and sub-domain configuration goes into section `domains { ... }`.

## Section: unique keyname - user defined
Pick your choice of an `unique keyname` to define your domain section in the routes configuration.

No default value.
```bash
localhost_8080 {
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
```bash
name = "mysampleapp routes"
```

### host
aah framework supports multi-domain routes configuration out-of-the-box. `host` used to determine domain routes for the incoming request.

It is required value, no default value.
```bash
host = "localhost"

host = "aahframework.org"
```

### port
aah framework supports multi-domain routes configuration out-of-the-box. `port` used to determine domain routes for the incoming request.

For port `80`, `443`, just put empty string or `80` as a value.

Default value is `8080`.
```bash
port = "80"
```

### subdomain
Indicates the current domain section is subdomain of existing parent domain.

Default value is `false`.
```bash
subdomain = true
```

### redirect_trailing_slash
Redirect trailing slash is to enable automatic redirection if the current route can't be matched but a `route` for the path with (without) the trailing slash exists.

**For e.g.:** if `/foo/` is requested but a route only exists for `/foo`, the client is redirected to `/foo` with http status code `301` for `GET` requests and `307` for all other request methods as per `RFC7231`.

Default value is `true`.
```bash
redirect_trailing_slash = true
```

### method_not_allowed
aah framework supports out-of-the-box `405 MethodNotAllowed` status with `Allow` header as per `RFC7231`. Perfect for RESTful APIs.

The router checks if another method is allowed for the current route, if the current request can not be routed. If this is the case, the request is answered with `MethodNotAllowed` and HTTP status code `405`. If no other Method is allowed, the request is delegated to the `not_found` controller if defined otherwise default one.

Default value is `true`.
```bash
method_not_allowed = true
```

### auto_options
aah framework supports out-of-the-box `OPTIONS` request replies. User defined `OPTIONS` routes take priority over the automatic replies. Perfect for RESTful APIs.

Default value is `true`.
```bash
auto_options = true
```

### Section: not_found { ... }
Mapping your custom `NotFound` implementation. It is when no matching route is found. If it is not set framework default is called. This is optional one.

Create your controller and action method with param called `isStatic bool`. Framework tells you whether route is `static route` or `application route`.

`controller` and `action` is required value if `not_found` section is defined.
```bash
not_found {
  controller = "App"
  action = "NotFound"
}
```

---

## Section: routes { ... }
Routes section is used to defining application routes, individual routes or namespace/group routes very easily.

Each route definition has following config attributes, some has default value if it's not set.

Pick your choice of `unique name` for each route definition. It is called as `route name` and used for Reverse URL generation.

Sample route definition:
```bash
# Usages
register_user { # route name
  # route config goes here
}

edit_user { # route name
  # route config goes here
}
```

### path
Path config attribute value is used to match incoming request by router. It can contain two types of parameters:

* `:name` - Named parameter : It is dynamic path segments. They match anything until the next `/` or the path end. For e.g.: `/blog/:category/:post`
* `*name` - Catch-all parameter : It match anything until the path end, including the directory index (the `/` before the catch-all). Since they match anything until the end, catch-all parameters `must` always be the final path element. For e.g.: `/assets/*filepath`

Note: `path` must begin with `/`.

It is required, no default value.
```bash
# Usages
path = "/users/:userId/profile"

path = "/users/:userId"

path = "/users"
```

### method
Method config attribute value is `HTTP` method. It can be multiple `HTTP` methods with comma separated. It can be lowercase or uppercase.

Default value is `GET`.
```bash
# Usages
method = "POST"

method = "PUT,PATCH"
```

### controller
Mapping the controller to be called for mapped URL path.

* `controller` attribute supports with or without package prefix. For e.g.: `v1.User` or `User`
* `controller` attribute supports both naming conventions. For e.g.: `User` or `UserController`

It is required, no default value.
```bash
# Usages
controller = "User"

controller = "UserController"

controller = "v1.User"

controller = "v1.UserController"
```

### action
Mapping the action/method name in the controller to be called for mapped URL path.

Default values are mapped based on `HTTP` method. ***Note: for multiple HTTP method mapping no default value, you have to provide one***.

* `GET` - action is `Index`
* `POST` - action is `Create`
* `PUT` - action is `Update`
* `PATCH` - action is `Update`
* `DELETE` - action is `Delete`
* `OPTIONS` - action is `Options`
* `HEAD` - action is `Head`
* `TRACE` - action is `Trace`

```bash
action = "EditUser"
```

---

## Namespace/Group routes { ... }

Configuring namespace/group routes in aah framework is easy. Simply define route within route definition to make that as namespace/group routes.

If you're not interested in namespace/group, you can define every routes with full path as traditional approach.

#### Sample:
Defining route within route definition to make that as namespace/group routes.
```bash
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
