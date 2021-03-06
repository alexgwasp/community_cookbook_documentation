# How We Release Software

This guide for our maintainers lists the process for taking community contributions and releasing cookbook artifacts for consumption by the community.

## Release on every contribution

Smaller changes incur less risk, and releasing after each contribution ensures we get the changes to users and contributors as quickly as possible. We aim to ship new cookbook releases after any bugfix or feature enhancements are merged. For cookbooks where automated testing has not been implemented there be some delay, as manual testing is necessary.

## Follow Semver

Infrastructure stability for our users is critical so we aim to release predictably versioned cookbooks. Following Semver allows users to pin cookbook requirements in wrapper cookbooks or environment files, and avoid breaking changes. See <http://semver.org/>

Cookbooks follow a `MAJOR.MINOR.PATCH` versioning scheme based on [Semantic Versioning](http://semver.org/).  

Given a version number `MAJOR.MINOR.PATCH`:

    `MAJOR` version releases (e.g. 1.x -> 2.x) will include breaking or backwards-incompatible changes.
        Example: When updating a cookbook to use new chef-client features that are not available in older versions.
        Example: When removing support for a platform, or version of a platform.
    `MINOR` version releases (e.g. 1.1 -> 1.2) will include new features, bug fixes, and will be backwards-compatible to the best of the Maintainers' abilities.
        Example: When adding a new custom resource to a cookbook.
    `PATCH` version releases (e.g. 1.1.1 -> 1.1.2) will include backwards-compatible bug fixes.
        Example: Fixing a misspelled attribute in a recipe.

When incrementing a cookbook version, the following conditions will apply:

    When `MAJOR` increases, `MINOR` and `PATCH` will be reset to zero (e.g. 11.X.X -> 12.0.0)
        Note: New features that did not exist in version 1.1.0 may be released in 2.0.0 without any intermediary releases.
    When `MINOR` increases, `PATCH` will be reset to zero (e.g. 11.3.x -> 11.4.0)

## Test Before Releasing

Many cookbooks have integration tests that run using kitchen-dokken in Travis CI. For these cookbooks we're preforming integration testing on every commit. For others integration tests must be run locally and this should be done before any release to ensure high quality releases.

## Always update the changelog

Understanding changes between releases is important for users consuming our cookbooks. Each release should include a dated changelog entry that includes detailed descriptions of changes. Breaking changes should be clearly called out, and best effort should be made to describe the impact of changes instead of just including PR titles.

## Use Stove for tag management / file pruning

Cookbook artifacts on Github are rather large and include files such as Gemfiles, test cookbooks, and large numbers of other development files. These files are not necessary for the cookbook to function in an end user environment, and we've chosen to use Stove to package a minimal cookbook artifact for upload to the Supermarket. Stove includes an internal whitelist of files, which will be included in the Supermarket artifact, and also handles tagging the git repo on release.
