# Konsole OS

Konsole OS is an Android terminal application that combines a touch-friendly terminal, a Termux-compatible Linux userspace, and a launcher-style dashboard. It lets users run command-line tools on Android, manage terminal sessions, and reach shell and device utilities from one app.
<img width="1536" height="1024" alt="Hunru Technologies Tech Branding Poster" src="https://github.com/user-attachments/assets/f84eea25-073b-4749-9a1d-7277369982ae" />

**Start here:** [Quickstart](#quickstart) · [User Guide](docs/USER_GUIDE.md) · [Developer Guide](docs/DEVELOPER_GUIDE.md) · [Security](SECURITY.md) · [License](LICENSE.md)

## Table of contents

- [Screenshots](#screenshots)
- [Features](#features)
- [Technology](#technology)
- [Prerequisites](#prerequisites)
- [Quickstart](#quickstart)
- [Build configuration](#build-configuration)
- [Documentation](#documentation)
- [Related package sources](#related-package-sources)
- [Contributing](#contributing)

## Screenshots

The repository includes Android captures of the app's terminal, dashboard, app launcher, command palette, and macro workflow.

| Terminal | System Dashboard |
| --- | --- |
| ![Konsole OS terminal with its HUD and terminal toolbar](public/assets/aistudio/screenshot_4_0efb16ea.png) | ![Konsole OS System Dashboard showing telemetry and terminal options](public/assets/aistudio/screenshot_6_bcb7aa97.png) |

See the [User Guide screenshots](docs/USER_GUIDE.md#screenshots) for the app launcher, command palette, Macro Lab, and macro editor. These captures are illustrative; UI details can change between builds.

## Features

- Run interactive shell sessions with terminal rendering, keyboard shortcuts, extra keys, and session management.
- Install and run Linux command-line packages using the bundled Termux bootstrap and `apt`/`pkg` tooling.
- Open the System Dashboard to inspect device telemetry and verified Termux paths, switch shell behavior, and adjust common terminal options.
- Browse installed apps and use the command palette and Macro Lab for configured shortcuts and automation.
- Integrate with Android file sharing, document picking, app shortcuts, notifications, and the Termux `RUN_COMMAND` service.

## Technology

| Area | Implementation |
| --- | --- |
| Android application | Java, Android SDK, AndroidX, Material Components |
| Build | Gradle 8.13.2 Android Gradle Plugin, Gradle wrapper |
| Terminal | `terminal-emulator` and `terminal-view` Android library modules |
| Shared platform code | `termux-shared` module |
| Native code | C/C++ built with Android NDK/ndk-build |
| Linux userspace | Termux bootstrap archives (default `apt-android-7`) |
| Markdown UI | Markwon |
| CI | GitHub Actions; unit tests run with JUnit and Robolectric |

The Android app has no server-side database. Terminal files and package state live in the app's private Android storage; Android preferences store app settings. Packages are obtained through configured Termux repositories from inside the app.

## Prerequisites

- JDK 17 for the documented CI-compatible build environment.
- Android SDK Platform 36 and Android build tools available to Gradle.
- Android NDK `29.0.14206865` (declared in `gradle.properties`).
- Android Studio is optional but useful for editing and running on an emulator/device.

The repository has no `.env.example`; build options are Gradle properties and optional environment variables described below.

## Quickstart

```bash
git clone https://github.com/hunter-zen-RU/Konsole-OS.git
cd Konsole-OS
./gradlew assembleDebug
```

Gradle downloads the required bootstrap archives during the build and verifies their SHA-256 checksums. This step needs network access on the first build. Install the resulting `app/build/outputs/apk/debug/app-debug.apk` on an Android device or emulator, or open the project in Android Studio and run the `app` configuration.

To run unit tests or build a release APK:

```bash
./gradlew testDebugUnitTest
./gradlew assembleRelease
```

Release signing can be configured with `KEYSTORE_FILE`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, and `KEY_PASSWORD`. If no keystore is supplied, the Gradle configuration falls back to the repository's debug keystore settings; do not use that fallback for a production release.

## Build configuration

Project defaults are in `gradle.properties`: `minSdkVersion=21`, `targetSdkVersion=28`, `compileSdkVersion=36`, and `ndkVersion=29.0.14206865`. `TERMUX_PACKAGE_VARIANT` selects the bootstrap (`apt-android-7` by default; `apt-android-5` is also supported). Other build-time overrides used by the scripts include `TERMUX_APP_VERSION_NAME`, `TERMUX_APK_VERSION_TAG`, `TERMUX_SPLIT_APKS_FOR_DEBUG_BUILDS`, `TERMUX_SPLIT_APKS_FOR_RELEASE_BUILDS`, and `JITPACK_NDK_VERSION`.

## Documentation

- [User Guide](docs/USER_GUIDE.md): install/use the app, configure its dashboard and troubleshoot common issues.
- [Developer Guide](docs/DEVELOPER_GUIDE.md): architecture, modules, build/test workflows, Android integration API, CI and contribution notes.
- [Documentation home](docs/en/index.md): guide directory and visual reference links.
- [Security policy](SECURITY.md)
- [License](LICENSE.md)

## Related package sources

This repository contains the Android application and its terminal libraries, not the package recipes. See [Termux packages](https://github.com/termux/termux-packages) and [Konsole packages](https://github.com/hunter-zen-RU/Konsole-packages) for the package ecosystem used by the app.

## Contributing

Contributions are welcome. Review the [Developer Guide](docs/DEVELOPER_GUIDE.md#contributing) and [security policy](SECURITY.md), make a focused change on a branch, run the relevant Gradle checks, and submit a pull request with a summary and verification notes.

[Back to top](#konsole-os)
