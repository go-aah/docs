## Changelog

#### Features

  * Auto Parse and Bind - Controller action parameters, refer to [doc](/request-parameters-auto-bind.html), [#26]({{aah_github_issues_url}}/26).
  * Centralized Error Handler, refer to [doc](/centralized-error-handler.html), [#57]({{aah_github_issues_url}}/57).
  * Secure HTTP headers, refer to [configuration](/security-config.html#section-http-header), [#88]({{aah_github_issues_url}}/88).
  * Content Negotiation, refer to [configuration](/app-config.html#section-content-negotiation), [#75]({{aah_github_issues_url}}/75).
  * Max Request Body size at [global](/app-config.html#max-body-size) and [individual route](/routes-config.html#max-body-size) level [#83]({{aah_github_issues_url}}/83).
  * Added convenient method to save multipart uploaded file [#82]({{aah_github_issues_url}}/82).
  * Registering external JSON Library, refer to [doc](/external-json-library.html), [#79]({{aah_github_issues_url}}/79).

#### Enhancements

  * aah server header made configurable, refer to [config](/app-config.html#header), [#98]({{aah_github_issues_url}}/98).
  * aah application PID file path made configurable, refer to [config](/app-config.html#pid-file), [#99]({{aah_github_issues_url}}/99)
  * Custom Render implementation support added, refer to [doc](/reply.html#implementing-custom-rendering), [#103]({{aah_github_issues_url}}/103).
  * Code improvements, Documentation, Website improvements.

#### Bug Fixes

  * `i18n` fallback to default store when locale configuration file/key is not defined, [#97]({{aah_github_issues_url}}/97).
  * Hot-Reload with VirtualBox port forwarding issue, [#92]({{aah_github_issues_url}}/92).
