# Getting started with aah framework

It is easy to start and work with aah framework. Let's make sure you have [Go environment ready](/goto?doc=prerequisites).

Here you will be learning-

* Installing aah framework and CLI tool
* Verifying your aah installation
* Creating your first aah application
* Running your aah application

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
	aah framework     v0.3
	aah cli tool      v0.3
	go[darwin/amd64]  v1.7.3
```

Congratulations, you have successfully installed aah framework and ready to use :).

## Let's Create your First aah application

It is very easy to use aah framework CLI tool. [Learn more about aah CLI tool](/goto?doc=aah_cli).

```bash
aah new -importPath=github.com/jeevatkm/aah-first-app
```

**Output:**
```bash
–––––––––––––––––––––––––––––––––––––––––––––––
   aah framework -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––

2017-03-14 01:36:27.030 INFO  - Your aah web application 'aah-first-app' created successfully at '/Users/jeeva/go-path/src/github.com/jeevatkm/aah-first-app'
2017-03-14 01:36:27.030 INFO  - You shall run your application: 'aah run -p=github.com/jeevatkm/aah-first-app'
```

## Run your aah application

```bash
aah run -p=github.com/jeevatkm/aah-first-app
```

**Output:**
```bash
2017-03-14 01:36:55.392 INFO  - Reading aah project file: /Users/jeeva/go-path/src/github.com/jeevatkm/aah-first-app/aah.project
2017-03-14 01:36:55.392 INFO  - Starting build for 'aah-first-app' [github.com/jeevatkm/aah-first-app]
2017-03-14 01:36:55.394 INFO  - Cleaning /Users/jeeva/go-path/src/github.com/jeevatkm/aah-first-app/app/aah.go
2017-03-14 01:36:55.394 INFO  - Cleaning /Users/jeeva/go-path/bin/aah.d/github.com/jeevatkm/aah-first-app/aah-first-app
2017-03-14 01:36:55.398 INFO  - Executing /usr/local/go/bin/go list -f {{.Imports}} github.com/jeevatkm/aah-first-app/app/...
2017-03-14 01:36:55.564 INFO  - Executing /usr/local/go/bin/go build -i -o /Users/jeeva/go-path/bin/aah.d/github.com/jeevatkm/aah-first-app/aah-first-app github.com/jeevatkm/aah-first-app/app
2017-03-14 01:36:56.415 INFO  - Build successful for 'aah-first-app' [github.com/jeevatkm/aah-first-app].
2017-03-14 01:36:56.415 INFO  - Executing /Users/jeeva/go-path/bin/aah.d/github.com/jeevatkm/aah-first-app/aah-first-app
2017-03-14 01:36:56.426 INFO  - App Name: aah-first-app
2017-03-14 01:36:56.426 INFO  - App Profile: dev
2017-03-14 01:36:56.426 INFO  - App Mode: web
2017-03-14 01:36:56.427 INFO  - Listening and serving HTTP on :8080
```

Open up your browser, navigate to address [http://localhost:8080](http://localhost:8080) to see you first aah application in action.
