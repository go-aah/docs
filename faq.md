Title: Frequently Asked Questions (FAQ)
Desc: aah framework Frequently Asked Questions (FAQ)
Keywords: faq, aah framework, web framework for Go
---
# Frequently Asked Questions (FAQ)

  * [How to update aah framework to the latest version?](#how-to-update-aah-framework-to-the-latest-version)
  * [How to update individual module bug fix release?](#how-to-update-individual-module-bug-fix-release)
  * [How to adapt to latest aah configuration?](#how-to-adapt-to-latest-aah-configuration)
  * [How to use aah framework before the version release?](#how-to-use-aah-framework-before-the-version-release)
  * [How to log all goroutine stacktrace?](#how-to-log-all-goroutine-stacktrace)
  * [Does aah has benchmark against other Go web framework?](#does-aah-has-benchmark-against-other-go-web-framework)
  * [Does aah supports Hot-Reload for Development?](#does-aah-supports-hot-reload-for-development)
  * [Is posting an arbitrary CSRF token pair (cookie and POST data) a vulnerability?](#is-posting-an-arbitrary-csrf-token-pair-cookie-and-post-data-a-vulnerability)
  * [Is it a problem that aah’s Anti-CSRF protection isn’t linked to a session?](#is-it-a-problem-that-aah-s-anti-csrf-protection-isn-t-linked-to-a-session)
  * [Why might a user encounter a Anti-CSRF validation failure after logging in?](#why-might-a-user-encounter-a-anti-csrf-validation-failure-after-logging-in)


### How to update aah framework to the latest version?

To update aah framework latest version run below command on your terminal/command prompt.
```cfg
go get -u aahframework.org/tools.v0/aah
```
If you're using package management tool, then refer to respective tool documentation for update. For example: `glide update`, etc.

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

### Is posting an arbitrary CSRF token pair (cookie and POST data) a vulnerability?

No, this is by design. Without a man-in-the-middle attack, there is no way for an attacker to send a Anti-CSRF token cookie to a victim's browser, so a successful attack would need to obtain the victim's browser's cookie via XSS or similar, in which case an attacker usually doesn't need CSRF attacks.

Some security audit tools flag this as a problem but as mentioned before, an attacker cannot steal a user's browser's Anti-CSRF cookie. "Stealing" or modifying your own token using Firebug, Chrome dev tools, etc. isn't a vulnerability.

### Is it a problem that aah's Anti-CSRF protection isn't linked to a session?

No, this is by design. Not linking CSRF protection to a session allows using the protection on sites such as a pastebin that allow submissions from anonymous users which don’t have a session.

### Why might a user encounter a Anti-CSRF validation failure after logging in?

For security reasons, Anti-CSRF tokens are rotated each time a user logs in. Any page with a form generated before a login will have an old, invalid Anti-CSRF token and need to be reloaded. This might happen if a user uses the back button after a login or if they login in a different browser tab.
