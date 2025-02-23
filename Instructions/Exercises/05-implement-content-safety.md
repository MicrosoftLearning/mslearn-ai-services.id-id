---
lab:
  title: Menerapkan Keamanan Konten Azure AI
---

# Menerapkan Keamanan Konten Azure AI

Dalam latihan ini, Anda akan menyediakan sumber daya Keamanan Konten, menguji sumber daya di Azure AI Studio, dan menguji sumber daya dalam kode.

## Memprovisikan sumber daya *Keamanan Konten*

Jika Anda belum memilikinya, Anda harus menyediakan sumber daya **Keamanan Konten** di langganan Azure Anda.

1. Buka portal Microsoft Azure di `https://portal.azure.com`, dan masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda.
1. Pilih **Buat sumber daya**.
1. Pada kolom pencarian, cari **Keamanan Konten**. Kemudian, dalam hasilnya, pilih **Buat** di bawah **Keamanan Konten Azure AI**.
1. Provisikan sumber daya menggunakan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Memilih atau membuat grup sumber daya*.
    - **Wilayah**, pilih **US Timur**
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Pilih **F0** (*gratis*), atau **S** (*standar*) jika F0 tidak tersedia.
1. Pilih **Tinjau + buat**, lalu pilih **Buat** untuk menyediakan sumber daya.
1. Tunggu hingga penyebaran selesai, lalu buka sumber dayanya.
1. Pilih **Kontrol Akses** di navigasi kiri, lalu pilih **+ Tambahkan** dan **Tambahkan penetapan peran**.
1. Gulir ke bawah untuk memilih peran **Pengguna Layanan Kognitif** dan pilih **Berikutnya**.
1. Tambahkan akun Anda ke peran ini, lalu pilih **Tinjau + tetapkan**.
1. Pilih **Manajemen Sumber Daya** di bilah navigasi kiri, dan pilih **Kunci dan Titik Akhir**. Biarkan halaman ini terbuka sehingga Anda dapat menyalin kunci nanti.

## Menggunakan Perisai Perintah Keamanan Konten Azure AI

Dalam latihan ini, Anda akan menggunakan Azure AI Studio untuk menguji Perisai Prompt Keamanan Konten dengan dua input sampel. Satu mensimulasikan permintaan pengguna, dan yang lainnya mensimulasikan dokumen dengan teks yang berpotensi tidak aman yang disematkan ke dalamnya.

