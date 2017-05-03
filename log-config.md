Title: aah Logging Configuration
Desc: aah log implements a simple, flexible & powerful logger. Currently it supports `console`, `file` (rotation by daily, size,  lines), logging receivers and logging stats. It also has a predefined 'standard' Logger accessible through helper functions `Error{f}`, `Warn{f}`, `Info{f}`, `Debug{f}`, `Trace{f}`.
Keywords: log, logger, aah logger, console log, file log, console logger, file logger
---
# aah Logging Configuration

aah log implements a simple, flexible & powerful logger. Currently it supports `console`, `file` (rotation by daily, size,  lines), logging receivers and logging stats. It also has a predefined 'standard' Logger accessible through helper functions `Error{f}`, `Warn{f}`, `Info{f}`, `Debug{f}`, `Trace{f}` which are easier to use than creating a Logger manually. That logger writes to standard error and prints log `Entry` details as per `DefaultPattern`.

```go
log.Info("Welcome ", "to ", "aah ", "logger")
log.Infof("%v, %v, & %v", "simple", "flexible", "powerful logger")
```

```bash
# Output:
2017-02-03 19:22:11.504 INFO  - Welcome to aah logger
2017-02-03 19:22:11.504 INFO  - simple, flexible, & powerful logger
```

Reference to [App Config](app-config.html), [Routes Config](routes-config.html), [Security Config](security-config.html).

## receiver
Receiver is where is log values gets logged. Currently framework supports `console` and `file` as a receivers.

Default value is `console`.
```bash
receiver = "file"
```

## level
Level indicates the logging levels like `ERROR`, `WARN`, `INFO`, `DEBUG` and `TRACE`. Config value can be in lowercase or uppercase.

Default value is `debug`.
```bash
level = "info"
```

## pattern
Pattern config defines the message flags and formatting while logging into receivers. aah framework logger supports following `format flags`-
```bash
Format flags: Usage of flag order is up to your pattern composition in the config.
  level     - outputs INFO, DEBUG, ERROR, so on
  time      - outputs local time as per format supplied
  utctime   - outputs UTC time as per format supplied
  longfile  - outputs full file name: /a/b/c/d.go
  shortfile - outputs final file name element: d.go
  line      - outputs file line number: L23
  message   - outputs given message along supplied arguments if they present
  custom    - outputs string as-is into log entry

The usage is %flagname:formating
```

Default value is `%time:2006-01-02 15:04:05.000 %level:-5 %custom:- %message`.
```bash
pattern = "%time:2006-01-02 15:04:05 %level:-5 %shortfile %line %custom:- %message"
```

## file
File config attribute is applicable only to `file` receiver type.

Default value is `aah-log-file.log`.
```bash
file = "myapp.log"
```

## Section: rotate { ... }
Rotate config section is applicable only to **`file`** receiver type.

### mode
Mode is used to determine rotate mode. Currently it supports `daily`, `lines` and `size`.

Default value is `daily`.
```bash
# daily rotation, it's default one.
rotate {
  mode = "daily"
}

# size based rotation
rotate {
  mode = "size"
  size = 500
}

# line based rotation
rotate {
  mode = "lines"
  lines = 100000
}
```

### size
This is applicable only to if `mode` is `size`.

Default value is 100MB.
```bash
# value is in MB, it is 500MB
size = 500
```

### lines
This is applicable only to if `mode` is `lines`.

Default value is unlimited.
```bash
lines = 100000
```
