# Release & Development Process

This page would give an idea how aah framework project Release and Development Process. I will be improving process efficiency  iteratively as project moves on. For now this set is to begin with.

## Table of Contents

  * [Insights](#insights)
  * [Release Cycle](release-cycle)
  * [Process to Release Framework/Library](#process-to-release-framework-library)
  * [Broadcast Framework Release](#broadcast-framework-release)

## Insights

aah framework internally uses `gopkg.in` codebase for [Package Versioning](versioning.html) and commitment to stable release version in-additional to user choice of package management.

  * Github `Tag` is Stable and Production ready.
  * Default branch is `v0-unstable`, `v1-unstable`, and so on. Development, Bug fix, Pull Request, happens on that.
      - Note: master is not a default branch for aah framework.
  * Branch `master` is kept as tidy codebase of aah framework.
  * In the being of every development iteration, version is updated from `v0` set to `v0-unstable` (v1, etc.)
  * In the release preparation package references are updated from `v0-unstable` to `v0` (v1, etc.)
  * Travis build, test cases and few manual dry-testing to ensure codebase is stable & ready for the release.
  * PR created from `v0-unstable` branch (v1, etc.) to `master` branch.
  * Release tag gets created from latest `commit sha`, typically readme file update commit for the release.

## Release Cycle

  * Security Issue - addressed as soon as possible, refer [how to report security vulnerabilities?](https://aahframework.org/security/vulnerabilities.html).
  * Features, Enhancements, Bug fixes is planned to release every 3 months cycle (being realistic).
      - Take a look on [Roadmap](https://github.com/go-aah/aah/projects/3).

## Process to Release Framework/Library

  * Make good effort for Unit testing and code coverage
  * Check Travis Build is alright and you get green ticket on Github.
  * Execute shell script with `library-name`, `version` and `release` as parameters.
      - `bash <(curl -s https://aahframework.org/dev-util) router v0 release`
      - It basically does find and replace of `v0-unstable` to `v0`.
  * Commit it as `preparing for v<version-number> release`.
      - For e.g.: `preparing for v0.5.2 release`.
  * Update `README.md` with appropriate information (status icons URLs, version no, date, etc).
  * Commit it as `readme update for v<version-number> release [ci skip]`.
      - Typically used for creating release tag with this `commit sha`.
      - We are building `branch` and `master` that is good enough for consistency check and release. Let's use the Travis CI resources optimally for aah framework project.
  * Push it to repo.
  * Create a `PR` from branch `v0-unstable` to `master` and Merge it.
  * Update the Documentation on `go-aah/docs` repo.
      - On `go-aah/docs` branch `master` is always the latest release.
      - Create a branch from master for previous release.
          - For e.g.: new release `v0.6` so create branch called `v0.5` from master (preserving previous release docs).
      - Those previous release branch docs are accessible from https://docs.aahframework.org by version-number.
      - Update the documentation for release and add Release Notes, Changelogs and Migration Info.
      - Push it `go-aah/docs` repo.
  * Create Release Tag and Publish
      - Tag name must comply with `SemVer` For example: v0.1, v1.0, v1.0.1 etc.
      - Release Title is `v<version-number> Release`.
      - In Describe release text box, put the links to official website for release notes, changelogs, migration info.
      - Hit the Publish release button.
  * Refer Broadcast section about making release announcement.

## Broadcast Framework Release

We have to broadcast new release of Framework on -

**aah community:** Framework and Libraries release.

  * Twitter: https://twitter.com/aahframework
  * Gitter Online Chat: https://gitter.im/aahframework/community
  * Google Groups: https://groups.google.com/forum/#!forum/aahframework

**Global Audience:** Only Framework release.

  * Golang-nuts: https://groups.google.com/forum/#!forum/golang-nuts
  * Golang Reddit: https://www.reddit.com/r/golang/
