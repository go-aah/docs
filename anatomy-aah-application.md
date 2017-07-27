Title: Anatomy of a aah application
Desc: The layout of a aah application is standardized to keep things as simple as possible and has component of MVC architectural pattern.
Keywords: anatomy, layout, mvc, architectural, directories, aah application layout, aah application anatomy
---
# Anatomy of a aah application

The layout of a aah application is standardized to keep things as simple as possible and has component of MVC architectural pattern.

```
app                   ⇒ Application Go source codes directory
  └ controllers       ⇒ Application controllers
  └ models            ⇒ Application business layer
  └ security          ⇒ Application security implementation
  └ aah.go            ⇒ aah application main entry point (generated code)
config                ⇒ Configurations files
  └ aah.conf          ⇒ Application configuration file
  └ routes.conf       ⇒ Application Routes configuration file
  └ security.conf     ⇒ Application Security configuration file
  env                 ⇒ Environment profile configurations
    └ dev.conf        ⇒ Dev - Environment profile config
    └ prod.conf       ⇒ Prod - Environment profile config
i18n                  ⇒ Internationalization and Localization message files
  └ messages.en       ⇒ Message file
  └ messages.en-US    ⇒ Message file
  └ messages.en-CA    ⇒ Message file
views                 ⇒ Template files - not created for API (aah supports partial inheritance)
  └ common            ⇒ Common view template files, it can be imported to any page template
  └ layouts           ⇒ Application view layout files, master template for page template
  └ pages             ⇒ Page template files, corresponding template for controller action
static                ⇒ Static public assets - not created for API
  └ css               ⇒ CSS files
  └ js                ⇒ Javascript files
  └ img               ⇒ Image files
logs                  ⇒ Logs directory (default is console on 'dev' profile)
  └ app-name.log      ⇒ Application log file
tests                 ⇒ Go test source directory (upcoming), use Go standard way for unit tests
build                 ⇒ Application build directory
.gitignore            ⇒ Typical Go .gitignore file and aah project ignore files
aah.project           ⇒ aah project configuration; build config, hot-reload, etc.
app-name.pid          ⇒ Application PID file (created during app startup)
```

#### Packaged aah application will have following directories/files
```
bin                    ⇒ Application binary file
aah.sh                 ⇒ *nix startup file
aah.cmd                ⇒ Windows startup file
```

#### Packaged aah application won't have following directories/files
```
app                    ⇒ Application Go source directory (compiled into binary under `bin` directory)
logs                   ⇒ Logs directory (created during app startup)
tests                  ⇒ Go source codes directory (not for production)
.gitignore             ⇒ Typical Go .gitignore file (not for production)
aah.project            ⇒ aah project configuration (not for production)
app-name.pid           ⇒ Application PID file (created during app startup)
```

### The `app` directory

The `app` directory contains all the Go source codes for the application. Initially `controllers` and `models` directory gets created by `aah new` command. Of course you can have as many as packages in the app directory for your application.

### The `config` directory

The `config` directory contains the application's configuration files. aah configuration file syntax is very similar to HOCON syntax. The quick start application will have the following config files. Organize the way you want it.

  * `aah.conf` - the main configuration file for the application, [know more](app-config.html).
  * `routes.conf` - the routing configuration file for the application, [know more](routes-config.html).
  * `security.conf` - the security configuration file for the application, [know more](security-config.html).

### The `i18n` directory

The `i18n` directory contains internationalization and localization message files. Message filename format is  `message.<Language-ID>`. Language is combination of `Language + Region` value. aah framework implements Language code as per  two-letter `ISO 639-1` standard and Region code as per two-letter `ISO 3166-1` standard, [know more](i18n.html).

### The `views` directory (Not applicable to API application)

The `views` directory contains application view templates. aah framework supports partial inheritance with Go template engine, [know more](views.html). There are three main directories:

  * `common` - Common view template files, it can be imported to any page template via `import` template func.
  * `layouts` - Application view layout template files. You can have as many you want.
  * `pages` - Page view template files. Respective view file for controllers action.
      - For example: `<controller-name>/<action-name>.<ext>`
      - For example: `/<namespace>/<controller-name>/<action-name>.<ext>` (namespace directory structure is supported)

### The `static` directory (Not applicable to API application)

The `static` directory contains static assets that are served directly. It contains three sub-directories for images, CSS and JavaScript files.

  * By default `static` directory mapped as `/static` in [routes.conf](routes-config.html). You can customize it in the config. For example: `/assets`
  * You can use several directories under `static` directory as per your need. Just organize it appropriately, it helps you :)

***Note: If you want you can use `static` directory for file delivery for your API application.***

### The `logs` directory

The `logs` directory is for application logs and default location of server access log.

### The `tests` directory

The `tests` directory is for application functional tests (`upcoming`). Use Go conventional way `<source-file>_test.go` for unit test your Go source codes.

### The `.gitignore` file

The `.gitignore` file includes standard Go language ignores from Github and additionally aah framework ignores:

  * `aah.go`, `*.pid`, `build/*`
