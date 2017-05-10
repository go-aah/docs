Title: aah.project Configuration File
Desc: aah.project file holds configuration values for aah application project. It is used in build, project info, etc.
Keywords: aah.project, project file, build config, build file, flags, ldflags, tags, build excludes
---
# aah project file - `aah.project`

aah.project file holds the application info and build configuration values.

### Table of Contents

  * [build { ... }](#section-build)

## Section: build { ... }
Build section is used during aah application compile and build process.

### binary_name
Application binary name.

Default value is `name` attribute value from `aah.conf`.
```bash
binary_name = ""
```

### version
Used as fallback if `git commit sha` or `AAH_APP_VERSION` environment value is not available.
```bash
version = "0.0.1"
```

### dep_get
If application is missing any dependencies in `build import path` during a compile and build process, aah CLI executes `go get <package>`. If it's false then build shows error with list of dependencies which are not available.

Default value is `false`.
```bash
dep_get = true
```

### log_level
Log level is used for aah CLI tool logging.

Default value is `info`.
```bash
log_level = "info"
```

### flags
Flag values is supplied to `go build` command.

Default value is `["-i"]`
```bash
flags = ["-i"]
```

### ldflags
Ldflags is supplied to `go build` command as `-ldflags ...`.

Default value is `empty` string.
```bash
ldflags = ""
```

### tags
Tags is supplied to `go build` command as `-tags ...`.

Default value is `empty` string.
```bash
tags = ""
```

### ast_excludes
AST excludes is used for `aah.Context` inspection and generating aah application main Go file. For valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]`
```bash
ast_excludes = ["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]
```

### excludes
Packing excludes is used to exclude file/directory during aah application build archive. For valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app", "tests", "logs"]`
```bash
excludes = ["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app", "tests", "logs"]
```
