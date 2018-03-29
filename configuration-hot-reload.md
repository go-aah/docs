Title: Configuration Hot-Reload and Reinitialize
Desc: aah seamless configuration hot-reload and reinitialize for the aah application.
Keywords: config hot-reload, sighup, reload
---
# Configuration Hot-Reload and Reinitialize

aah provides seamless configuration hot-reload and reinitialize of aah application via `SIGHUP` on non-`dev` environment profiles. It enables aah production application to adapt to on-demand, immediately required changes on configuration, routes, security, view, i18n and logger without restart.

Hot-Reload and Reinitialize performs:

  * Reloads configuration from directory `config/*`
  * Reinitializes the application variables, routes, views, i18n, security and logger
  * Reinitializes the access log and dump log if enabled

Any error occurs in the reload and reinitialize process will not affect running application.

<br>
<div class="alert alert-info-blue">
<p><strong>Note:</strong> Server related configuration changes such as address, port, timeouts, SSL certs, redirect http to https, let's encrypt requires restart.</p>
</div>
