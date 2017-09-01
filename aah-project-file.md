Title: aah.project Configuration File
Desc: aah.project file aah application project file. It holds configuration values such as build, project info, hot-reload, etc.
Keywords: aah.project, project file, build config, hot-reload config, build file, flags, ldflags, tags, build excludes
---
# aah project file - `aah.project`

aah.project file holds the application info, build configuration and hot-reload values.

### Table of Contents

  * [build { ... }](#section-build)
  * [hot_reload { ... }](#section-hot-reload)  <span class="badge lb-xs">Since v0.7</span>

## Section: build { ... }
Build section is used during aah application compile and build process.

### binary_name
Application binary name.

Default value is `name` attribute value from `aah.conf`.
```cfg
binary_name = ""
```

### version
Used as fallback if `git commit sha` or `AAH_APP_VERSION` environment value is not available.
```cfg
version = "0.0.1"
```

### dep_get
If application is missing any dependencies in `build import path` during a compile and build process, aah CLI executes `go get <package>`. If it's false then build shows error with list of dependencies which are not available.

Default value is `false`.
```cfg
dep_get = true
```

### log_level
Log level is used for aah CLI tool logging.

Default value is `info`.
```cfg
log_level = "info"
```

### flags
Flag values is supplied to `go build` command.

Default value is `["-i"]`
```cfg
flags = ["-i"]
```

### ldflags
Ldflags is supplied to `go build` command as `-ldflags ...`.

Default value is `empty` string.
```cfg
ldflags = ""
```

### tags
Tags is supplied to `go build` command as `-tags ...`.

Default value is `empty` string.
```cfg
tags = ""
```

### ast_excludes
AST excludes is used for `aah.Context` inspection and generating aah application main Go file. For valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]`
```cfg
ast_excludes = ["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]
```

### excludes
Packing excludes is used to exclude file/directory during aah application build archive. For valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app", "tests", "logs"]`
```cfg
excludes = ["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app", "tests", "logs"]
```

## Section: hot_reload { ... }

Hot-Reload is development purpose to help developer. Read more about implementation [here]({{aah_github_issues_url}}/4).

Note: Do not use hot-reload feature for production purpose, it's not recommended.

### enable
To enable hot-reload for development purpose.

Default value is `true`.
```cfg
enable = true
```

## Section: watch { ... }

Watch configuration - files/directories exclusion list.

### dir_excludes

To Exclude directory from watch list.
```cfg
# Note: static directory not required to be monitored, since server delivers
# up-to-date file on environment profile `dev`.
dir_excludes = [".*"]
```

### file_excludes

To Exclude file from watch list.
```cfg
file_excludes = [".*", "_test.go", "LICENSE", "README.md"]
```
