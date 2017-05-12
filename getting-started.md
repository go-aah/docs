Title: Getting started with aah framework
Desc: It is easy to start and work with aah framework. Installing aah framework and CLI tool, Creating your first aah application, Running your aah application, etc.
Keywords: getting started, aah framework, aah first application
---
# Getting started with aah framework

It is easy to start and work with aah framework. Let's make sure you have [Go environment ready](prerequisites.html).

Here you will be learning-

* [Installing aah framework and CLI tool](#installing-aah-framework-and-cli-tool)
* [Verifying your aah installation](#verify-your-aah-installation)
* [Creating your first aah application](#let-s-create-your-first-aah-application)
* [Running your aah application](#run-your-aah-application)
* [Next move onto app configuration](app-config.html)

## Installing aah framework and CLI tool

Execute below `go get` on your terminal/command prompt.

```bash
go get -u aahframework.org/tools.v0/aah
```

## Verify your aah installation

Execute below command.

```bash
aah version
```

**Output:** (you will get similar output with aah and Go version)
```bash
–––––––––––––––––––––––––––––––––––––––––––––––
   aah framework -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––

Version Info:
	aah framework     v0.5
	aah cli tool      v0.5
	go[darwin/amd64]  v1.8
```

Congratulations, you have successfully installed aah framework and ready to use :).

## Let's create your first aah application

It is very easy to use aah framework CLI tool. [Learn more about aah CLI tool](aah-cli-tool.html).

```bash
# new sub-command is interactive one to create application for quick start
aah new
```

**Output:**
```bash
–––––––––––––––––––––––––––––––––––––––––––––––
   aah framework -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––

Welcome to interactive way to create your aah application, press ^C to exit :)

Based on your inputs, aah CLI tool generates the aah application structure
for you.

Enter your application import path: github.com/jeevatkm/aah-first-app

Choose your application type (web or api), default is 'web': web

Choose your session store (cookie or file), default is 'cookie': file

Your aah web application created successfully at '/Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app'
You shall run your application: 'aah run -importPath=github.com/jeevatkm/aah-first-app'

Go to https://docs.aahframework.org to learn more and customize your aah application.

```

## Run your aah application

```bash
aah run -importPath=github.com/jeevatkm/aah-first-app

# OR
aah run -ip=github.com/jeevatkm/aah-first-app

# OR
cd $GOPATH/src/github.com/jeevatkm/aah-first-app

aah run
```

**Output:**
```bash
––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.5 -  https://aahframework.org
––––––––––––––––––––––––––––––––––––––––––––––––––––

2017-05-08 23:11:52.511 INFO  - Loading aah project file: /Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app/aah.project
2017-05-08 23:11:52.511 INFO  - Compile starts for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-05-08 23:11:53.613 INFO  - Compile successful for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-05-08 23:11:53.623 INFO  - aah framework v0.5, requires ≥ go1.8
2017-05-08 23:11:53.626 INFO  - Session file store is initialized at path: /Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app/sessions
2017-05-08 23:11:53.627 INFO  - aah application initialized successfully
2017-05-08 23:11:53.627 INFO  - App Name: aah-first-app
2017-05-08 23:11:53.627 INFO  - App Version: 0.0.1
2017-05-08 23:11:53.627 INFO  - App Build Date: 2017-05-08T23:11:52-07:00
2017-05-08 23:11:53.627 INFO  - App Profile: dev
2017-05-08 23:11:53.627 INFO  - App TLS/SSL Enabled: false
2017-05-08 23:11:53.627 INFO  - App Session Mode: stateful
2017-05-08 23:11:53.627 INFO  - aah go server running on :8080
```

Open up your browser, navigate to address [http://localhost:8080](http://localhost:8080) to see you first aah application in action.

## Next move onto application configuration

Learn more about [aah application configuration](app-config.html) and customize your application.
