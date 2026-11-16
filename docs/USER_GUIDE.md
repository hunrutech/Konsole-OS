# Konsole OS User Guide

This guide describes the app behavior implemented in this repository. Some optional automation actions require Android permissions and may be limited by the device manufacturer or Android version.

**Documentation:** [Project overview and quickstart](../README.md) · [Documentation home](en/index.md) · [Developer Guide](DEVELOPER_GUIDE.md) · [Security policy](../SECURITY.md)

## Table of contents

- [Screenshots](#screenshots)
  - [Terminal and command-line interaction](#terminal-and-command-line-interaction)
  - [System Dashboard](#system-dashboard)
  - [Installed-app drawer and command palette](#installed-app-drawer-and-command-palette)
  - [Macro Lab and macro editor](#macro-lab-and-macro-editor)
- [Getting started](#getting-started)
- [Core workflows](#core-workflows)
  - [Use Konsole commands](#use-konsole-commands)
  - [Run commands and manage sessions](#run-commands-and-manage-sessions)
  - [Install command-line packages](#install-command-line-packages)
  - [Use System Dashboard](#use-system-dashboard)
  - [Browse installed apps](#browse-installed-apps)
  - [Configure macros and automation](#configure-macros-and-automation)
  - [Open or share files](#open-or-share-files)
  - [Launch a command from another Android app](#launch-a-command-from-another-android-app)
- [Configuration and settings](#configuration-and-settings)
  - [Macro data and permissions](#macro-data-and-permissions)
- [Troubleshooting and FAQ](#troubleshooting-and-faq)

## Screenshots

The following captures are included in the repository as a visual orientation to the main app screens. Their content (including telemetry readings, installed apps, package output, and macro examples) is sample data from the time of capture, not a promise of current device state.

### Terminal and command-line interaction

![Terminal showing Konsole shell commands and the top HUD](../public/assets/aistudio/screenshot_4_0efb16ea.png)

The terminal header provides quick access to the dashboard, installed-app drawer, and command palette. The lower extra-key row is configurable in the dashboard.

### System Dashboard

![System Dashboard with live CPU, memory, network, battery and terminal controls](../public/assets/aistudio/screenshot_6_bcb7aa97.png)

### Installed-app drawer and command palette

| App drawer | Command palette |
| --- | --- |
| ![Installed-app drawer with filter field and app grid](../public/assets/aistudio/screenshot_7_7eb5d302.png) | ![Command palette with searchable command entries](../public/assets/aistudio/screenshot_8_7aeb1d27.png) |

### Macro Lab and macro editor

| Macro Lab | Macro rule builder |
| --- | --- |
| ![Macro Lab with templates, a saved cron macro, test controls, and history](../public/assets/aistudio/screenshot_9_65870551.png) | ![Macro editor showing trigger, constraints, action steps and save button](../public/assets/aistudio/screenshot_10_68150666.png) |

The screenshots above are present in `public/assets/aistudio/`. Older listing screenshots under `fastlane/metadata/android/en-US/images/phoneScreenshots/` depict a previous Termux-branded app version and are not used as references for the current Konsole OS UI.

## Getting started

1. Install Konsole OS using an APK distributed by the project, or build one using the [README quickstart](../README.md#quickstart).
2. Open the app. On first launch the app initializes its Termux home and prefix directories and installs the bootstrap environment for the build's selected architecture and package variant.
3. Use the terminal prompt to run shell commands. `apt` and `pkg` are available after bootstrap setup; for example, `pkg update` refreshes package metadata.
4. Grant the requested **Files and media** permission if you want shell access to shared folders such as Downloads. Konsole OS then creates its shared configuration folder and Termux storage links.
5. Use the session controls to create, rename, switch, and close terminal sessions. The app may ask Android for other permissions when a feature needs them.

> **Note:** Linux commands run in the app's Android userspace. This is not a general-purpose Linux installation and Android permission boundaries still apply.

## Core workflows

### Use Konsole commands

The app intercepts a set of Konsole commands before forwarding ordinary input to the shell. The full live help is available with `kons help`. Common entry points include:

| Command | Purpose |
| --- | --- |
| `kons dashboard` / `dash` | Open the System Dashboard. |
| `kons apps` | Open the installed-app drawer while the terminal activity is available. |
| `kons palette` | Open the command palette. |
| `kons macro lab` | Open Macro Lab. `kons macro <name-or-id>` invokes a saved macro through the macro CLI. |
| `kons switch konsole` / `kons switch termux` | Change behavior mode. In Termux mode, non-`kons` commands are passed to the normal shell. |
| `kons sys` | Print a system telemetry summary. |
| `kons hud on` / `kons hud off` | Show or hide the terminal telemetry HUD. |
| `kons set name=value` | Save a custom shell variable. |
| `kons alias name=command` | Save a command alias. |
| `kons setup-fish` | Install/configure Fish if the bootstrap supports it. |

Other native command families include `wifi`, `bluetooth`, `music`, `call`, `contacts`, `volume`, `calc`, `flashlight`, `vibrate`, `brightness`, `dnd`, `wallpaper`, `battery`, `launch`, `settings`, and package/plugin commands under `kons`. Their availability and Android permission requirements vary by device. Use `kons help` for the command descriptions in the installed build. Commands not handled by Konsole are normally sent to the shell.

For example, to create a reusable alias and variable:

```text
kons alias ll=ls
kons set PROJECT=demo
```

Then `ll` expands to `ls` and `$PROJECT` is expanded by Konsole's command dispatcher. These are Konsole configuration values; they are not a substitute for your shell's own alias and environment syntax. The current terminal command parser splits the alias assignment at whitespace, so use a one-token value through this command; aliases with more complex command text can be edited in `$konsole/customize/aliases.xml` while preserving valid XML.

### Run commands and manage sessions

- Start typing in the terminal to interact with the active shell.
- Use the extra-key toolbar for terminal keys that are awkward to type on a phone. The session list and new-session action are available from the terminal UI; named sessions can be renamed and terminated.
- Use the failsafe session option when the configured shell startup files prevent a normal shell from opening.
- Android app shortcuts can launch the main terminal and the app can act as a launcher on devices that allow users to choose a Home app.

### Install command-line packages

In a terminal session, use the package tools supplied by the bootstrap, such as `pkg update`, `pkg install <package>`, and `apt` commands. Package availability and repository configuration depend on the configured Termux-compatible package source and Android architecture.

If `apt` or `pkg` is missing or misconfigured, open **System Dashboard** and use **Repair apt/pkg**. The action verifies/repairs the prefix setup and rewrites the shell environment file; it does not guarantee that an unreachable package repository can be fixed.

### Use System Dashboard

Open **System Dashboard** from the terminal navigation. The dashboard refreshes CPU, memory, network, battery, and storage information while visible. It also exposes verified paths such as `$PREFIX` and `$HOME` and shortcuts to settings, the app list, and Macro Lab.

Available quick controls include:

| Control | Effect |
| --- | --- |
| Shell mode | Switches Konsole or Termux behavior mode and synchronizes shell state files. |
| Status bar/HUD visibility | Changes the Konsole status-bar visibility property. |
| Extra keys | Shows or hides the terminal toolbar. |
| Keep screen on | Enables or disables the terminal keep-awake preference. |
| Soft keyboard | Enables or disables the software keyboard preference. |
| Margin adjustment | Enables or disables terminal margin adjustment. |
| Font size | Decreases or increases terminal text size. |

The dashboard has navigation actions to return to the terminal, open Android app settings, open Macro Lab, browse installed apps, and repair apt/pkg. Telemetry is refreshed on a one-second visible-screen ticker; readings are device estimates, not a system monitoring guarantee.

### Browse installed apps

Open the app list from the dashboard's **Apps** action. The dialog is an app-launching view based on packages visible to Android; whether every installed app appears can depend on Android package visibility and device policy.

### Configure macros and automation

Open **Macro Lab** from the dashboard to inspect and configure macro rules supported by the app. The macro engine supports trigger/constraint evaluation and command dispatch. App-launch observation requires enabling Konsole's accessibility service in Android Accessibility settings. Only enable this service if you intend to use that automation: Android may disclose screen content to accessibility services, and the service can perform configured user actions.

The app also includes notification-trigger and scheduled/cron receiver components. Their operation depends on user-created configuration and Android background execution/notification access settings; exact syntax and available triggers are defined by the app's configuration code and UI.

Typical Macro Lab flow:

1. Open **Macro Lab** and tap **New Macro** or choose a quick template.
2. Give the rule a name and select a trigger. For a cron trigger, enter five fields in this order: minute, hour, day of month, month, day of week. For example, `*/15 * * * *` is every 15 minutes.
3. Add optional runtime constraints such as charging, battery bounds, Wi-Fi/SSID, or screen-on state.
4. Add one or more ordered actions. Shell actions run in the background; session actions write to the active terminal; native actions include notifications, toasts, vibration, app launch, URL opening, flashlight, and accessibility gestures.
5. Save the rule, use **Test** to exercise it, and review the execution history. The **Force** action in the UI can bypass constraints; use it only when you understand the rule's effects.
6. Turn on the rule's enable switch to arm scheduled/event triggers. For notification/app/accessibility actions, enable the relevant Android listener/service when prompted or from system settings.

Cron accepts `*`, comma-separated values, ranges, and `/step` forms. Presets include `@hourly`, `@daily`, `@midnight`, `@weekly`, and `@every_1m`, `@every_5m`, `@every_15m`, and `@every_30m`. The implementation schedules approximate Android alarms and background work; Doze and vendor battery controls can delay them.

### Open or share files

Use Android's Share or Open With flow to send supported files to Konsole OS. The app accepts common application, audio, image, message, multipart, text, and video MIME types for sharing, and application/audio/image/text/video types for viewing. The Android document provider exposes Termux documents to compatible file pickers, subject to Android's URI permission grants.

When a file is shared, Android grants the receiving app temporary access to the supplied content URI. A sender must keep that grant valid for the operation; the app cannot read arbitrary external paths just because a path string was shared.

### Launch a command from another Android app

The app exposes the Termux-compatible `RUN_COMMAND` service. This is an integration for other Android applications, not a terminal command for end users. It requires the app-specific dangerous permission and explicit user consent; see [RUN_COMMAND integration](DEVELOPER_GUIDE.md#run_command-integration).

## Configuration and settings

Open **Settings** from the terminal navigation or dashboard. The settings screen includes terminal I/O, terminal view, and debugging preferences. The dashboard offers a smaller set of frequently changed display and shell options. Preferences are stored by Android and persist across app launches.

The Settings screen groups preferences as follows:

- **Terminal I/O**: input behavior and extra-key/keyboard settings.
- **Terminal view**: display behavior and terminal rendering options.
- **Debugging**: diagnostic options for investigating app behavior.

The exact preference names and available controls are build/version dependent; inspect the in-app screen for the installed build. The dashboard controls listed above write through the shared preferences manager and refresh terminal styling immediately.

The shell environment has app-private `$HOME` and `$PREFIX` locations. Use the paths shown by the dashboard rather than assuming a path copied from another Android installation. No `.env` file is used by the installed app.

Konsole also creates a shared-storage directory at `/storage/emulated/0/Konsole` (shown to shell users as `$konsole`) when Android grants the required storage access. Configuration files live in `$konsole/customize/`:

The app uses Android's shared-storage permission for terminal access. On Android versions that expose the permission in app settings, open **Settings → Apps → Konsole OS → Permissions** and allow **Files and media** (wording can vary by Android version/manufacturer). The app requests storage permission during its permission flow, initializes the Konsole directory after access is granted, and refreshes this setup when you return to the terminal. Termux public-folder shortcuts are created under `$HOME/storage/`, including `shared` and `downloads`.

| File | Contents |
| --- | --- |
| `ui.xml` | UI properties such as `statusBarVisible`; default also contains text/prompt colors, font size, cursor style, transparency, and app-grid columns. |
| `behavior.xml` | Behavior mode and shell behavior properties. Default mode is `konsole`. |
| `aliases.xml` | User aliases (`<alias name="..." value="..."/>`). |
| `variables.xml` | User variables (`<variable name="..." value="..."/>`). |
| `init.kons` | Startup script file created by default. |
| `../variable.txt` | Generated list of built-in and custom variables. |

Prefer `kons set KEY=value` and `kons alias NAME=command` for supported updates; these persist through the configuration manager. When shared storage is unavailable, a feature may fail to write there; Macro Lab has an internal-storage fallback for macro data.

Built-in live variable names include `$time`, `$date`, `$battery`, `$cpu_cores`, `$cpu_usage`, `$ram_total`, `$ram_used`, `$ram_free`, `$storage_free`, `$device_name`, `$android_version`, `$ip_address`, `$wifi_ssid`, and `$network_type`. Values can be device/permission dependent. `$konsole` expands to the shared Konsole directory.

### Macro data and permissions

Macro rules are stored as JSON under `$konsole/macros/macros.json` when writable, with a private app-files fallback. Execution history is retained in `history.json` (up to 60 records). Macro triggers include manual, cron, interval, battery/power, Wi-Fi, screen, Bluetooth/headset, notification, and app-open events. Actions can run shell commands, write to the active terminal session, display notifications/toasts, vibrate, control flashlight, launch apps/open URLs, or perform accessibility taps/swipes. Constraints can require charging/discharging, battery bounds, Wi-Fi/SSID, or an interactive screen.

Notification triggers require enabling Konsole's notification listener in Android settings. App-open and tap/swipe actions require its accessibility service. Android system settings and OS restrictions govern whether these events/actions are available; macro rules are user-configured and may execute shell commands or Android actions, so review them before enabling.

## Troubleshooting and FAQ

| Symptom | Resolution |
| --- | --- |
| Terminal shows an installation/bootstrap error | Retry with a stable network connection; the first build/install needs the bootstrap archive. If the app has already been installed, use the dashboard repair action for apt/pkg issues. |
| `apt`/`pkg` cannot find packages | Run `pkg update`, check network access and configured repositories, then try **Repair apt/pkg** if the tools themselves are missing. |
| `ls` or another command reports permission denied in `/storage/emulated/0/Download` | Grant Konsole OS **Files and media** access in Android app permissions, then return to the app. The app retries storage initialization when it resumes. As an alternative, use `~/storage/downloads` after storage setup creates the link. |
| `$konsole` expands but its folder is missing | Grant shared-storage access and return to the terminal. The app creates `/storage/emulated/0/Konsole` and its configuration folders after permission is available. If access was denied, enable it in Android app permissions and reopen/resume Konsole OS. |
| A background command/session does not start | Android restricts background starts. Grant the requested display-over-other-apps permission only if the feature requires it, and check battery optimization/device background limits. |
| Commands stop when the app is backgrounded | Android and vendor power management can terminate background/phantom processes. Keep the app foregrounded for critical work and review device battery restrictions. |
| Macro does not react to app launches | Enable the Konsole automation accessibility service and verify the macro trigger/constraints in Macro Lab. Accessibility-driven actions are optional and may be restricted by Android. |
| Shared file cannot be opened | Check that the sending app grants access to the selected URI and that the MIME type is supported by Android's share/view intent filters. |
| Dashboard telemetry is unavailable or incomplete | Android may limit individual system readings. Reopen the dashboard and check the relevant device permission/system settings; the dashboard updates while visible. |
| Shell starts in failsafe mode only | Inspect shell startup configuration from a failsafe session and correct the offending startup command. |

For reproducible app bugs, include Android version/device, app build version, steps to reproduce, and relevant logs. Follow [SECURITY.md](../SECURITY.md) for security-sensitive reports.

## Related documentation

- [README quickstart](../README.md#quickstart) for installation/build prerequisites.
- [Documentation home](en/index.md) for the project documentation index.
- [Developer Guide](DEVELOPER_GUIDE.md) for architecture, configuration schemas, Android integrations, CI, and contribution workflow.
- [Security policy](../SECURITY.md) for vulnerability reporting.

[Back to top](#konsole-os-user-guide)