1. Di tab browser lain, buka halaman Keamanan Konten [Azure AI Studio](https://ai.azure.com/explore/contentsafety) dan masuk.
1. Di bawah **Moderasi konten teks** pilih **Cobalah**.
1. Pada halaman **Memoderasi konten teks**, di bawah **Layanan Azure AI** pilih sumber daya Keamanan Konten yang Anda buat sebelumnya.
1. Pilih **Beberapa kategori risiko dalam satu kalimat**. Tinjau teks dokumen untuk potensi masalah.
1. Pilih **Jalankan pengujian** dan tinjau hasilnya.
1. Secara opsional, ubah tingkat ambang batas dan pilih **Jalankan pengujian** lagi.
1. Di bilah navigasi kiri, pilih **Pendeteksian materi yang dilindungi untuk teks**.
1. Pilih **Lirik yang dilindungi** dan perhatikan bahwa ini adalah lirik lagu yang telah diterbitkan.
1. Pilih **Jalankan pengujian** dan tinjau hasilnya.
1. Di bilah navigasi kiri, pilih **Moderasi konten gambar**.
1. Pilih **Konten melukai diri sendiri**.
1. Perhatikan bahwa semua gambar diburamkan secara default di AI Studio. Anda juga harus menyadari bahwa konten seksual dalam sampel sangat ringan.
1. Pilih **Jalankan pengujian** dan tinjau hasilnya.
1. Di bilah navigasi kiri. pilih **Perisai perintah** 
1. Pada **halaman Perisai perintah**, di bawah **Layanan Azure AI** pilih sumber daya Keamanan Konten yang Anda buat sebelumnya.
1. Pilih **Perintah & konten serangan dokumen**. Tinjau perintah pengguna dan teks dokumen untuk potensi masalah.
1. Pilih **Jalankan pengujian**.
1. Di **Lihat hasil**, verifikasi bahwa serangan Jailbreak terdeteksi di perintah pengguna dan dokumen.

    > [!TIP]
    > Kode tersedia untuk semua sampel di AI Studio.

1. Di bawah **Langkah berikutnya**, di bawah **Lihat kode** pilih **Lihat kode**. Jendela **Kode sampel** ditampilkan.
1. Gunakan panah bawah untuk memilih Python atau C# lalu pilih **Salin** untuk menyalin kode sampel ke clipboard.
1. Tutup layar **Kode sampel**.

### Mengonfigurasi aplikasi Anda

Anda sekarang akan membuat aplikasi di C# atau Python.

#### C#

##### Prasyarat

* [Visual Studio Code](https://code.visualstudio.com/) di salah satu [platform yang didukung](https://code.visualstudio.com/docs/supporting/requirements#_platforms).
* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) adalah kerangka kerja target untuk latihan ini.
* [Ekstensi C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) untuk Visual Studio Code.

##### Persiapan

Lakukan langkah-langkah berikut untuk menyiapkan Visual Studio Code untuk latihan tersebut.

1. Mulai Visual Studio Code dan di tampilan Explorer, klik **Buat Proyek .NET** dengan memilih **Aplikasi Konsol**.
1. Pilih folder di komputer Anda, dan berikan nama untuk proyek. Pilih **Buat proyek** dan terima pesan peringatan.
1. Di panel Explorer, perluas Penjelajah Solusi dan pilih **Program.cs**.
1. Buat dan jalankan proyek dengan memilih **Jalankan** -> **Jalankan tanpa Penelusuran Kesalahan**. 
1. Di Penjelajah Solusi, klik kanan proyek C# dan pilih **Tambahkan Paket NuGet.**
1. Cari **Azure.AI.TextAnalytics** dan pilih versi terbaru.
1. Cari Paket NuGet kedua: **Microsoft.Extensions.Configuration.Json 8.0.0**. File proyek sekarang seharusnya mencantumkan dua paket NuGet.

##### Tambahkan kode

1. Tempelkan kode sampel yang Anda salin sebelumnya di bawah bagian **ItemGroup** .
1. Gulir ke bawah untuk menemukan *Ganti dengan _key langganan dan titik akhir Anda sendiri*.
1. Di portal Azure, pada halaman Kunci dan Titik Akhir, salin salah satu Kunci (1 atau 2). Ganti **<your_subscription_key>** dengan nilai ini.
1. Di portal Azure, pada halaman Kunci dan Titik Akhir, salin Titik Akhir. Tempelkan nilai ini ke dalam kode Anda untuk menggantikan **<your_endpoint_value>**.
1. Di **Azure AI Studio**, salin nilai **Permintaan pengguna**. Tempelkan ini ke dalam kode Anda untuk menggantikan **<test_user_prompt>**.
1. Gulir ke bawah ke **<this_is_a_document_source>** dan hapus baris kode ini.
1. Di **Azure AI Studio**, salin nilai **Dokumen** .
1. Gulir ke bawah ke **<this_is_another_document_source>** dan tempelkan nilai dokumen Anda.
1. Pilih **Jalankan** -> **Jalankan tanpa Penelusuran Kesalahan** dan verifikasi bahwa ada serangan yang terdeteksi. 

#### Python

##### Prasyarat

* [Visual Studio Code](https://code.visualstudio.com/) di salah satu [platform yang didukung](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* [Ekstensi Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) terinstal untuk Visual Studio Code.

* [Modul permintaan](https://pypi.org/project/requests/) terinstal.

1. Buat file Python baru dengan ekstensi **.py** dan beri nama yang sesuai.
1. Tempelkan kode sampel yang Anda salin sebelumnya.
1. Gulir ke bawah untuk menemukan bagian berjudul *Ganti dengan _key langganan dan titik akhir Anda sendiri*.
1. Di portal Azure, pada halaman Kunci dan Titik Akhir, salin salah satu Kunci (1 atau 2). Ganti **<your_subscription_key>** dengan nilai ini.
1. Di portal Azure, pada halaman Kunci dan Titik Akhir, salin Titik Akhir. Tempelkan nilai ini ke dalam kode Anda untuk menggantikan **<your_endpoint_value>**.
1. Di **Azure AI Studio**, salin nilai **Permintaan pengguna**. Tempelkan ini ke dalam kode Anda untuk menggantikan **<test_user_prompt>**.
1. Gulir ke bawah ke **<this_is_a_document_source>** dan hapus baris kode ini.
1. Di **Azure AI Studio**, salin nilai **Dokumen** .
1. Gulir ke bawah ke **<this_is_another_document_source>** dan tempelkan nilai dokumen Anda.
1. Dari terminal terintegrasi untuk file Anda, jalankan program, misalnya:

    - `.\prompt-shield.py`

1. Validasi bahwa serangan terdeteksi.
1. Secara opsional, Anda dapat bereksperimen dengan konten pengujian dan nilai dokumen yang berbeda.
