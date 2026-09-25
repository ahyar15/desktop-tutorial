**TUGAS 2 RANCANGAN PROYEK & SOFTWARE REQUIREMENTS SPECIFICATION (SRS)**

**Sistem Informasi Peminjaman Ruangan dan Fasilitas Kampus**

Mata Kuliah: Web Framework

Kelompok: Ahqiyar

Sultan Syahbanta

Fadhlun

# 1. Judul Proyek dan Justifikasi

## 1.1 Judul Proyek

**SIPRUFA --- Sistem Informasi Peminjaman Ruangan dan Fasilitas Kampus**

## 1.2 Latar Belakang Masalah

Di banyak kampus, proses peminjaman ruangan kelas, aula, laboratorium,
atau fasilitas pendukung (proyektor, sound system, kursi tambahan, dan
sejenisnya) masih dilakukan secara manual --- misalnya lewat WhatsApp,
datang langsung ke petugas sarana-prasarana, atau mengisi buku
peminjaman fisik. Analoginya seperti mencoba memesan meja restoran hanya
lewat telepon tanpa sistem reservasi: rawan miskomunikasi, dua pihak
bisa sama-sama \"dapat meja\" yang sama pada jam yang sama, dan tidak
ada catatan yang mudah ditelusuri kembali.

Pendekatan manual semacam ini menimbulkan beberapa masalah nyata:

-   Bentrok jadwal (double booking) karena tidak ada validasi otomatis
    antar-pengajuan.

-   Proses persetujuan lambat karena bergantung pada petugas yang harus
    dihubungi satu per satu.

-   Tidak ada data historis yang rapi untuk keperluan evaluasi
    pemanfaatan ruangan oleh pihak kampus.

-   Mahasiswa/dosen sulit mengetahui status ketersediaan ruangan secara
    real-time.

## 1.3 Justifikasi Pemilihan Proyek

Proyek ini dipilih karena beberapa alasan berikut:

1.  Masalah nyata dan relevan --- hampir semua kampus memiliki proses
    peminjaman ruangan/fasilitas, sehingga sistem ini punya manfaat
    langsung yang mudah dipahami dosen maupun pengguna akhir.

2.  Cakupan fitur pas untuk tugas kelompok --- memiliki alur CRUD,
    autentikasi, validasi bisnis (cek bentrok jadwal), serta peran
    pengguna berbeda (mahasiswa/dosen sebagai peminjam, admin sebagai
    penyetuju), sehingga cukup menantang tanpa terlalu kompleks untuk
    dikerjakan dalam satu semester.

3.  Cocok dengan tech stack yang diwajibkan --- kebutuhan REST API
    (FastAPI) yang dikonsumsi antarmuka modern (Next.js) dengan data
    relasional (PostgreSQL/MySQL) sangat pas untuk merepresentasikan
    entitas seperti Ruangan, Fasilitas, Pengguna, dan Peminjaman beserta
    relasinya.

4.  Berorientasi implementasi berbasis Python di sisi backend --- sesuai
    arahan dosen bahwa logika bisnis inti (validasi, autentikasi,
    pengolahan data) dibangun dengan Python (FastAPI), bukan sekadar
    halaman statis.

# 2. Software Requirements Specification (SRS)

## 2.1 Pendahuluan

### 2.1.1 Tujuan

Dokumen ini bertujuan mendefinisikan kebutuhan fungsional dan
non-fungsional dari Sistem Informasi Peminjaman Ruangan dan Fasilitas
Kampus (SIPRUFA), sebagai acuan bagi tim pengembang dalam merancang,
membangun, dan menguji sistem.

### 2.1.2 Ruang Lingkup

Sistem ini digunakan untuk mengelola proses pengajuan, persetujuan, dan
pemantauan peminjaman ruangan serta fasilitas kampus secara daring
(bukan aplikasi web statis, melainkan sistem berbasis layanan
Python/FastAPI di sisi server dengan antarmuka Next.js di sisi klien).
Lingkup sistem mencakup:

