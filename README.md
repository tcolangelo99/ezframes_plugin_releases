# EzFrames Plugin Releases

Public release assets for installers and auto-updates.

## Extreme pack

The Extreme pack is an optional add-on for NVIDIA graphics cards. It unlocks two Interpolation
Quality presets in the EzFrames panel:

- **Balanced Turbo (NVIDIA)**: the Balanced model on NVIDIA TensorRT, about 3x faster than Balanced
  on an RTX 4070 Ti.
- **Extreme (NVIDIA)**: the best-looking interpolation EzFrames offers (about 10 output frames
  per second at 1080p on an RTX 4070 Ti).

It also runs **Depth Map** (Timeline Actions): a steady grayscale depth pass of any layer.

Every other preset works without it.

### What it is

A private runtime that lives in its own folder, `%LOCALAPPDATA%\EzFramesPlugin\trt`: Python,
PyTorch, NVIDIA TensorRT, the Extreme, RIFE 4.25 and Depth Map model code and their trained weights. None of it is
inside the plugin installer: the pack installer downloads it onto your computer from the official
sources. It touches nothing system-wide (no system Python, no PATH changes, no drivers).

- Download: about 3.6 GB. On disk afterwards: about 5 GB.
- Install time: about 23 minutes on a fast connection with an RTX 4070 Ti (slower cards take longer;
  about 12 minutes on a 6 GB card, which skips Extreme's and Depth Map High's engines). Most of it is
  building GPU engines for your card; on a 50 Mbit/s connection the download alone takes about 10 minutes
  more.
- Nothing to configure.

### Requirements

- NVIDIA GeForce RTX 20-series or newer (Turing or later) with **6 GB of video memory** for the pack,
  Balanced Turbo and Depth Map, and **8 GB** for Extreme. On a 6 GB card the pack installs without
  Extreme's engines and Extreme stays locked. On 8 GB, Extreme at 1080p and Balanced Turbo at every
  supported size fit. Extreme at 1440p, or with other programs using the GPU, may stop with "The GPU ran
  out of memory" (`trt_vram`); close GPU-heavy apps or use Balanced Turbo.
- NVIDIA driver **580 or newer** (update it from nvidia.com or the NVIDIA app).
- Windows 10 or 11, 64-bit.
- Microsoft Visual C++ 2015-2022 Redistributable (x64). Most PCs already have it; if not, the
  installer stops and tells you.
- Free space on the drive that holds your user profile (usually C:): about 10 GB while installing,
  because the downloads and the installed files exist side by side until the end; about 5 GB after.

### Install

1. In the EzFrames panel open **System Check** and find the **Extreme pack** row.
2. Click **Install Extreme pack**. The button is greyed out, with the reason shown in the row, when
   your GPU does not qualify (see Troubleshooting).
3. Confirm. By continuing you accept NVIDIA's TensorRT and CUDA licenses.

### While it installs

- The row shows the progress, for example "Installing 34%: Installing PyTorch and TensorRT", with
  the downloaded amount during downloads. The steps: downloads, unpacking Python, installing PyTorch
  and TensorRT, unpacking the model weights, recording file hashes, then building GPU engines
  (the Balanced Turbo engine first, which also serves as the pack's self-check, then the Depth Map engines,
  about 5 minutes for Standard and 8 for High, then the five Extreme engines for 1920x1080, about 4 minutes,
  shown one by one with the elapsed time). It ends with "Installed".
- Balanced Turbo is ready as soon as its engine is built, a few minutes into the install: the row then says
  "Installed; building Extreme engines 3/5 (flow)..." and Balanced Turbo jobs run. Depth Map and Extreme wait for
  the install to finish.
- You can keep working, close the panel or close After Effects: the install runs on its own and
  keeps going. Reopen the panel to see where it is.
- There is no Cancel button. If the PC restarts or the install is stopped, the row says "Install
  did not finish." Click Install again: it resumes and skips what is already done.

