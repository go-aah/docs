## Migration Guide

<div class="alert alert-info-blue">
<p><strong>FAQ:</strong>
  <ul>
    <li><a href="/faq.html#how-to-update-aah-framework-to-the-latest-version">How to update aah framework to the latest version?</a></li>
    <li><a href="/faq.html#how-to-adapt-to-latest-aah-configuration">How to adapt to latest aah configuration?</a></li>
  </ul>
</p>
</div>

### aah.conf

Time Format configuration enhanced to support more formats. So remove these two `date` and `datetime` config and add following config under `format { ... }`
```cfg
# Time format for auto parse and bind. aah tries to parse the
# time value in the order they defined till it gets success
# otherwise returns the error.
time = [
  "2006-01-02T15:04:05Z07:00",
  "2006-01-02T15:04:05Z",
  "2006-01-02 15:04:05",
  "2006-01-02"
]
```

### Breaking changes

#### routes.conf

`not_found { ... }` configuration section in `routes.conf` have been removed in-favor of [Centralized Error Handler](centralized-error-handler.html).

#### ahttp.Request

`Payload` field have been removed in-favor of [Auto parse and bind](/request-parameters-auto-bind.html) feature. Also `Body()` method is introduced in `v0.8` to get access to request body if need be.
