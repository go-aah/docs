Title: aah CLI Tool
Desc: aah CLI tool is development buddy for developers to take advantage aah framework capabilities.
Keywords: aah CLI tool, ClI tool, hot-reload, cross compile, commands, run, new, build, version, list, clean
---
# aah CLI Tool

Here you will learn aah CLI flags, commands and it's usage. It's very handy for development, Continuous Integration (CI) and deployment preparation.

aah CLI is fully POSIX compliant flags (includes short and long versions) and command aliases too.

### Available Commands
  * Global Flags
      - `-h, --help` - show help
      - `-v, --version` - print aah framework versions and go version
  * [list](#command-list)
  * [new](#command-new)
  * [run](#command-run)
  * [build](#command-build)
  * [clean](#command-clean)
  * [switch](#command-switch)
  * [update](#command-update)
  * [generate](#command-generate)
  * [help](#command-help)
  * [Cross Compile Build](#cross-compile-build)

## Command: list
<span class="badge lb-sm">Since v0.6</span> `list` command, alias `l` - it scans GOPATH and lists all the import paths of aah project.

```cfg
‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
           aah framework v0.10 - https://aahframework.org           
____________________________________________________________________
# Report improvements/bugs at https://github.com/go-aah/aah/issues #

INFO Scanning GOPATH: /Users/jeeva/go/...

INFO 4 aah projects were found, import paths are:
    github.com/go-aah/tutorials/domain-subdomain
    github.com/go-aah/tutorials/html-minify
    github.com/go-aah/tutorials/i18n-url-query-param
    github.com/go-aah/website
```


## Command: new

`new` command, alias `n` - helps you to quick start new aah Web and API application. It is interactive command. Just provide an appropriate inputs.

```cfg
aah new
```


## Command: run

`run` command, alias `r` - is used to run the aah application.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> It is recommended to use <code>build</code> command to create build artifact and deploy it on server instead of using run command.</p>
</div>

Supported options are:
```cfg
-i value, --importpath value  Import path of aah application
-e value, --envprofile value  Environment profile name to activate. e.g: dev, qa, prod
-c value, --config value      External config file for overriding aah.conf values
```

Use `aah help run` to learn more.


## Command: build

`build` command, alias `b` - is used to create aah application build artifact for deployment. Default location and name is `<app-base-dir>/build/<app-binary-name>-<app-version>-<goos>-<goarch>.zip` or you can use your custom artifact name too.

Supported options are:
```cfg
-i value, --importpath value  Import path of aah application
-e value, --envprofile value  Environment profile name to activate. e.g: dev, qa, prod
-o value, --output value      Output of aah application build artifact. Default is '<app-base-dir>/build/<app-binary-name>-<app-version>-<goos>-<goarch>.zip'
```

Artifact naming convention:

  * `<app-binary-name>-<app-version>-<goos>-<goarch>.zip`
      - For e.g.: `aahwebsite-381eaa8-darwin-amd64.zip`

Use `aah help build` to learn more.


## Command: clean

<span class="badge lb-sm">Since v0.7</span> `clean` command, alias `c` - is to clean generated files and build directory of the aah project.

Supported options are:
```cfg
-i value, --importpath value  Import path of aah application
```

Use `aah help clean` to learn more.


## Command: switch

<span class="badge lb-sm">Since v0.9</span> `switch` command, alias `s` - is to switch between aah `release` (currently on your GOPATH) and latest `edge` version.

Supported options are:
```cfg
-v value, --version value  To mention latest release or edge version (default: "edge")
-w, --whoami   To know which version is currently active
-r, --refresh  To refresh edge version to the latest codebase  # since v0.10
```

Use `aah help switch` to learn more.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>Currently it works with only GOPATH. Gradually I will add vendorize support too.</li>
  <li>It always operates on latest edge version and current release version on your GOPATH, specific version is not supported.</li>
</ul>
</p>
</div>


## Command: update

<span class="badge lb-sm">Since v0.10</span> `update` command, alias `u` - is to update your aah to the latest release version on your GOPATH.

Use `aah help update` to learn more.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>Currently it works with only GOPATH. Gradually I will add vendorize support too.</li>
  <li>It always operates on aah latest release version, specific version is not supported.</li>
</ul>
</p>
</div>


## Command: generate

<span class="badge lb-sm">Since v0.10</span> `generate` command, alias `g` - is to generate boilerplate code, configurations and complement scripts, etc.

Use `aah generate help` to learn more about available generate sub commands.

#### Example of systemd service file generate
```cfg
aah generate script --name systemd --importpath github.com/user/appname

# Short form
aah g s -n systemd -i github.com/user/appname

# If you're on app directory
aah g s -n systemd
```

#### Example of docker file generate
```cfg
aah generate script --name docker --importpath github.com/user/appname

# Short form
aah g s -n docker -i github.com/user/appname

# If you're on app directory
aah g s -n docker
```


## Command: help

`help` command alias `h` - helps you to learn aah command usage.

```cfg
# list of available commands
aah help

# to know about one command
aah help build
```

## Cross Compile Build
Set environment variables `GOOS` and `GOARCH` before executing `aah build` command. List of available GOOS and GOARCH values, [click here](https://golang.org/doc/install/source#environment).

#### Building linux binary on macOS
```cfg
env GOOS=linux GOARCH=amd64 aah build --output=/Users/jeeva/build

#Output:
...
Your application artifact is here: /Users/jeeva/build/myapp-99bf7df-linux-amd64.zip
```

#### Building windows exe on macOS
```cfg
env GOOS=windows GOARCH=amd64 aah build --output=/Users/jeeva/build

#Output:
...
Your application artifact is here: /Users/jeeva/build/myapp-99bf7df-windows-amd64.zip
```
