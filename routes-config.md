Title: aah Routes Configuration
Desc: Application Routes configuration - domains, subdomains and wildcard subdomain.
Keywords: routes config, routes configuration, namespace routes, group routes, routes
---
# aah Routes Configuration

aah Routes configuration is used to configure application domains and subdomains static and application routes.

Learn [configuration syntax](configuration.html).

### Table of Contents

  * [Domain Configuration](#domain-configuration) - `domain_key_name { ... }`
  * [Static Routes Configuration](static-files.html) - `static { ... }`
  * [Routes Configuration](#routes-configuration) - `routes { ... }`
  * [Samples of Nested Routes (aka Group Routes, Namespace)](#samples-of-nested-routes)
      - [Tips for nested Routes](#tips-for-nested-routes)
      - [Demo](#demo)

## Domain Configuration

One or more domain/subdomain configuration gets configured.

`domain_name_key { ... }` configuration goes under the config section `domains { ... }` in `routes.conf`.
<br>
```bash
#------------------------------------------------------------------------------
# Domain/subdomain Key Name
# Choose an `unique keyname` to define domain section and its configuration.
# Tip: domain name address, port no values to create a domain key, etc.
#
# Doc: https://docs.aahframework.org/routes-config.html
#------------------------------------------------------------------------------
domain_key_name {
  # Name of the domain routes config.
  name = "app name routes"

  # aah natively multi-tenants, it supports domains and subdomains.
  # Config `host` is used to determine domain/subdomain for the incoming request.
  # Wildcard subdomain is supported too.
  #
  # It is environment specific values, so define it in profile configuration.
  # For e.g: for aah website `prod` profile has mapping for
  # aahframework.org, docs.aahframework.org.
  host = "localhost"

  # Port is used to along with config `host` to identify incoming request.
  #
  # For standard port `80` and `443`, put empty string or a value
  # Default value is `8080`.
  port = "80"

  # Denotes this domain is a subdomain. Wildcard subdomain supported.
  # Default value is `false`.
  subdomain = false

  # Redirect trailing slash is to enable automatic redirection if the current
  # route can't be matched but a `route` for the path with (without)
  # the trailing slash exists.
  #
  # Default value is `true`.
  redirect_trailing_slash = true

  # aah supports `405 MethodNotAllowed` status with `Allow` header as per
  # `RFC7231`. Perfect for RESTful APIs.
  #
  # Default value is `true`.
  method_not_allowed = true

  # aah supports HTTP `OPTIONS` request automatic replies.
  # User defined `OPTIONS` routes take priority over the automatic replies.
  # Perfect for RESTful APIs.
  #
  # Default value is `true`.
  auto_options = true

  # Default auth is used when route does not have attribute `auth` defined.
  # Supported values are `auth_scheme_key_name`, `anonymous` and `authenticated`.
  #
  # Default value is empty string.
  default_auth = "form_auth"

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
  # Choose an `unique name` for each route name, its unique within one domain
  # configuration. Route name is used to get Route URL in the Controller.
  # For e.g.: ctx.RouteURL("index")
  # -----------------------------------------------------------------------------
  index {
    # Path is used to match incoming request URL. It must begins with `/`.
    # aah supports two types of path parameter definition:
    #   - `:variablename` is called named parameter
    #   - `*variablename` is called catch-all parameter
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
    #   - Supports multiple `HTTP` verbs with comma separated
    #   - Supports lowercase and uppercase
    #
    # Default value is `GET`.
    method = "GET"

    # Controller mapping for the mapped `path` and `method`.
    # It is fully qualified package path with controller name.
    #
    # For e.g.- (controller defined with or without package prefix)
    #   - UserController
    #   - /v1/user/UserController
    #   - /v2/user/UserController
    #   - /v1/admin/permission/PermissionsController
    #   - /v2/admin/permission/PermissionsController
    #
    # It is required value, no default.
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
    # Default value is derived:
    #   - Inherits the parent route `auth` attribute value if defined
    #   - Otherwise, inherits the `default_auth` attribute config value if defined
    #   - Otherwise it becomes undefined (empty string)
    #
    # When undefined, possible outcomes could be:
    #   - If auth schemes defined in security.conf then `403 Forbidden`
    #   - Otherwise aah treats it as `anonymous`
    auth = "anonymous"

    # Max HTTP request body size, ability to control route request body size.
    # It gets applied to payload supported HTTP verbs such as POST, PUT and DELETE.
    #
    # Default value is from app config `request.max_body_size` which is `5mb`.
    max_body_size = "5mb"

    # Anti-CSRF config attribute is used to enable/disable CSRF protection.
    #   - For RESTful API route disable it
    #   - Generally do not disable Anti-CSRF for the web applications
    #
    # Default value is `true`.
    anti_csrf_check = true
  }

  # and so on ..
}
```

## Samples of Nested Routes

Configuring nested routes (aka group routes, namespace) in aah is easy. Basically just define `routes { ... }` with route any definition.

#### To give an idea, take a look at below config structure:

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
path | Inherits parent `path` config as-is if child one is not defined otherwise prefixes parent `path` with child path
method | Provided value otherwise defaults is `GET`.
controller | Inherits parent `controller` config as-is if child one is not defined
action | Value is chosen based on HTTP verb if its not defined
auth | Inherits parent `auth` config as-is if child one is not defined
authorization | Inherits parent `authorization` config as-is if child one is not defined
max_body_size | If its not provided then uses `request.max_body_size` config value from `aah.conf`
anti_csrf_check | Default value is true for web applications. It does not apply for REST API application. <br><br>**Note:** If you have APIs within web application then set this attribute to `false`


### Demo

```bash
# Let's say you have controller `UserController` and
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
#### NOTE: This is to demonstrate the nested routes. Always go with your creativity.

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
                # Inherits from parents
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
