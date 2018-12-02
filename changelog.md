## Changelog

#### Features

  * Console Module - Ability to add CLI commands into aah application easily [#198]({{aah_issues_url}}/198), [documentation](console-commands.html)
  * Cache Manager - Pluggable cache datastores. OOTB - In-memory, Redis, Memcache supported [#203]({{aah_issues_url}}/203), [documentation](cache.html)
  * Diagnosis & Profiling - provides on-demand profiling feature [#216]({{aah_issues_url}}/216), [documentation](diagnosis-and-profiling.html)
  * View Engine - added partials support for template include anywhere under `<app-base-dir>/views/**` [#217]({{aah_issues_url}}/217), [documentation](template-funcs.html#func-include)

#### Enhancements

  * Go Modules - aah framework fully adapted to Go modules and supports as default package management [#200]({{aah_issues_url}}/200)
    - Enhanced command `aah migrate` logic and grammar to do incremental code migration to current version aah    
    - Enhanced command `aah list` to work with Go modules and aah applications
    - Enhanced command `aah new` and quick start application template with Go Modules support
    - Removed command `aah update` and `aah switch` in-favor of Go modules
    - Removed `.v0` version strategies in-favor of Go Modules
    - Removed `--importpath` argument from CLI commands
    - Introduced new face of import path name `aahframe.work`
  * Enhanced in-home routing implementation using Radix-tree algorithm - One of the notable feature is "Coexistence of static path segment and path parameter segment". [#160]({{aah_issues_url}}/160), [documentation](routing.html)
  * Configurable Signal for Hot-Reload on non-dev environment profiles and publishes Application event `OnConfigHotReload` [#218]({{aah_issues_url}}/218), [documentation](configuration-hot-reload.html)
  * Added optional `catch-all` route support in the router, disabled by default [#212]({{aah_issues_url}}/212), [documentation](routes-config.html#catch-all-route-configuration)
  * Organized the auto-generated files under directory `<app-base-dir>/app/generated` [#199]({{aah_issues_url}}/199)
  * Improved HTML minify library - now does inline CSS and JS minify too `aahframe.work/minify/html`
  * Overall code, performance and documentation improvements along with Go Modules capability

#### Bug Fixes

  * Fixed - Access log: static files get logged always (twice) [#219]({{aah_issues_url}}/219)
  * Fixed - Hot reload does not detect changes [#215]({{aah_issues_url}}/215)