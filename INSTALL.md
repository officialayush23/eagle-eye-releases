# Eagle Eye installation guide (v0.1.0)

This guide takes a Windows PC from nothing to a first inspection report: what the PC needs, which file to download, installing, first start, phone capture, the optional verifier, updating, uninstalling and fixing common problems.

## 1. What the PC needs

| | Needed | Why |
|---|---|---|
| Windows | Windows 10 or 11, 64-bit | The installer and engine are built for Windows x64. |
| GPU | **NVIDIA GPU with driver 580 or newer** | Every model (text reading, part detection, photo placement) runs on the GPU. Eagle Eye never falls back to the CPU: without a supported GPU it opens but refuses to check photos. Developed and tested on an RTX 3050 6 GB laptop GPU. |
| Memory | 8 GB RAM recommended | Large drawings and full-resolution photos are held in memory during a run. |
| Disk | About 6 GB free while installing (full setup), about 4 GB afterwards, plus room for your inspections | The setup file is 1.68 GiB; the installed engine and models take about 2.2 GiB; drawings, photos and reports are added per inspection. |
| Rights | No admin rights | It installs for the current Windows user only. |
| Internet | Only for downloading | After installation, processing works with no internet. The phone needs the same local Wi-Fi as the PC, not the internet. |
| Not needed | Python, Node.js, CUDA Toolkit, cuDNN | Everything the engine needs, including the CUDA and cuDNN runtime libraries, is inside the runtime pack. Only the NVIDIA **driver** must be installed. |

**Check the driver:** open PowerShell and run

```powershell
nvidia-smi --query-gpu=name,driver_version --format=csv,noheader
```

