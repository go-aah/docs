Title: Routing URL
Desc: aah framework routing implementation is Radix tree algorithm.
Keywords: routing url, routing algorithm, routing
---
# Routing URL

aah framework supports domain and subdomains seamlessly and reverse route lookup by route name effectively.

aah framework uses routing algorithm implementation from [httprouter](https://github.com/julienschmidt/httprouter) developed by [@julienschmidt](https://github.com/julienschmidt). Customized by author of aah framework [@jeevatkm](https://github.com/jeevatkm). Router supports path variables in the routing pattern and matches against the request method and scales better.

The router is optimized for high performance and a small memory footprint. It scales well even with very long paths and a large number of routes. A compressing dynamic trie (radix tree) structure is used for efficient matching.

## Configuration

Router config is very flexible in nature with default values for effective usage.

  * [Learn more about configuration syntax](configuration.html)
  * [aah router configuration](routes-config.html)

## Features

[Learn more about routes configuration](routes-config.html), how to advantage of these features.

  * Domain, sub-domains and wildcard sub-domains supported seamlessly.
  * **Static Files** - Serve static file or directory via `http.ServeContent`.
  * **Directory Listing** - Directory listing is supported, presented clean and nicely.
  * **Custom NotFound** - Register your custom controller `not found` action to handle this behavior.
  * **Only explicit matches** - By design of this router, a request can only match exactly one or no route. As a result, there are also no unintended matches, which makes it great for SEO and improves the user experience.
  * **Stop caring about trailing slashes** - Choose the URL style you like, the router automatically redirects the client if a trailing slash is missing or if there is one extra. Of course it only does so, if the new path has a handler. If you don't like it, you can turn off this behavior `redirect_trailing_slash = false` in the [routes config](routes-config.html).
  * **Path auto-correction** - Besides detecting the missing or additional trailing slash at no extra cost, the router can also fix wrong cases and remove superfluous path elements (like `../` or `//`). Is [CAPTAIN CAPS LOCK](http://www.urbandictionary.com/define.php?term=Captain+Caps+Lock) one of your users? HttpRouter can help him by making a case-insensitive look-up and redirecting him to the correct URL.
  * **Parameters in your routing pattern** - Stop parsing the requested URL path, just give the path segment a name and the router delivers the dynamic value to you. Because of the design of the router, path parameters are very cheap.
  * **Zero Garbage** - The matching and dispatching process generates zero bytes of garbage. In fact, the only heap allocations that are made, is by building the slice of the key-value pairs for path parameters. If the request path contains no parameters, not a single heap allocation is necessary.
  * **No server crashes** - aah framework gracefully logs error information and display the message.
  * **Perfect for APIs** - The router design encourages to build sensible, hierarchical RESTful APIs. Router has builtin native support for [OPTIONS requests](http://zacstewart.com/2012/04/14/http-options-method.html) and `405 Method Not Allowed` replies.
