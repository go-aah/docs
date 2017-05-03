Title: Anatomy of a aah application
Desc: The layout of a aah application is standardized to keep things as simple as possible and has component of MVC architectural pattern.
Keywords: anatomy, layout, mvc, architectural, directories, aah application layout, aah application anatomy
---
# Anatomy of a aah application

The layout of a aah application is standardized to keep things as simple as possible and has component of MVC architectural pattern. After a first successful run, a aah application layout looks like below:

```bash
app                     ⇒ Application Go source codes directory
  └ controllers         ⇒ Application controllers
  └ models              ⇒ Application business layer
  └ aah.go              ⇒ aah application main entry point (generated code)
config                  ⇒ Configurations files
  └ aah.conf            ⇒ Application configuration file
  └ routes.conf         ⇒ Application Routes configuration file
  └ security.conf       ⇒ Application Security configuration file
  env                   ⇒ Environment profile configurations
    └ dev.conf          ⇒ Dev - Environment profile config
    └ prod.conf         ⇒ Prod - Environment profile config
i18n                    ⇒ Internalization and Localization message files
  └ messages.en         ⇒ Message file
  └ messages.en-US      ⇒ Message file
  └ messages.en-CA      ⇒ Message file
views                   ⇒ Template files (aah supports partial inheritance with Go template engine)
  └ common              ⇒ Common view template files, it can be imported to any page template
  └ layouts             ⇒ Application view layout template files, master template for page template
  └ pages               ⇒ Page view template files, corresponding view template for controllers action
static                  ⇒ Static public assets
  └ css                 ⇒ CSS files
  └ js                  ⇒ Javascript files
  └ img                 ⇒ Image files
logs                    ⇒ Logs directory (based log configuration, default is console on 'dev' profile)
  └ app-name.log        ⇒ Application log file
tests                   ⇒ Go source codes directory for functional tests, use Go conventional way for unit tests
build                   ⇒ Application build directory
.gitignore              ⇒ Typical Go .gitignore file and aah project ignore files
aah.project             ⇒ aah project configuration; name, build config, etc.
app-name.pid            ⇒ Application PID file
```

#### Packaged aah application will have following directories/files
```bash
bin                    ⇒ Application binary file
aah                    ⇒ *nix startup file
aah.cmd                ⇒ Windows startup file
```

#### Packaged aah application won't have following directories/files
```bash
app                    ⇒ Application Go source codes directory (compiled into binary file kept under `bin` directory)
logs                   ⇒ Logs directory (created during app startup)
tests                  ⇒ Go source codes directory (nor for production)
.gitignore             ⇒ Typical Go .gitignore file (not for production)
aah.project            ⇒ aah project configuration (not for production)
app-name.pid           ⇒ Application PID file (created during app startup)
```

### The `app` directory

The `app` directory contains all the Go source codes for the application. Initially these `controllers` and `models` directory gets created during `aah new` command. Of course you can as many as package needed in the app directory for you application.

### The `config` directory

The `config` directory contains the application's configuration files. aah configuration file syntax is similar to HOCON syntax not 100%. It has two important files-

  * `aah.conf` - the main configuration file for the application, [know more](app-config.html).
  * `routes.conf` - the routing configuration file for the application, [know more](routes-config.html).

### The `i18n` directory

The `i18n` directory contains internalization and localization message files. Message filename format is  `message.<Language-ID>`. Language is combination of `Language + Region` value. aah framework implements Language code is as per  two-letter `ISO 639-1` standard and Region code is as per two-letter `ISO 3166-1` standard, [know more](i18n.html).

### The `views` directory (Not applicable to API mode)

The `views` directory contains application view templates. aah framework supports partial inheritance with Go template engine, [know more](views.html). There are three main directories:

  * `common` - Common view template files, it can be imported to any page template via `import` template func.
  * `layouts` - Application view layout template files. You can have as many you want for page template.
  * `pages` - Page view template files. Respective view file for controllers action. For example: `<controller-name>/<action-name>.<ext>` (namespace directory structure is not supported).

### The `static` directory (Not applicable to API mode)

The `static` directory contains static assets that are served directly by Go HTTP server.

  * This directory is contains three sub-directories for images, CSS stylesheets and JavaScript files.
  * By default `static` directory mapped as `/assets` in [routes.conf](routes-config.html). You can customize it in the config.
  * You can use several directories under `static` directory for your need. Just organize appropriately :)

### The `logs` directory

The `logs` directory is for application logs.

### The `tests` directory

The `tests` directory is for application functional tests. Use Go conventional way `<source-file>_test.go` for unit test your Go source codes.

### The `.gitignore` file

The `.gitignore` file includes standard Go language ignores from Github and additionally aah framework ignores:

  * `aah.go`, `tmp/`, `*.pid`
