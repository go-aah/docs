Title: Versioning and Package Management
Desc: aah releases per Semantic Versioning. aah commits to provide stable release version of framework and it's libraries.
Keywords: versioning, package management, stable version, release version
---
# Versioning and Package Management

aah releases per [Semantic Versioning](https://semver.org/). <span class="badge lb-sm">Since v0.12.0</span> aah uses Go modules fully as a package management to provide stable release version of framework and it's libraries. 

For framework `v0.11.4` and below versions, aah internally uses `gopkg.in` service codebase to provide versioning support on top of package management tools.

**go.mod**

```bash
require aahframe.work v0.12.0
```

**Import Usage**

```go
import "aahframe.work"
```

### High Level Info

aah codebase is hosted on [Github](https://github.com/go-aah) and Development happens on branch `edge` and branch `master` is tidy codebase of each release. Github `tags` are stable and production ready.

  * [Learn more about aah release and development process](release-and-development-process.html)

### Package Management

<span class="badge lb-sm">Since v0.12.0</span> aah uses **Go modules** fully as a package management. aah HIGHLY RECOMMENDS you to use `go.mod`.

Command `aah new` generates new aah application with `go.mod` definitions.
