Title: aah Application Binary
Desc: Learn more about aah application binary internals, run flags, OS signals, cross compile build and artifact naming convention.
Keywords: aah binary, aah app binary, flags, os signals, cross compile, artifact, artifact naming
---
# Learn more about aah Application Binary

Page describe the aah application binary artifact details.

## Table of Contents

  * [Input Flags](#flags)
  * [Start/Stop script](#start-stop-script)
  * [OS Signals](#os-signals)
  * [Cross Compile Build](#cross-compile-build)
  * [Build Artifact Naming Convention](#build-artifact-naming-convention)

## Flags

Application binary supports following input flags.

  * `-version` - display the binary name, version and build date.
  * `-config` - absolute path of external configuration file, it gets merge into application config before the initialize.
  * `-profile` - environment profile name to activate. e.g: dev, qa, prod.

### version

`version` flag outputs the binary name, version, and build date. Also build information is available via `aah.AppBuildInfo()` method at runtime.

```bash
# For e.g.: application binary name is `aahwebsite`
aahwebsite -version

# output
Binary Name : aahwebsite
Version     : 381eaa8
Build Date  : 2017-04-25T21:00:45-07:00
```

### config

Supplying external configuration file. It gets merged into application configuration before the initialize.

```bash
aahwebsite -config /etc/aahwebsite/site.conf
```

### profile

Environment profile name to active on startup.

```bash
aahwebsite -profile qa
```

## Start/Stop script

aah build artifact contains two startup scripts named `aah.sh` is for `*NIX` and `aah.cmd` is for `Windows`. However you can create your own startup scripts as per your need with supported input [flags](#flags).

  * `*NIX` script supports `{start|stop|restart|version}`
  * `Windows` script supports `{start|stop|version}`

Note: `start` command accepts profile and config arguments.

```bash
# For e.g: `aah.sh` *NIX start script with arguments.
./aah.sh start qa
./aah.sh start qa /Users/jeeva/external-config.conf


# To stop
./aah.sh stop
```

## OS Signals

aah application binary listens to `SIGINT` and `SIGTERM` OS signal. On receiving these signals application performs the graceful shutdown with timeout of config value `server.timeout.grace_shutdown` from `aah.conf` and then fires the `OnShutdown` server event.

## Cross Compile Build

Since `go1.5` we can build cross platform build easily. aah framework supports it, refer [Cross Compile Build](aah-cli-tool.html#cross-compile-build)

## Build Artifact Naming Convention

aah build produces the build artifact name as `<app-binary-name>-<version>-<goos>-<goarch>.zip`. Also you can supply your custom name for the artifact via `-o` or `--output`.

For e.g.: `aahwebsite-381eaa8-darwin-amd64.zip`
