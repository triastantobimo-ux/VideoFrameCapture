# VideoFrameCapture

A native Windows desktop app that extracts frames from video files at a configurable frame rate. It turns any recording into an ordered set of still images (PNG/JPEG) plus a self-contained HTML gallery per batch — ready for **UI/UX audits, AI analytics, image datasets, motion review**, or capturing specific moments outside a web app.

> In short: load a video → set FPS & format → Start capture → get a folder of numbered frames + an `index.html` gallery.

---

## Table of Contents

- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
  - [1. Get the app](#1-get-the-app)
  - [2. Download FFmpeg (required)](#2-download-ffmpeg-required)
  - [3. Place ffmpeg.exe in the right folder](#3-place-ffmpegexe-in-the-right-folder)
  - [4. Run the app](#4-run-the-app)
- [Usage](#usage)
- [Output](#output)
- [Settings](#settings)
- [Repository Notes (why ffmpeg.exe is not included)](#repository-notes)
- [Third-Party Licenses](#third-party-licenses)

---

## Key Features

- **Browse any video** supported by FFmpeg (MP4, MKV, AVI, MOV, WebM, etc.) with full info: duration, resolution, FPS, and codec.
- **Flexible capture settings** — FPS presets (e.g. `0.5` = 1 frame every 2 seconds) or a **Custom** value.
- **Image format**: PNG (lossless) or JPEG with a quality slider (higher = better quality, larger files).
- **Export folder** of your choice, with a *"Remember this location"* option for future sessions.
- **Tidy batches** — every export creates its own batch folder (video name + `yyyyMMdd_HHmmss` timestamp) containing numbered frames.
- **Automatic HTML gallery** (`index.html`) per batch — minimalist, self-contained, with a lightbox to enlarge frames.
- **In-app frame preview**, capture progress, and a Cancel button.
- **Light / Dark theme** (Fluent 2) and **bilingual UI**: English / Bahasa Indonesia.
- **New Session / Restart Session** without closing the app.
- Runs as a normal app — **no admin rights, no Developer Mode, no .NET install required** (self-contained).

## Tech Stack

| Component | Details |
|---|---|
| UI | WPF + [WPF-UI](https://github.com/lepoco/wpfui) 4.3.0 (Fluent 2 / Windows 11 style) |
| Runtime | .NET 10 (`net10.0-windows`, win-x64) — **self-contained**, runtime included in this repo |
| Engine | [FFmpeg](https://ffmpeg.org/) full build (maximum format support and performance) |
| MVVM helpers | [CommunityToolkit.Mvvm](https://github.com/CommunityToolkit/dotnet) 8.4.0 |
| Output | Numbered PNG/JPEG frames + an HTML gallery per batch |

---

## Installation

### Requirements

- Windows 10/11 **64-bit** (x64).
- No .NET install needed — the runtime is bundled (self-contained).
- A one-time internet connection to download FFmpeg (step 2).

### 1. Get the app

Clone this repository, or download the ZIP via **Code → Download ZIP** on GitHub and extract it:

```bash
git clone https://github.com/triastantobimo-ux/VideoFrameCapture.git
```

### 2. Download FFmpeg (required)

`ffmpeg.exe` is the app's main engine and is **intentionally not included in this repo** because it is ~217 MB (over GitHub's 100 MB per-file limit). Download it manually from one of these official sources:

| Source | Link | Notes |
|---|---|---|
| **gyan.dev (recommended)** | <https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-full.7z> | *Full build* — same as the one used during development (v9.0.2-full). Widest codec support. |
| gyan.dev (smaller) | <https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip> | *Essentials build* (~80 MB) — enough for common formats (MP4/MKV/WebM). |
| BtbN (GitHub) | <https://github.com/BtbN/FFmpeg-Builds/releases/latest> | Pick `ffmpeg-master-latest-win64-gpl.zip`. |
| Official FFmpeg page | <https://ffmpeg.org/download.html#build-windows> | Lists all Windows build providers. |

**WinGet alternative** (the app also auto-detects FFmpeg installed via WinGet):

```powershell
winget install --id Gyan.FFmpeg
```

### 3. Place ffmpeg.exe in the right folder

Extract the downloaded archive, take `ffmpeg.exe` (inside the `bin/` folder for gyan.dev builds), and place it at:

```
VideoFrameCapture\Assets\Tools\ffmpeg.exe
```

The correct final folder structure:

```
VideoFrameCapture/
├── VideoFrameCapture.exe          ← run this
├── VideoFrameCapture.dll
├── *.dll                          ← .NET 10 runtime (already included)
├── Assets/
│   ├── AppIcon.ico
│   └── Tools/
│       └── ffmpeg.exe             ← downloaded in step 2 (NOT in the repo)
└── videoframecapture.config.json  ← created automatically on first run
```

> The app looks for FFmpeg in this order: `Assets\Tools\ffmpeg.exe` → the WinGet install location → `C:\ffmpeg\bin\ffmpeg.exe`. Placing it in `Assets\Tools` is the most portable option.

### 4. Run the app

Double-click **`VideoFrameCapture.exe`**. Nothing else to install.

> If Windows SmartScreen appears (the app is not code-signed), click **More info → Run anyway**.

---

## Usage

1. Click **Browse** and select a video file.
2. Set the **FPS** (preset or Custom) — higher values produce more frames.
3. Choose the **image format** (PNG lossless / JPEG) and quality.
4. Pick an **export folder** (optional: check *Remember this location*).
5. Click **Start capture** and wait for progress to finish (can be cancelled at any time).
6. Open the results via **Batch folder**, **Open export folder**, or **Gallery** (`index.html`).

## Output

Each export produces one batch folder inside your chosen export folder:

```
<Export Folder>/
└── VideoName_yyyyMMdd_HHmmss/
    ├── frame_0001.png
    ├── frame_0002.png
    ├── ...
    └── index.html    ← self-contained HTML gallery (click a frame for the lightbox)
```

The `index.html` gallery opens in any browser and can be shared or used directly as UI/UX audit material or AI analytics input — no extra tools needed.

## Settings

Preferences are saved automatically in `videoframecapture.config.json` next to the app (created automatically, not tracked in this repo):

- `Theme` — Light / Dark
- `Language` — English / Bahasa Indonesia
- `RememberExportPath` & `LastExportPath` — last export location

Everything can also be changed from the app menu (Theme, Language, New Session, Restart Session, About).

---

## Repository Notes

- This repo contains a **ready-to-run build** (self-contained win-x64 publish output), not source code.
- **`Assets/Tools/ffmpeg.exe` is not committed** because it is ~217 MB, over GitHub's 100 MB per-file limit. It is excluded via [.gitignore](.gitignore) and must be downloaded manually — see [Installation steps 2–3](#2-download-ffmpeg-required).
- `videoframecapture.config.json` is also untracked because it holds per-user preferences; the app creates it automatically on first run.

## Third-Party Licenses

- **FFmpeg** is licensed under the [GPL](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) (gyan.dev full build: GPLv3). FFmpeg is not distributed in this repo; users download it themselves from the official sources above.
- **WPF-UI** — MIT, **CommunityToolkit.Mvvm** — MIT, **.NET** — MIT.