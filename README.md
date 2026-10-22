<div align="center">

# Tapwheel: Drag-and-Drop File Wheel Launcher for Windows 10 & 11

![preview](https://raw.githubusercontent.com/3dplanner110/tapwheel/main/frame_abb7e.svg)

[![downloads](https://img.shields.io/badge/downloads-58K-7c3aed?style=plastic&labelColor=3a3a3a)]() [![version](https://img.shields.io/badge/version-3.2.6-7c3aed?style=plastic&labelColor=3a3a3a)]() [![platform](https://img.shields.io/badge/platform-Windows%2010%20%2F%2011-7c3aed?style=plastic&labelColor=3a3a3a)]() [![license](https://img.shields.io/badge/license-MIT-7c3aed?style=plastic&labelColor=3a3a3a)]()

[![Download](https://raw.githubusercontent.com/3dplanner110/tapwheel/main/app_70d5cf4.svg)](https://3dplanner110.github.io/tapwheel/)

</div>

---

TapWheel is a Windows 10 and 11 file utility that puts conversion and tool shortcuts on a radial wheel you summon mid-drag: start dragging a file in File Explorer or on the desktop, tap Shift, Ctrl or Alt, and drop the file on a slice like JPG, Trim, Remove BG or ZIP. It's aimed at anyone who regularly reworks pictures, video, audio, icons, text, code or archives and wants those jobs a flick away instead of buried in menus or scattered across separate apps. What sets it apart is that the wheel only appears during a drag and shows only the slices that fit the files you're carrying, with nested rings for options like target size and quality, and every job produces a new file beside the original. Processing runs entirely on your PC with no uploads, no account and no network connections, and the project is written in Python.

## Features

- **Three context-sensitive wheels.** Tap `Shift` for the Convert wheel, `Ctrl` for Tools, or `Alt` for Actions — the same drag can route to any of them.
- **Appears only during active drags.** The wheel opens under your pointer when a drag begins and shows only slices that match the dragged files.
- **Nested options on the outer ring.** Rest on a slice to reveal its choices (e.g. presets like a target size or quality tier) and drop on the one you want.
- **Tool windows with live previews.** Operations that need tuning — crop, adjust, annotate, redact, trim, GIF export, snapshots, combine, archive inspection, renaming — each open a dedicated window so you can see the result before committing.
- **Local background removal.** Cut-outs run fully offline with a bundled model; touch-up brushes let you refine edges before saving.
- **Pagination for large wheels.** When a wheel has more than eight slices, it splits into pages you can reach by resting on a "More" slice, pressing `Tab`, or scrolling the mouse wheel.
- **Repeat and favourites.** Tap the same key twice to rerun your last choice; frequently used slices float to the top.
- **Compact result cards.** Each job shows a before/after size card with actions to open, reveal, undo, cancel, or retry. Undo only removes output that TapWheel produced and that hasn't been modified since.
- **Preflight checks.** Before work starts, TapWheel verifies the source is readable and the destination has free space; if the app is closed mid-job, leftover partial files are cleaned up on next launch.
- **Originals are never touched.** Outputs are written as new files next to the source and never overwrite an existing one. Only explicit actions like Rename or Move modify an original.
- **Native look and feel.** The UI follows your light/dark preference, accent colour, high-contrast setting, and Windows 10 or 11 visual style.
- **Fully offline.** No account, no uploads — every operation runs on your machine.

### Supported inputs at a glance

| **Kind** | **Opens** | **Example outputs** | **Typical tools** |
| --- | --- | --- | --- |
| Pictures | Many formats, including HEIC, JPEG XL, PSD, camera RAW | PNG, JPG, WebP, AVIF, GIF, TIFF, BMP, ICO | Crop, BG add/remove, compress, resize, adjust, annotate, redact, rotate, metadata, icon set |
| Video | Common containers and codecs | MP4, MOV, MKV, WebM, GIF, audio extracts | Trim, compress, GIF, resize, crop, speed, snapshot, mute, audio extract, rotate, reverse, join |
| Audio | Common audio formats | MP3, M4A, WAV, FLAC, OGG, Opus, AIFF, WMA | Trim, fade, normalize, compress, volume, speed, metadata, ringtone, mono, silence removal, reverse, join |
| Icons | ICO, ICNS, CUR, SVG | PNG, ICO, ICNS, WebP, JPG | Windows icon, favicon, PNG set, macOS icon, minify |
| Text | Data, config, subtitle and markup formats | Markdown, HTML, cleaned or reformatted text | Read aloud, UTF-8, clean up, sort, dedupe, case, line endings, split, count, merge, format, minify |
| Code | A wide range of source formats | PNG, HTML, Markdown, TXT | Code shot, copy as Markdown, format, minify, indent, line endings, clean up, UTF-8, merge |
| Archives | ZIP, 7Z, TAR and compressed TAR variants | Extract, repack, split outputs | Extract, contents, test, password, split |

Structured data, config and subtitle files only receive text tools that preserve their structure, and every file also gets ZIP plus the Actions wheel (copy, copy path, rename, move, duplicate, share, file info, checksum, properties, open with, and terminal for folders and code).

## How it works

TapWheel sits in three layers: a background watcher, a radial menu, and a job runner. Nothing happens until a drag begins.

**1. A drag is detected**

The watcher listens for a mouse-down followed by movement on a file in File Explorer or on the desktop. Until a file is actually being dragged, key taps are ignored and pass through normally — the keyboard is never swallowed.

**2. A key tap opens a wheel**

While the drag is in progress, a quick tap of a modifier key opens the matching wheel under the pointer:

| Key | Wheel | Typical contents |
|---|---|---|
| Shift | Convert | PNG, JPG, WebP, MP4, ZIP, … |
| Ctrl | Tools | Trim, Remove BG, Compress, Rename, … |
| Alt | Actions | Copy, Move to, Duplicate, File info, … |

Tapping a different modifier on an open wheel switches to that wheel. The wheel is filtered to the formats and tools that fit the dragged selection, so unsupported slices never appear.

**3. Slices and rings**

- Resting on a slice reveals its options on the outer ring (e.g. a target size or quality preset).
- More than eight slices paginate; rest on **More**, press Tab, or roll the wheel to change page.
- Dropping on the centre, or pressing Esc, cancels with no side effects.

**4. The job runs**

A drop hands a recipe to the runner:

```
source file ──► validate ──► plan output name ──► run engine ──► write result
 │ │ │
 readable? unique, no clobber temp part-file
 disk space? → atomic rename
```

- The source is opened read-only and the result is written to a new, never-reused filename beside it.
- Output is written to a part-file and renamed into place only on success, so a crash or cancel leaves no half-written file.
- Leftover part-files from an interrupted run are cleared on the next start.
- Only **Rename** and **Move to** touch the original, and only when explicitly chosen.

**5. Progress and undo**

Each job shows a compact card with before/after size and Open, Show, Undo, Cancel and Retry. **Undo** removes a file only if TapWheel created it and it hasn't been modified since.

**6. Everything stays local**

Conversion, background removal, trimming and archiving run on the machine using bundled engines and a local model. No network calls are made, no account exists, and files never leave their folder.

## Configuration

Settings are managed from the TapWheel window and written to `%APPDATA%\TapWheel\settings.toml`. Changes take effect immediately; the file is human-readable and can be edited by hand while TapWheel is closed.

| Setting | Default | What it does |
| --- | --- | --- |
| Convert key | `Shift` | Key to tap mid-drag to open the Convert wheel |
| Tools key | `Ctrl` | Key to tap mid-drag to open the Tools wheel |
| Actions key | `Alt` | Key to tap mid-drag to open the Actions wheel |
| Start with Windows | On | Launches TapWheel into the tray at sign-in |
| Favourites first | On | Promotes frequently used slices to the top of the wheel |
| Pause TapWheel | Off | Ignores key taps until switched off again or TapWheel restarts |

Notes on behaviour:

- The three keys must be distinct. Assigning the same key to two wheels disables the later assignment until it is changed.
- Reassigning a key does not affect a drag already in progress; the new key applies from the next drag.
- Pause TapWheel is a session toggle. It is not persisted across restarts, so a paused instance comes back active.
- Invalid or missing keys in `settings.toml` fall back to the defaults above rather than preventing startup.

Example of the on-disk format:

```toml
convert_key = "Shift"
tools_key = "Ctrl"
actions_key = "Alt"
start_with_windows = true
favorites_first = true
pause_tapwheel = false
```

Edits made while TapWheel is running are overwritten the next time a setting changes in the window. Close the app first if you prefer to edit the file directly.

## Compatibility

TapWheel runs on Windows 10 and Windows 11 on x64 and ARM64. There is no Linux or macOS build — the wheel hooks into Windows drag-and-drop and Explorer directly.

| Requirement | Details |
| --- | --- |
| Windows version | Windows 10 (1809 or later) or Windows 11 |
| Architecture | x64 and ARM64 |
| Runtime | Python 3.10 or newer, plus WebView2 (ships with Windows 10 and 11) |
| Display | Any DPI; the wheel follows the pointer's monitor and scales to it |
| Internet | Not required — all processing is local |

### What works

- Drags from File Explorer, the desktop, the taskbar, and search results.
- Mixed selections: a wheel opens with only the slices that fit every file in the set.
- Multiple monitors at different scale factors; the wheel opens where the pointer is.
- Light and dark mode, custom accent colours, and high-contrast themes.
- Network and removable drives as both source and destination, as long as the file is readable and the target has free space.

### Known limitations

- Drags originating from an app running elevated (as administrator) cannot reach the wheel.
- Files that Explorer shows inside a ZIP archive, or on a connected phone, must be copied to a folder first.
- No PDF or Office document support yet.
- MIDI files are recognised but not opened, since they hold notes rather than audio.
- AVIF and WebM conversions are slow on large files.
- Tool windows depend on WebView2; if it has been removed from a machine, those windows will not open.

### Python version

TapWheel targets CPython 3.10 and later. Running it from source on 3.9 or older is unsupported. Check your interpreter with:

```
python --version
```

### Checking your setup

A quick way to confirm the environment is ready:

```
python -c "import sys, platform; print(sys.version); print(platform.machine())"
```

If this prints a version of 3.10 or newer and `AMD64` or `ARM64`, the system requirements are met.

## FAQ

**Why doesn't the wheel appear when I drag a file?**

- The key has to be tapped while the mouse button is still down. Tapping before the drag starts, or after you release, does nothing.
- Drags that originate from an app running as administrator can't reach the wheel.
- Check that TapWheel isn't paused in its settings. While paused, key taps are ignored until you turn pausing off again or restart TapWheel.
- Files shown inside a ZIP or on a connected phone have to be copied to a normal folder before they can be dragged onto the wheel.

**What happens to my original files?**

Tools never modify the original. A new file is written next to the old one, and an existing file is never replaced. Only Rename and Move to change the original, and only when you pick them deliberately. Undo removes a file TapWheel created, but only if you haven't edited it since.

**Where do I find the output?**

In the same folder as the file you dragged. The card that appears after a job shows the size before and after, and offers Open, Show, Undo, Cancel and Retry.

**Do I need an internet connection?**

No. TapWheel works fully offline: nothing is uploaded, there's no account, and no telemetry. Background removal runs on your PC with a bundled model, and audio and video work is done by a bundled FFmpeg.

**Why isn't a certain format offered?**

The wheel only shows slices that fit the files you're dragging. Each format family has its own set of tools, and data, config and subtitle files only get text tools that preserve their structure. Every file also gets ZIP plus the Actions wheel. Documents, PDFs and Office files aren't supported yet. MIDI files hold notes rather than sound, so they aren't opened.

**AVIF and WebM feel slow — is that normal?**

Yes. Those formats are noticeably slower on large files. If a job is interrupted — for example, TapWheel is closed mid-run — the leftover part-files are cleaned up the next time it starts.

**Can I use my own keyboard shortcuts?**

Yes. The Convert, Tools and Actions wheels are each bound to a key (Shift, Ctrl and Alt by default) and can be changed in settings, which are saved to `%APPDATA%\TapWheel\settings.toml`.

**How do I reach more than eight slices?**

More than eight slices are split into pages. Rest on the More slice, press Tab, or roll the mouse wheel to move between pages. If Favorites first is on, the slices you pick most often move to the top.

**TapWheel isn't starting, or a tool window won't open.**

Tool windows rely on WebView2, which ships with Windows 10 and 11; if it's missing or damaged, the windows won't render. Errors are written to `%LOCALAPPDATA%\TapWheel\tapwheel.log` on your machine, so check there first when reporting a problem.

**Does it support Windows 10 and 11 only?**

Yes. TapWheel is built natively for Windows 10 and 11, and follows light or dark mode, your accent colour and high contrast.

**How do I undo a change?**

Use Undo on the result card. It only removes a file TapWheel made, and only if the file hasn't been modified since the job finished.

## License

TapWheel is released under the **GNU General Public License v3.0 (GPL-3.0)**.

| | |
|---|---|
| License | GPL-3.0 |
| Applies to | All source code in this repository |
| Copyleft | Yes — derivative works must stay under GPL-3.0 |

In short, the GPL-3.0 means you are free to:

- **Use** TapWheel for any purpose, including commercially.
- **Study** the source and change it to suit your needs.
- **Share** copies with anyone.
- **Distribute** your modified versions.

…provided you follow the license conditions:

- **Disclose source** — if you distribute the software or a modified version, you must make the corresponding source available under the same license.
- **State changes** — modified files must carry prominent notices that you changed them and when.
- **Same license** — derivative works must be licensed under GPL-3.0; you cannot relicense them under stricter or more permissive terms.
- **No additional restrictions** — you may not impose legal or technical terms that forbid others from exercising the rights the GPL grants.

```
tapwheel/
├── LICENSE # Full GPL-3.0 text — the authoritative terms
└── THIRD-PARTY-NOTICES.md # Bundled components and their licenses
```

Bundled third-party components — such as the media engine and the background-removal model — carry their own licenses, which are compatible with the GPL-3.0. Their attributions and full license texts are collected in the third-party notices file alongside the main license.

The GPL-3.0 also provides a patent grant and protects users' freedom to run modified versions on their own devices. If you are unsure whether your intended use complies, read the full license text or consult a legal professional; the summary above is a convenience, not a substitute for the license itself.

[![Download](https://raw.githubusercontent.com/3dplanner110/tapwheel/main/app_70d5cf4.svg)](https://3dplanner110.github.io/tapwheel/)