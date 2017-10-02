Title: aah Logging
Desc: aah provides simple, flexible logger for real world application needs. It supports Context, Fields, `console`, `file` receivers, Unified logger, child logger and drop-in replacement for standard logger.
Keywords: logging, logger, aah logger, console logger, file logger
---
# aah Logging

aah provides simple, flexible logger for real world application needs. It supports context, fields, `console`, `file` receivers, unified logger, child logger and drop-in replacement for standard logger.

  * [Understanding aah Logging](#understanding-aah-logging)
  * [Context, Fields, Child Logger](#context-fields-child-logger) <span class="badge lb-xs">Since v0.9</span>
  * [Human Readable and Machine Parsable Logging](#human-readable-and-machine-parsable-logging)
  * [Unified Logging](#unified-logging)
  * [Send Log to Centralized Logging System](#send-log-to-centralized-logging-system)
  * [Drop-in replacement for Go Standard Logger](#drop-in-replacement-for-go-standard-logger)
  * [Log Configuration](log-config.html)

<br>
Reference to [Server Access Log](server-access-log.html), [Server Dump Log](server-dump-log.html).

## Understanding aah Logging

aah initializes the application default logger based on configured values from `log { ... }` with context of Application Name and Application Instance Name. You can simple import `log` module anywhere you need and log it. All the log entries are sent to configured receiver.

```go
import "aahframework.org/log.v0"

// simply log it
log.Info("Welcome ", "to ", "aah ", "logger")
log.Infof("This is %s application logger", "example")
```

#### Composable Log Pattern

aah provides composable log pattern as follows. Note: log pattern is not applicable of JSON log format.

<span class="badge lb-sm">Since v0.9</span> `appname`, `insname`, `reqid`, `principal` flags supported.

<div class="alert alert-info-green">
<p><strong>Pro Tip:</strong></p>
<p>Application instance name (flag <code>insname</code>) is a key information when you have aah application cluster setup.</p>
</div>

```cfg
FmtFlags is the list of log format flags supported by aah log library
Usage of flag order is up to format composition.
  level     - outputs ERROR, WARN, FATAL, PANIC, INFO, DEBUG, TRACE
  appname   - outputs Application Name
  insname   - outputs Application Instance Name
  reqid     - outputs Request ID from HTTP header
  principal - outputs Logged-In subject primary principal value
  time      - outputs local time as per format supplied
  utctime   - outputs UTC time as per format supplied
  longfile  - outputs full file name: /a/b/c/d.go
  shortfile - outputs final file name element: d.go
  line      - outputs file line number: L23
  message   - outputs given message along supplied arguments if they present
  fields    - outputs field values into log entry
  custom    - outputs string as-is into log entry
```
<br>
**aah Application Default Log Pattern**
```cfg
%time:2006-01-02 15:04:05.000 %level:-5 %appname %insname %reqid %principal %message %fields
```

#### Sample: Text log format
<pre>
2017-10-01 23:41:05.983 INFO  aahwebsite sfo-aahweb-01 aah go server running on :8080
2017-10-01 23:41:10.337 DEBUG aahwebsite sfo-aahweb-01 59d1df869bf2340ffaab2e28 Calling interceptor: DocController.Before
2017-10-01 23:41:10.338 DEBUG aahwebsite sfo-aahweb-01 59d1df869bf2340ffaab2e28 Calling controller: DocController.ShowDoc
...
...
2017-10-01 23:41:26.845 WARN  aahwebsite sfo-aahweb-01 Interrupt signal received
2017-10-01 23:41:26.846 DEBUG aahwebsite sfo-aahweb-01 Event [OnShutdown] publishing in synchronous mode
2017-10-01 23:41:26.846 INFO  aahwebsite sfo-aahweb-01 aah application shutdown successful
</pre>

#### Sample: JSON log format
<pre>
{"level":"INFO","timestamp":"2017-10-01T23:41:52-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"aah go server running on :8080"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:00-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","request_id":"59d1dfb89bf234103a1b35c2","message":"Calling interceptor: DocController.Before"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:00-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","request_id":"59d1dfb89bf234103a1b35c2","message":"Calling controller: DocController.ShowDoc"}
...
...
{"level":"WARN","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"Interrupt signal received"}
{"level":"DEBUG","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"Event [OnShutdown] publishing in synchronous mode"}
{"level":"INFO","timestamp":"2017-10-01T23:42:50-07:00","app_name":"aahwebsite","instance_name":"sfo-aahweb-01","message":"aah application shutdown successful"}
</pre>

#### Supported Log levels

  * ERROR
  * WARN
  * FATAL
  * PANIC
  * INFO
  * DEBUG
  * TRACE

<div class="alert alert-info-green">
<p><strong>Pro Tip:</strong></p>
<p>In <code>aah</code> you could conditionally perform extra logging on certain log level. It could reduce your processing time for some use case. For example: Do extra processing and log only on level <code>DEBUG</code> using <code>IsLevelDebug()</code>; that particular block for DEBUG level only.</p>
</div>

## Context, Fields, Child Logger

<span class="badge lb-sm">Since v0.9</span> context, fields and child logger supported.

  * **Context:** In aah, you can create logger with context, which is `log.Fields` values. Those values gets logged with every log entry. For example: aah application creates the default logger with application name and application instance name.
      - Use `ctx.Log().*` for logging in the controller and further to get context based logging which includes log entry with Request ID and Primary Principal value.
  * **Fields:** `WithFields` and `WithField` is used to log fields and values with current log entry.
  * **Child Logger:** You can create child logger from aah logger instance. Upon creation it inherits parent logger context values once.
      - After creation, any changes to context value on parent or child does not affect each other.
      - All the log entries made by child logger; logged in parent receiver.
      - Child logger is handy when you want have logger per functionality/package/file, etc. For example: you could create new application child logger via `aah.NewChildLogger(fields)`.

## Human Readable and Machine Parsable Logging

aah supports both human readable and machine parseable log format.

**Human Readable:** This is default log format called `text`.

**Machine Parsable:** This log format is called `json`. Per log entry is in JSON format per line. For example: You can use `Telegraf` tool from InfluxDB to send log entry to InfluxDB. Also refer to section [Send Log to Centralized Logging System](#send-log-to-centralized-logging-system)

## Unified Logging

Surely you will be using many libraries with aah framework. Those might provide standard Go logger for logging. This is where unified logging shines.

Use `log.ToGoLogger()` to bind that logger with aah logger then you get unified logging at one place.

## Send Log to Centralized Logging System

Even though aah provides out-of-the-box JSON log format. You might need further extensibility to send your log entries to centralized logging system such as Kibana, Loggly, Splunk, etc.

Use `Hook` capability to register your hooks. Hooks are executed in goroutine and each hook receives log entry.

**For example:** Adding hook into aah application default logger.
```go
// Create your logger Hook as type of `log.HookFunc`
func kibanaHook(e log.Entry) {
  // Your logger hook logic goes here.
}

func registerLoggerHooks(_ *aah.Event)  {
  aah.AddLoggerHook("kibana", kibanaHook)
  // so on ...
}

func init() {
  aah.OnStart(registerLoggerHooks)  
}
```

## Drop-in replacement for Go Standard Logger

aah provides drop-in replacement for Go standard Logger. You just import `aahframework.org/log.v0` it.
