---
lab:
  title: Mengelola Keamanan Layanan Azure AI
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Mengelola Keamanan Layanan Azure AI

Keamanan merupakan pertimbangan penting untuk aplikasi apa pun, dan sebagai pengembang Anda harus memastikan bahwa akses ke sumber daya seperti layanan Azure AI dibatasi hanya untuk mereka yang memerlukannya.

Akses ke layanan Azure AI biasanya dikontrol melalui kunci autentikasi, yang dihasilkan saat Anda pertama kali membuat sumber daya layanan Azure AI.

## Mengkloning repositori di Visual Studio Code

Anda akan mengembangkan kode menggunakan Visual Studio Code. File kode untuk aplikasi Anda telah disediakan di repositori GitHub.

> **Tips**: Jika Anda sudah mengkloning repositori **mslearn-ai-services**, buka repositori di Visual Studio Code. Atau, ikuti langkah-langkah ini untuk mengkloningnya ke lingkungan pengembangan Anda.

1. Memulai Visual Studio Code.
2. Buka palet (SHIFT+CTRL+P) dan jalankan **Git: Perintah klon** untuk mengkloning repositori `https://github.com/MicrosoftLearning/mslearn-ai-services` ke folder lokal (tidak masalah folder mana).
3. Setelah repositori dikloning, buka folder di Visual Studio Code.
4. Tunggu sementara file tambahan diinstal untuk mendukung proyek kode C# di repositori, jika diperlukan

    > **Catatan**: Jika Anda diminta untuk menambahkan aset yang diperlukan guna membangun dan men-debug, pilih **Tidak Sekarang**.

5. Luaskan folder `Labfiles/02-ai-services-security`.

Kode untuk C# dan Python telah disediakan. Luaskan folder bahasa pilihan Anda.

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

## Mengelola kunci autentikasi

Saat Anda membuat sumber daya layanan Azure AI, dua kunci autentikasi dibuat. Anda dapat mengelola ini di portal Azure atau dengan menggunakan antarmuka baris perintah Azure (CLI).

1. Di portal Azure, buka sumber daya layanan Azure AI Anda dan tampilkan halaman **Kunci dan Titik Akhir**. Halaman ini berisi informasi yang Anda perlukan untuk terhubung ke sumber daya Anda dan menggunakannya dari aplikasi yang Anda kembangkan. Khususnya:
    - *Titik akhir* HTTP tempat aplikasi klien dapat mengirim permintaan.
    - Dua *kunci* yang dapat digunakan untuk autentikasi (aplikasi klien dapat menggunakan salah satu kunci. Praktik yang umum adalah menggunakan satu untuk pengembangan, dan satu lagi untuk produksi. Anda dapat dengan mudah membuat ulang kunci pengembangan setelah pengembang menyelesaikan pekerjaan mereka untuk mencegah akses lanjutan).
    - *Lokasi* tempat sumber daya dihosting. Ini diperlukan untuk permintaan ke beberapa (tetapi tidak semua) API.
2. Anda kini dapat menggunakan perintah berikut untuk mendapatkan daftar kunci layanan Azure AI dengan mengganti *&lt;resourceName&gt;* dengan nama sumber daya layanan Azure AI Anda, dan *&lt;resourceGroup&gt;* dengan nama grup sumber daya tempat Anda membuatnya.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    Perintah ini mengembalikan daftar kunci untuk sumber daya layanan Azure AI Anda - terdapat dua kunci, bernama **key1** dan **key2**.

    > **Tips**: Jika Anda belum mengautentikasi Azure CLI, jalankan `az login` dan masuk ke akun Anda.

