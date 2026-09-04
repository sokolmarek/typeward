---
title: How updates work
description: Typeward checks GitHub for a newer release, verifies its signature, and installs it only after you confirm, plus how to update by hand.
---

This page explains how Typeward moves you to a newer version, what an update check sends, and how to update by hand instead. Release builds from v0.1.0 onward check for updates and can install them, and nothing installs without your confirmation.

## The check after launch

With **Check automatically** on, Typeward runs one silent check about ten seconds after launch. The delay keeps the check off the startup path, so it never slows the window appearing. Failures on this path stay silent, so an offline launch or an unreachable manifest never interrupts you.

If a newer version exists, two things happen. A notification appears naming the new version, in the form **Typeward 0.2.0 is available** with the body **You're on 0.1.0. Open Settings to install it.**, and the **Update available** dialog opens. That notification is shown once per version, so a release you keep postponing does not announce itself at every launch, while a newer release does. The dialog does not block the app, so you can keep working and decide later.

With the toggle off, no automatic check runs, and manual checks from **Check now** still work. The toggle is a local setting stored with the rest of your preferences on this machine, and nothing about it travels anywhere. See [Settings reference](/reference/settings/).

## What a check sends

A check is a plain HTTPS `GET` for `latest.json`, a manifest attached to the newest release in the `typeward/typeward` repository on `github.com`. No account, no install identifier, no telemetry. The hint under **Check automatically** states the same guarantee: "Shortly after launch, look for a newer release and prompt you to install it. The check is a plain HTTPS GET to GitHub with no identifiers, and updates never install without your confirmation." See [Privacy and network behavior](/reference/privacy-and-network/).

## The update prompt

The **Update available** dialog names the new version and the one you are on. Under **What's new**, it shows the release notes as plain text straight from the release. A release without notes says so. The dialog offers two choices:

- **Later** dismisses the dialog. Nothing has been downloaded.
- **Install and relaunch** downloads the update, shows **Downloading… NN%** while it runs, then closes the app and reopens it on the new version.

The dialog states the consequence: "The app closes and reopens on the new version. Unsaved work is saved by autosave first." Nothing installs without that button. Once the download starts the dialog cannot be dismissed, because the relaunch takes over from there. On Windows the installer runs in passive mode and replaces the running executable as part of the handoff.

If an install fails, an **Update failed to install** message appears and the app keeps running on the current version. Nothing is half-applied.

## Package signatures

Every update package carries an updater signature, checked against a public key compiled into the app. Typeward rejects a package that does not verify instead of installing it. Each platform entry in the release manifest carries such a signature.

That signature is separate from operating-system code signing, which the installers do not have. SmartScreen and Gatekeeper still warn about a freshly downloaded installer, and [License and third-party components](/reference/license/) explains why. The update path does not depend on those operating-system signatures, because it verifies its own.

## The manual check

A manual check always reports its outcome, unlike the silent one. **Settings → About** has a **Check for updates** row with a **Check now** button, which shows **Checking…** while it runs. The result is then one of three: the **Update available** dialog, a **You're up to date** toast reading **No newer version available.**, or a **Couldn't check for updates** error carrying the reason.

## Installs from a .deb or .rpm package

A release publishes one signed artifact per package type, and a `.deb` can only be updated by a `.deb`. The v0.1.0 manifest carries entries for the `.deb`, `.rpm`, and AppImage builds on both architectures, so an in-app update works for each of them.

Should a release ever omit the artifact matching how you installed, a check says so rather than failing. On a copy installed from a `.deb` or `.rpm`, the message reads **Updates come from your package manager**, and it points you at installing the new version the same way or downloading it from the releases page. On any other install kind, the message reads **No update available for this build**.

## Manual installation

Updating by hand stays available whether or not the updater is on. Download the newest installer from the [Typeward releases page](https://github.com/typeward/typeward/releases), verify it, and run it over the installation you have. The per-platform steps are on [Install on Windows](/getting-started/install-windows/), [Install on macOS](/getting-started/install-macos/), and [Install on Linux](/getting-started/install-linux/).

Each release carries installers for Windows, macOS, and Linux, a `SHA256SUMS` file listing every asset, and the changelog for that version. Verify the download against `SHA256SUMS` before you run it.

Your projects and settings survive the reinstall. They live outside the installation, so replacing the application in place keeps every project, template, custom theme, and preference. See [Data locations, credentials, and uninstall](/reference/data-locations/).

If you built Typeward yourself, pull the repository and repeat the [Build from source](/getting-started/build-from-source/) steps.

## Builds with no update key

The update public key lives in the app configuration and is read at build time. A build without it, such as a fork that strips the key, keeps the updater dormant rather than broken: no launch check runs, and **Check now** answers with an information toast reading **Updates aren't configured yet**, with the body **This build predates automatic updates. Grab new versions from the Typeward releases page.** No request leaves the machine in that state.

Published releases carry the key, so this applies to your own builds rather than to an installer from the releases page.

## The version you are running

The **Version** row in **Settings → About** shows the exact build, taken from the package version at build time. That is the number to quote in a bug report, and the number an update check compares against.

## See also

- [Privacy and network behavior](/reference/privacy-and-network/)
- [Settings reference](/reference/settings/)
- [Data locations, credentials, and uninstall](/reference/data-locations/)
- [Build from source](/getting-started/build-from-source/)
