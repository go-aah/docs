Title: aah init.go file
Desc: init.go - aah application customization and initialization file.
Keywords: init.go, application customization, application initialization, aah
---
# aah init.go file

<span class="badge lb-sm">Since v0.10</span> aah framework brought `init.go` file to evolves aah towards next step.

<div class="alert alert-info-green">
<p><strong>Best Practice:</strong> Define your implementations in an appropriate package and register it in <code>init.go</code>. It adds more flexibility, organized and  maintainable aah application. Its located at <code><app-base-dir>/app/init.go</code>.</p>
</div>


Register your -

  * Server Extensions
  * Middleware's
  * View engines
  * etc.

For example, have a look at [aah website init.go](https://github.com/go-aah/website/blob/master/app/init.go) file.
