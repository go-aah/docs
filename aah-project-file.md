Title: aah.project Configuration File
Desc: aah.project file aah application project file. It holds configuration values such as build, project info, hot-reload, etc.
Keywords: aah.project, project file, build config, hot-reload config, build file, flags, ldflags, tags, build excludes
---
# `aah.project` file

aah.project file comprises of application info, build configuration, hot-reload and [VFS](/vfs.html) configurations.

### Table of Contents

  * [build { ... }](#section-build)
  * [log { ... }](#section-log)
  * [hot_reload { ... }](#section-hot-reload)
  * [vfs { ... }](vfs.html#adding-custom-directory-mount) <span class="badge lb-xs">Since v0.11.0</span>

## Section: build { ... }

```bash
# ---------------------------------------------------------------------------
# Build section is used during aah application run and build command.
#
# Doc: https://docs.aahframework.org/aah-project-file.html#section-build
# ---------------------------------------------------------------------------
build {
  # Application binary name
  # Default value is `name` attribute value from `aah.conf`
  #binary_name = "samplenamenew"

  # Used as fallback version, if
  #   - `AAH_APP_VERSION` environment value
  #   - `git commit sha`
  # Not available
  version = "0.0.1"

  # If application misses any dependencies in `build import path`
  # during compile and build, aah CLI will try to get the missing
  # dependencies using 'go get <package>'.
  #
  # Default value is `true` since v0.11.0
  #dep_get = true

  # `-i` may not be required for go1.10 and above, however no impact.
  flags = ["-i"]

  ldflags = ""

  tags = ""

  # AST excludes is used for `aah.Context` inspection and for generating aah
  # application main Go file.
  #
  # For valid pattern syntax, refer to https://golang.org/pkg/path/filepath/#Match
  ast_excludes = ["*_test.go", ".*", "*.bak", "*.tmp", "vendor"]

  # Packing excludes is used to exclude file/directory during aah application
  # build archive.
  #
  # Note: It is also applicable to aah single binary build. This list is excluded
  # from embedding.
  #
  # For valid pattern syntax, refer to https://golang.org/pkg/path/filepath/#Match
  excludes = ["*.go", ".*", "*.bak", "*.tmp", "*.pid",
    "vendor", "app", "build", "tests", "logs", "sessions"]
}
```

## Section: log { ... }

```bash
# ---------------------------------------------------------------------------
# Logger configuration for aah CLI tool
# ---------------------------------------------------------------------------
log {
  # Log level
  # Default value is `info`.
  #level = "info"

  # Log colored output
  # Default value is `true`.
  #color = false
}
```

## Section: hot_reload { ... }

```bash
# ---------------------------------------------------------------------------
# Hot-Reload helps developer for rapid development.
# Read more about implementation here - https://aahframework.org/issues/4
#
# NOTE: Do not use hot-reload for production purposes, it is not recommended.
# Use app binary reload capability - `kill -HUP <process-id>` instead.
# ---------------------------------------------------------------------------
hot_reload {
  # Default value is `true`.
  enable = true

  # Watch configuration - files/directories exclusion list.
  #
  # For valid pattern syntax, refer to https://golang.org/pkg/path/filepath/#Match
  watch {
    # Note: static directory is not required to be monitored since aah server
    # delivers up-to-date file on environment profile `dev`.
    dir_excludes = [".*", "vendor", "build", "logs", "sessions"]

    file_excludes = [".*", "*.pid", "_test.go", "LICENSE", "README.md"]
  }
}
```

## Section: vfs { ... }

```bash
#------------------------------------------------------------------------
# Virtual FileSystem (VFS) configuration
#
# Doc: https://docs.aahframework.org/vfs.html
#------------------------------------------------------------------------
vfs {
  # Adding custom mount points.
  #
  # Configured mount point directories and sub-directories
  # are embedded into the aah app binary during single binary build.
  #
  # Note: Embed packaging uses the exclusion list from config `build.excludes`.
  mount {
    # Choose a unique key name
    #my_mount {
    #  # Mount point (path separator is '/').
    #  mount_path = "/my_mount"
    #
    #  # Physical FileSystem path.
    #  # Note: Only absolute path is accepted.
    #  physical_path = "/Users/jeeva/path/to/physical"
    #}
  }

  # This config speeds up the single binary build process.
  # No need to spend time in gzipping for compressed content type.
  #
  # Even without this config, aah decides best on what to gzip.
  no_gzip = [".png", ".jpeg", ".jpg", ".gif", ".bmp", ".tiff", ".tif"
    ".woff", ".woff2"]
}
```
