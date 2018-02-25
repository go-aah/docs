Title: aah init.go file
Desc: init.go - aah application customization and initialization file.
Keywords: init.go, application customization, application initialization, aah
---
# aah init.go file

<span class="badge lb-sm">Since v0.10</span> aah framework brought `init.go` file for application customization and initialization. Register your -

  * Server Extensions
  * Middleware's
  * View engines
  * etc.

Define your implementation in an appropriate package and register it at `init.go`. It adds more flexibility and meaningful way to organize your application. It is located at `<app-base-dir>/app/init.go`.

For example, have a look at [aah website init.go](https://github.com/go-aah/website/blob/master/app/init.go) file.
