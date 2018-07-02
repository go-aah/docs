## Changelog

#### Features

  * WebSocket feature [#126]({{aah_issues_url}}/126), [documentation](websocket.html)
  * aah Single binary feature `aah build --single` [#156]({{aah_issues_url}}/156), [documentation](vfs.html)
  * Command `aah migrate` to fix/upgrade application codebase to latest aah version [#116]({{aah_issues_url}}/116)
  * OAuth2 auth scheme 3-legged flow [#187]({{aah_issues_url}}/187), [documentation](auth-schemes/oauth2.html)
  * File search option in aah single binary `<app-binary> -list "regex"` [#179]({{aah_issues_url}}/179)
  * `JSONSecure` to prevent Cross Site Script Inclusion (XSSI) attacks [#158]({{aah_issues_url}}/158)

#### Enhancements

  * Configurable option of Authorization (roles and permissions) for each route [#162]({{aah_issues_url}}/162), [documentation](authorization.html#via-configuration)
  * Automatic semantic route configuration for auth scheme `form` [#184]({{aah_issues_url}}/184), [documentation](auth-schemes/form.html#auto-semantic-route-configuration)
  * Hot-reload view files without watcher support (no server restart) on profile `dev` [#189]({{aah_issues_url}}/189)
  * Added `OnPreShutdown` server extension point [#185]({{aah_issues_url}}/185), [documentation](server-extension.html#event-onpreshutdown)
  * Added `OnHeaderReply` server extension point to manipulate Response headers application wide [#181]({{aah_issues_url}}/181), [documentation](server-extension.html#event-onheaderreply)
  * Added option to override to the `path` value in nested routes using `^` [#190]({{aah_issues_url}}/190)
  * No more intermediate startup scripts (aah.sh, aah.cmd), same experience with single and non-single binary [#178]({{aah_issues_url}}/178), [#183]({{aah_issues_url}}/183) 
  * Added support `www => non-www` and vice versa [#177]({{aah_issues_url}}/177), [documentation](app-config.html#section-server-redirect)
  * No hassle, single domain application does not need domain mapping configuration on environment profile [#163]({{aah_issues_url}}/163)
  * Request and Response code optimization [#157]({{aah_issues_url}}/157)
  * Removed deprecated methods in-favour of new command `aah migrate code` [#186]({{aah_issues_url}}/186)
  * Removed `render.pretty` config option in-favor of REST client editor tool and browser support [#165]({{aah_issues_url}}/165)
  * Overall code and documentation improvements
