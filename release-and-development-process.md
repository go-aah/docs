Title: Release & Development Process
Desc: aah framework project Release and Development Process.
Keywords: release process, development process, dev process, release cycle
---
# Release & Development Process

This document would give an idea about aah Development and Release Process. I will be improving process efficiency iteratively as project grow.

### Table of Contents

  * [Insights](#insights)
  * [Release Cycle](#release-cycle)
  * [API Stability](#api-stability)
  * [Process to Release Framework and Library](#process-to-release-framework-and-library)
  * [Broadcast Framework Release](#broadcast-framework-release)

## Insights

aah internally uses `gopkg.in` codebase for [Package Versioning](versioning.html) and commitment to provide stable release version in-addition to the user choice of package management tools like glide, govendor, etc.

  * Github `Tag` is Stable and Production ready.
  * Default branch is `v0-edge`, `v1-edge`, and so on. Development, Bug fix, Pull Request, happens on that.
      - Note: master is not a default branch.
  * Branch `master` is kept as tidy codebase of aah.
  * In the being of every development iteration, package version number increment and updated to `0.1.0-edge`
  * In the release preparation package version is update from `0.1.0-edge` to `0.1.0`
  * Travis build, test cases and manual testing to ensure codebase is stable & ready for the release.
  * PR created from `v0-edge` branch (v1, etc.) to `master` branch.
  * Release tag gets created from latest `commit sha`, typically readme file update commit of the release.

## Release Cycle

  * Security fix is gets addressed and released as soon as possible, refer to [how to report security issues?]({{aah_domain_url}}/security/vulnerabilities.html).
  * Bug fixes and minor enhancements are planned to release on monthly basis.
      - Please Note: If zero bug fixes then this release may not happen.
  * Feature and Enhancement is planned to release every 3 to 6 months cycle.
      - Smaller feature or enhancement might get released earlier.
      - Have a look on [Roadmap](https://github.com/go-aah/aah/projects/3).

## API Stability

aah and it's libraries will be maintained in a compatible way `between major version` unless some vital bugs prevent me from doing so. I don't take API changes lightly.

## Process to Release Framework and Library

  * Make good effort for Unit testing and code coverage
  * Check Travis Build is alright and you get green ticket on Github
  * Update `README.md` with appropriate information (status icons URLs, version no, date, etc)
  * Commit it as `readme update for v<version-number> release [ci skip]`
      - Typically used for creating release tag with this `commit sha`
      - We are building `branch` and `master` that is good enough for consistency check and release. Let's use the Travis CI resources optimally for aah project
  * Push it to repo
  * Create a `PR` from branch `vX-edge` to `master` and Merge it
  * Update the Documentation on `go-aah/docs` repo
      - Branch `master` is always the latest release documentation
      - Create a branch from master for previous release
          - These previous release branch docs are accessible from https://docs.aahframework.org by version-number
          - For e.g.: new release `v0.6` so create branch called `v0.5` from master (preserving previous release docs)
      - Update the documentation on the branch `master` for the release and add Release Notes, Changelog and Migration Info
      - Push it `go-aah/docs` repo
  * Create a Release Tag and Publish
      - Tag name must comply with `SemVer` For example: v0.1, v1.0.0, v1.0.1 etc
      - Release Title is `v<version-number> Release`
      - In `Describe` release text box, put the links to official website for release notes, changelog, migration info. Typically https://docs.aahframework.org/release-notes.html
      - Hit the `Publish` release button
  * Refer [Broadcast](#broadcast-framework-release) section for making release announcement

## Broadcast Framework Release

We have to broadcast new releases of framework and libraries on -

**aah community:** Framework and Libraries release.

  * Twitter: https://twitter.com/aahframework
  * Gitter Online Chat: https://gitter.im/aahframework/community
  * Google Groups: https://groups.google.com/forum/#!forum/aahframework
  * Google+: https://plus.google.com/u/0/102130029195315523240

**Global Audience:** Only Framework release.

  * Golang Nuts: https://groups.google.com/forum/#!forum/golang-nuts
  * Golang Reddit: https://www.reddit.com/r/golang/
  * Golang Google+: https://plus.google.com/communities/114112804251407510571
  * Golang Slack: https://gophers.slack.com/messages/showandtell/

