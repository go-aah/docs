Title: Getting started with aah framework
Desc: It is very easy to start and work with aah framework. Installing aah framework and CLI tool, Creating your first aah application, Running your aah application, etc.
Keywords: getting started, aah framework, aah first application, web framework for Go lang
---
# Getting started with aah framework

It is easy to start and work with aah framework. Let's make sure you have [Go environment ready](prerequisites.html).

Here you will be learning-

* [Installing aah framework and CLI tool](#installing-aah-framework-and-cli-tool)
* [Verifying your aah installation](#verify-your-aah-installation)
* [Creating your first aah application](#let-s-create-your-first-aah-application)
* [Running your aah application](#run-your-aah-application)
* [Learning aah project structure](anatomy-aah-application.html)
* [Next move onto app configuration](app-config.html)

## Installing aah framework and CLI tool

Execute below `go get` on your terminal/command prompt. <span class="badge lb-xs">Since v0.7</span> aah CLI is fully POSIX flags (short & long) for commands. Also commands has `alias` too.

```
go get -u aahframework.org/tools.v0/aah
```

<div class="alert alert-info alert-info-blue">
<p><strong>Please Note:</strong></p>
<p>If you get an error "<u>RPC failed; HTTP 301 curl 22 The requested URL returned error: 301</u>" then set the below git config and try again.</p>
<p><pre>
git config --global http.https://aahframework.org.followRedirects true
</pre></p>
</div>


## Verify your aah installation

Just execute `aah` command on your terminal/command prompt. Learn more about [aah CLI](aah-cli-tool.html).

```
aah
```

**Output:** (you will get similar output)
```
jeeva@mb-pro:~$ aah
–––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.7 -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––––––––
# Report improvements/bugs at https://github.com/go-aah/aah/issues

Usage:
  aah [global options] command [command options] [arguments...]

Commands:
  new, n       Create new aah 'web' or 'api' application (interactive)
  run, r       Run aah framework application
  build, b     Build aah application for deployment
  list, l      List all aah projects in GOPATH
  clean, c     Cleans the aah generated files and build directory
  help, h      Shows a list of commands or help for one command

Global Options:
  -h, --help     show help
  -v, --version  print aah framework version and go version
```

Congratulations, you have successfully installed aah framework and it's ready to use :).

## Let's create your first aah application

It is very easy to use aah framework CLI tool. [Learn more about aah CLI tool](aah-cli-tool.html).

```
# `new` command, alias `n` - is an interactive one to create aah application for quick start
aah new
```

**Output:**
```
––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.7 -  https://aahframework.org
––––––––––––––––––––––––––––––––––––––––––––––––––––
# Report improvements/bugs at https://github.com/go-aah/aah/issues


Welcome to interactive way to create your aah application, press ^C to exit :)

Based on your inputs, aah CLI tool generates the aah application structure for you.

Enter your application import path: github.com/jeevatkm/aah-first-app

Choose your application type (web or api), default is 'web': web

Choose your application Auth Scheme (form, basic), default is 'none': form

Choose your session store (cookie or file), default is 'cookie': file

Your aah web application was created successfully at '/Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app'
You shall run your application via the command: 'aah run --importpath github.com/jeevatkm/aah-first-app'

Go to https://docs.aahframework.org to learn more and customize your aah application.

```

## Run your aah application

```
aah run --importpath github.com/jeevatkm/aah-first-app

# OR
aah run -i github.com/jeevatkm/aah-first-app

# OR
cd $GOPATH/src/github.com/jeevatkm/aah-first-app

aah run
```

**Output:**
```
––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.5 -  https://aahframework.org
––––––––––––––––––––––––––––––––––––––––––––––––––––

2017-05-08 23:11:52.511 INFO  Loading aah project file: /Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app/aah.project
2017-05-08 23:11:52.511 INFO  Compile starts for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-05-08 23:11:53.613 INFO  Compile successful for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-05-08 23:11:53.623 INFO  aah framework v0.5, requires ≥ go1.8
2017-05-08 23:11:53.626 INFO  Session file store is initialized at path: /Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app/sessions
2017-05-08 23:11:53.627 INFO  aah application initialized successfully
2017-05-08 23:11:53.627 INFO  App Name: aah-first-app
2017-05-08 23:11:53.627 INFO  App Version: 0.0.1
2017-05-08 23:11:53.627 INFO  App Build Date: 2017-05-08T23:11:52-07:00
2017-05-08 23:11:53.627 INFO  App Profile: dev
2017-05-08 23:11:53.627 INFO  App TLS/SSL Enabled: false
2017-05-08 23:11:53.627 INFO  App Session Mode: stateful
2017-05-08 23:11:53.627 INFO  aah go server running on :8080
```

Open up your browser, navigate to address [http://localhost:8080](http://localhost:8080) to see you first aah application in action.

You will see a page with text-

```
Welcome to aah framework - Web Application
```

## Learning aah project structure

Learn more [aah project structure](anatomy-aah-application.html).

## Next move onto application configuration

Learn more about [aah application configuration](app-config.html) and customize your application.
