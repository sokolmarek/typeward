---
title: Install on Linux
description: Install Typeward on Linux from the AppImage, the .deb, or the .rpm, verify the download, and set up a keyring for optional integrations.
---

This guide installs Typeward on Linux from the AppImage, the `.deb`, or the `.rpm`, and ends with the app running. For another platform, see [Install on Windows](/getting-started/install-windows/) or [Install on macOS](/getting-started/install-macos/).

## Requirements

- **glibc 2.35 or newer.** The Linux packages are built on Ubuntu 22.04, which sets that floor. It matters most for the AppImage, which carries no dependency metadata and fails to start on an older system.
- **WebKitGTK 4.1.** The `.deb` declares `libwebkit2gtk-4.1-0`, `libgtk-3-0`, and `libdbus-1-3`, so your distribution has to package WebKitGTK 4.1 for the install to resolve.
- **An X11 or a Wayland session.** Both work.
- **No TeX distribution on x86_64.** Those packages bundle the Tectonic engine, so LaTeX compiles without one. Typeward uses a TeX distribution instead when you have one installed.
- **A TeX distribution on ARM64.** The ARM64 packages ship without the Tectonic engine that the x86_64 packages carry, because upstream Tectonic has no build for every ARM target. A `tectonic` command you install yourself works too. See [Choosing a compile engine](/getting-started/compile-engines/).
- **The `typst` command for Typst projects.** No Linux package bundles it. See [Typst projects](/getting-started/typst/).

## Download and install

Every release attaches three package formats, each built for x86_64 and for ARM64. Pick the one your distribution uses.

| Package | Distributions | File name on x86_64 | File name on ARM64 |
| --- | --- | --- | --- |
| AppImage | Anything at or above the glibc floor, with nothing to install, Arch and other rolling releases included | `Typeward_<version>_amd64.AppImage` | `Typeward_<version>_aarch64.AppImage` |
| `.deb` | The Debian family: Debian 12 and newer, Ubuntu 22.04 and newer, Mint, Pop!_OS, elementary | `Typeward_<version>_amd64.deb` | `Typeward_<version>_arm64.deb` |
| `.rpm` | The RPM family: Fedora 36 and newer, openSUSE, the RHEL derivatives | `Typeward-<version>-1.x86_64.rpm` | `Typeward-<version>-1.aarch64.rpm` |

Every asset carries the release number in its file name, so `<version>` throughout this page stands for that number, for example `0.1.0`. Linux is one build per architecture rather than one per distribution, and the `.rpm` name uses hyphens and a release suffix where the other two use underscores.

