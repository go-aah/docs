Title: Release & Development Process
Desc: Insight into aah framework project Release and Development Process.
Keywords: release process, development process, dev process, release cycle
---
# Release & Development Process

This page would give an idea how aah framework Development and Release Process. I will be improving process efficiency iteratively as project moves on.

## Table of Contents

  * [Insights](#insights)
  * [Release Cycle](#release-cycle)
  * [Process to Release Framework/Library](#process-to-release-framework-library)
  * [Broadcast Framework Release](#broadcast-framework-release)

## Insights

aah framework internally uses `gopkg.in` service for [Package Versioning](versioning.html) and commitment to provide stable release version in-addition to user choice of package management.

  * Github `Tag` is Stable and Production ready.
  * Default branch is `v0-unstable`, `v1-unstable`, and so on. Development, Bug fix, Pull Request, happens on that.
      - Note: master is not a default branch for aah framework.
  * Branch `master` is kept as tidy codebase of aah framework.
  * In the being of every development iteration, version is updated from `v0` set to `v0-unstable` (v1, etc.)
  * In the release preparation package references are updated from `v0-unstable` to `v0` (v1, etc.)
  * Travis build, test cases and few manual dry-testing to ensure codebase is stable & ready for the release.
  * PR created from `v0-unstable` branch (v1, etc.) to `master` branch.
  * Release tag gets created from latest `commit sha`, typically readme file update commit of the release.

## Release Cycle

  * Security fix is gets addressed and released as soon as possible, refer [how to report security vulnerabilities?](https://aahframework.org/security/vulnerabilities.html).
  * Bug fixes and minor enhancements are planned to release every month on 3rd week
      - Please Note: If zero bug fixes then this release may not happen.
  * Feature and Enhancement is planned to release every 3 to 4 months cycle
      - Smaller feature or enhancement might get released earlier.
      - Have a look on [Roadmap](https://github.com/go-aah/aah/projects/3).

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
          - These previous release branch docs are accessible from https://docs.aahframework.org by version-number.
          - For e.g.: new release `v0.6` so create branch called `v0.5` from master (preserving previous release docs).
      - Update the documentation on the branch `master` for the release and add Release Notes, Changelog and Migration Info.
      - Push it `go-aah/docs` repo.
  * Create a Release Tag and Publish
      - Tag name must comply with `SemVer` For example: v0.1, v1.0, v1.0.1 etc.
      - Release Title is `v<version-number> Release`.
      - In `Describe` release text box, put the links to official website for release notes, changelog, migration info.
      - Hit the `Publish` release button.
  * Refer Broadcast section about making release announcement.

## Broadcast Framework Release

We have to broadcast new release of Framework on -

**aah community:** Framework and Libraries release.

  * Twitter: https://twitter.com/aahframework
  * Gitter Online Chat: https://gitter.im/aahframework/community
  * Google Groups: https://groups.google.com/forum/#!forum/aahframework
  * Google+: https://plus.google.com/u/0/102130029195315523240

**Global Audience:** Only Framework release.

  * Golang-nuts: https://groups.google.com/forum/#!forum/golang-nuts
  * Golang Reddit: https://www.reddit.com/r/golang/
