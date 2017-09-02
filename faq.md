Title: Frequently Asked Questions (FAQ)
Desc: aah framework Frequently Asked Questions (FAQ)
Keywords: faq, aah framework, web framework for Go
---
# Frequently Asked Questions (FAQ)

  * [How to update aah framework to the latest version?](#how-to-update-aah-framework-to-the-latest-version)
  * [How to use aah framework before the version release?](#how-to-use-aah-framework-before-the-version-release)
  * [How to update individual module bug fix release?](#how-to-update-individual-module-bug-fix-release)
  * [How to adapt to latest aah configuration?](#how-to-adapt-to-latest-aah-configuration)
  * [How to log all goroutine stacktrace?](#how-to-log-all-goroutine-stacktrace)
  * [Does aah has benchmark against other Go web framework?](#does-aah-has-benchmark-against-other-go-web-framework)
  * [Does aah supports Hot-Reload for Development?](#does-aah-supports-hot-reload-for-development)


### How to update aah framework to the latest version?

To update aah framework latest version run below command on your terminal/command prompt.
```cfg
go get -u aahframework.org/tools.v0/aah
```
If you're using package management tool, then refer to respective tool documentation for update. For example: `glide update`, etc.


### How to try aah framework before the release version?

Of-course you can. Just run below command.
```cfg
go get -u aahframework.org/tools.v0-unstable/aah
```

Refer every package with `-unstable` as suffix:
```go
// For example:
import "aahframework.org/aah.v0-unstable"
```

### How to update individual module bug fix release?

aah is modularized design and implementation. So any bug/enhancement can be fixed and released at individual module level without releasing entire framework. How do I update it:

Let's say `i18n` module is released bug fix:
```cfg
# Check the existing version
aah -v

# Run the module update
go get -u aahframework.org/i18n.v0

# Update the aah CLI
go install aahframework.org/tools.v0/aah

# Finally check the module version
aah -v
```

### How to adapt to latest aah configuration?

The quick and best way to know about latest configuration is to:

  * Create a new project based on your choice (web or api).
  * Compare your application configuration files (`config/*`) with new configurations then merge it to yours.
  * Congrats! you're on new configurations.

### How to log all goroutine stacktrace?

It is very simple to do aah framework. Just set the below config to true. You will get all the stacktrace in the log.
```cfg
runtime {
  debug {
    # Whether to collect all the Go routines details or not.
    # Default value is `false`
    all_goroutines = true
  }
}
```

### Does aah has benchmark against other Go web framework?

Well, aah framework goal is to achieve full stack web framework capabilities for modern Web & API application. As you know we have many micro frameworks and few full stack in the Go community with own set of goals and own perspectives.

I have responded to aah user on [Github Issue #81](https://github.com/go-aah/aah/issues/81#issuecomment-315589889), about what I have done. Thanks.

### Does aah supports Hot-Reload for Development?

Yes, aah detects the file change(s) on aah project then it automatically stops the server, builds it and start the server with updated codebase. I'm sure it needs an improvement, so keep me posted. Thanks.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>For static file changes server is not restarted, server is restarted only for Go Source code and Template files. Refer to <a href="https://github.com/go-aah/aah/issues/4">Github Issue #4</a> for implementation details.</p>
</div>
