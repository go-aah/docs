# aah Request Lifecycle

Incoming requests are handled by the server via routes. Each route describes an HTTP endpoint with a `path`, `method`, `controller`, and `action`. Each incoming request passes through a pre-defined list of steps, user-defined steps and optional server extensions.

It would be nice to have diagram to explain the Lifecycle, will do my best in the upcoming release.

## Lifecycle

  * `aah.Context` is prepared for the request. i.e. parsing and creating `ahttp.Request` instance and `ahttp.ResponseWriter` instance.
  * `OnRequest` server extension point: Always called, the `aah.Context` object instance is passed via event data. `aah.Context` is decorated with `SetURL` and `SetMethod` methods. Calls to these methods will impact how the request is routed and can be used for rewrite rules. Note: route is not evaluated at this point.
  * Route Lookup: Based on request path.
    * If it's static file route then server process that request via `http.ServeContent`.
    * If no route found then below actions performed. Finally it skips to `OnPreReply` server extension point and writing response on the wire.
      * If `Redirect Trailing Slash` opportunity is found then server redirects that request to the new URL. It can be controlled via [`routes.conf`](routes-config.html).
      * It does HTTP auto `OPTIONS`, User defined `OPTIONS` take precedence over auto. It can be controlled via [`routes.conf`](routes-config.html).
      * It does HTTP auto `405 Method Not Allowed`. It can be controlled via [`routes.conf`](routes-config.html).
      * If custom not found controller is defined, server calls that controller action.
  * Route Found: `aah.Context` is update with targeted controller and action information. If controller or action is not found in the registry. Flow skips to `OnPreReply` server extension point with `404 Not Found` and writing response on the wire.
    * Path Variables are parsed and available at this point.
  * Parse Session Cookie if the session mode is `stateful`
  * `OnPreAuth` server extension point **`upcoming`**.
  * Authenticate request **`upcoming`**.
  * `OnPostAuth` server extension point **`upcoming`**.
  * Read and Parse Request for non `GET` method. Payload, Form, Query, Multi-part based on content-type.
  * Validate request values **`upcoming`**
  * User-defined middleware(s) execution (basically before `m.Next(ctx)` call).
  * Per-Controller interceptor `Before` is called if exists.
  * Per-Controller-Action interceptor `Before<ActionName>` is called if exists.
  * Targeted controller `Action` is called.
  * Per-Controller-Action interceptor `After<ActionName>` is called if exists.
  * Per-Controller interceptor `After` is called if exists.
  * Per-Controller interceptor `Finally` is called if exists.
  * ***Note:*** If any `panic` happens around controller action interceptor `Panic` is called on that controller.
  * User-defined middleware(s) execution (basically after `m.Next(ctx)` call).
  * If Response is already sent via `ctx.Res` and called `ctx.Reply().Done()` the request completes here.
  * If it's a Redirects reply then redirects it.
  * Determines response content-type if not set in reply instance.
  * If it's HTML content-type then
    * Finds a view and master layout if provided in the reply instance it uses that.
    * Populates view args.
  * `OnPreReply` server extension point: Always called, you're allowed to modified `ctx.Reply()` it will reflect on response. Except when-
    * Static file request.
    * `ctx.Reply().Done()` called, refer godoc for more info.
    * `ctx.Reply().Redirect(...)` is called.
  * Render reply instance based on content-type, if any errors it logs and sends meaningful error message.
  * Writing reply on the wire-
    * Write Header(s).
    * Write Status - default is 200 OK, if not provided.
    * Write rendered bytes.
  * `OnAfterReply` server extension point: Always called. Response is already written on the wire. Don't bother `ctx.Reply()`, however `ctx.Res` holds valuable information (bytes written, status). Except when-
    * Static file request.
    * `ctx.Reply().Done()` called, refer godoc for more info.
    * `ctx.Reply().Redirect(...)` is called.