3. Untuk menguji layanan Azure AI, Anda dapat menggunakan **curl** - alat baris perintah untuk permintaan HTTP. Di folder **02-ai-services-security**, buka **rest-test.cmd** dan edit perintah **curl** yang ada di dalamnya (ditunjukkan di bawah), ganti *&lt;yourEndpoint&gt;* dan *&lt;yourKey&gt;* dengan URI titik akhir dan kunci **Key1** untuk menggunakan API Analisis Teks di sumber daya layanan Azure AI Anda.

    ```bash
    curl -X POST "<yourEndpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: 81468b6728294aab99c489664a818197" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

4. Simpan perubahan Anda, lalu jalankan perintah berikut:

    ```
    ./rest-test.cmd
    ```

Perintah mengembalikan dokumen JSON yang berisi informasi tentang bahasa yang terdeteksi dalam data input (yang seharusnya bahasa Inggris).

5. Jika kunci disusupi, atau pengembang yang memilikinya tidak lagi memerlukan akses, Anda dapat membuat ulang di portal atau dengan menggunakan Azure CLI. Jalankan perintah berikut untuk membuat ulang kunci **key1** Anda (menggantikan *&lt;resourceName&gt;* dan *&lt;resourceGroup&gt;* untuk sumber daya Anda).

    ```
    az cognitiveservices account keys regenerate --name <resourceName> --resource-group <resourceGroup> --key-name key1
    ```

Daftar kunci untuk sumber daya layanan Azure AI Anda dikembalikan - perhatikan bahwa **key1** telah berubah sejak terakhir kali Anda mengambilnya.

6. Jalankan kembali perintah **rest-test** dengan kunci lama (Anda dapat menggunakan panah **^** di keyboard untuk menelusuri perintah sebelumnya), dan pastikan bahwa hal tersebut kini gagal.
7. Edit perintah *curl* di **rest-test.cmd** dengan mengganti kunci dengan nilai **key1** yang baru, dan simpan perubahannya. Kemudian jalankan kembali perintah **rest-test** dan pastikan bahwa itu berhasil.

> **Tips**: Dalam latihan ini, Anda menggunakan nama lengkap parameter Azure CLI, seperti **--resource-group**.  Anda juga dapat menggunakan alternatif yang lebih pendek, seperti **-g**, untuk membuat perintah Anda tidak terlalu bertele-tele (tetapi sedikit lebih sulit untuk dipahami).  [Referensi perintah CLI Layanan Azure AI](https://docs.microsoft.com/cli/azure/cognitiveservices?view=azure-cli-latest) mencantumkan opsi parameter untuk setiap perintah CLI layanan Azure AI.

## Akses kunci aman dengan Azure Key Vault

Anda dapat mengembangkan aplikasi yang menggunakan layanan Azure AI dengan menggunakan kunci untuk autentikasi. Namun, ini berarti bahwa kode aplikasi harus dapat memperoleh kuncinya. Salah satu opsi adalah menyimpan kunci dalam variabel lingkungan atau file konfigurasi tempat aplikasi disebarkan, tetapi pendekatan ini membuat kunci rentan terhadap akses yang tidak sah. Pendekatan yang lebih baik saat mengembangkan aplikasi di Azure adalah dengan menyimpan kunci dengan aman di Azure Key Vault, dan memberikan akses ke kunci tersebut melalui *identitas terkelola* (dengan kata lain, akun pengguna yang digunakan oleh aplikasi itu sendiri).

### Buat brankas kunci dan tambahkan rahasia

Pertama, Anda perlu membuat brankas kunci dan menambahkan *rahasia* untuk kunci layanan Azure AI.

1. Catat nilai **key1** untuk sumber daya layanan Azure AI Anda (atau salin nilai ke clipboard).
2. Di portal Microsoft Azure, pada halaman **Beranda**, pilih tombol **&#65291;Buat sumber daya**, cari *Key Vault*, dan buat sumber daya **Key Vault** dengan pengaturan berikut:

    - **Tab Dasar**
        - **Langganan**: *Langganan Azure Anda*
        - **Grup sumber daya**: *Grup sumber daya yang sama dengan sumber daya layanan Azure AI Anda*
        - **Nama brankas kunci**: *Masukkan nama yang unik*
        - **Wilayah**: *Wilayah yang sama dengan sumber daya layanan Azure AI Anda*
        - **Tingkat harga**: Standar
    
    - Tab **Akses konfigurasi**
        -  **Model izin**: Kebijakan akses brankas
        - Gulir ke bawah ke bagian **Kebijakan akses** dan pilih pengguna Anda menggunakan kotak centang di sebelah kiri. Kemudian, pilih **Tinjau + buat**, dan pilih **Buat** untuk membuat sumber daya Anda.
     
3. Tunggu hingga penerapan selesai, lalu buka sumber daya brankas kunci Anda.
4. Di panel navigasi kiri, pilih **Rahasia** (di bagian Objek).
5. Pilih **+ Hasilkan/Impor** dan tambahkan rahasia baru dengan pengaturan berikut:
    - **Opsi unggah**: Manual
    - **Nama**: AI-Services-Key *(penting untuk mencocokkan hal ini dengan tepat, karena nanti Anda akan menjalankan kode yang mengambil rahasia berdasarkan nama ini)*
    - **Nilai**: *Kunci layanan Azure AI **key1** Anda*
6. Pilih **Buat**.

### Membuat perwakilan layanan

Untuk mengakses rahasia di brankas kunci, aplikasi Anda harus menggunakan prinsip layanan yang memiliki akses ke rahasia tersebut. Anda akan menggunakan antarmuka baris perintah (CLI) Azure untuk membuat prinsip layanan, menemukan ID objeknya, dan memberikan akses ke rahasia di Azure Vault.

1. Jalankan perintah Azure CLI berikut, ganti *&lt;spName&gt;* dengan nama unik yang sesuai untuk identitas aplikasi (misalnya, *ai-app* dengan inisial Anda ditambahkan di bagian akhir; nama harus unik dalam penyewa Anda). Ganti juga *&lt;subscriptionId&gt;* dan *&lt;resourceGroup&gt;* dengan nilai yang benar untuk ID langganan Anda dan grup sumber daya yang berisi sumber daya brankas kunci dan layanan Azure AI Anda:

    > **Tips**: Jika Anda tidak yakin dengan ID langganan Anda, gunakan perintah **az account show** untuk mengambil informasi langganan Anda - ID langganan adalah atribut **id** di output. Jika Anda melihat kesalahan pada objek yang sudah ada, pilih nama unik lain.

    ```
    az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
    ```

Output dari perintah ini mencakup informasi tentang prinsip layanan baru Anda. Ini akan terlihat mirip dengan ini:

    ```
    {
        "appId": "abcd12345efghi67890jklmn",
        "displayName": "api://ai-app-",
        "password": "1a2b3c4d5e6f7g8h9i0j",
        "tenant": "1234abcd5678fghi90jklm"
    }
    ```

Catat nilai **appId**, **password**, dan **tenant** - Anda akan memerlukannya nanti (jika menutup terminal ini, Anda tidak akan dapat ambil kata sandinya; jadi penting untuk mencatat nilainya sekarang - Anda dapat menempelkan output ke dalam file teks baru di mesin lokal untuk memastikan Anda dapat menemukan nilai yang Anda perlukan nanti!)

2. Untuk mendapatkan **ID objek** perwakilan layanan Anda, jalankan perintah Azure CLI berikut, ganti *&lt;appId&gt;* dengan nilai ID aplikasi prinsipal layanan Anda.

    ```
    az ad sp show --id <appId>
    ```

3. Salin nilai `id` di json yang dikembalikan sebagai respons. 
3. Untuk menetapkan izin bagi prinsipal layanan baru Anda untuk mengakses rahasia di Brankas Kunci Anda, jalankan perintah Azure CLI berikut, ganti *&lt;keyVaultName&gt;* dengan nama sumber daya Azure Key Vault Anda dan * &lt;objectId&gt;* dengan nilai dari nilai ID prinsipal layanan yang baru saja Anda salin.

    ```
    az keyvault set-policy -n <keyVaultName> --object-id <objectId> --secret-permissions get list
    ```

### Menggunakan prinsip layanan dalam aplikasi

Anda kini siap menggunakan identitas prinsipal layanan dalam aplikasi sehingga hal tersebut dapat mengakses kunci layanan Azure AI rahasia di brankas kunci Anda dan menggunakannya untuk menyambungkan ke sumber daya layanan Azure AI Anda.

> **Catatan**: Dalam latihan ini, kami akan menyimpan kredensial utama layanan dalam konfigurasi aplikasi dan menggunakannya untuk mengautentikasi identitas **ClientSecretCredential** dalam kode aplikasi Anda. Latihan ini bagus untuk pengembangan dan pengujian, tetapi dalam aplikasi produksi nyata, administrator akan menetapkan *identitas terkelola* ke aplikasi sehingga aplikasi tersebut menggunakan identitas utama layanan untuk mengakses sumber daya, tanpa menembolok atau menyimpan sandi.

1. Di terminal Anda, beralihlah ke folder **C-Sharp** atau **Python** bergantung pada preferensi bahasa Anda dengan menjalankan `cd C-Sharp` atau `cd Python`. Kemudian, jalankan `cd keyvault_client` untuk menavigasi ke folder aplikasi.
2. Instal paket yang perlu Anda gunakan untuk Azure Key Vault dan API Analitik Teks di sumber daya layanan Azure AI Anda dengan menjalankan perintah yang sesuai untuk preferensi bahasa Anda:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    dotnet add package Azure.Identity --version 1.5.0
    dotnet add package Azure.Security.KeyVault.Secrets --version 4.2.0-beta.3
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    pip install azure-identity==1.5.0
    pip install azure-keyvault-secrets==4.2.0
    ```

