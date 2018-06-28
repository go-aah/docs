Title: Versioning and Package Management
Desc: aah releases per Semantic Versioning. aah commits to provide stable release version of framework and it's libraries.
Keywords: versioning, package management, stable version, release version
---
# Versioning and Package Management

aah releases per [Semantic Versioning](https://semver.org/). aah commitment to provide stable release version of framework and it's libraries. aah internally uses `gopkg.in` service codebase to provide versioning support on top of package management tools.

`aahframework.org/<library-name>.vX` points to appropriate tag versions; `X` denotes version number and it's a stable release for production use.

```go
import "aahframework.org/aah.v0"
```

### High Level Info

aah codebase is hosted on [Github](https://github.com/go-aah) and Development happens on branch `vX-edge` for example: `v0-edge`, `v1-edge`, etc. and branch `master` is tidy codebase of each release. Github `tags` are stable and production ready.

  * [Learn more about aah release and development process](release-and-development-process.html)

### Package Management

<div class="alert alert-info-blue">
<p>aah HIGHLY RECOMMENDS you to use your choice of package management tool to lock libraries version for your application. Beside that aah also provides versioned release too, if you're just using <code>go get</code>.</p>
</div>

To ensure aah have been tested with `glide`, `dep` and `govendor` package management tools.

#### How do I use it?

It is simple, go to aah application base directory and execute the commands from respective tool which you're using.

For example: aahframework.org [website codebase](https://github.com/go-aah/website) uses `glide` package management tool.

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