-   Manajemen akun pengguna dengan peran (mahasiswa/dosen,
    admin/petugas).

-   Manajemen data ruangan dan fasilitas.

-   Pengajuan, validasi, dan persetujuan peminjaman.

-   Notifikasi status peminjaman.

-   Laporan rekap penggunaan ruangan dan fasilitas.

### 2.1.3 Definisi, Akronim, dan Singkatan

  ---------------- ------------------------------------------------------
  **Istilah**      **Keterangan**

  SRS              Software Requirements Specification

  API              Application Programming Interface

  REST             Representational State Transfer --- gaya arsitektur
                   komunikasi API

  JWT              JSON Web Token, digunakan untuk autentikasi pengguna

  ORM              Object Relational Mapping, penghubung objek Python
                   dengan tabel database

  CRUD             Create, Read, Update, Delete --- operasi dasar
                   terhadap data

  Admin/Petugas    Pengguna yang mengelola data ruangan dan menyetujui
                   peminjaman

  Peminjam         Mahasiswa atau dosen yang mengajukan peminjaman
  ---------------- ------------------------------------------------------

## 2.2 Deskripsi Umum

### 2.2.1 Perspektif Produk

SIPRUFA adalah sistem baru yang berdiri sendiri (bukan pengembangan dari
sistem lama). Sistem terdiri dari tiga lapisan utama: antarmuka pengguna
(Next.js), layanan backend berbasis Python (FastAPI) yang menangani
seluruh logika bisnis dan komunikasi data, serta basis data relasional
(PostgreSQL/MySQL) sebagai penyimpanan data.

### 2.2.2 Fungsi Produk (Ringkasan)

-   Registrasi dan login pengguna dengan peran berbeda.

-   Melihat ketersediaan ruangan/fasilitas secara real-time.

-   Mengajukan peminjaman dengan validasi otomatis terhadap bentrok
    jadwal.

-   Menyetujui atau menolak pengajuan peminjaman (oleh admin).

-   Melihat riwayat dan status peminjaman.

-   Mengelola data ruangan dan fasilitas (oleh admin).

-   Membuat laporan rekap penggunaan ruangan.

### 2.2.3 Karakteristik Pengguna

  ------------------ ------------------------- ---------------------------
  **Peran**          **Deskripsi**             **Tingkat Keahlian Teknis**

  Mahasiswa/Dosen    Mengajukan dan memantau   Pengguna umum, tidak perlu
  (Peminjam)         status peminjaman         keahlian teknis khusus
                     ruangan/fasilitas         

  Admin/Petugas      Mengelola data ruangan,   Terbiasa menggunakan
  Sarpras            fasilitas, dan memproses  aplikasi berbasis
                     persetujuan               web/dashboard

  Super Admin        Mengelola akun pengguna   Memahami dasar administrasi
  (opsional)         dan konfigurasi sistem    sistem
  ------------------ ------------------------- ---------------------------

### 2.2.4 Batasan (Constraints)

-   Sistem wajib dibangun dengan Next.js di sisi frontend, FastAPI
    (Python) di sisi backend, dan PostgreSQL atau MySQL sebagai basis
    data, sesuai arahan dosen.

-   Komunikasi antara frontend dan backend dilakukan melalui REST API
    berformat JSON.

-   Sistem hanya digunakan oleh civitas akademika kampus terkait (bukan
    publik umum).

### 2.2.5 Asumsi dan Ketergantungan

-   Pengguna memiliki akses internet dan perangkat (komputer/smartphone)
    dengan browser modern.

-   Data master ruangan dan fasilitas awal disiapkan/diinput oleh admin
    sebelum sistem digunakan pengguna umum.

