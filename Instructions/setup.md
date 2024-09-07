---
lab:
  title: Penyiapan Lingkungan Lab
  module: Setup
---

# Penyiapan Lingkungan Lab

Latihan ini dimaksudkan untuk diselesaikan di lingkungan lab yang dihosting. Jika Anda ingin menyelesaikannya di komputer Anda sendiri, Anda dapat melakukannya dengan menginstal perangkat lunak berikut. Anda mungkin mengalami dialog dan perilaku yang tidak terduga saat menggunakan lingkungan Anda sendiri. Karena berbagai kemungkinan konfigurasi lokal, tim kursus tidak dapat mendukung masalah yang mungkin Anda temui di lingkungan Anda sendiri.

> **Catatan**: Petunjuk di bawah ini adalah untuk komputer Windows 11. Anda juga dapat menggunakan Linux atau MacOS. Anda mungkin perlu menyesuaikan instruksi lab untuk OS yang Anda pilih.

### Sistem Operasi Dasar (Windows 11)

#### Windows 11

Instal Windows 11 dan terapkan semua pembaruan.

#### Azure Stack Edge

Instal [Edge (Chromium)](https://microsoft.com/edge)

### .NET Core SDK

1. Unduh dan instal dari https://dotnet.microsoft.com/download (unduh .NET Core SDK - bukan hanya runtime bahasa umum). Jika Anda menjalankan lab dalam kursus ini di komputer sendiri, Anda harus memiliki .NET 8.0.

### C++ Redistributable

1. Unduh dan instal Visual C++ Redistributable (x64) dari https://aka.ms/vs/16/release/vc_redist.x64.exe.

### Node.JS

1. Unduh versi LTS terbaru dari https://nodejs.org/en/download/ 
2. Instal menggunakan opsi default

### Python (dan paket yang diperlukan)

1. Unduh versi 3.11 dari https://docs.conda.io/en/latest/miniconda.html 
2. Jalankan pengaturan untuk menginstal - **Penting**: Pilih opsi untuk menambahkan Miniconda ke variabel PATH dan mendaftarkan Miniconda sebagai lingkungan Python default.
3. Setelah instalasi, buka prompt Anaconda dan masukkan perintah berikut untuk menginstal paket: 

```
pip install flask requests python-dotenv pylint matplotlib pillow
pip install --upgrade numpy
```

### Azure CLI

1. Unduh dari https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest 
2. Instal menggunakan opsi default

### Git

1. Unduh dan instal dari https://git-scm.com/download.html, menggunakan opsi default


### Visual Studio Code (dan ekstensi)

1. Unduh dari https://code.visualstudio.com/Download 
2. Instal menggunakan opsi default 
3. Setelah instalasi, mulai Visual Studio Code dan pada tab **Extensions** (CTRL+SHIFT+X), cari dan instal ekstensi berikut dari Microsoft:
    - Python
    - C#
    - Azure Functions
    - PowerShell
