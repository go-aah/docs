Title: Versioning and Package Management
Desc: aah framework release versions as per Semantic Versioning. aah commitment to provide stable version release of framework and it's libraries.
Keywords: versioning, package management, stable version, release version
---
# Versioning and Package Management

aah release versions as per [Semantic Versioning](http://semver.org/).

aah commitment to provide stable release of framework and it's libraries. aah internally uses `gopkg.in` service codebase to provide versioning and `go get` service.

`aahframework.org/<library-name>.vX` points to appropriate tag versions; `X` denotes version number and it's a stable release for production use.

```go
import "aahframework.org/aah.v0"
```

### High Level Info

aah codebase is hosted on [Github](https://github.com/go-aah) and Development happens on branch `vX-edge` for example: `v0-edge`, `v1-edge`, etc. and branch `master` is tidy codebase of each release. Github `tags` are stable and production ready.

  * [Learn more about aah release and development process](release-and-development-process.html)

### Package Management

aah **highly recommends** you to use your choice of package management tool and vendorize your application dependencies. Beside that aah also provides versioned release too, if you're just using `go get`.

Just to make sure, I have tested aah application with `glide`, `golang/dep` and `govendor` package management tools. It is doing alright.

#### How do I use it?

It is simple, go to your application base directory and execute the commands from respective tool which you're using.

For example: aahframework.org [website codebase](https://github.com/go-aah/website) uses glide package management tool.

```bash
# as an example, application base directory
# refer respective tool documentation for the usage.
cd $GOPATH/src/aah-first-app

# glide tool
glide init
glide update

# govendor tool
govendor init
govendor add +external
```
