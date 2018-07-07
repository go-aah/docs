Title: Configuration Hot-Reload and Application Reinitialize
Desc: aah seamless configuration hot-reload and reinitialize for the aah application.
Keywords: config hot-reload, sighup, reload
---
# Configuration Hot-Reload and Application Reinitialize

aah provides seamless configuration hot-reload and application reinitialize via `SIGHUP` on non-`dev` environment profiles. It enables aah production application to adapt to on-demand, immediately required changes without server restart.

Hot-Reload and Reinitialize performs:

  * Reloads configuration from directory `config/*`
  * Reinitializes the application variables, routes, views, i18n, security and loggers
  * Reinitializes the access log and dump log if enabled

Any error occurs in the reload and reinitialize process will not affect running application.

<br>
<div class="alert alert-info-blue">
<p><strong>Note:</strong> Server related configuration changes such as address, port, timeouts, SSL certs, redirect http to https, let's encrypt requires server restart.</p>
</div>

### For example

```bash
$ kill -HUP <process-id>
```

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
