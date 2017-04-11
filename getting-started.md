# Getting started with aah framework

It is easy to start and work with aah framework. Let's make sure you have [Go environment ready](prerequisites.html).

Here you will be learning-

* [Installing aah framework and CLI tool](#installing-aah-framework-and-cli-tool)
* [Verifying your aah installation](#verify-your-aah-installation)
* [Creating your first aah application](#let-s-create-your-first-aah-application)
* [Running your aah application](#run-your-aah-application)
* [Next to app configuration](app-config.html)

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
	aah framework     v0.4
	aah cli tool      v0.4
	go[darwin/amd64]  v1.7.3
```

Congratulations, you have successfully installed aah framework and ready to use :).

## Let's create your first aah application

It is very easy to use aah framework CLI tool. [Learn more about aah CLI tool](aah-cli-tool.html).

```bash
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

Do you want session management (Y/n): Y
	Choose your session scope 'stateless' or 'stateful', default is 'stateless': stateful

Your aah web application created successfully at '/Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app'
You shall run your application: 'aah run -importPath=github.com/jeevatkm/aah-first-app'
```

## Run your aah application

```bash
aah run -p=github.com/jeevatkm/aah-first-app

# OR
cd $GOPATH/src/github.com/jeevatkm/aah-first-app

aah run
```

**Output:**
```bash
–––––––––––––––––––––––––––––––––––––––––––––––
   aah framework -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––

2017-04-11 11:17:58.128 INFO  - Loading aah project file: /Users/jeeva/go-home/src/github.com/jeevatkm/aah-first-app/aah.project
2017-04-11 11:17:58.128 INFO  - Compile starts for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-04-11 11:17:59.085 INFO  - Compile successful for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-04-11 11:17:59.098 INFO  - App Name: aah-first-app
2017-04-11 11:17:59.098 INFO  - App Version: 0.0.1
2017-04-11 11:17:59.098 INFO  - App Build Date: 2017-04-11T11:17:58-07:00
2017-04-11 11:17:59.098 INFO  - App Profile: dev
2017-04-11 11:17:59.098 INFO  - App TLS/SSL Enabled: false
2017-04-11 11:17:59.098 INFO  - App Session Mode: stateful
2017-04-11 11:17:59.098 INFO  - aah server running on :8080
```

Open up your browser, navigate to address [http://localhost:8080](http://localhost:8080) to see you first aah application in action.

## Next move on to application configuration

Learn more about [application configuration](app-config.html) and customize your application.
