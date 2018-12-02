Title: aah CLI Tool
Desc: aah CLI tool is development buddy for developers to take advantage aah capabilities.
Keywords: aah CLI tool, ClI tool, hot-reload, cross compile, commands, run, new, build, version, list, clean, generate, migrate, switch, update
---
# aah CLI Tool

Introduction to aah CLI tool, it's very handy tool for development, Continuous Integration (CI) and deployment preparation.

aah CLI is fully POSIX compliant flags (includes short and long versions) and command aliases too.

### Table of Contents

  * [Commands](#commands)
  * [Cross Compile Build](#cross-compile-build)
  * [vgo Support on-demand](#vgo-support)
  * [AAHPATH](#aahpath)

## Commands

<div class="alert alert-info-green">
<p><strong>Best Practice:</strong> For production use (aka <code>-profile prod</code>), it is recommended to follow build and deploy approach instead of using <code>aah run</code>.</p>
</div>

```cfg
jeeva@mb-pro:~$ aah
---------------------------------------------------------------
                    aah framework v0.11.0
---------------------------------------------------------------
# Report improvements/bugs at https://aahframework.org/issues #

Usage:
  aah [global options] command [command options] [arguments...]

Commands:
  new, n          Creates new aah 'web', 'api' or 'websocket' application (interactive)
  run, r          Runs aah application (supports hot-reload)
  build, b        Builds aah application for deployment (single or non-single)
  list, l         Lists all the aah projects on your GOPATH
  clean, c        Cleans the aah generated files and build directory
  switch, s       Switches between aah release and edge version
  update, u       Updates aah to the latest release version on your GOPATH
  generate, g     Generates boilerplate code, configurations, complement scripts (systemd, docker), etc.
  migrate, m      Migrates application codebase to current version of aah (currently beta)
  help, h         Shows a list of commands or help for one command

Global Options:
  -y, --yes      Automatic yes to prompts. Assume "yes" as answer to all prompts and run non-interactively.
  -h, --help     Shows help
  -v, --version  Prints cli, aah, go and aah libraries version
```

## Cross Compile Build

Set environment variables `GOOS` and `GOARCH` before executing `aah build` command. List of available GOOS and GOARCH values, [click here](https://golang.org/doc/install/source#environment).

#### Building linux binary on macOS

```bash
env GOOS=linux GOARCH=amd64 aah build --output /Users/jeeva/build

# Output:
...
Application artifact is here: /Users/jeeva/build/myapp-99bf7df-linux-amd64.zip
```

#### Building windows exe on macOS

```bash
env GOOS=windows GOARCH=amd64 aah build --output /Users/jeeva/build

# Output:
...
Application artifact is here: /Users/jeeva/build/myapp-99bf7df-windows-amd64.zip
```


## vgo Support

In `v0.11.0`, aah CLI brings `vgo` on-demand support. aah user could try out `vgo` via setting environment varaiable.

```bash
env AAHVGO=true aah run
```

## AAHPATH

`AAHPATH` is introduced in v0.11.0.

  * It is step towards aah CLI binary distribution. `v0.11.0` includes Homebrew channel 
  * Also started a work for adapting to `Versioned Go (vgo)` into aah

```bash
# `AAHPATH` default value is -
$HOME/.aah
```