---
lab:
  title: Mulai menggunakan Layanan Azure AI
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Mulai menggunakan Layanan Azure AI

Dalam latihan ini, Anda akan memulai Layanan Azure AI dengan membuat sumber daya **Layanan Azure AI** di langganan Azure dan menggunakannya dari aplikasi klien. Tujuan dari latihan ini bukan untuk mendapatkan keahlian dalam layanan tertentu, melainkan untuk menjadi akrab dengan pola umum untuk penyediaan dan bekerja dengan layanan Azure AI sebagai pengembang.

## Mengkloning repositori di Visual Studio Code

Anda akan mengembangkan kode menggunakan Visual Studio Code. File kode untuk aplikasi Anda telah disediakan di repositori GitHub.

> **Tips**: Jika Anda sudah mengkloning repositori **mslearn-ai-services**, buka dalam Visual Studio Code. Atau, ikuti langkah-langkah ini untuk mengkloningnya ke lingkungan pengembangan Anda.

1. Memulai Visual Studio Code.
2. Buka palet (SHIFT+CTRL+P) dan jalankan **Git: Perintah klon** untuk mengkloning repositori `https://github.com/MicrosoftLearning/mslearn-ai-services` ke folder lokal (tidak masalah folder mana).
3. Setelah repositori dikloning, buka folder di Visual Studio Code.
4. Tunggu sementara file tambahan diinstal untuk mendukung proyek kode C# di repositori, jika diperlukan

    > **Catatan**: Jika Anda diminta untuk menambahkan aset yang diperlukan guna membangun dan men-debug, pilih **Tidak Sekarang**.

5. Luaskan folder `Labfiles/01-use-azure-ai-services`.

Kode untuk C# dan Python telah disediakan. Luaskan folder bahasa pilihan Anda.

## Memprovisikan sumber daya Layanan Azure AI

Layanan Azure AI adalah layanan berbasis cloud yang merangkum kemampuan kecerdasan buatan yang dapat Anda masukkan ke dalam aplikasi Anda. Anda dapat menyediakan sumber daya layanan Azure AI individual untuk API tertentu (misalnya, **Bahasa** atau **Visi**), atau Anda dapat menyediakan sumber daya **Layanan Azure AI** tunggal yang menyediakan akses ke beberapa API layanan Azure AI melalui satu titik akhir dan kunci. Dalam hal ini, Anda akan menggunakan satu sumber daya **Layanan Azure AI**.

1. Buka portal Microsoft Azure di `https://portal.azure.com`, dan masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda.
2. Di bilah pencarian teratas, cari *layanan Azure AI*, pilih **Layanan Azure AI**, dan buat sumber daya akun multi-layanan layanan Azure AI dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya (jika Anda menggunakan langganan terbatas, Anda mungkin tidak memiliki izin untuk membuat grup sumber daya baru - gunakan yang disediakan)*
    - **Wilayah**: *Pilih wilayah yang tersedia*
    - **Nama**: *Masukkan nama unik*
    - **Tingkat harga**: Standar S0
3. Pilih kotak centang yang diperlukan dan buat sumber daya.
4. Tunggu hingga penyebaran selesai, lalu lihat detail penyebaran.
5. Buka sumber daya dan lihat lhaaman **Kunci dan Titik Akhir**. Halaman ini berisi informasi yang Anda perlukan untuk terhubung ke sumber daya Anda dan menggunakannya dari aplikasi yang Anda kembangkan. Khususnya:
    - *Titik akhir* HTTP tempat aplikasi klien dapat mengirim permintaan.
    - Dua *kunci* yang dapat digunakan untuk autentikasi (aplikasi klien dapat menggunakan salah satu kunci untuk mengautentikasi).
    - *Lokasi* tempat sumber daya dihosting. Ini diperlukan untuk permintaan ke beberapa (tetapi tidak semua) API.

## Gunakan Antarmuka REST

API layanan Azure AI berbasis REST, sehingga Anda dapat menggunakannya dengan mengirimkan permintaan JSON melalui HTTP. Dalam contoh ini, Anda akan menjelajahi aplikasi konsol yang menggunakan **Bahasa** REST API untuk melakukan deteksi bahasa; tetapi prinsip dasarnya sama untuk semua API yang didukung oleh sumber daya Layanan Azure AI.

> **Catatan**: Dalam latihan ini, Anda dapat memilih untuk menggunakan REST API dari **C#** atau **Python**. Dalam langkah-langkah di bawah ini, lakukan tindakan yang sesuai untuk bahasa pilihan Anda.

1. Di Visual Studio Code, perluas folder **C-Sharp** atau **Python** tergantung pada preferensi bahasa Anda.
2. Lihat konten folder **rest-client**, dan perhatikan bahwa folder tersebut berisi file untuk setelan konfigurasi:

    - **C#**: appsettings.json
    - **Python**: .env

    Buka file konfigurasi dan perbarui nilai konfigurasi yang dikandungnya untuk mencerminkan **titik akhir** dan **kunci**autentikasi untuk sumber daya layanan Azure AI Anda. Simpan perubahan Anda.

