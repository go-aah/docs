# aah project file - `aah.project`

aah project holds configuration values for application build. The configuration are-

## name
Name of the project, it is used for binary name if `build.binary_name` is not present. Whitespace is not recommend in the `name` field. If present whitespace will be replaced with `_`.

Default value is base directory name from application import path.
```bash
name = "mysampleapp"
```

## version
Used as fallback if git versioning or `AAH_APP_VERSION` environment value is not available.
```bash
version = "0.0.1"
```

## Section: build { ... }
Build section is used during aah application compile and build command.

### binary_name
If not supplied or empty then 'name' attribute is used for binary name.

Default value is `empty` string.
```bash
binary_name = ""
```

### go_get
If application is missing any dependencies during a compile and build, aah CLI will do `go get <package>`. If it's false build will show error with list of dependencies is not available in `GOPATH`.

Default value is `true`.
```bash
go_get = true
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
AST excludes is used for `aah.Context` inspection and generating aah application main Go file. Valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]`
```bash
ast_excludes = ["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]
```

### excludes
Packing excludes is used to exclude file/directory during aah application build archive. Valid exclude patterns refer [here](https://golang.org/pkg/path/filepath/#Match).

Default value is `["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app"]`
```bash
excludes = ["*.go", "*_test.go", ".*", "*.bak", "*.tmp", "vendor", "app"]
```
