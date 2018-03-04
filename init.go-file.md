Title: aah init.go file
Desc: init.go - aah application customization and initialization file.
Keywords: init.go, application customization, application initialization, aah
---
# aah init.go file

<span class="badge lb-sm">Since v0.10</span> aah framework brought `init.go` file to evolves aah towards next step.

Define an implementations in an appropriate package and register it in `init.go`. It adds more flexibility, organized and  maintainable aah application. Its located at `<app-base-dir>/app/init.go`.

Register your -

  * Server Extensions
  * Middleware's
  * View engines
  * etc.

For example, have a look at [aah website init.go](https://github.com/go-aah/website/blob/master/app/init.go) file.
