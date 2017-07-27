## Migration Guide

<div class="alert alert-info alert-info-blue">
<p><strong>Tip:</strong></p>
<p>To adapt to latest configuration from release. Create a new project, compare new configurations (under `config/*`) with yours then merge it. Bingo! you're on to new configurations.</p>
</div>

### aah CLI tool

aah CLI tool is improved a lot in `v0.7` release. It supports POSIX flags (long & short), command alias for quick use. No change command names except `version` command, it become flag i.e. `aah -v` or `aah --version`.

**Run** aah to see an improved CLI.
```cfg
aah
```

**You will see similar output:**
```cfg
–––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.7 -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––––––––
# Report improvements/bugs at https://github.com/go-aah/aah/issues

Usage:
  aah [global options] command [command options] [arguments...]

Commands:
  new, n       Create new aah 'web' or 'api' application (interactive)
  run, r       Run aah framework application (supports hot-reload)
  build, b     Build aah application for deployment
  list, l      List all aah projects in GOPATH
  clean, c     Cleans the aah generated files and build directory
  help, h      Shows a list of commands or help for one command

Global Options:
  -h, --help     show help
  -v, --version  print aah framework version and go version
```

### aah security library

In `v0.7` release, aah achieves major milestone of security implementation - Authentication & Authorization. I have done lots of work in `security` library. Even though there is a change which might break aah `v0.6` if you
have used `aah.AppSecurityManager()` call, I doubt it. Otherwise `no issues and no code change required in your application`. So rest easy and upgrade it to latest version.

Only update required is, configuration. Add the following config into `aah.conf`.
```cfg
# --------------------------------------------------------------
# Application Security
# Doc: https://docs.aahframework.org/security-config.html
# --------------------------------------------------------------
include "./security.conf"
```

### aah.conf Configuration

In aah.conf `runtime.pooling { ... }` configuration is no longer required, so please remove it. Since aah automatically manages the memory pooling effectively without any settings.


### aah framework

To update aah framework to `v0.7`, refer to [FAQ - How to update aah framework to the latest version?](/faq.html?#how-to-update-aah-framework-to-the-latest-version)
