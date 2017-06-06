## Changelog

#### Features

* [#52](https://github.com/go-aah/aah/issues/52) Pluggable Minifier, HTML Minify, refer to [tutorial](/tutorial/html-minify.html).
* [#42](https://github.com/go-aah/aah/issues/42) `aah list` command, ability to [list](aah-cli-tool.html#command-list) current existing aah projects from `GOPATH`.

#### Enhancements

* [#49](https://github.com/go-aah/aah/issues/49) Wildcard Subdomain support, refer to [tutorial](/tutorial/domain-subdomain-and-wildcard-subdomain.html).
* [#53](https://github.com/go-aah/aah/issues/53) Configurable `Cache-Control` header by [MIME-Type](static-files.html#cache-control) for static files.
* [#55](https://github.com/go-aah/aah/issues/55) Bind Go Logger to aah log receiver. You can have unified log of third-party libs to one log file.
* [#50](https://github.com/go-aah/aah/issues/50) Improved handling of controllers sub-packages in auto view resolve. Take full-control over view rendering.
* [#51](https://github.com/go-aah/aah/issues/51) Config option `default_layout = false` to disable to default layout.
* [#55](https://github.com/go-aah/aah/issues/55) HTTP server error log goes to aah log receiver, unified.
* Code Improvements, Doc, Logging.

#### Bug Fixes

* [#50](https://github.com/go-aah/aah/issues/50) View auto resolve bug fix.
* [#59](https://github.com/go-aah/aah/issues/59) session clear fix while redirect happens.
