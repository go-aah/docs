Title: aah init.go file
Desc: init.go - aah application customization and initialization file.
Keywords: init.go, application customization, application initialization, aah
---
# aah init.go file

Introduced in <span class="badge lb-sm">v0.10.0</span>. aah brought `init.go` file to evolve towards the next step.

<div class="alert alert-info-green">
<p><strong>Best Practice:</strong> Define extensions and custom implementations in an appropriate package and register it in <code>init.go</code>. It brings a more flexible, organized and maintainable aah application. It is located at <code>&lt;app-base-dir&gt;/app/init.go</code>.</p>
</div>


**Register -**

  * Server Extensions Points
      - Application Events
      - Subscribe to HTTP Events
      - Subscribe to WebSocket Events
  * Middleware's
  * Application Error Handler
  * Custom Template Functions
  * Custom Session Store
  * Custom Value Parser - Auto bind
  * Custom Validation Functions
  * Custom View Engine
  * etc.

Create an application and have a look at the file `app-base-dir/app/init.go` to view a sample.