3. Lihat konten folder **keyvault-client**, dan perhatikan bahwa folder tersebut berisi file untuk pengaturan konfigurasi:
    - **C#**: appsettings.json
    - **Python**: .env

    Buka file konfigurasi dan perbarui nilai konfigurasi yang dikandungnya untuk mencerminkan pengaturan berikut:
    
    - **Titik akhir** untuk sumber daya Layanan Azure AI Anda
    - Nama sumber daya **Azure Key Vault** Anda
    - **Penyewa** untuk perwakilan layanan Anda
    - **appId** untuk perwakilan layanan Anda
    - **Kata sandi** untuk perwakilan layanan Anda

     Simpan perubahan Anda dengan menekan **CTRL+S**.
4. Perhatikan bahwa folder **keyvault-client** berisi file kode untuk aplikasi klien:

    - **C#**: Program.cs
    - **Python**: keyvault-client.py

    Buka file kode dan tinjau kode yang ada di dalamnya, perhatikan detail berikut:
    - Namespace untuk SDK yang Anda instal telah diimpor
    - Kode dalam fungsi **Utama** mengambil pengaturan konfigurasi aplikasi, lalu menggunakan kredensial prinsipal layanan untuk mendapatkan kunci layanan Azure AI dari brankas kunci.
    - Fungsi **GetLanguage** menggunakan SDK untuk membuat klien untuk layanan, dan kemudian menggunakan klien untuk mendeteksi bahasa teks yang dimasukkan.
5. Masukkan perintah berikut untuk menjalankan program:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python keyvault-client.py
    ```

6. Saat diminta, masukkan beberapa teks dan tinjau bahasa yang terdeteksi oleh layanan. Misalnya, coba masukkan "Halo", "Bonjour", dan "Gracias".
7. Setelah Anda selesai menguji aplikasi, masukkan "berhenti" untuk menghentikan program.

## Membersihkan sumber daya

Jika Anda tidak menggunakan sumber daya Azure yang dibuat di lab ini untuk modul pelatihan lainnya, Anda dapat menghapusnya untuk menghindari dikenakan biaya lebih lanjut.

1. Buka portal Azure di `https://portal.azure.com`, dan di bilah pencarian atas, cari sumber daya yang Anda buat di lab ini.

2. Pada halaman sumber daya, pilih **Hapus** dan ikuti instruksi untuk menghapus sumber daya. Atau, Anda dapat menghapus seluruh grup sumber daya untuk membersihkan semua sumber daya secara bersamaan.

## Informasi selengkapnya

Untuk informasi selengkapnya tentang mengamankan layanan Azure AI, lihat [dokumentasi keamanan Layanan Azure AI](https://docs.microsoft.com/azure/ai-services/security-features).