## 2.3 Kebutuhan Fungsional (Functional Requirements)

  -------- ---------------------------------------- ----------------------
  **ID**   **Deskripsi Kebutuhan**                  **Aktor Terkait**

  FR-01    Sistem menyediakan registrasi dan login  Semua pengguna
           pengguna dengan autentikasi berbasis     
           token (JWT), serta pembatasan akses      
           sesuai peran (role-based access).        

  FR-02    Sistem menampilkan daftar ruangan dan    Peminjam, Admin
           fasilitas beserta status ketersediaannya 
           dalam bentuk kalender/jadwal.            

  FR-03    Sistem memungkinkan peminjam mengajukan  Peminjam
           peminjaman ruangan/fasilitas dengan      
           mengisi tanggal, jam, keperluan, dan     
           (opsional) unggah surat pendukung.       

  FR-04    Sistem melakukan validasi otomatis untuk Sistem (backend)
           mencegah bentrok jadwal (double booking) 
           pada ruangan/waktu yang sama.            

  FR-05    Sistem memungkinkan admin menyetujui     Admin
           atau menolak pengajuan peminjaman        
           disertai catatan.                        

  FR-06    Sistem mengirimkan notifikasi status     Sistem, Peminjam
           peminjaman (disetujui/ditolak/menunggu)  
           kepada peminjam.                         

  FR-07    Sistem menampilkan riwayat peminjaman    Peminjam
           milik masing-masing pengguna.            

  FR-08    Sistem menyediakan fitur kelola data     Admin
           ruangan dan fasilitas (tambah, ubah,     
           hapus) bagi admin.                       

  FR-09    Sistem menyediakan laporan rekap         Admin
           penggunaan ruangan/fasilitas dalam       
           periode tertentu, dengan opsi ekspor     
           data.                                    

  FR-10    Sistem memungkinkan peminjam membatalkan Peminjam
           pengajuan sebelum disetujui admin.       
  -------- ---------------------------------------- ----------------------

## 2.4 Kebutuhan Non-Fungsional (Non-Functional Requirements)

  -------------- --------------- -----------------------------------------
  **Kategori**   **ID**          **Deskripsi**

  Performa       NFR-01          Waktu respons untuk operasi umum (lihat
                                 jadwal, ajukan peminjaman) tidak lebih
                                 dari 2 detik dalam kondisi jaringan
                                 normal.

  Keamanan       NFR-02          Autentikasi menggunakan JWT, kata sandi
                                 disimpan dalam bentuk hash (bukan teks
                                 biasa), dan akses fitur dibatasi
                                 berdasarkan peran pengguna.

  Usabilitas     NFR-03          Antarmuka mudah digunakan oleh pengguna
                                 awam teknologi, responsif untuk diakses
                                 lewat desktop maupun perangkat mobile.

  Keandalan      NFR-04          Sistem melakukan pencadangan (backup)
                                 basis data secara berkala untuk mencegah
                                 kehilangan data.

  Skalabilitas   NFR-05          Struktur basis data dan API dirancang
                                 agar mudah menambah jenis
                                 ruangan/fasilitas baru tanpa perombakan
                                 besar.

  Pemeliharaan   NFR-06          Kode backend dan frontend disusun modular
                                 mengikuti praktik umum FastAPI dan
                                 Next.js agar mudah dikembangkan lanjut.
  -------------- --------------- -----------------------------------------

## 2.5 Kebutuhan Antarmuka Eksternal

-   Antarmuka Pengguna: aplikasi web responsif dibangun dengan Next.js,
    dapat diakses melalui browser desktop maupun mobile.

-   Antarmuka Perangkat Lunak: komunikasi antara Next.js (client) dan
    FastAPI (server) melalui REST API berformat JSON, dengan dokumentasi
    otomatis (Swagger/OpenAPI) dari FastAPI.

-   Antarmuka Perangkat Keras: server aplikasi dan server basis data
    (PostgreSQL/MySQL), dapat berupa VPS atau layanan cloud.

-   Antarmuka Komunikasi: protokol HTTPS untuk keamanan data, serta
    layanan SMTP (opsional) untuk pengiriman notifikasi email.

## 2.6 Arsitektur Sistem dan Tech Stack

