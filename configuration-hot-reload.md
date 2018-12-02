Title: Configuration Hot-Reload and Application Reinitialize
Desc: aah provides seamless configuration hot-reload and reinitialize for non-dev profiles.
Keywords: config hot-reload, sighup, reload
---
# Configuration Hot-Reload and Application Reinitialize

aah provides seamless configuration hot-reload and application reinitialize via OS Signal on non-`dev` environment profiles. It enables aah application to adapt to on-demand, immediately required changes without server/application restart.

Hot-Reload and Reinitialize performs:

  * Reloads configuration from directory `config/*`.
  * Reinitializes the application variables, routes, views, i18n, security and loggers, etc.
  * Reinitializes the access log and dump log if enabled.
  * <span class="badge lb-sm">Since v0.12.0</span> Publishes `OnConfigHotReload` [event](#event-onconfighotreload), [configuration](#configuration).

<br>
<div class="alert alert-info-blue">
<p><strong>Note:</strong> 
<ul>
<li>Any error occurs in the reload and reinitialize process will not affect running application.</li>
<li>Server related configuration changes such as address, port, timeouts, SSL certs, redirect http to https, let's encrypt requires server restart.</li>
</p>
</div>

### For example

```bash
# *nix OS
$ kill -HUP <process-id>
```

## Configuration 

<span class="badge lb-sm">Since v0.12.0</span> Hot-Reload configurable options and it goes under config section `runtime { ... }`.

```bash
config_hotreload {
  # Enable or disable Hot-Reload feature.
  #
  # Default value is `true`.
  enable = true

  # OS Signal to listen for application hot-reload on non-dev environment
  # profile. Supported signals are `SIGHUP`, `SIGUSR1` and `SIGUSR2`.
  #
  # Default value is `SIGHUP`.
  #
  # Note: OS Windows does not support `SIGUSR1`, `SIGUSR2` signals.
  signal = "SIGUSR1"
}
```

## Event: OnConfigHotReload

Event `OnConfigHotReload` is published just after the successful reload and reinitialize of aah application. Any error or failure event is not published.

**Supports Multiple:** Yes

```go
func performActionAfterHotReload(e *aah.Event)  {
  // logic here
}

func init()  {
  // By default, a given function is executed as added sequence
  // unless `priority` is specified.
  aah.App().OnConfigHotReload(performActionAfterHotReload)
}
```

## Sample Application Log

Application logs information would appear similar to below-

```bash
2018-05-19 22:51:08.452 WARN  aahwebsite sfo-aahweb-01 Hangup signal (SIGHUP) received
2018-05-19 22:51:08.452 INFO  aahwebsite sfo-aahweb-01 Application hot-reload and reinitialization starts ...
2018-05-19 22:51:08.455 INFO  aahwebsite sfo-aahweb-01 Configuration files reload succeeded
2018-05-19 22:51:08.456 INFO  aahwebsite sfo-aahweb-01 Configuration values reinitialize succeeded
2018-05-19 22:51:08.456 INFO  aahwebsite sfo-aahweb-01 Logging reinitialize succeeded
2018-05-19 22:51:08.456 INFO  aahwebsite sfo-aahweb-01 I18n reinitialize succeeded
2018-05-19 22:51:08.457 INFO  aahwebsite sfo-aahweb-01 Router reinitialize succeeded
2018-05-19 22:51:08.564 INFO  aahwebsite sfo-aahweb-01 View engine reinitialize succeeded
2018-05-19 22:51:08.564 INFO  aahwebsite sfo-aahweb-01 Security reinitialize succeeded
2018-05-19 22:51:08.564 INFO  aahwebsite sfo-aahweb-01 Access logging reinitialize succeeded
2018-05-19 22:51:08.564 INFO  aahwebsite sfo-aahweb-01 Application hot-reload and reinitialization was successful
```
