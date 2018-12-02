Title: Getting started with aah framework
Desc: It is very easy to start and work with aah framework.
Keywords: getting started, aah, aah Go web framework
---
# Getting started with aah

It is very easy to start and work with aah. Let's make sure you have [Go environment ready](prerequisites.html).

### Table of Contents

  * [Installing aah CLI and framework](#installing-aah-cli-and-framework)
    - [macOS](#macos)
    - [go get](#go-get)
  * [Verifying aah CLI](#verify-aah-cli)
  * [Creating first aah application](#creating-first-aah-application)
  * [Running aah application](#running-aah-application)

## Installing aah CLI and framework

### macOS

```bash
# installing aah CLI
brew install go-aah/tap/aah

# installing aah framework in GOPATH
go get -u aahframework.org/aah.v0
```

### go get

`go get` install aah CLI as well as framework in $GOPATH.

```bash
# aah requires ≥ go1.8
go get -u aahframework.org/tools.v0/aah
```

## Verify aah CLI

Checking aah CLI version. Learn more about [aah CLI](aah-cli-tool.html).

```bash
aah --version

# Output
cli v0.12.0
aah v0.11.0
go  v1.10.1
```

Congratulations, you have successfully installed aah CLI and framework and it's ready to use :).

## Creating first aah application

It is very easy to use aah framework CLI tool. Learn more about aah CLI using `aah help`.

<script src="https://asciinema.org/a/yhDOMPv0lMrWRUMJXctGuuc5m.js" id="asciicast-yhDOMPv0lMrWRUMJXctGuuc5m" data-speed="2" data-theme="monokai" data-rows="22" async></script>

## Running aah application

```bash
aah run --importpath github.com/jeevatkm/aah-first-app
```

<script src="https://asciinema.org/a/FyjDJHteWpGvjQ6mw15IP4342.js" id="asciicast-FyjDJHteWpGvjQ6mw15IP4342" data-speed="2" data-theme="monokai" data-rows="28" async></script>

Open up the browser then navigate to address [http://localhost:8080](http://localhost:8080) to see aah application in action.

<br>

**You will see a page like:**

<center>![aah first app]({{aah_cdn_host}}/assets/img/docs/aah-first-app-home-page.png)</center>
