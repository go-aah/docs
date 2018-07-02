Title: Frequently Asked Questions (FAQ)
Desc: aah framework Frequently Asked Questions (FAQ)
Keywords: faq, aah, aah Go web framework
---
# Frequently Asked Questions (FAQ)
  
  * [How to update aah to the latest version?](#how-to-update-aah-to-the-latest-version)
  * [Does aah support Package Management Tools?](#does-aah-support-package-management-tools)
  * [How to adapt to latest aah configuration?](#how-to-adapt-to-latest-aah-configuration)
  * [How to try aah edge version?](#how-to-try-aah-edge-version)
  * [How to log all goroutine stacktrace?](#how-to-log-all-goroutine-stacktrace)
  * [Does aah has benchmark against other Go web framework?](#does-aah-has-benchmark-against-other-go-web-framework)
  * [Does aah supports Hot-Reload for Development?](#does-aah-supports-hot-reload-for-development)
  * [Is posting an arbitrary CSRF token pair (cookie and POST data) a vulnerability?](#is-posting-an-arbitrary-csrf-token-pair-cookie-and-post-data-a-vulnerability)
  * [Is it a problem that aah’s Anti-CSRF protection isn’t linked to a session?](#is-it-a-problem-that-aah-s-anti-csrf-protection-isn-t-linked-to-a-session)
  * [Why might a user encounter a Anti-CSRF validation failure after logging in?](#why-might-a-user-encounter-a-anti-csrf-validation-failure-after-logging-in)

### How to update aah to the latest version?

To update aah latest version run below command on your terminal/command prompt. This procedure updates the aah only on GOPATH.

If you're using package management tool, then refer to respective tool documentation for update. For example: `glide update`, etc.

```bash
# Since v0.10 release
aah update

# OR
go get -u aahframework.org/tools.v0/aah
```

### Does aah support Package Management Tools?

Yes, of course. As described in [versioning documentation](versioning.html#package-management). aah works seamlessly with pacakge manangement tool (like `glide`, `dep`, `govendor`, etc).

For example: I have responded to aah user for `dep` tool, refer to [GitHub comment]({{aah_issues_url}}/109#issuecomment-327225582).

### How to adapt to latest aah configuration?

The best way is to have a look at aah documentation https://docs.aahframework.org on respective configuration docs.

### How to try aah edge version?

Of-course you can. <span class="badge lb-sm">Since v0.9</span> `aah switch` command makes it very easy to try edge version. Learn more, run `aah help switch`.

Just run the below command and then run your app as usual using `aah run`:

```bash
# Switches to latest edge version
aah switch   

# Switching back to stable release version
aah switch --value release

# If you're already on aah edge version, 
# you could refresh to get latest edge codebase
aah switch --refresh
```

### How to log all goroutine stacktrace?

It is very simple to do in aah. Just set the below config to true.

```bash
runtime {
  debug {
    # Whether to collect all the Go routines details or not.
    # Default value is `false`
    all_goroutines = true
  }
}
```

### Does aah has benchmark against other Go web framework?

Well, aah goal is to achieve full stack web framework capabilities for modern Web, API and WebSocket applications.

I have responded to aah user on [Github Issue #81](https://github.com/go-aah/aah/issues/81#issuecomment-315589889), about what I have done. Thanks.

### Does aah supports Hot-Reload for Development?

Yes, aah detects the file change(s) on aah project then it automatically stops the server, builds it and start the server with updated codebase.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Static file and template file changes reflects without aah server restart; server is restarted only for Go Source code.</p>
</div>

### Is posting an arbitrary CSRF token pair (cookie and POST data) a vulnerability?

No, this is by design. Without a man-in-the-middle attack, there is no way for an attacker to send a Anti-CSRF token cookie to a victim's browser, so a successful attack would need to obtain the victim's browser's cookie via XSS or similar, in which case an attacker usually doesn't need CSRF attacks.

Some security audit tools flag this as a problem but as mentioned before, an attacker cannot steal a user's browser's Anti-CSRF cookie. "Stealing" or modifying your own token using Firebug, Chrome dev tools, etc. isn't a vulnerability.

### Is it a problem that aah's Anti-CSRF protection isn't linked to a session?

No, this is by design. Not linking CSRF protection to a session allows using the protection on sites such as a pastebin that allow submissions from anonymous users which don’t have a session.

### Why might a user encounter a Anti-CSRF validation failure after logging in?

For security reasons, Anti-CSRF tokens are rotated each time a user logs in. Any page with a form generated before a login will have an old, invalid Anti-CSRF token and need to be reloaded. This might happen if a user uses the back button after a login or if they login in a different browser tab.
