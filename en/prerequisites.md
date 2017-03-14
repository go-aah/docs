# Standard Go Environment Readiness

aah framework does not have any specific prerequisites, it's a standard Go language environment.

## Go Installation

Follow the Go official [installation guide](https://golang.org/doc/install). There are plenty of contribution around the internet for Go installation. Few pointers from official guide.

  * [Using tarball installation on Linux, Max OS, and Free BSD](https://golang.org/doc/install#install)
  * [Mac OS Installer](https://golang.org/doc/install#osx)
  * [Windows](https://golang.org/doc/install#windows)

## Setting up $GOPATH

Understanding [GOPATH](https://github.com/golang/go/wiki/GOPATH) environment variable.

** *nix Platform **

For example: `/Users/jeeva/go-path` and adding $GOPATH into `$HOME/.profile`

```bash
echo 'export GOROOT=/usr/local/go' >> $HOME/.profile
echo 'export GOPATH=$HOME/go' >> $HOME/.profile
echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> $HOME/.profile
```

## Git Installation

There are plenty of contribution around the internet for Git installation on various platform.

  * The Git official guide - [Linux, Mac OS and Windows](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
