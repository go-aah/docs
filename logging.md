Title: aah Logging
Desc: aah provides simple logger for real world application needs. It supports context, fields, `console`, `file` receivers, unified logger, child logger and drop-in replacement for standard logger.
Keywords: logging, logger, aah logger, console logger, file logger
---
# aah Logging

aah provides simple logger for real world application needs. It supports context, fields, `console`, `file` receivers, unified logger, child logger and drop-in replacement for standard logger.

### Table of Contents

  * [Understanding aah Logging](#understanding-aah-logging)
  * [Context, Fields, Child Logger](#context-fields-child-logger) <span class="badge lb-xs">Since v0.9.0</span>
  * [Human Readable and Machine Parsable Logging](#human-readable-and-machine-parsable-logging)
  * [Unified Logging](#unified-logging)
  * [Logger Hook](#logger-hook)
  * [Log Configuration](log-config.html)

<br>
Learn about [Server Access Log](server-access-log.html) and [Server Dump Log](server-dump-log.html).

## Understanding aah Logging

aah initializes the application default logger based on configured values from `log { ... }` with context of Application Name and Application Instance Name.

Simply use Log methods -

  * `ctx.Log()` - available on Controller : it log entry with context information such as `appname`, `insname`, `reqid`, `principal`.
  * `aah.App().Log()` - application logger : use it for non request scenario's.

```go
func (a *AppController) MyAction(id string, info *models.MyModel) {

  // simply log it
  a.Log().Info("Welcome ", "to ", "aah ", "logger")
  a.Log().Infof("This is %s application logger", "example")

}
```

### Composable Log Pattern

aah provides composable log pattern as follows. Note: log pattern is not applicable for JSON log. <span class="badge lb-sm">Since v0.9.0</span> `appname`, `insname`, `reqid`, `principal` flags supported.

Refer [Log Configuration#Pattern](log-config.html#pattern)

<div class="alert alert-info-green">
<p><strong>Tip:</strong></p>
<ul>
  <li>Application instance name (flag <code>insname</code>) is a key information on aah application cluster setup.</li>
  <li>aah supports log levels (<code>IsLevelTrace()</code>, <code>IsLevelDebug()</code>, etc.), so conditionally performing additional logging is possible without affecting performance.</li>
</ul>
</div>

## Context, Fields, Child Logger

<span class="badge lb-sm">Since v0.9.0</span> context, fields and child logger is supported.

  * **Context:** You could create logger with context information via `log.Fields` values. Those values gets logged with every log entry.
      * `aah.App().Log()` - creates log entry with application name and application instance name
      * `ctx.Log()` - incoming request creates log entry with `request id`, `user principal` plus application context information such as `app name` and `app instance name`
  * **Fields:** `WithFields` and `WithField` is used to log fields and values with current log entry.
  * **Child Logger:** You can create child logger from existing aah logger instance. Upon creation it inherits parent logger context values once.
      - After creation, any changes to context value on parent or child does not affect each other.
      - All the log entries made by child logger; logged in parent receiver.
      - Child logger is handy when you want have logger per functionality/package/file, etc. 
          - For example: you could create new child logger via `aah.App().NewChildLogger(fields)`.

## Human Readable and Machine Parsable Logging

aah supports both human readable and machine parseable log format.

### Text log format

```bash
format = "text"
```

<br> **Example**

```
2017-10-01 23:41:05.983 INFO  aahwebsite sfo-aahweb-01 aah go server running on :8080
2017-10-01 23:41:10.337 DEBUG aahwebsite sfo-aahweb-01 59d1df869bf2340ffaab2e28 Calling interceptor: DocController.Before
2017-10-01 23:41:10.338 DEBUG aahwebsite sfo-aahweb-01 59d1df869bf2340ffaab2e28 Calling controller: DocController.ShowDoc
2017-10-01 23:41:26.845 WARN  aahwebsite sfo-aahweb-01 Interrupt signal received
2017-10-01 23:41:26.846 DEBUG aahwebsite sfo-aahweb-01 Event [OnShutdown] publishing in synchronous mode
2017-10-01 23:41:26.846 INFO  aahwebsite sfo-aahweb-01 aah application shutdown successful
```

### JSON log format

```bash
format = "json"
```

<br> **Example**

```json
{"level":"INFO","timestamp":"2017-10-01T23:41:52-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"aah go server running on :8080"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:00-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","request_id":"59d1dfb89bf234103a1b35c2","message":"Calling interceptor: DocController.Before"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:00-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","request_id":"59d1dfb89bf234103a1b35c2","message":"Calling controller: DocController.ShowDoc"}
{"level":"WARN","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"Interrupt signal received"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"Event [OnShutdown] publishing in synchronous mode"}
{"level":"INFO","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"aah application shutdown successful"}
```

## Unified Logging

Typically aah user could be using many libraries with aah. Those might provide standard Go logger for logging. This is where unified logging shines.

Use `log.ToGoLogger()` to bind that logger with aah logger then you get unified logging at one place.

## Logger Hook

Even though aah provides out-of-the-box JSON log format. You might need further extensibility to send your log entries to centralized logging system such as Kibana, Loggly, Splunk, etc.

Use `Hook` capability to register custom hooks. Hooks are executed in a goroutine and each hook receives log entry.

**Example:** Adding hook into aah application logger.

```go
// Create logger Hook as type of `log.HookFunc`
func kibanaHook(e log.Entry) {
  // logger hook logic goes here.
}

func RegisterLoggerHooks(_ *aah.Event)  {
  aah.App().AddLoggerHook("kibana", kibanaHook)
  // so on ...
}

// Register it on <app-base-dir>/app/init.go file
func init() {
  aah.App().OnStart(myloggerhook.RegisterLoggerHooks)  
}
```
