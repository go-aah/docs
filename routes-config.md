Title: aah Routes Configuration
Desc: Application Routes configuration - domains, subdomains and wildcard subdomain.
Keywords: routes config, routes configuration, namespace routes, group routes, routes, nested routes
---
# aah Routes Configuration

aah Routes configuration is used to configure application domains and subdomains, static and application routes.

Learn [configuration syntax](configuration.html).

### Table of Contents

  * [Domain Configuration](#domain-configuration) - `domain_key_name { ... }`
  * [Static Routes Configuration](static-files.html) - `static { ... }`
  * [Routes Configuration](#routes-configuration) - `routes { ... }`
  * [Samples of Nested Routes (aka Group Routes, Namespace)](#samples-of-nested-routes)
      - [Tips for nested Routes](#tips-for-nested-routes)
      - [Demo](#demo)

## Domain Configuration

One or more domain/subdomain configurations get configured.

`domain_name_key { ... }` configuration goes under config section `domains { ... }` in `routes.conf`.
<br>
```bash
#------------------------------------------------------------------------------
# Domain/subdomain Key Name
# Choose a `unique keyname` to define domain section and its configuration.
# Tip: domain name address, port no values could be used to create a 
# meaningful domain key, etc.
#
# Doc: https://docs.aahframework.org/routes-config.html#domain-configuration
#------------------------------------------------------------------------------
domain_key_name {
  # Name of the domain routes config.
  name = "app name routes"

  # aah is natively multi-tenant. It supports domains and subdomains.
  # Config `host` is used to determine domain/subdomain for the incoming request.
  # Wildcard subdomain is supported too.
  #
  # It is an environment specific value, so define it in profile configuration.
  # For example: In aah website, `prod` profile has mapping for
  # aahframework.org and docs.aahframework.org.
  host = "localhost"

  # Port is used along with config `host` to identify incoming request.
  #
  # For standard port `80` and `443`, put empty string or a value
  # Default value is `8080`.
  port = "80"

  # Denotes whether a domain is a subdomain. Wildcard subdomain is supported.
  # Default value is `false`.
  subdomain = false

  # Redirect trailing slash is to enable automatic redirection if current
  # route can't be matched but a `route` for the path with or without
  # the trailing slash exists.
  #
  # Default value is `true`.
  redirect_trailing_slash = true

  # aah supports `405 MethodNotAllowed` status with `Allow` header as per
  # `RFC7231`, perfect for RESTful APIs.
  #
  # Default value is `true`.
  method_not_allowed = true

  # aah supports HTTP `OPTIONS` request automatic replies.
  # User defined `OPTIONS` routes take priority over automatic replies.
  # It is perfect for RESTful APIs.
  #
  # Default value is `true`.
  auto_options = true

  # Default auth is used when route does not have attribute `auth` defined.
  # Supported values are
  #   - auth_scheme_key_name
  #   - anonymous
  #   - authenticated
  #
  # Default value is empty string.
  default_auth = "auth_scheme_key_name"

  # Anti-CSRF (Cross-Site Request Forgery protection)
  # aah protects all HTML forms automatically.
  #
  # Note: It is not applicable for API and WebSocket.
  #
  # Default value is `true`.
  anti_csrf_check = true

  #----------------------------------------------------------------------------
  # CORS (Cross-Origin Resource Sharing)
  # Doc: https://docs.aahframework.org/cors.html
  #----------------------------------------------------------------------------
  # Refer CORS documentation.
}
```

## Routes Configuration

`routes { ... }` configuration goes under the config section `domain_name_key { ... }` in `routes.conf`.
<br>
```bash
# -----------------------------------------------------------------------------
# Routes Configuration
#
# Doc: https://docs.aahframework.org/routes-config.html#routes-configuration
# -----------------------------------------------------------------------------
routes {
  # -----------------------------------------------------------------------------
  # Choose a `unique name` for each route name. It is unique within one domain
  # configuration. Route name is used to get the Route URL in the Controller-
  # ctx.RouteURL("index"), as an example.
  # -----------------------------------------------------------------------------
  index {
    # Path is used to match incoming request URL. It must begin with `/`.
    # aah supports two types of path parameter definitions:
    #   - `:paramName` is called as named parameter
    #   - `*paramName` is called as catch-all parameter
    #
    # Supports route constraints for the parameter.
    #
    # NOTE:
    # Each route section supports nested routes definitions `routes { ... }`
    # up to `n` level.
    #
    # It is required value, no default.
    path = "/"

    # HTTP verb/method mapping.
    #   - Supports multiple `HTTP` verbs as comma-separated values
    #   - Supports lowercase and uppercase
    #
    # Default value is `GET`.
    method = "GET"

    # Controller mapping for mapped `path` and `method`.
    # It becomes a fully qualified package path with controller name.
    #
    # Examples- (controller is defined with or without package prefix)
    #   - UserController
    #   - /v1/user/UserController
    #   - /v2/user/UserController
    #   - /v1/admin/permission/PermissionsController
    #   - /v2/admin/permission/PermissionsController
    #
    # It is the required value and not a default one.
    controller = "AppController"

    # Action mapping for the mapped controller.
    #
    # Default value is mapped based on config attribute `method`.
    #   GET         => Index
    #   POST        => Create
    #   PUT, PATCH  => Update
    #   DELETE      => Delete
    #   OPTIONS     => Options
    #   HEAD        => Head
    #   TRACE       => Trace
    action = "Index"

    # Auth scheme keyname (from security.auth_schemes { ... }) for the route.
    # Supported values are
    #   - auth_scheme_key_name
    #   - anonymous
    #   - authenticated
    #
    # Default value is derived in any one these three ways:
    #   - Checks if parent route `auth` attribute value is defined and inherits it.
    #   - If not, then checks if `default_auth` attribute config value is defined
    # and inherits it.
    #   - Otherwise, it becomes undefined (empty string)
    #
    # When undefined, possible outcomes are:
    #   - If auth schemes are defined in security.conf, then `403 Forbidden`
    #   - Otherwise, aah treats it as `anonymous`
    auth = "anonymous"

    # Max HTTP request body size gives the ability to control route request body size.
    # It gets applied to payload supported HTTP verbs, such as POST, PUT and DELETE.
    #
    # Default value is from app config `request.max_body_size` which is `5mb`.
    max_body_size = "5mb"

    # Anti-CSRF config attribute is used to enable/disable CSRF protection.
    #   - For RESTful API route, disable it
    #   - Generally do not disable Anti-CSRF for web applications
    #
    # Default value is `true`.
    anti_csrf_check = true
  }

  # and so on ..
}
```

## Samples of Nested Routes

Configuring nested routes (aka group routes and namespace) in aah is easy. Basically just define section `routes { ... }` with route any routes definition.

#### Take a look at the below config structure illustration:

```bash
# -----------------------------------------------------------------------------
# Routes Configuration
#
# Doc: https://docs.aahframework.org/routes-config.html#routes-configuration
# -----------------------------------------------------------------------------
routes {
  # unique route name
  <route_name1> {
    # this route attributes

    # child routes - level 1
    routes {
      # unique route name
      <route_name11> {
        # this route attributes

        # child routes - level 2
        routes {
          # unique route name
          <route_name111> {

            # you can go on any level with your creativity

          }        
        } # end - child routes - level 2
      }
    } # end - child routes - level 1
  }

  # unique route name
  <route_name2> {
    # same as above
  }
}
```

### Tips for nested Routes

Route Attribute | Description
--------------- | -----------
path | Inherits parent `path` config as-is if child config is not defined; otherwise aah prefixes parent `path` with child path
method | Provided value otherwise default value used; i.e. `GET`.
controller | Inherits parent `controller` config as-is if child config is not defined
action | Value is chosen based on HTTP verb if it is not defined
auth | Inherits parent `auth` config as-is if child config is not defined
authorization | Inherits parent `authorization` config as-is if child one is not defined
max_body_size | If it is not specified, then uses `request.max_body_size` config value from `aah.conf`
anti_csrf_check | Default value is true for web applications. It does not apply for REST API applications. <br><br>**Note:** If you have APIs within a web application, then set this attribute to `false`


### Demo

```bash
# Let us say controller `UserController` and
# it has actions `List`, Index, `Create`, `Update`, `Delete`, `Settings` and `UpdateSettings`.

# URLs are:
Get Users            - GET    /v1/users
Create User          - POST   /v1/users
Get User             - GET    /v1/users/:id
Update User          - PATCH  /v1/users/:id
Delete User          - DELETE /v1/users/:id
Get User Settings    - GET    /v1/users/:id/settings
Update User Settings - PATCH  /v1/users/:id/settings
```

<br>
#### NOTE: This is just to demonstrate the nested routes. Creativity has no end, right? :)

```bash
routes {
  v1_api {
    path = "/v1"

    routes {
      # /v1/users
      users {
        path = "/users"
        controller = "UserController"
        action = "List"

        routes {
          # /v1/users
          create_user {
            method = "POST"
          }

          routes {
            path = "/:id"

            routes {
              # /v1/users/:id
              get_user {
                # Inherits from parent[[s]]
              }

              # /v1/users/:id
              update_user {
                method = "PATCH"
              }

              # /v1/users/:id
              delete_user {
                method = "DELETE"
              }

              # /v1/users/:id/settings
              get_user_settings {
                path = "/settings"
                action = "Settings"
              }

              # /v1/users/:id/settings
              update_user_settings {
                path = "/settings"
                method = "PATCH"
                action = "UpdateSettings"
              }
            }
          }
        }
      } # end users routes section
    }
  } # end v1_api
}
```