3. Perhatikan bahwa folder **rest-client** berisi file kode untuk aplikasi klien:

    - **C#**: Program.cs
    - **Python**: rest-client.py

    Buka file kode dan tinjau kode yang ada di dalamnya, perhatikan detail berikut:
    - Berbagai ruang nama diimpor untuk mengaktifkan komunikasi HTTP
    - Kode dalam fungsi **Utama** mengambil titik akhir dan kunci untuk sumber daya layanan Azure AI Anda - ini akan digunakan untuk mengirim permintaan REST ke layanan Analisis Teks.
    - Program menerima masukan pengguna, dan menggunakan fungsi **GetLanguage** untuk memanggil API REST deteksi bahasa Analisis Teks untuk titik akhir layanan Azure AI Anda guna mendeteksi bahasa teks yang dimasukkan.
    - Permintaan yang dikirim ke API terdiri dari objek JSON yang berisi data masukan - dalam hal ini, kumpulan objek **dokumen**, yang masing-masing memiliki **id** dan **teks**.
    - Kunci untuk layanan Anda disertakan dalam header permintaan untuk mengautentikasi aplikasi klien Anda.
    - Respons dari layanan adalah objek JSON, yang dapat diurai oleh aplikasi klien.

4. Klik kanan pada folder **rest-client**, pilih *Buka di Terminal Terintegrasi* dan jalankan perintah berikut:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    pip install python-dotenv
    python rest-client.py
    ```

5. Saat diminta, masukkan beberapa teks dan tinjau bahasa yang dideteksi oleh layanan, yang dikembalikan dalam respons JSON. Misalnya, coba masukkan "Halo", "Bonjour", dan "Gracias".
6. Setelah Anda selesai menguji aplikasi, masukkan "berhenti" untuk menghentikan program.

## Menggunakan SDK

Anda dapat menulis kode yang menggunakan REST API layanan Azure AI secara langsung, tetapi ada kit pengembangan perangkat lunak (SDK) untuk banyak bahasa pemrograman populer, termasuk Microsoft C#, Python, Java, dan Node.js. Menggunakan SDK dapat sangat menyederhanakan pengembangan aplikasi yang menggunakan layanan Azure AI.

1. Di Visual Studio Code, perluas folder **sdk-client** di bawah folder **C-Sharp** atau **Python**, tergantung pada preferensi bahasa Anda. Kemudian jalankan `cd ../sdk-client` untuk mengubah ke folder **sdk-client** yang relevan.

2. Instal paket Text Analytics SDK dengan menjalankan perintah yang sesuai untuk preferensi bahasa Anda:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    ```

3. Lihat konten folder **sdk-client**, dan perhatikan bahwa folder tersebut berisi file untuk pengaturan konfigurasi:

    - **C#**: appsettings.json
    - **Python**: .env

    Buka file konfigurasi dan perbarui nilai konfigurasi yang dikandungnya untuk mencerminkan **titik akhir** dan **kunci**autentikasi untuk sumber daya layanan Azure AI Anda. Simpan perubahan Anda.
    
4. Perhatikan bahwa folder **sdk-client** berisi file kode untuk aplikasi klien:

    - **C#**: Program.cs
    - **Python**: sdk-client.py

    Buka file kode dan tinjau kode yang ada di dalamnya, perhatikan detail berikut:
    - Namespace untuk SDK yang Anda instal telah diimpor
    - Kode dalam fungsi **Main** mengambil titik akhir dan kunci untuk sumber daya layanan Azure AI Anda - ini akan digunakan dengan SDK untuk membuat klien untuk layanan Analisis Teks.
    - Fungsi **GetLanguage** menggunakan SDK untuk membuat klien untuk layanan, dan kemudian menggunakan klien untuk mendeteksi bahasa teks yang dimasukkan.

5. Kembali ke terminal, pastikan Anda berada di folder **sdk-client**, dan masukkan perintah berikut untuk menjalankan program:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python sdk-client.py
    ```

6. Saat diminta, masukkan beberapa teks dan tinjau bahasa yang terdeteksi oleh layanan. Misalnya, coba masukkan "Selamat tinggal", "Au revoir", dan "Hasta la vista".
7. Setelah Anda selesai menguji aplikasi, masukkan "berhenti" untuk menghentikan program.

> **Catatan**: Beberapa bahasa yang memerlukan rangkaian karakter Unicode mungkin tidak dikenali dalam aplikasi konsol sederhana ini.

## Membersihkan sumber daya

Jika Anda tidak menggunakan sumber daya Azure yang dibuat di lab ini untuk modul pelatihan lainnya, Anda dapat menghapusnya untuk menghindari dikenakan biaya lebih lanjut.

1. Buka portal Azure di `https://portal.azure.com`, dan di bilah pencarian atas, cari sumber daya yang Anda buat di lab ini.

2. Pada halaman sumber daya, pilih **Hapus** dan ikuti instruksi untuk menghapus sumber daya. Atau, Anda dapat menghapus seluruh grup sumber daya untuk membersihkan semua sumber daya secara bersamaan.

## Informasi selengkapnya

Untuk informasi selengkapnya tentang Azure AI Services, lihat [dokumentasi Layanan Azure AI](https://docs.microsoft.com/azure/ai-services/what-are-ai-services).