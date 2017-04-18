# aah CLI Tool

Here you will learn aah CLI tool usage and available commands. It comes very handy for development, Continuous Integration (CI) and deployment.

### Available Commands
  * [version](#command-version)
  * [help](#command-help)  
  * [new](#command-new)
  * [run](#command-run)
  * [build](#command-build)
  * [Cross Compile Build](#cross-compile-build)

## Command: version

To know about aah framework you're using and it's library versions.
```bash
# to know aah framework and CLI version
aah version

# to know all the version information
aah version -all
```

## Command: help

`help` command helps you to learn aah command usage.

```bash
# list of available commands
aah help

# to know one command
aah help build
```

## Command: new

`new` command is helps you to quick start new aah Web and API application. It is interactive command. Just provide appropriate inputs to make use of it.

```bash
aah new
```

## Command: run

`run` command is used to run the aah application for development purpose. It is recommended to use `build` command for deployments.

It has these parameters-

  * `-importPath | -ip` - aah application import path
  * `-config | -c` - External config for overriding aah.conf
  * `-profile | -p` - Environment profile name to activate. For e.g: dev, qa, prod. Default value is `dev`.

```bash
# run command usages

# if you're in app base dir, just execute
aah run

# anywhere you can execute
aah run -importPath=github.com/username/name

aah run -importPath=github.com/username/name -profile=qa

aah run -importPath=github.com/username/name -config=/path/to/config/external.conf -profile=qa
```

## Command: build

`build` command is used to create aah application build artifact for deployment. By default after successful, artifact will be available under directory `<app-base-dir>/build`.

It has these parameters-

  * `-importPath | -ip` - aah application import path.
  * `-artifactPath | -ap` - Output location application build artifact.

```bash
# build command usages

# if you're in app base dir, just execute
aah build

aah build -artifactPath=/Users/jeeva  # you can use short flag -ap

# anywhere you can execute
aah build -importPath=github.com/user/appname # you can use short flag -ip

aah build -importPath=github.com/user/appname -artifactPath=/Users/jeeva
```

## Cross Compile Build
Set environment variables `GOOS` and `GOARCH` before executing `aah build` command. List of available GOOS and GOARCH values, [click here](https://golang.org/doc/install/source#environment)

#### Building linux binary on Mac OS
```bash
env GOOS=linux GOARCH=amd64 aah build -ap=/Users/jeeva/build

#Output:
...
Your application artifact is here: /Users/jeeva/build/myapp-99bf7df-linux-amd64.zip
```

#### Building windows exe on Mac OS
```bash
env GOOS=windows GOARCH=amd64 aah build -ap=/Users/jeeva/build

#Output:
...
Your application artifact is here: /Users/jeeva/build/myapp-99bf7df-windows-amd64.zip
```
