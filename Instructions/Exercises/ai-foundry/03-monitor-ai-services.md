---
lab:
  title: Memantau Layanan Azure AI
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Memantau Layanan Azure AI

Layanan Azure AI dapat menjadi bagian penting dari keseluruhan infrastruktur aplikasi. Sangat penting untuk dapat memantau aktivitas dan mendapatkan peringatan tentang masalah yang mungkin memerlukan perhatian.

## Mengkloning repositori di Visual Studio Code

Anda akan mengembangkan kode menggunakan Visual Studio Code. File kode untuk aplikasi Anda telah disediakan di repositori GitHub.

> **Tips**: Jika Anda sudah mengkloning repositori **mslearn-ai-services**, buka dalam Visual Studio Code. Atau, ikuti langkah-langkah ini untuk mengkloningnya ke lingkungan pengembangan Anda.

1. Memulai Visual Studio Code.
2. Buka palet (SHIFT+CTRL+P) dan jalankan **Git: Perintah klon** untuk mengkloning repositori `https://github.com/MicrosoftLearning/mslearn-ai-services` ke folder lokal (tidak masalah folder mana).
3. Setelah repositori dikloning, buka folder di Visual Studio Code.
4. Tunggu sementara file tambahan diinstal untuk mendukung proyek kode C# di repositori, jika diperlukan

    > **Catatan**: Jika Anda diminta untuk menambahkan aset yang diperlukan guna membangun dan men-debug, pilih **Tidak Sekarang**.

5. Luaskan folder `Labfiles/03-monitor-ai-services`.

## Memprovisikan sumber daya Layanan Azure AI

Jika Belum memilikinya di langganan, Anda harus menyediakan sumber daya **Layanan Azure AI**.

1. Buka portal Microsoft Azure di `https://portal.azure.com`, dan masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda.
2. Di bilah pencarian teratas, cari *layanan Azure AI*, pilih **Layanan Azure AI**, dan buat sumber daya akun multi-layanan layanan Azure AI dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya (jika Anda menggunakan langganan terbatas, Anda mungkin tidak memiliki izin untuk membuat grup sumber daya baru - gunakan yang disediakan)*
    - **Wilayah**: *Pilih wilayah yang tersedia*
    - **Nama**: *Masukkan nama unik*
    - **Tingkat harga**: Standar S0
3. Pilih kotak centang yang diperlukan dan buat sumber daya.
4. Tunggu hingga penyebaran selesai, lalu lihat detail penyebaran.
5. Saat sumber daya telah diterapkan, buka dan lihat halaman **Kunci dan Titik Akhir**. Catat URI titik akhir - Anda akan membutuhkannya nanti.

## Konfigurasikan peringatan

Mari mulai memantau dengan menentukan aturan peringatan sehingga Anda dapat mendeteksi aktivitas di sumber daya layanan Azure AI Anda.

1. Di portal Microsoft Azure, buka sumber daya layanan Azure AI Anda dan lihat halaman **Peringatan** (di bagian **Pemantauan**).
2. Pilih **+ Buat** drop-down, lalu pilih **Aturan peringatan**
3. Di halaman **Buat aturan peringatan**, di bawah **Cakupan**, verifikasi bahwa sumber daya layanan Azure AI Anda terdaftar. (Tutup panel **Pilih sinyal** jika terbuka)
4. Pilih tab **Kondisi**, dan pilih tautan **Lihat semua sinyal** untuk menampilkan panel **Pilih sinyal** yang muncul di sebelah kanan, tempat Anda dapat memilih jenis sinyal untuk dipantau.
5. Dalam daftar **Jenis sinyal**, gulir ke bawah ke bagian **Log Aktivitas**, lalu pilih **Daftar Kunci (Akun API Cognitive Services)**. Lalu, pilih **Terapkan**.
6. Tinjau aktivitas selama 6 jam terakhir.
7. Pilih tab **Tindakan**. Perhatikan bahwa Anda dapat menentukan *grup tindakan*. Ini memungkinkan Anda untuk mengonfigurasi tindakan otomatis saat peringatan diaktifkan - misalnya, mengirim pemberitahuan email. Kami tidak akan melakukannya dalam latihan ini; tetapi dapat berguna untuk melakukan ini di lingkungan produksi.
8. Di tab **Detail**, atur **Nama aturan peringatan** ke **Peringatan Daftar Kunci**.
9. Pilih **Tinjau + buat**. 
10. Tinjau konfigurasi untuk peringatan. Pilih **Buat** dan tunggu aturan peringatan dibuat.
11. Kini Anda dapat menggunakan perintah berikut untuk mendapatkan daftar kunci layanan Azure AI dengan mengganti *&lt;resourceName&gt;* dengan nama sumber daya layanan Azure AI Anda, dan *&lt;resourceGroup&gt;* dengan nama grup sumber daya tempat Anda membuatnya.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    Perintah ini mengembalikan daftar kunci untuk sumber daya layanan Azure AI Anda.

    > **Catatan** Jika Anda belum masuk ke Azure CLI, Anda mungkin perlu menjalankannya `az login` sebelum perintah daftar kunci dapat berfungsi.