### Use it

Pick **Balanced Turbo (NVIDIA)** or **Extreme (NVIDIA)** under Interpolation Quality and run as
usual. The first job at a resolution the pack has not seen yet builds GPU engines once; the job
message then says "Building TensorRT engine 3/5 (flow) for this resolution (one-time)..." with the elapsed time.

- Extreme: 1080p is built during the install; every other size takes about 4 minutes once.
- Balanced Turbo: 720p to 4K landscape is built during the install; portrait and other sizes take a
  few minutes once.
- After some plugin updates the first job rebuilds its engines once, the same way.

**Depth Map** (Timeline Actions, **Depth Map on Selected Layer**) needs no engine builds after the
install: its engines cover every clip size and orientation. **Depth Map detail**: Standard (any 6 GB
card) or High (8 GB; crisper edges and small near objects, a little slower). The depth layer comes with
**Shift Edges** (Minimax) and **Feather** (Fast Box Blur) at 0: adjust them in Effect Controls.

With upscaling on, Extreme interpolates at the clip's own size and upscales afterwards, so a 1080p
clip upscaled 2x needs no new engines. The upscaler then runs on every in-between frame too, so
the job takes longer than without upscaling. Balanced Turbo does the same when the upscaled size is
above 4K or portrait.

Limits: Extreme does not work on 4K clips; use Balanced Turbo or Quality there. On an 8 GB card, Extreme at 1440p may run out of video memory
(see Requirements). Extreme output is not bit-for-bit repeatable: two runs of the same
clip give tiny, invisible differences in the in-between frames. Kept frames are always identical.

### Update and remove

- **Update**: when a plugin update ships a newer PyTorch or TensorRT, the row says "update
  available" and shows **Update Extreme pack** (about 3.6 GB, 10 minutes). Turbo and Extreme keep
  working until you update.
- **Remove**: **Remove Extreme pack** in the same row deletes the whole pack folder (about 5 GB,
  engines included). Turbo and Extreme are unavailable until you install it again.
- **Uninstalling EzFrames** (Windows Settings > Apps) removes the pack too, unless it is installing
  at that moment; then delete `%LOCALAPPDATA%\EzFramesPlugin\trt` by hand once the install ends.

### Troubleshooting

The full install log is `%LOCALAPPDATA%\EzFramesPlugin\trt\install.log`. **Export Diagnostics**
(System Check) saves a file that includes its last lines and the pack status: attach it when you
report a problem. The codes in parentheses are what the log and job results show.

