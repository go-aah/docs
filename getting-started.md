Title: Getting started with aah framework
Desc: It is very easy to start and work with aah framework.
Keywords: getting started, aah, aah Go web framework
---
# Getting started with aah

It is very easy to start and work with aah. Let's make sure you have [Go environment ready](prerequisites.html).

<br>
<div class="alert alert-info-blue">
<p><strong>Note:</strong> Since <code>v0.12.0</code> aah requires >= <code>go1.11</code>.</p>
</div>

### Table of Contents

  * [Installing aah CLI](#installing-aah-cli)
  * [Verifying aah CLI](#verify-aah-cli)
  * [Creating first aah application](#creating-first-aah-application)
  * [Running aah application](#running-aah-application)

## Installing aah CLI

aah user have multiple ways to install CLI into their system.

<br>
<div class="alert alert-info-blue">
<p><strong>Note:</strong> Everytime you update Go, you must reinstall aah CLI, using one of the methods below.</p>
</div>

#### Via Installer

macOS, Linux, BSD systems and Windows with Cygwin.

```bash
# Installs latest version of aah CLI
$ curl -s https://aahframework.org/install-cli | bash
# OR
$ wget -qO- https://aahframework.org/install-cli | bash
```

#### Install from Source for all platforms

```bash
$ git clone -b 'version-number' --single-branch --depth 1 https://github.com/go-aah/tools.git aah-cli

# on *nix systems
$ cd aah-cli/aah && env GO111MODULE=on go install

# on Windows
$ set GO111MODULE=on 
$ cd aah-cli/aah
$ go install
```

## Verify aah CLI

Checking aah CLI version. Learn more about [aah CLI](aah-cli-tool.html).

```bash
aah --version
```

Congratulations, you have successfully installed aah CLI and it's ready to use :).

## Creating first aah application

It is very easy to use aah CLI tool. Learn more about aah CLI using `aah help`.

<script src="https://asciinema.org/a/hz07lUuceCmDxdfMfjrv7nJhr.js" id="asciicast-hz07lUuceCmDxdfMfjrv7nJhr" data-speed="2" data-theme="monokai" data-rows="22" async></script>

## Running aah application

```bash
# Go to application base directory
aah run
```

<script src="https://asciinema.org/a/pBDbLienJ9FzgFFoXKMDKT5Jn.js" id="asciicast-pBDbLienJ9FzgFFoXKMDKT5Jn" data-speed="2" data-theme="monokai" data-rows="30" async></script>

Open up the browser then navigate to address [http://localhost:8080](http://localhost:8080) to see aah application in action.

<br>

**You will see a page like:**

<center>![aah first app]({{aah_cdn_host}}/assets/img/docs/aah-first-app-home-page.png)</center>
