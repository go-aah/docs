Title: aah Application Binary
Desc: Learn about aah application binary capabilities; such as run flags, hot-reload without restart, cross compile build and artifact naming convention.
Keywords: aah binary, aah app binary, flags, hot-reload, without restart, cross compile, artifact, artifact naming
---
# aah Application Binary

This page describe the aah application binary capabilities and artifact details.

<div class="alert alert-info-green">
<p><strong>News:</strong> Since <code>v0.11.0</code> aah supports single and non-single binary build artifact packaging.</p>
</div>

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Go binary have no runtime dependencies (such as Go installation, GOPATH, libraries, etc.), once binary is built for targeted Operating System (OS); then it runs on any machine of targeted OS.</p>
<p><u>For example:</u></p>
<p>Building aah binary for Linux 64-bit OS - <code>env GOOS=linux GOARCH=amd64 aah build &lt;args-goes-here></code></p>
</div>


### Table of Contents

  * [Input Flags](#input-flags)
  * [Start](#start)
  * [Stop](#stop)
  * [Reload](#reload)
  * [Finding Embedded Files](#finding-embedded-files) <sup>for aah single binary</sup>
  * [Cross Compile Build](#cross-compile-build)
  * [Build Artifact Naming Convention](#build-artifact-naming-convention)

## Input Flags

Application binary supports following input flags.

<script src="https://asciinema.org/a/kOM5GHnrMowBH6YYhTI57zfwh.js" id="asciicast-kOM5GHnrMowBH6YYhTI57zfwh" data-speed="2" data-theme="monokai" data-rows="14" async></script>

## Start

<span class="badge lb-sm">Since v0.11.0</span> No intermediate script to start, stop, etc.

To start aah application, simply use the binary input flags.

**For example**

```bash
$ /home/aah/website/bin/aahwebsite -profile prod
```

## Stop

aah application binary listens to `SIGINT` and `SIGTERM` OS signal. On receiving these signals -

  * application performs the graceful shutdown with timeout of `server.timeout.grace_shutdown` from `aah.conf`.
  * Then aah fires the `OnShutdown` server extension event.

```bash
$ kill -TERM <process-id>

# OR

$ kill -INT <process-id>
```

## Reload

<span class="badge lb-sm">Since v0.10.0</span> aah application binary supports `Hot-Reload` via `SIGHUP`. Basically application perform same steps as application start but reloading config, i18n, views, etc. without restart.

```bash
$ kill -HUP <process-id>
```

Application logs information similar to below-

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

## Finding Embedded Files

<span class="badge lb-sm">Since v0.11.0</span> aah supports single binary build packaging.

Once the single binary is build, there would be no easy way to know which files have got embedded into the binary. In real world scenario, it is important to have some mechanism to find out the contents. It could also be helpful for developers, devops, etc.

So aah provides easy convenient input flag called `-list`. It accepts regular expression as an input and print the file/directory path which matches with given regex pattern.

For regex syntax refer to https://golang.org/pkg/regexp or run `go doc regexp/syntax` on terminal.

<script src="https://asciinema.org/a/JOvC4imBfHy3T4OQd1iF5hdNz.js" id="asciicast-JOvC4imBfHy3T4OQd1iF5hdNz" data-speed="2" data-theme="monokai" data-rows="14" async></script>

## Cross Compile Build

Since `go1.5` we can build cross platform build easily. aah framework supports it, refer [Cross Compile Build](aah-cli-tool.html#cross-compile-build)

## Build Artifact Naming Convention

aah build produces the build artifact name as `<app-binary-name>-<version>-<goos>-<goarch>.zip`. Also you can supply your custom name for the artifact via `-o` or `--output`.

For e.g.: `aahwebsite-381eaa8-darwin-amd64.zip`
