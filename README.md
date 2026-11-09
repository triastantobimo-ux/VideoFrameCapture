# VideoFrameCapture

Aplikasi desktop Windows native untuk mengekstrak frame dari file video pada frame rate yang dapat diatur. Setiap rekaman diubah menjadi kumpulan gambar berurutan (PNG/JPEG) plus galeri HTML mandiri per batch — siap dipakai untuk **audit UI/UX, analitik AI, dataset gambar, motion review**, atau menangkap momen tertentu di luar aplikasi web.

> Ringkas: upload video → atur FPS & format → Start capture → dapatkan folder berisi frame bernomor + `index.html` gallery.

---

## Daftar Isi

- [Fitur Utama](#fitur-utama)
- [Teknologi](#teknologi)
- [Instalasi](#instalasi)
  - [1. Dapatkan aplikasi](#1-dapatkan-aplikasi)
  - [2. Download FFmpeg (wajib)](#2-download-ffmpeg-wajib)
  - [3. Taruh ffmpeg.exe di folder yang benar](#3-taruh-ffmpegexe-di-folder-yang-benar)
  - [4. Jalankan aplikasi](#4-jalankan-aplikasi)
- [Cara Pakai](#cara-pakai)
- [Hasil Output](#hasil-output)
- [Pengaturan](#pengaturan)
- [Catatan Repositori (kenapa ffmpeg.exe tidak ada di sini)](#catatan-repositori)
- [Lisensi Pihak Ketiga](#lisensi-pihak-ketiga)

---

## Fitur Utama

- **Browse video** apa pun yang didukung FFmpeg (MP4, MKV, AVI, MOV, WebM, dll.) dengan info lengkap: durasi, resolusi, FPS, dan codec.
- **Capture_settings fleksibel** — preset FPS (mis. `0.5` = 1 frame setiap 2 detik) atau nilai **Custom** sesuai kebutuhan.
- **Format gambar**: PNG (lossless) atau JPEG dengan slider kualitas (lebih tinggi = lebih bagus, file lebih besar).
- **Folder ekspor** bebas pilih, dengan opsi *"Remember this location"* untuk sesi berikutnya.
- **Batch rapi** — setiap ekspor membuat folder batch sendiri (nama video + timestamp `yyyyMMdd_HHmmss`) berisi frame bernomor.
- **Galeri HTML otomatis** (`index.html`) per batch — minimalis, self-contained, dengan lightbox untuk memperbesar frame.
- **Preview frame** langsung di aplikasi, progress capture, dan tombol Cancel.
- **Tema Light / Dark** (Fluent 2) dan **dwibahasa**: English / Bahasa Indonesia.
- **Sesi Baru / Ulangi Sesi** tanpa tutup aplikasi.
- Berjalan sebagai aplikasi normal — **tanpa admin, tanpa Developer Mode, tanpa install .NET** (self-contained).

## Teknologi

| Komponen | Detail |
|---|---|
| UI | WPF + [WPF-UI](https://github.com/lepoco/wpfui) 4.3.0 (Fluent 2 / Windows 11 style) |
| Runtime | .NET 10 (`net10.0-windows`, win-x64) — **self-contained**, runtime sudah termasuk di repo |
| Engine | [FFmpeg](https://ffmpeg.org/) full build (dukungan format & performa maksimal) |
| Pola MVVM | [CommunityToolkit.Mvvm](https://github.com/CommunityToolkit/dotnet) 8.4.0 |
| Output | Frame PNG/JPEG bernomor + galeri HTML per batch |

---

## Instalasi

### Persyaratan

- Windows 10/11 **64-bit** (x64).
- Tidak perlu install .NET — runtime sudah dibundel (self-contained).
- Koneksi internet sekali untuk download FFmpeg (langkah 2).

### 1. Dapatkan aplikasi

Clone repositori ini, atau download ZIP via tombol **Code → Download ZIP** di GitHub lalu extract:

```bash
git clone https://github.com/triastantobimo-ux/VideoFrameCapture.git
```

### 2. Download FFmpeg (wajib)

`ffmpeg.exe` adalah engine utama aplikasi dan **sengaja tidak disertakan di repo** karena ukurannya ±217 MB (melebihi batas file GitHub 100 MB). Download manual dari salah satu sumber resmi berikut:

| Sumber | Link | Keterangan |
|---|---|---|
| **gyan.dev (rekomendasi)** | <https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-full.7z> | *Full build* — sama dengan yang dipakai saat pengembangan (v9.0.2-full). Dukungan codec paling lengkap. |
| gyan.dev (lebih kecil) | <https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip> | *Essentials build* (±80 MB) — cukup untuk format umum (MP4/MKV/WebM). |
| BtbN (GitHub) | <https://github.com/BtbN/FFmpeg-Builds/releases/latest> | Pilih `ffmpeg-master-latest-win64-gpl.zip`. |
| Halaman resmi FFmpeg | <https://ffmpeg.org/download.html#build-windows> | Daftar semua penyedia build Windows. |

**Alternatif via WinGet** (aplikasi juga otomatis mendeteksi FFmpeg hasil install WinGet):

```powershell
winget install --id Gyan.FFmpeg
```

### 3. Taruh ffmpeg.exe di folder yang benar

Extract arsip yang didownload, ambil file `ffmpeg.exe` (di dalam folder `bin/` untuk build gyan.dev), lalu letakkan di:

```
VideoFrameCapture\Assets\Tools\ffmpeg.exe
```

Struktur folder akhir yang benar:

```
VideoFrameCapture/
├── VideoFrameCapture.exe          ← jalankan ini
├── VideoFrameCapture.dll
├── *.dll                          ← runtime .NET 10 (sudah termasuk)
├── Assets/
│   ├── AppIcon.ico
│   └── Tools/
│       └── ffmpeg.exe             ← hasil download langkah 2 (TIDAK ada di repo)
└── videoframecapture.config.json  ← dibuat otomatis saat pertama kali jalan
```

> Aplikasi mencari FFmpeg berurutan di: `Assets\Tools\ffmpeg.exe` → lokasi install WinGet → `C:\ffmpeg\bin\ffmpeg.exe`. Jadi menaruh di `Assets\Tools` adalah cara paling portabel.

### 4. Jalankan aplikasi

Double-click **`VideoFrameCapture.exe`**. Tidak perlu install apa pun lagi.

> Jika Windows SmartScreen muncul (aplikasi belum di-sign), klik **More info → Run anyway**.

---

## Cara Pakai

1. Klik **Browse** dan pilih file video.
2. Atur **FPS** (preset atau Custom) — makin besar nilai, makin banyak frame yang dihasilkan.
3. Pilih **format gambar** (PNG lossless / JPEG) dan kualitasnya.
4. Tentukan **folder ekspor** (opsional: centang *Remember this location*).
5. Klik **Start capture** dan tunggu progress selesai (bisa di-Cancel kapan saja).
6. Buka hasil lewat **Batch folder**, **Open export folder**, atau **Gallery** (`index.html`).

## Hasil Output

Setiap ekspor menghasilkan satu folder batch di folder ekspor pilihan:

```
<Folder Ekspor>/
└── NamaVideo_yyyyMMdd_HHmmss/
    ├── frame_0001.png
    ├── frame_0002.png
    ├── ...
    └── index.html    ← galeri HTML self-contained (klik frame untuk lightbox)
```

Galeri `index.html` bisa langsung dibuka di browser mana pun, dibagikan, atau dipakai sebagai bahan audit UI/UX / input analitik AI tanpa alat tambahan.

## Pengaturan

Preferensi disimpan otomatis di `videoframecapture.config.json` di sebelah aplikasi (file ini dibuat otomatis dan tidak ikut di repo):

- `Theme` — Light / Dark
- `Language` — English / Bahasa Indonesia
- `RememberExportPath` & `LastExportPath` — lokasi ekspor terakhir

Semua juga bisa diubah dari menu aplikasi (Theme, Language, New Session, Restart Session, About).

---

## Catatan Repositori

- Repo ini berisi **build siap jalan** (hasil publish self-contained win-x64), bukan source code.
- **`Assets/Tools/ffmpeg.exe` tidak di-commit** karena ±217 MB > batas GitHub 100 MB per file. File ini di-ignore via [.gitignore](.gitignore) dan wajib didownload manual — lihat [Instalasi langkah 2–3](#2-download-ffmpeg-wajib).
- `videoframecapture.config.json` juga tidak di-track karena berisi preferensi per-pengguna; aplikasi membuatnya otomatis saat pertama dijalankan.

## Lisensi Pihak Ketiga

- **FFmpeg** berlisensi [GPL](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) (full build gyan.dev: GPLv3). FFmpeg tidak didistribusikan dalam repo ini; pengguna mengunduhnya sendiri dari sumber resmi di atas.
- **WPF-UI** — MIT, **CommunityToolkit.Mvvm** — MIT, **.NET** — MIT.