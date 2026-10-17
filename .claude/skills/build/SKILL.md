---
name: build
description: Decide which build targets an app is ready for, from what it is and where it actually ran during development; assess each target's conventions, install, lifecycle, and the interface changes its platform needs; then package and verify the artifacts. The release skill relies on it. Use when the owner asks for a build or a release. Not for publishing.
---

# Build an app for its targets

A build is the app packaged for one target, an operating system, architecture, and package format, so a person there can install it, run it, and keep using it. Two failures are common. Building for a platform the app has never run on ships an untested app. Shipping only the compiled program ships something that does not behave like an app on its platform: no launcher entry, no icon, files that will not open from the file manager, and a binary that fails on the systems people actually use.

Use it when the owner asks for a build, or when the **release** skill needs one. Not on every commit or milestone.

## 1. Choose the targets

If the owner named targets, build those, and still report the evidence for each. When a named target has no record of the app running there, say so and ask before building it. Otherwise decide from the records, not from what the stack could compile:

- **What the product is:** the overview, pitch, tasks, and INTERFACE say whether it is a desktop window, a phone app, a web page, or a service, and which platforms its boundaries exclude.
- **Where it actually ran:** the testing record, diary entries, and the owner's reviews say which systems, display servers, architectures, and devices it ran and was checked on.
- **What the code assumes:** platform-specific code, input devices, dependencies that work only on some systems, and paths or services it expects.

Sort every target the stack could produce:

- **Ready:** the app ran there with the core journey checked, or the owner confirmed it there. Build it.
- **Untested:** the stack supports it, but no record shows the app running there. Do not build it; tell the owner what would make it ready, normally a run of the core journey there.
- **Out of scope:** the product's form or boundaries exclude it. Do not build it, and do not ask.

Record the decision in `<version folder>/build/targets.md` from [the template](templates/TARGETS.md).

## 2. Research a new target

A target built for the first time needs a brief on how comparable apps are distributed, installed, updated, and removed there, using the **journeys** skill; the platform's own guidelines count as references. It suits a background agent while you start the assessment. Reuse an existing brief that covers the target.

## 3. Assess each ready target

Inspect the app as it exists: the libraries it links and loads, the files and folders it reads and writes, its identity strings (application id, window class, data-folder names), how it receives files from the operating system, and how its window is framed. Write `<version folder>/build/<target>.md` from [the template](templates/TARGET.md): conventions, install, lifecycle, interface accommodations, build, and the builds record.

Ask the owner before implementing anything that changes what a person receives: package formats, the oldest supported system, a new dependency, or an interface change. Routine choices follow the brief.

## 4. Package

In the app repository:

- the app changes the assessment calls for, such as an application id or a window icon;
- the platform integration files, such as desktop entries, icons, file-type declarations, and catalog metadata, in one packaging folder;
- the project's license, MIT unless the owner chose another, and any third-party licenses, inside every artifact;
- one packaging script per target that builds, runs the test suite, and writes the artifacts and nothing else, run the same way by CI and by a developer;
- the target's job in the release workflow, from the **release** skill's template;
- the README's install section for the target;
- the project version set to the version being built, without a `v` prefix.

Separate targets are independent parts: package them in parallel, one worktree each, when there are several.

## 5. Build and verify

Build each artifact the way CI will, on the target's base system, such as a container of the oldest supported system, not on a newer development machine. Before an artifact goes to release, check:

- the test suite passes on that base;
- the artifact starts on the base system and on the development system, and the core journey's first step works where a display is available;
- the integration files pass the platform's validators;
- it needs nothing from the host beyond what the README names, checked from its linked libraries and required symbol versions;
- every artifact has a checksum.

Record the results in the target's document. An artifact that failed a check does not go to release. Report the targets decision, the artifact paths and checksums, the checks run and not run, and any decision waiting on the owner. Building does not authorize a commit, a tag, or a publication.
