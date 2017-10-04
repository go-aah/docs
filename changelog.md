## Changelog

#### Features

  * Anti-CSRF Protection, refer to [documentation](/anti-csrf-protection.html), [#115]({{aah_github_issues_url}}/115)
  * Server Dump Log - Request & Response, refer to [documentation](/server-dump-log.html), [#87]({{aah_github_issues_url}}/87)
  * `aah switch` command, refer to [documentation](/aah-cli-tool.html#command-switch), [#113]({{aah_github_issues_url}}/113)

#### Enhancements

  * Added `scrypt` and `pbkdf2` password encoders. Also you can add your custom password encoder too, refer to [documentation](/password-encoders.html), [#110]({{aah_github_issues_url}}/110)
  * Added HTTP -> HTTPS redirect, refer to [configuration](/app-config.html#section-server-ssl-redirect-http), [#112]({{aah_github_issues_url}}/112)
  * Context Based Logger (App Name, App Instance Name, Request ID, Primary Principal and Custom fields) and Child Logger, refer to [documentation](/logging.html), [#111]({{aah_github_issues_url}}/111)
  * Added option to disable color on console logging, refer to [configuration](/log-config.html#log-color) and [CLI configuration](/aah-project-file.html#section-log), [#108]({{aah_github_issues_url}}/108)
  * `aah new` command enhanced to create more customized Web and API application (security, etc)
  * Code improvements, Documentation, Website improvements.

#### Bug Fixes

  * Fixed Hot-Reload server 502 error on first request with Virtual Machine  [#120]({{aah_github_issues_url}}/120)
