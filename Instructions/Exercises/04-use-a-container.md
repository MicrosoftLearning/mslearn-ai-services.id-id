---
lab:
  title: Menggunakan Kontainer Layanan Azure AI
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Menggunakan Kontainer Layanan Azure AI

Menggunakan layanan Azure AI yang dihosting di Azure memungkinkan pengembang aplikasi untuk fokus pada infrastruktur untuk kode mereka sendiri sekaligus memanfaatkan layanan scalable yang dikelola oleh Microsoft. Namun, dalam banyak skenario, organisasi memerlukan kontrol lebih besar atas infrastruktur layanan mereka dan data yang diteruskan antar layanan.

Banyak dari API layanan Azure AI dapat dikemas dan disebarkan dalam sebuah *kontainer*, memungkinkan organisasi untuk menghosting layanan Azure AI dalam infrastruktur mereka sendiri; misalnya di server Docker lokal, Azure Container Instances, atau kluster Azure Kubernetes Service. Layanan Azure AI dalam kontainer perlu berkomunikasi dengan akun layanan Azure AI berbasis Azure untuk mendukung penagihan; tetapi data aplikasi tidak diteruskan ke layanan back-end, dan organisasi memiliki kontrol lebih besar atas konfigurasi penyebaran kontainer mereka, memungkinkan solusi kustom untuk autentikasi, skalabilitas, dan pertimbangan lainnya.

## Mengkloning repositori di Visual Studio Code

Anda akan mengembangkan kode menggunakan Visual Studio Code. File kode untuk aplikasi Anda telah disediakan di repositori GitHub.

> **Tips**: Jika Anda sudah mengkloning repositori **mslearn-ai-services**, buka dalam Visual Studio Code. Atau, ikuti langkah-langkah ini untuk mengkloningnya ke lingkungan pengembangan Anda.

1. Memulai Visual Studio Code.
2. Buka palet (SHIFT+CTRL+P) dan jalankan **Git: Perintah klon** untuk mengkloning repositori `https://github.com/MicrosoftLearning/mslearn-ai-services` ke folder lokal (tidak masalah folder mana).
3. Setelah repositori dikloning, buka folder di Visual Studio Code.
4. Tunggu sementara file tambahan diinstal untuk mendukung proyek kode C# di repositori, jika diperlukan

    > **Catatan**: Jika Anda diminta untuk menambahkan aset yang diperlukan guna membangun dan men-debug, pilih **Tidak Sekarang**.

5. Luaskan folder `Labfiles/04-use-a-container`.

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
5. Saat sumber daya telah diterapkan, buka dan lihat halaman **Kunci dan Titik Akhir**. Anda akan memerlukan titik akhir dan salah satu kunci dari halaman ini dalam prosedur selanjutnya.

## Terapkan dan jalankan kontainer Analisis Sentimen