| You see | What to do |
|---|---|
| "...no NVIDIA GPU was detected" or "...none was found" (`no_nvidia_gpu`) | The pack needs an NVIDIA card. Use Balanced or Quality. |
| "...needs an RTX 20-series or newer NVIDIA GPU (found compute capability X)" | The card is too old for the CUDA version the pack uses. Use Balanced or Quality. |
| "NVIDIA driver X is too old; needs 580 or newer." (`driver_too_old`) | Update the NVIDIA driver, restart After Effects, then install. |
| "Extreme (NVIDIA) needs an NVIDIA GPU with 8 GB VRAM (found N GB)..." | Extreme needs 8 GB. Use Balanced Turbo (NVIDIA), which works on 6 GB. |
| "The Extreme pack needs an NVIDIA GPU with 6 GB VRAM (found N GB)" | Cards under 6 GB are not supported. Use Balanced or Quality. |
| "Depth Map needs the latest Extreme pack..." | Click "Update Extreme pack (adds Depth Map)" in System Check: about 180 MB, then 5 to 13 minutes of engine building. |
| "N GB free, M GB needed" (`disk_space`) | Free space on the user-profile drive, then click Install again. |
| "Install the Microsoft Visual C++ 2015-2022 Redistributable (x64) first..." (`vcredist_missing`) | Install it from Microsoft, then click Install again. |
| `download_failed` | A download server did not answer. Check the connection (and any proxy or firewall rule for the hosts below), then click Install again; it resumes. |
| `hash_mismatch` | A downloaded file was damaged or changed in transit. Click Install again; if it repeats, report it (see above). |
| `pip_failed`, `split_failed`, `bad_lock`, `bad_input`, `internal` | An installer step failed. Click Install again; if it repeats, report it (see above). |
| `no_cuda` during install, or a job failing `trt_no_cuda` | PyTorch could not reach the NVIDIA card. Update the driver and restart Windows. |
| `vram` during install, or a job failing `trt_vram` | The card ran out of video memory. Close other GPU-heavy apps (games, browsers with video, other renders) and retry. On an 8 GB card, Extreme above 1080p may not fit: use Balanced Turbo. |
| `engine_build_failed`, or a job failing `trt_engine_build_failed` | Building a GPU engine failed (a job stops one after 60 minutes). Retry with fewer apps open; if it repeats, report it (see above). |
| A job failing `trt_driver_failed` | Something unexpected in the pack. Retry; if it repeats, report it (see above). |
| A job failing `trt_pack_missing` | The pack or a plugin file is missing. Install the pack again; if it still fails, run Repair from the Start menu. |
| "Install did not finish." / `interrupted` | The install stopped (restart, crash, sleep). Click Install again; it resumes. |
| "Extreme pack install did not start" (`schtasks_failed`) | Windows Task Scheduler refused the install task. Restart Windows and try again; if it repeats, report it (see above). |
| "Extreme pack is still installing..." | Wait for the row to say "Installed". |
| "Extreme engines are still building (3/5). Use Balanced Turbo meanwhile." | The install is finishing; use Balanced Turbo or wait for the row to say "Installed". |
| "Extreme pack engines are still building... Depth Map is ready when System Check says Installed." | The same: Depth Map waits for its engines. |
| "Depth Map High detail needs an NVIDIA GPU with 8 GB VRAM..." | Use Depth Map detail: Standard. |
| "Extreme pack is not installed. Install it from System Check." | Install it, or pick another preset. |
| `remove_failed` | A pack file was in use. Close After Effects and anything else that may use it, then Remove again. |
| `verify_failed`, `not_installed` (only from a manual pack check) | Pack files are missing or changed. Remove the pack and install it again. |

### Privacy: what is downloaded from where

The installer only downloads. It sends nothing to EzFrames and needs no account. Every file is
checked against a SHA-256 fingerprint fixed in the plugin, so a changed file is refused. Processing
never uploads your footage: frames stay on your PC.

| Host | What |
|---|---|
| `www.python.org` | Python 3.12 (embeddable package) |
| `download.pytorch.org`, `download-r2.pytorch.org` | PyTorch and Torch-TensorRT |
| `pypi.nvidia.com` | NVIDIA TensorRT |
| `files.pythonhosted.org` (PyPI) | Small Python libraries (numpy, Pillow and others) |
| `github.com` (EzFrames releases page, `trt-assets-v2`) | The model code and weights, unmodified |

Only if a model file cannot be fetched from the EzFrames releases page, the installer falls back to
the original sources: `github.com` (TNTwise's model release), `raw.githubusercontent.com` (the
authors' code at fixed versions), `huggingface.co` (the depth model's authors) and
`drive.usercontent.google.com` (the authors' Google Drive copies). GitHub delivers release files through its own download servers.

### Third-party licenses

Python (PSF license), PyTorch (BSD), NVIDIA TensorRT and CUDA (NVIDIA license terms, accepted when
you install), and the model code and weights (MIT and Apache-2.0), which are
listed with their sources and full license texts in
[THIRD_PARTY_NOTICES.txt](https://github.com/tcolangelo99/ezframes_plugin_releases/releases/download/trt-assets-v2/THIRD_PARTY_NOTICES.txt)
on the [`trt-assets-v2` release](https://github.com/tcolangelo99/ezframes_plugin_releases/releases/tag/trt-assets-v2).
