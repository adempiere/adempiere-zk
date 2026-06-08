# ADempiere ZK UI

[![Continuous Integration with Gradle](https://github.com/adempiere/adempiere-zk/actions/workflows/build.yml/badge.svg)](https://github.com/adempiere/adempiere-zk/actions/workflows/build.yml)
[![GitHub release](https://img.shields.io/github/v/release/adempiere/adempiere-zk)](https://github.com/adempiere/adempiere-zk/releases)
[![License](https://img.shields.io/badge/license-GPL--2.0-blue)](LICENSE)
[![Docker Image](https://img.shields.io/badge/docker-ghcr.io%2Fadempiere%2Fadempiere--zk-blue)](https://github.com/adempiere/adempiere-zk/pkgs/container/adempiere-zk)

ZK UI customization layer for ADempiere, built on top of
[adempiere/zk-ui](https://github.com/adempiere/zk-ui).

---

## Overview

This repository produces a customized ZK-based web UI for ADempiere.
At build time it downloads the base `.war` from
[adempiere/zk-ui](https://github.com/adempiere/zk-ui), overlays additional
Java classes and resources from
[adempiere/adempiere-customizations](https://github.com/adempiere/adempiere-customizations),
and packages the result as a deployable `.war` artifact.

The Docker image is published to the GitHub Container Registry and is used
as part of the [adempiere-ui-gateway](https://github.com/adempiere/adempiere-ui-gateway) stack.

---

## How this repository relates to adempiere-customizations

```
adempiere/zk-ui  (upstream ZK base)
       │
       │  zk-ui.war  (downloaded at build time)
       ▼
adempiere/adempiere-zk  ◄──  adempiere/adempiere-customizations
       │                         ├── base/src/main/java/   Java patches
       │                         └── patches/              library JARs
       │
       │  gradle clean refreshDependences releaseZK
       ▼
  build/release/adempiere-zk.war
       │
       ▼
  ghcr.io/adempiere/adempiere-zk:<version>
```

---

## Customizations

The customization layer is managed in
[adempiere/adempiere-customizations](https://github.com/adempiere/adempiere-customizations),
a dedicated repository with the following structure:

```
adempiere-customizations/
├── base/
│   ├── src/main/java/     ← Java patches: override or extend ADempiere
│   │                         models, processes, validators, callouts, etc.
│   │                         Use standard ADempiere package names:
│   │                         org/compiere/model/, org/adempiere/*, etc.
│   └── build.gradle       ← Generic library dependencies
│                             (applicable to all implementors using this stack)
├── patches/
│   └── build.gradle       ← Organization-specific library dependencies
│                             (localization JARs, industry-specific libraries)
└── [additional modules]/  ← Optional further patch modules
```

### Applying your own customizations

1. **Fork** [adempiere/adempiere-customizations](https://github.com/adempiere/adempiere-customizations)
   into your own GitHub organization
2. **Add** your Java classes under `base/src/main/java/`, your library
   dependencies in `patches/build.gradle`, and any additional patch modules
3. **Publish** your fork to GitHub Packages under your own Maven coordinates
4. **Fork** this repository and update `build.gradle`: replace the
   `adempiere-customizations` Maven URL and artifact coordinates with your own

Full instructions for adding Java patches, library dependencies, and patch
modules are in the
[adempiere-customizations README](https://github.com/adempiere/adempiere-customizations).

---

## Building locally

### Prerequisites

- Java 17
- Gradle 9
- A GitHub personal access token (classic) with `read:packages` scope,
  stored in `~/.gradle/gradle.properties`:

```properties
deployUsername=<your-github-username>
deployToken=<your-classic-PAT>
```

### Build steps

```bash
gradle clean refreshDependences releaseZK
```

- `refreshDependences` — downloads `zk-ui.war` from
  [adempiere/zk-ui](https://github.com/adempiere/zk-ui) release `1.2.2`
  and extracts its classes
- `releaseZK` — compiles this project's sources, merges the customization
  layer, and produces `build/release/adempiere-zk.war`

---

## Docker image

```
ghcr.io/adempiere/adempiere-zk:latest
ghcr.io/adempiere/adempiere-zk:<version>
```

---

## CI/CD

- **`build.yml`** — runs on every push and pull request; compiles and
  packages the WAR
- **`publish.yml`** — triggers on GitHub release creation; builds the WAR,
  then builds and pushes the Docker image to `ghcr.io/adempiere/adempiere-zk`

---

## License

GNU General Public License v2.0 — see [LICENSE](LICENSE).