1. On the [Typeward releases page](https://github.com/typeward/typeward/releases), download the package for your distribution and architecture.
2. Verify the file with the checksum and provenance checks in [Verify your download](#verify-your-download).
3. Install the package with the steps for its format: [Run the AppImage](#run-the-appimage), [Install the Debian package](#install-the-debian-package), or [Install the RPM package](#install-the-rpm-package).

### Run the AppImage

1. Make the file executable with `chmod +x Typeward_<version>_amd64.AppImage`.
2. Start Typeward with `./Typeward_<version>_amd64.AppImage`.

The AppImage is self-contained and runs from wherever you put it. It installs no desktop entry and registers no file types.

### Install the Debian package

Install the package with apt, which resolves the dependencies the package declares on WebKitGTK, GTK, and D-Bus:

```sh
sudo apt install ./Typeward_<version>_amd64.deb
```

### Install the RPM package

Install the `.rpm` with your package manager:

```sh
sudo dnf install ./Typeward-<version>-1.x86_64.rpm
```

On openSUSE, use `sudo zypper install` with the same file name. If your package manager reports a missing WebKitGTK, install your distribution's WebKitGTK 4.1 package and run the command again.

### Set Typeward as the default editor

The `.deb` and the `.rpm` install a desktop entry, so Typeward appears in your application menu. Both register three file types: `.tex` (**LaTeX Source**, `text/x-tex`), `.typ` (**Typst Source**, `text/x-typst`), and `.bib` (**BibTeX Bibliography**, `text/x-bibtex`). Opening one of those files from your file manager launches Typeward with the file, or hands it to the running window when Typeward is already open.

Registering the types does not always make Typeward the default application for them. If your file manager still opens `.tex` files somewhere else, set Typeward as the default from your file manager's Open With dialog.

## Verify your download

Every release attaches a `SHA256SUMS` file listing the checksum of every asset. A matching checksum proves the file was not corrupted or swapped in transit.

1. Download `SHA256SUMS` from the same release into the folder holding your package.
2. Check what you downloaded with `sha256sum -c SHA256SUMS --ignore-missing`, which prints `OK` for every file that matches.

If a file fails, delete it and download it again. Never run it.

Each asset also carries a GitHub build-provenance attestation. With the [GitHub CLI](https://cli.github.com/) installed, this command confirms the file came out of the release workflow in the Typeward source repository:

```sh
gh attestation verify Typeward_<version>_amd64.AppImage --repo typeward/typeward
```

## Unsigned packages

Typeward packages are not signed, so the checksum and the attestation are what establish that a file is the one that was released. [License and third-party components](/reference/license/) explains why the builds are unsigned. Every release repeats both checks in a "Verify your download" section of its release notes, alongside a section on code signing. The updater signature is a separate mechanism, and every release does carry that. See [How updates work](/reference/updates/).

## Check that it worked

You should now see the Typeward window, which starts with the first-run setup on a new installation.

If Typeward does not start:

1. Start Typeward from a terminal, so that any startup error is printed rather than lost.
2. Check that your glibc is 2.35 or newer with `ldd --version`.
3. For the AppImage, check that your distribution provides FUSE 2, which Debian and Ubuntu package as `libfuse2`. AppImages mount themselves with FUSE.
4. For the `.deb` or the `.rpm`, check that your distribution's WebKitGTK 4.1 package is installed.

## Set up a keyring for integration credentials

Typeward has no account and no sign-in, so nothing about the app itself needs a keyring. The system keyring, reached through the Secret Service D-Bus API, holds a credential only when you connect an optional integration that has one to store:

- A WebDAV password for [cloud sync](/projects/cloud-sync/)
- A Zotero Web API key, or a Mendeley client secret and token, for [reference managers](/references/connecting-reference-managers/)
- An API key for a cloud [AI provider](/ai/overview/)

GNOME desktops get Secret Service from `gnome-keyring`, and KDE Plasma gets it from KWallet, so nothing needs setting up there. Minimal and window-manager setups (i3, Sway, and similar) often run no keyring service at all. To store integration credentials on one of those, install and start `gnome-keyring` or KWallet.

Without a keyring the rest of the app is unaffected: editing, compiling, preview, templates, autosave, and version history all work. [Git in Typeward](/projects/git/) works too, because git remotes use your own git credential helper and never the app keyring. When no Secret Service provider is available, saving a credential fails with a clear error instead of succeeding quietly. Typeward never falls back to writing secrets into a file.

The keyring may ask you to unlock it the first time Typeward stores or uses a credential. That prompt comes from your desktop environment, not from Typeward. Entries are named `typeward.<provider>`, so you can find and delete them in Seahorse, KWalletManager, or `secret-tool`.

## Updating

Typeward checks for updates on its own. About ten seconds after launch, a release build looks for a newer version and raises the **Update available** dialog if it finds one. Nothing downloads until you select **Install and relaunch**, and you can turn the launch check off with **Check automatically** under **Settings → About**. A release publishes one signed package per format, so an AppImage updates from an AppImage, a `.deb` from a `.deb`, and an `.rpm` from an `.rpm`.

To update by hand instead:

1. Download the newer package from the [Typeward releases page](https://github.com/typeward/typeward/releases).
2. Install it the way you installed the first one.

Apt and your RPM package manager upgrade the existing install in place, and an AppImage takes the update when you overwrite the file. Your projects, settings, and version history are untouched by any of this. See [How updates work](/reference/updates/).

## Uninstalling

Remove the `.deb` or the `.rpm` with your package manager, or delete the AppImage file. Uninstalling leaves your projects where they are. Typeward creates new projects in a `Typeward` folder inside your Documents folder, which is `~/Documents/Typeward` on a default setup and on a system without `xdg-user-dirs`. When `xdg-user-dirs` names another folder as Documents, the `Typeward` folder sits inside that one.

Uninstalling also leaves the app data in `~/.local/share/com.typeward.app`, the window state in `~/.config/com.typeward.app`, and every `typeward.<provider>` entry you stored in the keyring. See [Data locations, credentials, and uninstall](/reference/data-locations/).

## Next steps

- [Your first project](/getting-started/first-project/): create a project and compile your first PDF.
- [Choosing a compile engine](/getting-started/compile-engines/): how Typeward picks between a TeX distribution you have installed and the bundled Tectonic engine.
- [Build from source](/getting-started/build-from-source/): build Typeward yourself instead of installing a package.
