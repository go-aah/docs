Title: aah Routing
Desc: aah supports domain and subdomains seamlessly and reverse route lookup by route name effectively.
Keywords: routing url, routing algorithm, routing
---
# aah Routing

aah supports domain and subdomains seamlessly and reverse route lookup by route name effectively.

The router is optimized for high performance and a small memory footprint. It scales well with very long paths and a large number of routes. A radix tree structure is used for efficient matching.

aah internally uses customized version of [httprouter](https://github.com/julienschmidt/httprouter) developed by [@julienschmidt](https://github.com/julienschmidt).

### Table of Contents

  * [Highlights](#highlights)
  * [Route Definitions](#route-definitions)
  * [Route Constraints](#route-constraints)
      - [Supported Constraints](#supported-constraints)

## Highlights

  * **URI Parameters** - Give the path segment a name and access it as action method parameters.
  * **Perfect for RESTful APIs** - Build sensible, hierarchical RESTful APIs. Router has builtin native support for [OPTIONS requests](http://zacstewart.com/2012/04/14/http-options-method.html) and `405 Method Not Allowed` replies.
  * **Only explicit matches** - a request can only match exactly one or no route. As a result, there are also no unintended matches, which makes it great for SEO and improves the user experience.
  * **Stop caring about trailing slashes** - Choose the URL style you like, the router automatically redirects the client if a trailing slash is missing or if there is one extra. Of course it only does if the new path has a controller action. Behavior could be disabled at domain level using `redirect_trailing_slash = false` in the [routes config](routes-config.html).
  * **Path auto-correction** - Besides detecting the missing or additional trailing slash at no extra cost, the router can also fix wrong cases and remove superfluous path elements (like `../` or `//`). Is [CAPTAIN CAPS LOCK](http://www.urbandictionary.com/define.php?term=Captain+Caps+Lock) one of your users? router can help user by making a case-insensitive look-up and redirecting user to the correct URL.
  * **Zero Garbage** - The matching and dispatching process generates zero bytes of garbage. In fact, the only heap allocations that are made, is by building the slice of the key-value pairs for path parameters. If the request path contains no parameters, not a single heap allocation is necessary.

## Route Definitions

aah `routes.conf` composed of four major config sections. Such `domains`, `domain`, `static routes` and `application routes`.

Learn [routes configuration](routes-config.html) attributes and its purpose.

```bash
# -----------------------------------------------------------------------------
# app name - Application Routes Configuration
#
# Refer documentation to explore and configure routes.
# Doc: https://docs.aahframework.org/routing.html
# -----------------------------------------------------------------------------
domains {
  # Choose an `unique keyname` to define domain section and its configuration.
  # Tip: Use app name, domain name address, port no values to create a domain key.
  #
  # For e.g.: aahframework { ... }, docs_aahframework { ... }
  domain_key_name {

    #
    # Domain configurations - for example host, port, CORS, etc.
    #

    # Static files Routes Configuration
    # Optional section, for e.g.: API and WebSocket apps
    static {
      # ...
    }

    # Routes Configuration
    routes {

      # domain routes goes here

    } # end - routes

  } # end - domain configurations



  # Choose an `unique keyname` to define domain section and its configuration.
  # Tip: Use app name, domain name address, port no values to create a domain key.
  #
  # For e.g.: aahframework { ... }, docs_aahframework { ... }
  domain_key_name {

    #
    # Domain configurations - for example host, port, CORS, etc.
    #

    # Static files Routes Configuration
    # Optional section, for e.g.: API and WebSocket apps
    static {
      # ...
    }

    # Routes Configuration
    routes {

      # domain routes goes here

    } # end - routes

  } # end - domain configurations

} # end - all application domains
```

## Route Constraints

Route constraints gives capability to restrict/validate route parameters (aka URI parameters) before request sent to controller action.

```bash
# Syntax
:parameterName[constraints]
```

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
<li>Constraint failure would result in 400 BadRequest via aah <a href="error-handling.html">error handling</a> flow.</li>
<li>Supports multiple constraints on parameter, separated by a <code>,</code> for e.g.: <code>:paramName[gte=10,lte=50]</code> </li>
<li>Data type constraints are implicit in aah</li>
</ul></p>
</div>

### Examples

```bash
# Examples of route path parameters with constraints

#------------------------------------------------------------------------------
# Example - Data type constraints are implicit in aah
# Path              : /v1/users/:id
# Controller Action : func (u *UserController) Info(id int) { .. }

# Requests
/v1/users/10001           => pass, would reach controller action
/v1/users/myname          => fail, 400 BadRequest

#------------------------------------------------------------------------------
# Example - ISBN 13 route parameter constraint
# Path              : /v1/books/:isbn[isbn13]/excerpt
# Controller Action : func (b *BookController) Excerpt(isbn string) { .. }

# Requests - ISBN numbers taken from https://www.isbn-13.info/example
/v1/books/978-1-56619-909-4/excerpt   => pass, would reach controller action
/v1/books/9781566199094/excerpt       => pass, would reach controller action
/v1/books/1-56619-909-3/excerpt       => fail, 400 BadRequest
/v1/books/1566199093/excerpt          => fail, 400 BadRequest
/v1/books/dshgdshgdsjhgdshg/excerpt   => fail, 400 BadRequest

#------------------------------------------------------------------------------
# Example - oneof and alpha route parameter constraints
# Path              : /v1/temperature/:scale[oneof=celsius fahrenheit,alpha]
# Controller Action : func (w *WeatherController) Temperature(scale string) { .. }

# Requests
/v1/temperature/fahrenheit      => pass, would reach controller action
/v1/temperature/celsius         => pass, would reach controller action
/v1/temperature/3463543         => fail, 400 BadRequest
/v1/temperature/blabla          => fail, 400 BadRequest
#------------------------------------------------------------------------------
```

### Supported Constraints

Constraint | Description | Example
---------- | ----------- | -------
alpha | Validates a string contains only ASCII alpha characters | `:paramName[alpha]`
alphanum | Validates a string contains only ASCII alphanumeric characters | `:paramName[alphanum]`
ascii | Validates a string contains only ASCII characters | `:paramName[ascii]`
numeric | Validates a string contains a basic numeric value. basic excludes exponents etc. | `:paramName[numeric]`
email | Validates a string contains a valid email. NOTE: This may not conform to all possibilities of any rfc standard, but neither does any email provider accept all possibilities | `:paramName[email]`
len | Validates - for numbers: the value is equal to the parameter given. For strings: the value length is exactly that number of characters | `:paramName[len=10]`
max | Validates - for numbers: the value is less than or equal to the parameter given. For strings: the value length is at most that number of characters | `:paramName[max=10]`
min | Validates - for numbers: the value is greater or equal to the parameter given. For strings: the value length is at least that number of characters | `:paramName[min=10]`
eq | Validates - for strings & numbers, the value is equal to the parameter given | `:paramName[eq=10]`
ne | Validates - for strings & numbers, the value is not equal to the parameter given | `:paramName[ne=10]`
gt | Validates - for numbers: the value is greater than the parameter given. For strings: the value length is greater than that number of characters | `:paramName[gt=10]`
gte | Same as 'min' above. Kept both to make terminology with `len` easier | `:paramName[gte=10]`
lt | Validates - for numbers: the value is less than the parameter given. For strings: the value length is less than that number of characters | `:paramName[lt=10]`
lte | Same as 'max' above. Kept both to make terminology with `len` easier | `:paramName[lte=10]`
oneof | Validates - for strings, ints, and uints; the value is one of the values in the parameter. The parameter should be a list of values separated by whitespace. Values may be strings or numbers | `:paramName[oneof=red green]`
base64 | Validates a string value contains a valid base64 value | `:paramName[base64]`
btc_addr | Validates a string value contains a valid bitcoin address. The format of the string is checked to ensure it matches one of the three formats P2PKH, P2SH and performs checksum validation | `:paramName[btc_addr]`
isbn | Validates a string value contains a valid isbn10 or isbn13 value | `:paramName[isbn]`
isbn10 | Validates a string value contains a valid isbn10 value | `:paramName[isbn10]`
isbn13 | Validates a string value contains a valid isbn13 value | `:paramName[isbn13]`
uuid | Validates a string value contains a valid UUID | `:paramName[uuid]`
uuid3 | Validates a string value contains a valid version 3 UUID | `:paramName[uuid3]`
uuid4 | Validates a string value contains a valid version 4 UUID | `:paramName[uuid4]`
uuid5 | Validates a string value contains a valid version 5 UUID | `:paramName[uuid5]`
latitude | Validates a string value contains a valid latitude | `:paramName[latitude]`
longitude | Validates a string value contains a valid longitude | `:paramName[longitude]`
ssn | Validates a string value contains a valid U.S. Social Security Number | `:paramName[ssn]`
ip | Validates a string value contains a valid IP Address | `:paramName[ip]`
ipv4 | Validates a string value contains a valid v4 IP Address | `:paramName[ipv4]`
ipv6 | Validates a string value contains a valid v6 IP Address | `:paramName[ipv6]`
cidr | Validates a string value contains a valid CIDR Address | `:paramName[cidr]`
cidrv4 | Validates a string value contains a valid v4 CIDR Address | `:paramName[cidrv4]`
cidrv6 | Validates a string value contains a valid v6 CIDR Address | `:paramName[cidrv6]`
tcp_addr | Validates a string value contains a valid resolvable TCP Address | `:paramName[tcp_addr]`
tcp4_addr | Validates a string value contains a valid resolvable v4 TCP Address | `:paramName[tcp4_addr]`
tcp6_addr | Validates a string value contains a valid resolvable v6 TCP Address | `:paramName[tcp6_addr]`
udp_addr | Validates a string value contains a valid resolvable UDP Address | `:paramName[udp_addr]`
udp4_addr | Validates a string value contains a valid resolvable v4 UDP Address | `:paramName[udp4_addr]`
udp6_addr | Validates a string value contains a valid resolvable v6 UDP Address | `:paramName[udp6_addr]`
ip_addr | Validates a string value contains a valid resolvable IP Address | `:paramName[ip_addr]`
ip4_addr | Validates a string value contains a valid resolvable v4 IP Address | `:paramName[ip4_addr]`
ip6_addr | Validates a string value contains a valid resolvable v6 IP Address | `:paramName[ip6_addr]`
unix_addr | Validates a string value contains a valid Unix Address | `:paramName[unix_addr]`
mac | Validates a string value contains a valid MAC Address | `:paramName[mac]`
hostname | Validates a string value is a valid Hostname according to [RFC 952](https://tools.ietf.org/html/rfc952), [RFC 1123](https://tools.ietf.org/html/rfc1123) | `:paramName[hostname]`
fqdn | Validates a string value contains a valid FQDN | `:paramName[fqdn]`