12. Beralih kembali ke browser yang berisi portal Microsoft Azure, dan segarkan **Halaman peringatan** Anda. Anda akan melihat peringatan **Sev 4** yang tercantum dalam tabel (jika tidak, tunggu hingga lima menit dan segarkan kembali).
13. Pilih peringatan untuk melihat detailnya.

## Visualisasikan metrik

Selain menentukan peringatan, Anda dapat melihat metrik untuk sumber daya layanan Azure AI Anda untuk memantau pemanfaatannya.

1. Di portal Azure, di halaman sumber daya layanan Azure AI Anda, pilih **Metrik** (di bagian **Pemantauan**).
2. Jika tidak ada bagan yang ada, pilih **+ Bagan baru**. Kemudian dalam daftar **Metrik**, tinjau kemungkinan metrik yang dapat Anda visualisasikan dan pilih **Total Panggilan**.
3. Dalam daftar **Agregasi**, pilih **Hitung**.  Ini akan memungkinkan Anda untuk memantau total panggilan ke sumber daya Layanan Azure AI Anda; yang berguna dalam menentukan seberapa banyak layanan digunakan selama periode waktu tertentu.
4. Untuk menghasilkan beberapa permintaan ke layanan Azure AI, Anda akan menggunakan **curl** - alat baris perintah untuk permintaan HTTP. Di editor Anda, buka **rest-test.cmd** dan edit perintah **curl** yang ada di dalamnya (ditampilkan di bawah), ganti *&lt;yourEndpoint&gt;* dan *&lt;yourKey&gt;* dengan URI titik akhir dan kunci **Key1** untuk menggunakan API Analitik Teks di sumber daya layanan Azure AI Anda.

    ```
    curl -X POST "<your-endpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <your-key>" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

5. Simpan perubahan Anda, lalu jalankan perintah berikut:

    ```
    ./rest-test.cmd
    ```

    Perintah mengembalikan dokumen JSON yang berisi informasi tentang bahasa yang terdeteksi dalam data input (yang seharusnya bahasa Inggris).

6. Jalankan kembali perintah **rest-test** beberapa kali untuk menghasilkan beberapa aktivitas panggilan (Anda dapat menggunakan tombol **^** untuk menelusuri perintah sebelumnya).
7. Kembali ke halaman **Metrik** di portal Microsoft Azure dan segarkan bagan jumlah **Total Panggilan**. Mungkin perlu beberapa menit agar panggilan yang Anda lakukan menggunakan *curl* terlihat di bagan - terus segarkan bagan hingga diperbarui untuk menyertakannya.

## Membersihkan sumber daya

Jika Anda tidak menggunakan sumber daya Azure yang dibuat di lab ini untuk modul pelatihan lainnya, Anda dapat menghapusnya untuk menghindari dikenakan biaya lebih lanjut.

1. Buka portal Microsoft Azure di`https://portal.azure.com`, dan di bilah pencarian atas, cari sumber daya yang Anda buat di lab ini.

2. Pada halaman sumber daya, pilih **Hapus** dan ikuti instruksi untuk menghapus sumber daya. Atau, Anda dapat menghapus seluruh grup sumber daya untuk membersihkan semua sumber daya secara bersamaan.

## Informasi selengkapnya

Salah satu opsi untuk memantau layanan Azure AI adalah dengan menggunakan *pengelogan diagnostik*. Setelah diaktifkan, pengelogan diagnostik menangkap informasi yang kaya tentang penggunaan sumber daya layanan Azure AI Anda, dan dapat menjadi alat pemantauan dan penelusuran kesalahan yang berguna. Diperlukan waktu lebih dari satu jam setelah menyiapkan pengelogan diagnostik untuk menghasilkan informasi apa pun, itulah sebabnya kami belum menjelajahinya dalam latihan ini; tetapi Anda dapat mempelajarinya lebih lanjut di [dokumentasi Layanan Azure AI](https://docs.microsoft.com/azure/ai-services/diagnostic-logging).
