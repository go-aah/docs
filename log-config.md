Title: aah Logging Configuration
Desc: aah log implements a simple, flexible, non-blocking logger. Tt supports `console`, `file` (rotation by daily, size,  lines), max history (`upcoming`). It also has a predefined 'standard' Logger. It can be used as drop-in replacement for standard go logger with features.
Keywords: log, logger, aah logger, console log, file log, console logger, file logger
---
# aah Logging Configuration

aah log package implements a simple, flexible, non-blocking logger. It supports `console`, `file` (rotation by daily, size, lines), max history (`upcoming`). It also has a predefined 'standard' Logger accessible through helper functions `Error{f}`, `Warn{f}`, `Info{f}`, `Debug{f}`, `Trace{f}`, `Print{f,ln}`, `Fatal{f,ln}`, `Panic{f,ln}` which are easier to use than creating a Logger manually. Default logger writes to standard error and prints log `Entry` details as per `DefaultPattern`.

aah log package can be used as drop-in replacement for standard go logger with features.

```go
log.Info("Welcome ", "to ", "aah ", "logger")
log.Infof("%v, %v, %v", "simple", "flexible", "non-blocking logger")
```

```bash
# Output:
2017-05-03 19:22:11.504 INFO  - Welcome to aah logger
2017-05-03 19:22:11.504 INFO  - simple, flexible, non-blocking logger
```

Reference to [App Config](app-config.html), [Routes Config](routes-config.html), [Security Config](security-config.html).

## receiver
Receiver is where the log values gets logged. Currently framework supports `console` and `file` receivers. And you can add `Hooks` into aah logger, for sending log data to splunk, kibana, etc.

Default value is `console`.
```cfg
receiver = "file"
```

## level
Level indicates the logging levels like `ERROR`, `WARN`, `INFO`, `DEBUG` and `TRACE`. Config value can be in lowercase or uppercase.

Default value is `debug`.
```cfg
level = "info"
```

## format
To define log entry output format. Supported formats are `text` and `json`.

Default value is `text`.
```cfg
format = "json"
```

## pattern
Pattern config defines the message flags and formatting while logging into receivers. aah framework logger supports the following `format flags`-
```cfg
Format flags: Usage of flag order is up to your pattern composition in the config.
  level     - outputs INFO, DEBUG, ERROR, so on
  time      - outputs local time as per format supplied
  utctime   - outputs UTC time as per format supplied
  longfile  - outputs full file name: /a/b/c/d.go (do not use for production)
  shortfile - outputs final file name element: d.go (do not use for production)
  line      - outputs file line number: L23 (do not use for production)
  message   - outputs given message along supplied arguments if they present
  custom    - outputs string as-is into log entry

The usage is %flagname:format
```

***Note: Pattern is not is applicable for JSON format.***

Default value is `%time:2006-01-02 15:04:05.000 %level:-5 %message`.
```cfg
pattern = "%time:2006-01-02 15:04:05 %level:-5 %shortfile %line %custom:- %message"
```

## file
File config attribute is applicable only to **`file`** receiver type.

Default value is `aah-log-file.log`.
```cfg
file = "myapp.log"
```

## Section: rotate { ... }
Rotate config section is applicable only to **`file`** receiver type.

### policy
Policy is used to determine rotate policy. Currently it supports `daily`, `lines` and `size`.

Default value is `daily`.
```cfg
# daily rotation, it's default one.
rotate {
  policy = "daily"
}

# size based rotation
rotate {
  policy = "size"
  size = "512mb"
}

# line based rotation
rotate {
  policy = "lines"
  lines = 100000
}
```

### size
This is applicable only to if **`policy`** is `size`.

Default value is 512mb.
```cfg
size = "100mb"
```

### lines
This is applicable only to if **`policy`** is `lines`.

Default value is 100000.
```cfg
lines = 50000
```