Sistem dirancang dengan arsitektur client-server tiga lapis
(three-tier), di mana setiap lapisan memiliki tanggung jawab terpisah
--- mirip pembagian tugas di sebuah restoran: pelayan (frontend)
menerima permintaan pelanggan, dapur (backend) mengolah permintaan itu
sesuai resep/aturan, dan gudang bahan (database) menyimpan semua data
yang dibutuhkan.

  ---------------- ---------------------------- -------------------------
  **Lapisan**      **Teknologi**                **Peran**

  Frontend         Next.js (React + TypeScript) Menyajikan antarmuka
                                                pengguna, form pengajuan
                                                peminjaman, kalender
                                                ketersediaan, dan
                                                dashboard admin.

  Backend          FastAPI (Python)             Menangani seluruh logika
                                                bisnis: autentikasi,
                                                validasi bentrok jadwal,
                                                proses persetujuan, dan
                                                menyediakan REST API.

  Database         PostgreSQL atau MySQL        Menyimpan data pengguna,
                                                ruangan, fasilitas, dan
                                                riwayat peminjaman secara
                                                relasional.

  ORM              SQLAlchemy / SQLModel        Menjembatani objek Python
                                                dengan tabel-tabel di
                                                basis data.

  Autentikasi      JWT (JSON Web Token)         Mengelola sesi login dan
                                                pembatasan akses berbasis
                                                peran.

  Deployment       Docker, Vercel (FE),         Mempermudah proses
  (opsional)       VPS/Railway (BE)             containerization dan
                                                penyebaran aplikasi.
  ---------------- ---------------------------- -------------------------

### Alur Komunikasi Sistem

1\) Pengguna berinteraksi dengan antarmuka Next.js → 2) Next.js mengirim
permintaan (request) ke REST API FastAPI → 3) FastAPI memproses logika
bisnis (misalnya cek bentrok jadwal) dan berkomunikasi dengan database →
4) Database mengembalikan/menyimpan data → 5) FastAPI mengirim respons
JSON kembali ke Next.js untuk ditampilkan ke pengguna.

## 2.7 Rancangan Entitas Basis Data (ERD Ringkas)

Berikut entitas utama yang akan disimpan dalam basis data relasional:

  ----------------- -----------------------------------------------------
  **Entitas**       **Atribut Utama**

  User              id, nama, email, password_hash, role
                    (mahasiswa/dosen/admin)

  Ruangan           id, nama_ruangan, lokasi/gedung, kapasitas, status

  Fasilitas         id, nama_fasilitas, jumlah, ruangan_id (opsional,
                    jika melekat pada ruangan tertentu)

  Peminjaman        id, user_id, ruangan_id, tanggal, jam_mulai,
                    jam_selesai, keperluan, status
                    (menunggu/disetujui/ditolak), catatan_admin

  Notifikasi        id, user_id, pesan, status_dibaca, tanggal_dikirim
  ----------------- -----------------------------------------------------

Relasi utama: satu User dapat memiliki banyak Peminjaman (1-ke-banyak);
satu Ruangan dapat memiliki banyak Peminjaman pada waktu berbeda
(1-ke-banyak); satu Ruangan dapat memiliki banyak Fasilitas
(1-ke-banyak).

## 2.8 Daftar Use Case

  ---------- -------------------------- ----------------------------------
  **ID**     **Nama Use Case**          **Aktor**

  UC-01      Registrasi dan Login       Semua pengguna

  UC-02      Lihat Ketersediaan         Peminjam, Admin
             Ruangan/Fasilitas          

  UC-03      Ajukan Peminjaman          Peminjam
             Ruangan/Fasilitas          

  UC-04      Kelola Persetujuan         Admin
             Peminjaman                 

  UC-05      Kelola Data Ruangan dan    Admin
             Fasilitas                  

  UC-06      Lihat Riwayat dan Status   Peminjam
             Peminjaman                 

  UC-07      Batalkan Pengajuan         Peminjam
             Peminjaman                 

  UC-08      Cetak/Ekspor Laporan       Admin
             Penggunaan Ruangan         
  ---------- -------------------------- ----------------------------------
