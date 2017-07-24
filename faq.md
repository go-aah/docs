Title: Frequently Asked Questions (FAQ)
Desc: aah framework Frequently Asked Questions (FAQ)
Keywords: faq, aah framework
---
# Frequently Asked Questions (FAQ)

  * [How to update aah framework to the latest version?](#how-to-update-aah-framework-to-the-latest-version)
  * [How to use aah framework before the version release?](#how-to-use-aah-framework-before-the-version-release)
  * [How to log all goroutine stacktrace?](#how-to-log-all-goroutine-stacktrace)
  * [Does aah has benchmark against other Go web framework?](#does-aah-has-benchmark-against-other-go-web-framework)
  * [Does aah supports Hot-Reload for Development?](#does-aah-supports-hot-reload-for-development)


### How to update aah framework to the latest version?

To update aah framework latest version run below command on your terminal/command prompt.
```
go get -u aahframework.org/tools.v0/aah
```
If you're using package management tool, then refer to respective tool documentation for update. For example: `glide update`, etc.


### How to use aah framework before the version release?

Of-course you can. Just run below command.
```
go get -u aahframework.org/tools.v0-unstable/aah
```

Refer every package with `-unstable` as suffix:
```go
// For example:
import "aahframework.org/aah.v0-unstable"
```

### How to log all goroutine stacktrace?

It is very simple to do aah framework. Just set the below config to true. You will get all the stacktrace in the log.
```
runtime {
  debug {
    # Whether to collect all the Go routines details or not.
    # Default value is `false`
    all_goroutines = true
  }
}
```

### Does aah has benchmark against other Go web framework?

Well, aah framework goal is to achieve full stack web framework capabilities for modern Web & API application. As you know we have many micro frameworks and few full stack in the Go community with own set of goals and own perspectives. Gradually I will add benchmark against other framework with perspective.

I have responded to aah user on [Github Issue #81](https://github.com/go-aah/aah/issues/81#issuecomment-315589889), about what I have done. Thanks.

### Does aah supports Hot-Reload for Development?

Yes, aah detects the file change(s) on aah project then it automatically stops the server, builds and start the server with updated codebase. I'm sure it needs an improvement, so keep me posted. Thanks.

<div class="alert alert-info alert-info-blue">
<p><strong>Note:</strong></p>
<p>For static file changes server is not restarted, server is restarted only for Go Source code and Template files. Refer to <a href="https://github.com/go-aah/aah/issues/4">Github Issue #4</a> for implementation details.</p>
</div>
