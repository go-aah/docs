## Changelog

#### Features

  * Validator feature for route path params and struct fields [Form, JSON, XML] [documentation](/validation.html), [#132]({{aah_github_issues_url}}/132)
  * CORS (Cross-Origin Resource Sharing) [documentation](/cors.html), [#133]({{aah_github_issues_url}}/133)
  * Added Pug View Engine (formerly known as Jade), `aah new` command is enhanced to get view engine input [#151]({{aah_github_issues_url}}/151)
  * `aah generate` command is added to generate Dockerfile (dev & prod) and systemd service file [#134]({{aah_github_issues_url}}/134), [#135]({{aah_github_issues_url}}/135)
  * `aah update` command is added to update framework to latest version on GOPATH [#114]({{aah_github_issues_url}}/114)

#### Enhancements

  * Application configuration reload via signal `SIGHUP` with no-restart [documentation](/configuration-hot-reload.html), [#123]({{aah_github_issues_url}}/123)
  * Added `init.go` file for aah application to evolve aah [documentation](/init.go-file.html), [#142]({{aah_github_issues_url}}/142)
  * Use parent path and controller in child nodes when not defined [#137]({{aah_github_issues_url}}/137)
  * Controller level error handler [documentation](/error-handling.html), [#132]({{aah_github_issues_url}}/132)
  * Use `public_assets.dir` directory value for Static file mapping when `base_dir` is not defined [#141]({{aah_github_issues_url}}/141)
  * Error stacktrace presentation is improved to read easily; also added config `runtime.debug.strip_src_base` to strip base path [documentation](/error-handling.html#sample-easy-to-read-error-stacktrace), [#148]({{aah_github_issues_url}}/148)
  * aah framework website revamped - improved search capabilities, readability, new UI and nice logo
  * Code improvements and documentation
