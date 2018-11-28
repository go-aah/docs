## Migration Guide

[FAQ: How to update aah to latest version?](faq.html#how-to-update-aah-to-latest-version)

### Migrate application codebase

Go to aah application base directory and run-

```bash
# *nix systems and Windows (Cygwin, Cmder and ConEmu)
$ env GO111MODULE=on aah migrate code

# Windows
$ set GO111MODULE=on
$ aah migrate code
```