It should print your GPU and a driver version of 580 or higher, for example `NVIDIA GeForce RTX 3050 6GB Laptop GPU, 616.92`. If the command is not found or the version is lower, install the latest driver from [nvidia.com/drivers](https://www.nvidia.com/Download/index.aspx) and restart the PC.

## 2. Choose a download

Everything is on the [v0.1.0 release page](https://github.com/officialayush23/eagle-eye-releases/releases/tag/v0.1.0).

- **Full setup (recommended):** `EagleEye-0.1.0-Full-gpu-Setup.exe`, 1.68 GiB. Contains the app, the engine, all models and Microsoft's WebView2 runtime. After the download, installation needs no internet. Use this for most PCs and for PCs that will be offline.
- **Small setup:** `EagleEye-0.1.0-Small-Setup.exe`, 3.9 MiB. Installs the app only; on first start it downloads the runtime pack (`eagle-eye-0.1.0-windows-x64-gpu.zip`, 1.47 GiB) from this release, checks its SHA-256 and unpacks it. A broken download resumes where it stopped. Needs about 4 GB free for the download plus the unpacked engine.

You never need to download the runtime pack zip yourself.

**Optional: check the download.** In the folder with the downloaded file:

```powershell
(Get-FileHash .\EagleEye-0.1.0-Full-gpu-Setup.exe -Algorithm SHA256).Hash.ToLower()
```

Compare it with the line for that file in `SHA256SUMS.txt`:

```
764fd36b26d9f817cf10798f8e55238105f0a584948be64ed7a3a0ebc7c60d70  EagleEye-0.1.0-Full-gpu-Setup.exe
9e8b977cffc51a796bbb99575cc5d42972b88f88f55ed51f815eb0cfc8d91f6f  EagleEye-0.1.0-Small-Setup.exe
77800f80ee80ec04b630baf1f187076708a5948aa6a9b716128dd1530b025a49  eagle-eye-0.1.0-windows-x64-gpu.zip
6b1fd19d2c68b9abf92cbcdf1adc927f93a63b8e662f1cc8c8db161da9bff033  eagle-eye-verifier-model.zip
e0395621fdfea7c1ad18f624c6a0679d1d85709fe44fd89ce8a71bfe51085835  eagle-eye-verifier-runtime-gpu.zip
```

## 3. Install

1. Run the setup file.
2. **"Windows protected your PC"** may appear, because this release is not code-signed yet. Click **More info**, check that the file name is the one you downloaded, then click **Run anyway**.
3. Follow the installer. It installs for your Windows user only (no admin prompt). The full setup also installs Microsoft Edge WebView2 if the PC does not have it.
4. Start **Eagle Eye** from the Start menu.

## 4. First start

The first screen is **Welcome to Eagle Eye: choose where your inspections are processed**.

1. Under **Process on this PC**, Eagle Eye checks the NVIDIA driver. You should see *"A supported NVIDIA GPU and driver were found."* If it says no supported GPU was found, see Troubleshooting below.
2. Click **Install included GPU pack** (full setup) or **Download GPU pack** (small setup). Progress is shown on the screen; with the full setup this is unpacking only and takes a few minutes. The pack is checked before it is used, and a failed installation leaves any earlier working pack in place.
3. Click **Start local processor**. The engine starts in the background and the main window opens.
4. **Windows Firewall** may ask whether Eagle Eye may communicate on networks. Allow **Private networks** if you want phone capture over your Wi-Fi. The desktop app works without it.

Open **Settings & system** at any time to see the system check: Windows version, memory, free disk, GPU and driver, where each model runs, and which models are installed. Anything missing is listed with what to do.

## 5. Your first inspection

1. **Drawings → add a drawing.** Upload the GA drawing as a PDF (vector PDFs are read exactly) or as images, one per sheet. Optionally add the wire list, label list and IO allocation (Excel): their columns are found by name, and the upload panel has an **Excel template for the lists** if you have none. Enter the drawing number and revision if the file does not show them clearly. Eagle Eye builds the expected checklist from the sheets and lists.
2. **Cabinets → add the cabinet.** Give its serial number and pick the registered drawing revision.
3. **Inspections → new inspection** for that cabinet (assembly, final and so on). The inspection shows a **shot list**: which areas to photograph and how close.
4. **Add photos**, either way:
   - **From the desktop:** drop photos (JPEG, PNG, HEIC) or a PDF of photos onto the inspection.
   - **From a phone:** see section 6.

   Each photo is checked when it arrives: sharpness, glare, tilt, whether the labels are big enough to read, and whether it shows *this* cabinet (a photo of another cabinet is held and flagged, never used as evidence).
5. **Run now.** Progress is shown step by step. Every step runs on the GPU, one model at a time.
6. **Results** shows every checklist row as PASS, FAIL, REVIEW, NOT PHOTOGRAPHED or NOT CHECKED, with the photo area and the drawing area side by side. A FAIL needs two independent signals in photos of a confirmed cabinet; anything doubtful is REVIEW with the reason. You can record your own decision on any row (with your name and a reason); the tool's result stays visible next to it.
7. **Reports:** download the Excel checklist report or the PDF report with marked photos. On **Results** you can also fill your own QC checklist template (Excel) from the results.

**Where your data is:** drawings, photos, the database and reports are in `C:\Users\<you>\.eagleeye`. Engine logs are in `%LOCALAPPDATA%\com.kaladhua.eagleeye\logs`.

## 6. Phone capture

1. Connect the phone and the PC to the **same Wi-Fi**. Allow Eagle Eye on Private networks if Windows asked (section 4).
2. On the PC, open the inspection and find **Phone capture**. Scan its QR code with the phone camera.
3. The phone opens the capture page in its browser (no app to install). It shows the shot list grouped by area, the checklist with evidence, and the cabinet's history.
4. Take each requested photo. The phone shows the upload, then the verdict: accepted, or a retake request with the reason (blurred, glare, text too small, cut off, or "this may not be this cabinet").
5. The QR link opens only that inspection. Other devices on the Wi-Fi cannot reach the rest of the app.

Photos are queued on the phone if the Wi-Fi drops and are sent when it returns. Keep the capture page open until uploads finish.

## 7. Optional: the verifier (second opinion)

The verifier is a small vision-language model (Qwen3-VL-2B-Instruct with llama.cpp) that looks at REVIEW items and adds an opinion next to them. It never changes a result and can never make a FAIL on its own.

1. Install Eagle Eye and finish **First start** (section 4) first.
2. Download both `eagle-eye-verifier-model.zip` (1.45 GiB) and `eagle-eye-verifier-runtime-gpu.zip` (0.62 GiB) into one folder.
3. Close Eagle Eye. In PowerShell, in the folder with the two zips, run:

```powershell
$root   = "$env:LOCALAPPDATA\com.kaladhua.eagleeye"
$name   = Get-Content "$root\runtime.json" -Raw | ConvertFrom-Json
$models = "$root\runtimes\$name\models"
Expand-Archive .\eagle-eye-verifier-model.zip       -DestinationPath $models -Force
Expand-Archive .\eagle-eye-verifier-runtime-gpu.zip -DestinationPath $models -Force
Get-ChildItem "$models\verifier" | Select-Object Name, Length
```

Both zips unpack into the same `verifier` folder inside the installed runtime. The listing should include `llama-server.exe`, `Qwen3VL-2B-Instruct-Q4_K_M.gguf` and `mmproj-Qwen3VL-2B-Instruct-Q8_0.gguf`.

4. Start Eagle Eye again. **Settings & system** shows the verifier as installed, and the next run adds its opinion to REVIEW items.

Installing a new runtime pack (an update) creates a new runtime folder. Repeat step 3 after an update.

## 8. Updating

Download the new version's setup file and run it; it installs over the current version. A new runtime pack is installed next to the old one and only selected once it is complete, so a failed update leaves the working version in place. Your inspections in `C:\Users\<you>\.eagleeye` are not touched.

## 9. Uninstalling

**Windows Settings → Apps → Installed apps → Eagle Eye → Uninstall.** If the uninstaller offers to delete app data, that removes the installed runtime pack and logs (`%LOCALAPPDATA%\com.kaladhua.eagleeye`). Your inspection records in `C:\Users\<you>\.eagleeye` are kept; delete that folder yourself if you want them gone.

## 10. Troubleshooting

| Problem | What to do |
|---|---|
| "No supported NVIDIA GPU (driver 580 or newer) was found" | Run the `nvidia-smi` check from section 1. Install or update the NVIDIA driver, restart the PC and reopen Eagle Eye. On laptops with two GPUs, the NVIDIA GPU must be enabled (not switched off in the BIOS or vendor tool). |
| "Port 8765 is busy" | Another Eagle Eye engine is still running. Close Eagle Eye, end any `eagleeye-engine.exe` in Task Manager, and open it again. |
| "The engine stopped during startup" or "Engine startup timed out" | Open `%LOCALAPPDATA%\com.kaladhua.eagleeye\logs\engine.err.log`. Most often the GPU driver is too old or the GPU is busy with another heavy program. |
| A run fails with "Eagle Eye processes on an NVIDIA GPU only…" | The GPU was not usable for one of the models (driver problem, or the GPU ran out of memory). Close other GPU-heavy programs (games, other AI tools) and run again. |
| Small setup: the pack download stops | Click **Download GPU pack** again: it resumes from where it stopped and checks the file before using it. Or use the full setup. |
| The phone cannot open the QR link | Phone and PC must be on the same Wi-Fi (not a guest network that isolates devices). Allow Eagle Eye on Private networks in Windows Firewall; if the PC's Wi-Fi is set to Public, change it to Private. |
| A photo is held as "not this cabinet" | Its labels did not match the drawing. Check that it is the right cabinet; if it is, confirm it in **Flags** with your name and a reason, and it is used. |
| Many rows are NOT PHOTOGRAPHED | Follow the inspection's shot list: those areas have no usable photo yet. |
| "Windows protected your PC" | The release is not code-signed yet. **More info → Run anyway** after checking the file name (and optionally the SHA-256, section 2). |

## 11. Known limits of v0.1.0

- NVIDIA GPU only; there is no CPU mode.
- Windows only.
- The installers are not code-signed.
- Wire landing, BOM part numbers, ducts and drawing notes are listed as NOT CHECKED for the inspector to check by hand.
- Accuracy depends on photo quality: close, sharp, straight-on photos of each area give the most PASS / FAIL results; distant or blurred photos give REVIEW.
