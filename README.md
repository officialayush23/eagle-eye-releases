# Eagle Eye

AI vision QC for control cabinets. Give it the cabinet's GA drawing and photos of the built cabinet, and it fills the QC checklist (PASS / FAIL / REVIEW for every row) and writes a marked-up report: every finding boxed on the photo, next to the matching part of the drawing.

Built by team **Kala Dhua** for the Emerson PS03 "Eagle Eye" problem statement.

- **Runs on your PC.** Drawings, photos and reports stay in a local folder. After installation no internet is needed to process inspections.
- **Phone capture.** Scan a QR code in the app with a phone on the same Wi-Fi to take the photos it asks for, with quality and "wrong cabinet" checks while you shoot.
- **NVIDIA GPU required.** All models run on the GPU. Without a supported NVIDIA GPU the app opens but refuses to check photos.

## Download (v0.1.0)

| File | Size | What it is |
|---|---|---|
| [`EagleEye-0.1.0-Full-gpu-Setup.exe`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/EagleEye-0.1.0-Full-gpu-Setup.exe) | 1.68 GiB | **Recommended.** Everything included: app, engine, models, WebView2. Installs offline. |
| [`EagleEye-0.1.0-Small-Setup.exe`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/EagleEye-0.1.0-Small-Setup.exe) | 3.9 MiB | Small setup. Downloads the 1.47 GiB runtime pack from this release on first start. |
| [`eagle-eye-0.1.0-windows-x64-gpu.zip`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/eagle-eye-0.1.0-windows-x64-gpu.zip) | 1.47 GiB | The runtime pack the small setup downloads. You do not need to download it yourself. |
| [`eagle-eye-verifier-model.zip`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/eagle-eye-verifier-model.zip) + [`eagle-eye-verifier-runtime-gpu.zip`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/eagle-eye-verifier-runtime-gpu.zip) | 1.45 GiB + 0.62 GiB | **Optional** second opinion (Qwen3-VL-2B) on REVIEW items. Advice only: it never changes a result. |
| [`SHA256SUMS.txt`](https://github.com/officialayush23/eagle-eye-releases/releases/download/v0.1.0/SHA256SUMS.txt) | | Checksums of every file above. |

**Full installation guide, requirements, phone setup, verifier and troubleshooting: [INSTALL.md](INSTALL.md).**

## Requirements in short

Windows 10 or 11 (64-bit), an NVIDIA GPU with driver **580 or newer**, 8 GB RAM (recommended), about 6 GB free disk while installing (4 GB after the setup file is deleted) plus space for your inspections. No Python, Node or admin rights needed.

## Quick start

1. Download `EagleEye-0.1.0-Full-gpu-Setup.exe` and run it (Windows may warn that the publisher is unknown: **More info → Run anyway**).
2. Open **Eagle Eye**, choose **Process on this PC**, click **Install included GPU pack**, then **Start local processor**.
3. Add the GA drawing (PDF or images, plus optional wire / label / IO lists), add the cabinet, start an inspection, and add photos from the desktop or scan the QR with your phone.
4. Run the inspection: open **Results** for the checklist with evidence, and download the Excel or PDF report.

## Licences

Eagle Eye bundles third-party components (ONNX Runtime, NVIDIA CUDA / cuDNN runtime libraries, PP-OCR via RapidOCR, DINOv2, DISK + LightGlue, PARSeq, OpenCV and others). Their licence and notice files are installed with the runtime pack (`notices/` folder). The optional verifier uses Qwen3-VL-2B-Instruct (Apache-2.0) and llama.cpp (MIT).
