# Versioning and Package

aah framework release versions according to [Semantic Versioning](http://semver.org/).

aah commits to provide stable release of framework and it's libraries. aah framework uses `gopkg.in` service codebase internally to provide versioning.

`aahframework.org/<library-name>.vX` points to appropriate tag versions; `X` denotes version number and it's a stable release for production use.

It's recommended to use this way:
```go
import "aahframework.org/aah.v0"
```

### High Level Info

aah codebase is hosted on [Github](https://github.com/go-aah) and Development happens on branch `vX-unstable` for example: `v0-unstable`, `v1-unstable`, etc. and branch `master` is tidy codebase of each release. Github `tags` are stable and production ready releases.

  * [Learn more about aah release and development process](release-and-development-process.html)
