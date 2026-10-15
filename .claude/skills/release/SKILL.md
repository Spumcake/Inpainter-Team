---
name: release
description: Publish an owner-named version people can download. Confirm every target's build passed with the build skill, prepare the release branch and notes, have the owner tag, let CI build and attach the artifacts with checksums, then verify the published release. Use when the owner asks to publish a version or install the release workflow.
---

# Publish a release

A release is a version tag on the release branch, normally `main`, with the built artifacts, a checksum file, and release notes on the repository host's release page. It relies on the **build** skill, which decides the targets, packages them, and verifies the artifacts. This skill turns a verified build into a published version and checks what was published.

Only when the owner asks to publish a version or install the release workflow. Do not merge, tag, or push unless the owner asks for that step.

## Choose the version

The owner names the version. Without a project convention, use semantic versions: a patch, such as 1.0.1, for fixes and packaging that do not change what the app does; a minor version for new features; a major version for milestones or incompatible changes to stored data. Never reuse or move a published tag.

## Install the workflow

Once per project, adapt [the workflow template](templates/release.yml) into the app repository's workflow folder. It must:

- run only for version tags;
- fail unless the tag matches the project's version and the tagged commit is on the release branch;
- fail unless release notes for that version exist in the repository;
- build each ready target with its packaging script on its base system, including the test suite;
- publish every artifact and a `SHA256SUMS` file under the title "<product> <version>", with the version's notes as the body, and fail if an expected artifact is missing.

Pin actions and downloaded tools to versions. The workflow and each target's packaging script must agree; the target's build document names both.

## Before tagging

Confirm and report each item:

- the project version on the commit to be tagged is the owner's version;
- `build/targets.md` is current, and each ready target's document records a passing build of that commit;
- the release notes exist where the workflow reads them, normally `release-notes/v<version>.md`;
- the commit is on the release branch, which the owner merges on GitHub;
- the README's install sections match the artifact names;
- the repository has a license, and the artifacts carry it with any third-party licenses.

## Release notes

Write them for a person deciding whether to download, from the diary entries and commits since the previous release. Start with what changed for them: new behavior, fixes, and install or compatibility changes. Then the supported systems, what the host must provide, known issues, and how to check a download against `SHA256SUMS`. Plain sentences, no commit hashes or internal names.

## Publish

The owner tags and pushes. Give the commands with the owner's version, and run them only if asked:

```bash
git tag -a v<version> <commit> -m "<product> <version>"
git push origin v<version>
```

## Verify the release

After the workflow finishes, check the published release, not the local build: the title, notes, every expected artifact, and `SHA256SUMS` are on the page; each download matches its checksum; each artifact starts on a supported system. Record the result in each target's build document and report the release URL. If the workflow failed, report the failing step; never claim artifacts exist before the workflow attached them.

## A bad release

Never move, delete, or reuse a published tag. Fix the problem on `dev`, build it, and publish the next patch version, adding a line to the faulty release's notes that points to the fix. Deleting or hiding a release is the owner's decision.
