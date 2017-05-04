Title: Versioning and Package Management
Desc: aah framework release versions according to Semantic Versioning. aah commitment to provide stable versioned release of framework and it's libraries.
Keywords: versioning, package management, stable version, release version
---
# Versioning and Package Management

aah framework release versions according to [Semantic Versioning](http://semver.org/).

aah commitment to provide stable release of framework and it's libraries. aah framework uses `gopkg.in` service codebase internally to provide versioning.

`aahframework.org/<library-name>.vX` points to appropriate tag versions; `X` denotes version number and it's a stable release for production use.

It's recommended to use this way:
```go
import "aahframework.org/aah.v0"
```

### High Level Info

aah codebase is hosted on [Github](https://github.com/go-aah) and Development happens on branch `vX-unstable` for example: `v0-unstable`, `v1-unstable`, etc. and branch `master` is tidy codebase of each release. Github `tags` are stable and production ready releases.

  * [Learn more about aah release and development process](release-and-development-process.html)

### Package Management

aah framework doesn't provide explict package management tool. Feel free to use your choice of tool.

Just to make sure, aah application is tested with glide and govendor package management tools. It is doing alright.

#### How to use it?

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
