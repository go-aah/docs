Title: Frequently Asked Questions (FAQ)
Desc: aah framework Frequently Asked Questions (FAQ)
Keywords: faq, aah framework
---
# Frequently Asked Questions (FAQ)

## Table of Contents

  * How to update aah framework to latest version?
  * How to log all goroutine stacktrace?
  * Does aah framework has benchmark against other Go web framework?



### How to update aah framework to latest version?

To update aah framework run below command on your terminal/command prompt.
```
go get -u aahframework.org/tools.v0/aah
```
If you're using package management tool, then refer to respective tool documentation for update. For example: `glide update`, etc.

### How to log all goroutine stacktrace?

It is very simple to do aah framework. Just set the below config to true.
```
runtime {
  debug {
    # Whether to collect all the Go routines details or not.
    # Default value is `false`
    all_goroutines = true
  }
}
```

### Does aah framework has benchmark against other Go web framework?

Well, aah framework goal is to achieve full stack web framework capabilities for modern Web & API application. As you know we have many micro frameworks and few full stack in the Go community with own set of goals and perspectives. Gradually I will add benchmark against other framework with perspective.

I have responded to aah user on [Github Issue #81](https://github.com/go-aah/aah/issues/81#issuecomment-315589889), what I have done. Thanks.
