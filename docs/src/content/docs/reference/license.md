---
title: License and third-party components
description: "Typeward is MIT-licensed with the Commons Clause: what the license grants, what it withholds, and the third-party code it bundles."
---

This page explains what the Typeward license lets you do, what it withholds, where the source and the installers come from, and which third-party components ship inside the application. Typeward is licensed under the MIT License with the Commons Clause condition, and the whole application lives in one repository, [typeward/typeward on GitHub](https://github.com/typeward/typeward). For the network requests Typeward can make, see [Privacy and network behavior](/reference/privacy-and-network/).

:::note
The `LICENSE` file in the repository root is the authoritative text, and this page is a summary rather than legal advice.
:::

## What the license grants

The MIT License is the base grant, so you may do all of the following with the copy of Typeward you are running.

- Run Typeward for any purpose, private or commercial, on as many machines as you like. There is no key, no registration, and no account to create.
- Read the source of every part of the application you are running.
- Change the source, run your modified version, and fork the project.
- Deploy Typeward across an organisation, commercial or otherwise, at no cost.
- Redistribute Typeward, original or modified, free of charge, with the license and copyright notices intact.

## The Sell condition

The Commons Clause withholds exactly one right that the MIT License would otherwise grant: you may not **Sell** Typeward.

In the words of the clause, to Sell means providing a product or service to third parties, for a fee or other consideration, whose value derives entirely or substantially from the functionality of Typeward. Charging for hosting or for consulting and support around Typeward counts as selling it. Every other permission above survives untouched, including commercial use of the application itself.

The condition applies to Typeward's own code. It places no condition on the third-party components listed below, which remain available to you under their own terms.

One practical consequence is worth knowing: package repositories that accept only licenses approved by the Open Source Initiative do not carry software under the Commons Clause. Debian, Fedora, Homebrew core, and F-Droid will not distribute Typeward, so the releases page is the install route. See [Install on Windows](/getting-started/install-windows/), [Install on macOS](/getting-started/install-macos/), and [Install on Linux](/getting-started/install-linux/).

## One build for everyone

A release is one build per platform and architecture, and every feature in that build is available to every user.

- No plans, tiers, trials, or license keys, and nothing behind a paywall
- No account of any kind, and no Typeward server holding your work
- No telemetry and no crash reporting

Projects are ordinary folders on your disk. For everything Typeward writes to disk, see [Data locations, credentials, and uninstall](/reference/data-locations/).

Optional third-party services are the only part of Typeward that reaches the network. [Reference manager accounts](/references/connecting-reference-managers/), [cloud sync](/projects/cloud-sync/), [git remotes](/projects/git/), and the [AI assistant](/ai/overview/) each stay off until you configure them with your own credentials, and Typeward stores those credentials in the operating system keyring rather than in a settings file. [DOI and arXiv lookup](/references/connecting-reference-managers/#add-a-citation-by-doi-or-arxiv-id) is the exception: it needs no account and no credentials, and it reaches the network only at the moment you look an identifier up.

## Source and releases

One repository holds the source, the issue tracker, and the releases.

| What | Where |
| --- | --- |
| Source, issues, and pull requests | [github.com/typeward/typeward](https://github.com/typeward/typeward) |
| Installers, release notes, and checksums | [github.com/typeward/typeward/releases](https://github.com/typeward/typeward/releases) |

The release workflow in that repository produces every installer from a tagged commit. The current release is v0.1.0, published on 4 September 2026 and described in its notes as a first public beta.

To build the application yourself rather than download it, see [Build from source](/getting-started/build-from-source/).

## Checksums and build provenance

Every release carries two checks you can run against the file you downloaded.

`SHA256SUMS` is a file attached to the release, listing the checksum of every asset in it. The release notes carry a "Verify your download" section with the command, and the per-platform commands are on [Install on Windows](/getting-started/install-windows/), [Install on macOS](/getting-started/install-macos/), and [Install on Linux](/getting-started/install-linux/).

GitHub attests each asset at build time. Running `gh attestation verify <file> --repo typeward/typeward` ties the file in your downloads folder to the workflow run and the tagged commit that produced it.

## Known limitations

- Typeward carries no Windows Authenticode signature and no Apple notarization. SmartScreen and Gatekeeper warn about the installers, and the release notes say so under a heading about code signing. Operating-system code signing is separate from the updater signature, which every release does carry. See [How updates work](/reference/updates/).
- Merge-conflict resolution for git pulls and real-time collaboration are not implemented, and no mobile build is published. Nothing is held back for a paid tier, because Typeward has none.
- v0.1.0 is the first build published outside the machine it was developed on, so its notes ask you to expect rough edges on other setups and to open an issue for anything you hit.

## Third-party components

Typeward links and bundles third-party code, each piece under its own license. `THIRD-PARTY-NOTICES.md` in the repository root lists them. The packaged application as a whole therefore carries no single license: the MIT grant with the Commons Clause covers Typeward's own code, and each third-party component is governed by the license named against it. These are the notable entries.

| Component | License |
| --- | --- |
| Tauri, wry, and the Tauri plugins | MIT OR Apache-2.0 |
| `tao`, Tauri's windowing crate | Apache-2.0 only |
| Solid.js and `@solidjs/router` | MIT |
| CodeMirror 6 packages | MIT |
| PDF.js | Apache-2.0 |
| KaTeX | MIT |
| DOMPurify | MPL-2.0 OR Apache-2.0 |
| Inter and JetBrains Mono | SIL Open Font License 1.1 |
| `harper-core`, `harper-tex`, `harper-typst` | Apache-2.0 |
| `git2`, the Rust bindings | MIT OR Apache-2.0 |
| libgit2, the C library | GPL-2.0-only WITH a linking exception |
| Tectonic | MIT |

No third-party component is redistributed in the Typeward repository as source. Every dependency resolves from its registry at build time.

Two entries need more explanation than a row holds.

libgit2 provides git support in Typeward and is GPL-2.0-only, which on its own would force its terms onto anything it is linked into. It ships a linking exception giving unlimited permission to link the compiled library into combinations with other programs and to distribute those combinations without restriction, and that exception is what makes shipping it inside an MIT-licensed application lawful. libgit2 is used unmodified, and [the libgit2 repository](https://github.com/libgit2/libgit2) is the corresponding source. See [Git in Typeward](/projects/git/).

`tao` is the second. Unlike the rest of the Tauri stack it offers no MIT option, so Apache-2.0 governs it. Apache-2.0 conflicts with nothing in MIT, but it carries obligations MIT does not: preserve the license and any `NOTICE` file with redistributions, state significant changes, and accept its patent grant along with the termination clause attached to it. PDF.js and the `harper-*` grammar crates are Apache-2.0 on the same footing, and DOMPurify is taken under its Apache-2.0 option. The `harper-*` crates are the engine behind on-device [grammar and spell checking](/editor/grammar-checking/).

## What the notices file leaves out

Three kinds of software sit outside the main dependency table.

- Tectonic is fetched per host during setup rather than committed to the repository, and the desktop installers copy the binary in as a sidecar. It ships in the Windows x64, Linux x86_64, and both macOS builds, and not in the ARM64 Windows or ARM64 Linux builds. It is MIT-licensed, and the notices file records that its MIT attribution is not yet collected alongside the build.
- The platform webview that Typeward renders through is an operating system component: WebView2 on Windows (proprietary, from Microsoft), WKWebView on macOS, and WebKitGTK on Linux (LGPL). Typeward distributes none of them. They are loaded at runtime, and the dynamic-linking allowance in the LGPL covers WebKitGTK on that basis.
- Tools you install yourself, such as `latexmk`, `pdflatex`, `typst`, `texlab`, `tinymist`, and `synctex`, are separate programs that Typeward invokes. They are under their own licenses and are not part of Typeward.

`THIRD-PARTY-NOTICES.md` is a curated summary rather than an exhaustive per-dependency manifest, and it says so. For the complete set, `cargo tree` and `npm ls` in a checkout list every transitive dependency.

## Contributions

Issues and pull requests are welcome in [the Typeward repository](https://github.com/typeward/typeward).

`CLAUDE.md` in the repository root, mirrored as `AGENTS.md`, is the architecture guide. Before touching compilation, IPC, the outbound HTTP allowlist, or anything that handles a path coming from a project, read its "Architecture seams" and "Security invariants" sections.

Five checks must pass before a change lands, and CI runs all of them:

- TypeScript typecheck
- Frontend test suite
- `cargo fmt` in check mode
- `cargo clippy` with warnings denied
- Rust test suite

The exact commands are in the repository README and under [Build from source](/getting-started/build-from-source/).

Three house conventions apply to every change:

- Code, files, and commit messages carry no emoji and no em dashes.
- Comments explain why rather than what.
- The project uses npm, not pnpm or yarn.

When you file a bug, include the build version from **Settings → About**, along with your operating system and how you installed Typeward.

## See also

- [Privacy and network behavior](/reference/privacy-and-network/)
- [Data locations, credentials, and uninstall](/reference/data-locations/)
- [Build from source](/getting-started/build-from-source/)
- [How updates work](/reference/updates/)
