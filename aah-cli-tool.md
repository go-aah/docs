# aah CLI Tool

Here you will learn aah CLI tool usage and available commands. It comes very handy for development, Continuous Integration (CI) and deployment.

### Available Commands
  * [version](#command-version)
  * [help](#command-help)  
  * [new](#command-new)
  * [run](#command-run)
  * [build](#command-build)

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

`build` command is used to create aah application build artifact for deployment. By default after successful, artifact will be available under directory `<app-base-dir>/aah-build`.

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
