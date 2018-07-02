Title: Getting started with aah framework
Desc: It is very easy to start and work with aah framework.
Keywords: getting started, aah, aah Go web framework
---
# Getting started with aah

It is very easy to start and work with aah. Let's make sure you have [Go environment ready](prerequisites.html).

### Table of Contents

  * [Installing aah CLI and framework](#installing-aah-cli-and-framework)
  * [Verifying aah CLI](#verify-aah-cli)
  * [Creating first aah application](#creating-first-aah-application)
  * [Running aah application](#running-aah-application)

## Installing aah CLI and framework

Execute below `go get` on your terminal/command prompt to install aah CLI and framework codebase.

```bash
# aah framework requires ≥ go1.8
go get -u aahframework.org/tools.v0/aah
```

## Verify aah CLI

Just execute `aah` command on your terminal/command prompt. Learn more about [aah CLI](aah-cli-tool.html).

```bash
aah
```

**Output:**

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
  -h, --help     Shows help
  -v, --version  Prints cli, aah, go and aah libraries version
```

Congratulations, you have successfully installed aah CLI and framework and it's ready to use :).

## Creating first aah application

It is very easy to use aah framework CLI tool. Learn more about aah CLI using `aah help`.

```bash
aah new
```

**Output:**

```cfg
---------------------------------------------------------------
                    aah framework v0.11.0
---------------------------------------------------------------
# Report improvements/bugs at https://aahframework.org/issues #


Welcome to interactive way to create your aah application, press ^C to exit :)

Based on your inputs, aah CLI tool generates the aah application structure for you.

Enter your application import path: github.com/jeevatkm/aah-first-app

Choose your application type (web, api or websocket), default is 'web':

Choose your application View Engine (go), default is 'go':

Choose your application Auth Scheme (form, basic), default is 'none': form

Choose your password hash algorithm (bcrypt, scrypt, pbkdf2), default is 'bcrypt':

Choose your session store (cookie or file), default is 'cookie':

Would you like to add API [/api/v1/*] within your Web App ([Y]es or [N]o)? default is 'N': Y

Would you like to add WebSocket [/ws/*] within your Web App ([Y]es or [N]o)? default is 'N': Y

Would you like to enable CORS ([Y]es or [N]o)? default is 'N': Y

Your aah web application was created successfully at '/Users/jeeva/go/src/github.com/jeevatkm/aah-first-app'
You shall run your application via the command: 'aah run --importpath github.com/jeevatkm/aah-first-app'

Go to https://docs.aahframework.org to learn more and customize your aah application.
```

## Running aah application

```bash
aah run --importpath github.com/jeevatkm/aah-first-app
```

**Output:**

```cfg
---------------------------------------------------------------
                     aah framework v0.11.0
---------------------------------------------------------------
# Report improvements/bugs at https://aahframework.org/issues #

Loaded aah project file: /Users/jeeva/go/src/github.com/jeevatkm/aah-first-app/aah.project
Hot-Reload enabled for environment profile: dev
Compile starts for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
Compile successful for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2018-07-01 00:08:36.384 INFO  aah framework v0.11.0-edge, requires ≥ go1.8
2018-07-01 00:08:36.386 DEBUG Event [OnInit] publishing in synchronous mode
2018-07-01 00:08:36.387 DEBUG aah-first-app Domain count: 1
2018-07-01 00:08:36.387 DEBUG aah-first-app Domain: localhost:8080, routes found: 11
2018-07-01 00:08:36.388 DEBUG aah-first-app Initializing authenticator for auth scheme 'form_auth'
2018-07-01 00:08:36.388 DEBUG aah-first-app Initializing authorizer for auth scheme 'form_auth'
2018-07-01 00:08:36.388 INFO  aah-first-app aah application initialized successfully
2018-07-01 00:08:36.389 INFO  aah-first-app App Base Directory: /Users/jeeva/go/src/github.com/jeevatkm/aah-first-app
2018-07-01 00:08:36.389 INFO  aah-first-app App Virtual Base Directory: /app
2018-07-01 00:08:36.389 INFO  aah-first-app App Name: aah-first-app
2018-07-01 00:08:36.389 INFO  aah-first-app App Version: 0.0.1
2018-07-01 00:08:36.389 INFO  aah-first-app App Build Date: 2018-07-01T00:08:33-07:00
2018-07-01 00:08:36.389 INFO  aah-first-app App Single Binary Mode: false
2018-07-01 00:08:36.389 INFO  aah-first-app App Profile: dev
2018-07-01 00:08:36.389 INFO  aah-first-app App TLS/SSL Enabled: false
2018-07-01 00:08:36.389 INFO  aah-first-app App View Engine: go
2018-07-01 00:08:36.389 INFO  aah-first-app App Session Mode: stateful
2018-07-01 00:08:36.389 INFO  aah-first-app App Anti-CSRF Enabled: true
2018-07-01 00:08:36.389 INFO  aah-first-app App Route Domains:
2018-07-01 00:08:36.389 INFO  aah-first-app       Host: localhost:8080, CORS Enabled: true
2018-07-01 00:08:36.389 INFO  aah-first-app App i18n Locales: en
2018-07-01 00:08:36.389 DEBUG aah-first-app Callback: addingHTMLMinifer, subscribed to event: OnInit
2018-07-01 00:08:36.389 DEBUG aah-first-app Callback: SubscribeHTTPEvents, subscribed to event: OnStart
2018-07-01 00:08:36.389 DEBUG aah-first-app Callback: SubscribeWebSocketEvents, subscribed to event: OnStart
2018-07-01 00:08:36.389 DEBUG aah-first-app Callback: RunCmdSetAppProxyPort, subscribed to event: OnStart
2018-07-01 00:08:36.389 INFO  aah-first-app App Shutdown Grace Timeout: 60s
2018-07-01 00:08:36.389 DEBUG aah-first-app Event [OnStart] publishing in synchronous mode
2018-07-01 00:08:36.389 INFO  aah-first-app aah go server running on :8080
```

Open up the browser then navigate to address [http://localhost:8080](http://localhost:8080) to see aah application in action.

<br>

**You will see a page like:**

<center>![aah first app]({{aah_cdn_host}}/assets/img/docs/aah-first-app-home-page.png)</center>
