## Migration Guide

<div class="alert alert-info-blue">
<p><strong>FAQ:</strong>
  <ul>
    <li><a href="/faq.html#how-to-update-aah-framework-to-the-latest-version">How to update aah framework to the latest version?</a></li>
    <li><a href="/faq.html#how-to-adapt-to-latest-aah-configuration">How to adapt to latest aah configuration?</a></li>
  </ul>
</p>
</div>

### Deprecated

<div class="alert alert-info-blue">
<p><strong>Please Note:</strong> Deprecated items will not break your functionality, planned to be removed in v1.0 release. However its good to adapt to latest changes.</p>
</div>

  * Method `AddServerTLSConfig` is deprecated, use method `aah.SetTLSConfig` instead.
  * Config `security.auth_schemes.<scheme-name>.password_encoder.type = "value"` is deprecated, use `security.auth_schemes.<scheme-name>.password_encoder = "value"` instead.
  * Config `build.log_level = "value"` is deprecated in `aah.project` file. Use `log.level = "value"` instead.
