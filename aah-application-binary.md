Title: aah Application Binary
Desc: Learn more about aah application binary capabilities; such as run flags, hot-reload without restart, cross compile build and artifact naming convention.
Keywords: aah binary, aah app binary, flags, hot-reload, without restart, cross compile, artifact, artifact naming
---
# aah Application Binary

This document describes the aah application binary capabilities and its artifact details.

<div class="alert alert-info-green">
<p><strong>News:</strong> Since <code>v0.11.0</code> aah supports single and non-single binary build artifact packaging.</p>
</div>

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Go binaries have no runtime dependencies (such as Go installation, GOPATH, libraries, etc.), once a binary is built for the targeted Operating System (OS), it runs on any machine of such targeted OS.</p>
<p><u>For example:</u></p>
<p>Building aah binary for Linux 64-bit OS - <code>env GOOS=linux GOARCH=amd64 aah build &lt;args></code></p>
</div>


### Table of Contents

  * [App Binary Help](#app-binary-help)
  * [Command Input Flags](#command-input-flags)
  * [Start](#start)
  * [Stop](#stop)
  * [Reload](#reload)
  * [Know your Embedded Files](#know-your-embedded-files) <sup>for aah single binary</sup>
  * [Cross Compile Build](#cross-compile-build)
  * [Build Artifact Naming Convention](#build-artifact-naming-convention)

## App Binary Help

<span class="badge lb-sm">Since v0.12.0</span> aah application binary evolved further. It provides powerful [console commands](console-commands.html) with POSIX flags support.

```bash
jeeva@pmb-pro:~$ thumbai --help
Name:
  thumbai - A Go Mod Repository, Go Vanity and Simple Proxy Server

Usage:
  thumbai [global options] command [command options] [arguments...]

Version:
  1.0.0-beta

Build Timestamp:
  2018-11-23T00:42:32-08:00

Description:
  THUMBAI provides easy to use Go Mod Repository, Go Vanity and Simple Proxy Server

Commands:
  run, r   Runs application server
  vfs, v   Provides access to app VFS instance to interact with it
  help, h  Shows a list of commands or help for one command

Global Options:
  --help, -h     Shows app help
  --version, -v  Print app build information

Copyright:
  © Jeevanandam M. (https://github.com/jeevatkm), All rights reserved.
```

## Command Input Flags

<span class="badge lb-sm">Since v0.12.0</span> Application binary comes with one more commands with respective input flags. Explore it via command help.

**For example: command `run`**

```bash
jeeva@pmb-pro:~$ thumbai help run
Name:
  thumbai run - Runs application server

Usage:
  thumbai run [command options] [arguments...]

Description:
  Runs application server.

Options:
  --envprofile value, -e value  Environment profile name to activate (e.g: dev, qa, prod) (default: "dev")
  --config FILE, -c FILE        External config FILE for adding or overriding 'config/**/*.conf' values
```

## Start

<span class="badge lb-sm">Since v0.11.0</span> [No intermediate script](/v0.10/anatomy-aah-application.html#packaged-aah-application-will-have-following-directories-files) to start, stop, etc.

To start aah application, simply use the binary with command.

**For example**

```bash
$ /home/app/thumbai/bin/thumbai run --envprofile prod --config /home/app/thumbai/thumbai.conf
```

## Stop

aah application binary listens to `SIGINT` and `SIGTERM` OS signal. On receiving these signals:

  * Application publishes `OnPreShutdown` server extension event.
  * Application performs the graceful shutdown with timeout of `server.timeout.grace_shutdown` from `aah.conf`.
  * Application publishes `OnPostShutdown` server extension event.

```bash
$ kill -TERM <process-id>
# OR
$ kill -INT <process-id>
```

## Reload

<span class="badge lb-sm">Since v0.10.0</span> aah application binary supports `Hot-Reload` via `SIGHUP`. Basically the app performs the same steps as `Start` but reloading config, i18n, views, etc. without actually restarting itself.

```bash
$ kill -HUP <process-id>
# OR if you have configured application via systemd
$ systemctl reload thumbai.service   # service thumbai reload 
```

Application logs inform of this reload process. Example below:

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

## Know your Embedded Files

<span class="badge lb-sm">Since v0.11.0</span> aah supports single binary build packaging.

aah provides command `vfs` and its sub-commands to explore files embedded into the application binary. It accepts regular expression as an input via flag `pattern` and will print the file/directory path which matches the given regex pattern.

**Idea behind command `vfs`**: Typically once the single binary is built, there would be no easy way to know which files have been embedded into the binary. In a real world scenario, it's important to have some mechanism to find out the contents. It could also be helpful for developers, devops, etc.

<br>

**Help: vfs sub-command `find`**

For regex syntax refer to https://golang.org/pkg/regexp or run `go doc regexp/syntax` on terminal.

```bash 
jeeva@pmb-pro:~$ thumbai vfs help find
Name:
  thumbai vfs find - Finds the embedded file/directory path that matches the given regex pattern

Usage:
  thumbai vfs find [command options] [arguments...]

Description:
  Finds the embedded file/directory path that matches the given regex pattern.

    Example:
      <app-binary> vfs find --pattern "conf$"

Options:
  --pattern value, -p value  Regex pattern to find the files in the app VFS instance
```

<br>

**Example: vfs sub-command `find`**

For regex syntax refer to https://golang.org/pkg/regexp or run `go doc regexp/syntax` on terminal.

```bash
jeeva@pmb-pro:~$ thumbai vfs find --pattern "conf"
drwxr-xr-x      0B Oct  4 18:18:00 /app/config
-r--r--r--   5.6KB Nov 23 08:41:06 /app/config/aah.conf
drwxr-xr-x      0B Oct 12 02:35:11 /app/config/env
-r--r--r--    867B Oct 12 02:41:41 /app/config/env/dev.conf
-r--r--r--   1.1KB Oct 15 01:35:54 /app/config/env/prod.conf
-r--r--r--   8.4KB Nov  3 21:56:04 /app/config/routes.conf
-r--r--r--   3.3KB Sep 18 06:00:45 /app/config/security.conf
```

## Cross Compile Build

Since `go1.5` we can build cross platform build easily. aah framework supports it, refer [Cross Compile Build](aah-cli-tool.html#cross-compile-build)

## Build Artifact Naming Convention

aah build produces the build artifact name as `<app-binary-name>-<version>-<goos>-<goarch>.zip`. Also you can supply your custom name for the artifact via `--output` or `-o`.

For e.g.: `aahwebsite-381eaa8-darwin-amd64.zip`
