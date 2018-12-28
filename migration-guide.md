## Migration Guide

aah aims to make the process of upgrading application to the latest release version as easy & convenient as possible. It just few steps away-

### Step 1: Install latest aah CLI

```bash
$ curl https://aahframework.org/install-cli | bash
#	OR
$ wget -qO- https://aahframework.org/install-cli | bash
```

### Step 2: Run migration command

Basically it does the migration of application codebase and view files. Go to aah application base directory and run-

```bash
# *nix systems and Windows (Cygwin, Cmder and ConEmu)
$ env GO111MODULE=on aah migrate code

# Windows
$ set GO111MODULE=on
$ aah migrate code
```

### Step 3: Learn how to use latest aah binary

In `v0.12.x` aah binary become more CLI friendly and POSIX flags support, refer to [Application Binary](/aah-application-binary.html)

<br><br>