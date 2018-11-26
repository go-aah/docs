Title: aah Log Configuration
Desc: aah log configuration
Keywords: log configuration, logger configuration, log config
---
# aah Log Configuration

This document describes aah log configurations. Typically log configuration done at environment profile level.

Learn about aah [configuration syntax](configuration.html).

### Table of Contents

  * [Pattern](#pattern)
  * [Configuration](#configuration)

## Pattern

List of aah supported log patterns. It used to compose log entry.

```
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

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Log pattern is not applicable to JSON log format.</p>
</div>

#### Example

```bash
pattern = "%time:2006-01-02 15:04:05 %level:-5 %shortfile %line %custom:- %message"
```

## Configuration

`log { ... }` section configuration goes in env/dev.conf (`dev { ... }`), env/prod.conf (`prod { ... }`), etc.

```bash
# -----------------------------------------------------------------------------
# Log Configuration
#
# Doc: https://docs.aahframework.org/logging.html
# Doc: https://docs.aahframework.org/log-config.html
# -----------------------------------------------------------------------------
log {
  # Receiver is used to configure where is log values gets logged to. 
  # aah supports `console` and `file` receivers. Hooks for extension.
  #
  # Default value is `console`.
  receiver = "file"

  # Level indicates the logging levels like `ERROR`, `WARN`, `INFO`, `DEBUG`,
  # `TRACE`, FATAL and PANIC. Config value can be in lowercase or uppercase.
  #
  # Default value is `debug`.
  level = "warn"

  # Format is to define log entry output format. Supported formats are `text` and `json`.
  #
  # Default value is `text`.
  format = "json"

  # Pattern is used to configure log entry format via receivers
  # into receivers. Learn more about flags
  # and format - https://docs.aahframework.org/log-config.html#pattern
  #
  # Default value is `%time:2006-01-02 15:04:05.000 %level:-5 %appname %insname %reqid %principal %message %fields`
  pattern = "%time:2006-01-02 15:04:05.000 %level:-5 %appname %insname %reqid %principal %message %fields"

  # File attribute is applicable only to `file` receiver type.
  #
  # Default value is `aah-log-file.log`.
  file = "appname.log"

  # Rotate config section is applicable only to `file` receiver type.
  #
  # Default rotation is 'daily'.
  rotate {
    # Policy is used to determine rotate policy. aah supports `daily`,
    # `lines` and `size` policies.
    #
    # Default value is `daily`.
    policy = "daily"

    # This is applicable only to if `mode` is `size`.
    #
    # Default value is 100MB.
    #size = 500

    # This is applicable only to if `mode` is `lines`.
    #
    # Default value is unlimited.
    #lines = 100000
  }
}
```

### Example of configuring Rotation Policy

```bash
# daily rotation, it's default one.
rotate {
  policy = "daily"
}
```

```bash
# size based rotation
rotate {
  policy = "size"
  size = "512mb"
}
```

```bash
# line based rotation
rotate {
  policy = "lines"
  lines = 100000
}
```
