## Migration Guide

<div class="alert alert-info-blue">
<p><strong>FAQ:</strong>
  <ul>
    <li><a href="/faq.html#how-to-update-aah-framework-to-the-latest-version">How to update aah framework to the latest version?</a></li>
    <li><a href="/faq.html#how-to-adapt-to-latest-aah-configuration">How to adapt to latest aah configuration?</a></li>
  </ul>
</p>
</div>

### Breaking changes

#### routes.conf

`not_found { ... }` configuration section in `routes.conf` have been removed in-favor of [Centralized Error Handler](centralized-error-handler.html).

#### ahttp.Request

`Payload` field have been removed in-favor of [Auto parse and bind](/request-parameters-auto-bind.html) feature. Also `Body()` method is introduced in `v0.8` to get access to request body if need be.
