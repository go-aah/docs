Title: aah CLI Tool
Desc: aah CLI tool is development buddy for developers to take advantage aah capabilities.
Keywords: aah CLI tool, ClI tool, hot-reload, cross compile, commands, run, new, build, version, list, clean, generate, migrate, switch, update
---
# aah CLI Tool

aah CLI it's a very handy tool for development, Continuous Integration (CI) and deployment preparation.

aah CLI is fully POSIX compliant flags (includes short and long versions) and command aliases too.

### Table of Contents

  * [Commands](#commands)
    - [aah new](#command-aah-new)
    - [aah run](#command-aah-run)
    - [aah runcmd](#command-aah-runcmd)
    - [aah build](#command-aah-build)
    - [aah list](#command-aah-list)
    - [aah clean](#command-aah-clean)
    - [aah generate](#command-aah-generate)
    - [aah migrate](#command-aah-migrate)
    - [aah help](#command-aah-help)
  * [Cross Compile Build](#cross-compile-build)
  * [AAHPATH](#aahpath)

## Commands

The best way to learn about aah CLI commands is to run `aah help` and explore further:

  * For command - `aah help <command>` 
  * Also for sub-commands - `aah <command> help` or `aah <command> help <sub-command>`

### Command: aah new

It's an interactive program that will make generate skeleton code to assist you to quick start aah application. It supports `web`, `api` or `websocket` types.

### Command: aah run

Runs aah application. It supports hot-reload (just code and refresh the browser to see your updates).

### Command: aah runcmd

Runs aah application console command. It's a handy development command to run user-defined console commands from aah CLI.

### Command: aah build

Builds aah application for deployment. It supports `single` & `non-single` binary. Learn more about [trade-off of build packaging](https://docs.aahframework.org/vfs.html).

### Command: aah list

It helps you view/discover all the aah application projects on your System. It automatically learns about new aah project when created via `aah new` and you can also teach it via `aah list --scan /base/dir/to/scan/aah-projects`.

### Command: aah clean

Cleans the aah generated files and build directory. Might be helpful once in a while.

### Command: aah generate

Increases productivity and helps developer on tedious tasks during application development - such as boilerplate code, configuration files, complementary scripts (systemd, docker), etc.

Currently it only supports complementary script generation. Remaining items are upcoming 😄.

### Command: aah migrate

This command will auto-migrate your aah code - it should be run everytime we release a new version of aah framework,
to make your code work and be compatible with the new aah version.
### Command: aah help

Of-course name speaks for itself.

## Cross Compile Build

Set environment variables `GOOS` and `GOARCH` before executing `aah build` command. List of available GOOS and GOARCH values, [click here](https://golang.org/doc/install/source#environment).

#### Building linux binary on macOS

```bash
# Use `aah help build` to learn more
env GOOS=linux GOARCH=amd64 aah build --single --output /Users/jeeva/build

# Output:
...
Application artifact is here: /Users/jeeva/build/myapp-99bf7df-linux-amd64.zip
```

#### Building Windows .exe on macOS

```bash
# Use `aah help build` to learn more
env GOOS=windows GOARCH=amd64 aah build --output /Users/jeeva/build

# Output:
...
Application artifact is here: /Users/jeeva/build/myapp-99bf7df-windows-amd64.zip
```

## AAHPATH

`AAHPATH` environment variable was introduced in v0.11.0.

  * It is step towards aah CLI binary distribution. Also `v0.11.0` includes Homebrew channel 

```bash
# `AAHPATH` default value is -
$HOME/.aah
```