Title: Standard Go Environment Readiness
Desc: aah framework does not have any specific prerequisites, it's a standard Go language environment.
Keywords: environment readiness, prerequisites, go lang environment
---
# Standard Go Environment Readiness

aah does not have any specific prerequisites, it's a standard Go language environment readiness.

## Go Installation

Follow the Go official [installation guide](https://golang.org/doc/install). There are plenty of contribution around the internet for Go installation. Few pointers from official guide.

  * [Using tarball installation on Linux, Max OS, and Free BSD](https://golang.org/doc/install#install)
  * [Mac OS Installer](https://golang.org/doc/install#osx)
  * [Windows](https://golang.org/doc/install#windows)

## Setting up the $GOPATH

  * Understanding [GOPATH](https://github.com/golang/go/wiki/GOPATH) and it's purpose.
  * Setting [GOPATH](https://golang.org/wiki/SettingGOPATH) on UNIX systems and Windows.

Don't forget to add your `bin` directory on the `PATH`.

```bash
# On *nix operating system
# Set value and then open new terminal session to reflect your change.
echo 'export PATH=$PATH:$GOPATH/bin' >> $HOME/.profile
```

## Git Installation

There are plenty of contribution around the internet for Git installation on various platform.

  * The Git official guide - [Linux, Mac OS and Windows](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