Banyak API layanan Azure AI yang biasa digunakan tersedia di citra kontainer. Untuk daftar lengkapnya, lihat [dokumentasi layanan Azure AI](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-container-support#containers-in-azure-ai-services). Dalam latihan ini, Anda akan menggunakan gambar penampung API Analisis Teks *Analisis Sentimen*; namun prinsip-prinsipnya sama untuk semua gambar yang tersedia.

1. Di portal Microsoft Azure, pada halaman **Beranda**, pilih tombol **&#65291;Buat sumber daya**, jelajahi *instans kontainer*, dan buat sumber daya **Instans Kontainer** dengan pengaturan berikut:

    - **Dasar-dasar**:
        - **Langganan**: *Langganan Azure Anda*
        - **Grup sumber daya**: *Pilih grup sumber daya yang berisi sumber daya layanan Azure AI Anda*
        - **Nama penampung**: *Masukkan nama yang unik*
        - **Wilayah**: *Pilih wilayah yang tersedia*
        - **Zona ketersediaan**: Tidak ada
        - **SKU**: Standar
        - **Sumber gambar**: Registri Lainnya
        - **Jenis gambar**: Publik
        - **Gambar**: `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:latest`
        - **Jenis OS**: Linux
        - **Ukuran**: 1 vcpu, memori 8 GB
    - **Jaringan**:
        - **Jenis jaringan**: Publik
        - **Label nama DNS**: *Masukkan nama unik untuk titik akhir penampung*
        - **Pelabuhan**: *Ubah port TCP dari 80 menjadi **5000***
    - **Lanjutan**:
        - **Kebijakan mulai ulang**: Pada kegagalan
        - **Variabel lingkungan**:

            | Tandai sebagai aman | Tombol | Nilai |
            | -------------- | --- | ----- |
            | Ya | `ApiKey` | *Kunci untuk sumber daya layanan Azure AI Anda* |
            | Ya | `Billing` | *URI titik akhir untuk sumber daya layanan Azure AI Anda* |
            | No | `Eula` | `accept` |

        - **Penggantian perintah**: [ ]
        - **Manajemen kunci**: Kunci yang dikelola Microsoft (MMK)
    - **Tag**:
        - *Jangan tambahkan tag apa pun*

2. Pilih **Tinjau + buat**, lalu pilih **Buat**. Tunggu hingga penyebaran selesai, lalu buka sumber daya yang disebarkan.
    > **Catatan** Harap diperhatikan bahwa menyebarkan kontainer Azure AI ke Azure Container Instances biasanya membutuhkan waktu 5-10 menit (provisi) sebelum siap digunakan.
3. Amati properti berikut dari sumber daya instans penampung Anda di halaman **Ringkasan**:
    - **Status**: Ini seharusnya *Berjalan*.
    - **Alamat IP**: Ini adalah alamat IP publik yang dapat Anda gunakan untuk mengakses instans kontainer Anda.
    - **FQDN**: Ini adalah *nama domain yang sepenuhnya memenuhi syarat* dari sumber daya instans kontainer, Anda dapat menggunakan ini untuk mengakses instans kontainer alih-alih alamat IP.

    > **Catatan**: Dalam latihan ini, Anda telah menerapkan gambar kontainer layanan Azure AI untuk analisis sentimen ke sumber daya Azure Container Instances (ACI). Anda dapat menggunakan pendekatan serupa untuk menerapkannya ke host *[Docker](https://www.docker.com/products/docker-desktop)* di komputer atau jaringan Anda sendiri dengan menjalankan perintah berikut (pada satu baris) untuk menerapkan kontainer analisis sentimen ke instans Docker lokal Anda, menggantikan *&lt;yourEndpoint&gt;* dan *&lt;yourKey&gt;* dengan URI titik akhir Anda dan salah satu kunci untuk sumber daya layanan Azure AI Anda.
    > Perintah akan mencari gambar di mesin lokal Anda, dan jika tidak menemukannya di sana, perintah akan menariknya dari registri gambar *mcr.microsoft.com* dan menyebarkannya ke instans Docker Anda. Saat penyebaran selesai, penampung akan mulai dan mendengarkan permintaan masuk pada port 5000.

    ```
    docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:latest Eula=accept Billing=<yourEndpoint> ApiKey=<yourKey>
    ```

## Gunakan kontainer

1. Di editor Anda, buka **rest-test.cmd**, dan edit perintah **curl** yang ada di dalamnya (ditampilkan di bawah), menggantikan *&lt;your_ACI_IP_address_or_FQDN&gt;* dengan alamat IP atau FQDN untuk kontainer Anda.

    ```
    curl -X POST "http://<your_ACI_IP_address_or_FQDN>:5000/text/analytics/v3.1/sentiment" -H "Content-Type: application/json" --data-ascii "{'documents':[{'id':1,'text':'The performance was amazing! The sound could have been clearer.'},{'id':2,'text':'The food and service were unacceptable. While the host was nice, the waiter was rude and food was cold.'}]}"
    ```

2. Simpan perubahan Anda terhadap skrip dengan menekan **CTRL+S**. Perhatikan bahwa Anda tidak perlu menentukan titik akhir atau kunci layanan Azure AI - permintaan diproses oleh layanan dalam kontainer. Kontainer pada gilirannya berkomunikasi secara berkala dengan layanan di Azure untuk melaporkan penggunaan untuk penagihan, tetapi tidak mengirim data permintaan.
3. Masukkan perintah berikut untuk menjalankan skrip:

    ```
    ./rest-test.cmd
    ```

4. Verifikasi bahwa perintah mengembalikan dokumen JSON yang berisi informasi tentang sentimen yang terdeteksi dalam dua dokumen input (yang harus positif dan negatif, dengan urutan demikian).

## Pembersihan

Jika Anda telah selesai bereksperimen dengan instans kontainer Anda, Anda harus menghapusnya.

1. Di portal Microsoft Azure, buka grup sumber daya tempat Anda membuat sumber daya untuk latihan ini.
2. Pilih sumber daya instans kontainer dan hapus.

## Membersihkan sumber daya

Jika Anda tidak menggunakan sumber daya Azure yang dibuat di lab ini untuk modul pelatihan lainnya, Anda dapat menghapusnya untuk menghindari dikenakan biaya lebih lanjut.

1. Buka portal Microsoft Azure di`https://portal.azure.com`, dan di bilah pencarian atas, cari sumber daya yang Anda buat di lab ini.

2. Pada halaman sumber daya, pilih **Hapus** dan ikuti instruksi untuk menghapus sumber daya. Atau, Anda dapat menghapus seluruh grup sumber daya untuk membersihkan semua sumber daya secara bersamaan.

## Informasi selengkapnya

Untuk informasi selengkapnya tentang kontainer layanan Azure AI, lihat [dokumentasi kontainer Layanan Azure AI](https://learn.microsoft.com/azure/ai-services/cognitive-services-container-support).
