# 👁️ IoT Face Recognition + Cloudflare D1 Logger

Project ini adalah sistem **Absensi & Keamanan Berbasis Wajah** yang berjalan di browser (Client-Side) menggunakan OpenCV.js. Sistem ini terintegrasi dengan **Wemos D1 Mini (ESP8266)** untuk kontrol perangkat keras dan **Cloudflare D1** untuk pencatatan log data secara *serverless*.

🔗 **Live Demo:** [https://difnsy.github.io/iot/](https://difnsy.github.io/iot/)

---

## 📺 Video Demo
Lihat demonstrasi cara kerja sistem ini di YouTube:

[![Tonton Video Demo](https://youtube.com/shorts/oq3I72Nmh1A?si=qqqamRNTMOFmsyBv)](https://youtube.com/shorts/oq3I72Nmh1A?si=qqqamRNTMOFmsyBv)

*(Klik gambar di atas untuk memutar video)*

---

## 🚀 Fitur Utama

1.  **Face Recognition di Browser**: Menggunakan **OpenCV.js** (LBP Cascade + Cosine Similarity) untuk mendeteksi dan mengenali wajah secara *real-time* tanpa perlu backend berat.
2.  **Cloud Logging (Serverless)**: Setiap kali wajah dikenali, data dikirim ke **Cloudflare D1 Database** melalui **Cloudflare Workers**.
3.  **IoT Hardware Control**: Terintegrasi dengan **Wemos D1 Mini (ESP8266)** untuk menyalakan indikator (LED/Relay) saat wajah terverifikasi.
4.  **Dual Mode**: Bisa berjalan offline (hanya deteksi) atau online (dengan logging & IoT).

---

## 🛠️ Teknologi yang Digunakan

* **Frontend**: HTML5, CSS3, JavaScript (Vanilla).
* **AI/CV Library**: OpenCV.js (WebAssembly).
* **Backend API**: Cloudflare Workers.
* **Database**: Cloudflare D1 (SQLite Serverless).
* **Hardware**: Wemos D1 Mini / ESP8266.

---

## ⚙️ Cara Instalasi & Persiapan

Ikuti langkah-langkah berikut untuk menjalankan sistem ini secara lokal maupun online.

### 1. Persiapan Backend (Cloudflare)
Bagian ini diperlukan jika Anda ingin mengaktifkan fitur pencatatan log (History).

1.  **Buat Worker:**
    * Masuk ke Dashboard Cloudflare > **Workers & Pages**.
    * Buat aplikasi Worker baru (pilih "Hello World" template).
2.  **Setup Database D1:**
    * Buka terminal komputer Anda, jalankan perintah Wrangler:
        ```bash
        npx wrangler d1 create nama-db-anda
        ```
    * Salin `database_id` yang muncul ke file `wrangler.toml` Anda.
3.  **Buat Tabel:**
    * Masuk ke menu D1 di dashboard Cloudflare atau gunakan terminal.
    * Jalankan perintah SQL berikut untuk membuat tabel:
        ```sql
        CREATE TABLE sensor (
          id INTEGER PRIMARY KEY AUTOINCREMENT,
          nilai INTEGER,
          keterangan TEXT,
          waktu DATETIME DEFAULT CURRENT_TIMESTAMP
        );
        ```
4.  **Deploy Worker:**
    * Salin kode API (backend) ke worker Anda.
    * Deploy dengan menekan tombol **Deploy** di dashboard atau via CLI: `npx wrangler deploy`.

### 2. Persiapan Hardware (IoT)
Bagian ini diperlukan untuk fitur kontrol fisik (Lampu/Relay).

1.  **Siapkan Perangkat:** Wemos D1 Mini atau ESP8266.
2.  **Program Perangkat:**
    * Buka Arduino IDE.
    * Pastikan kodingan ESP8266 Anda memiliki endpoint `/api/led` yang menerima method POST.
    * Upload sketch ke Wemos.
3.  **Koneksi Jaringan:**
    * Pastikan Wemos dan Laptop/HP yang membuka website berada di **jaringan Wi-Fi yang sama**.

### 3. Menjalankan Website (Frontend)
1.  **Buka Website:**
    * Clone repo ini: `git clone https://github.com/difnsy/iot.git`
    * Atau buka langsung via GitHub Pages.
2.  **Izin Kamera:**
    * Saat browser meminta izin kamera, klik **Allow/Izinkan**.

---

## 🚀 Cara Penggunaan

### Tahap 1: Pendaftaran Wajah (Enrollment)
1.  Buka website.
2.  Masuk ke menu/bagian **Enroll**.
3.  Isi **Nama Anda** pada kolom input.
4.  Klik tombol **Enroll**.
5.  Gerakkan wajah sedikit (kiri, kanan, senyum) hingga progress bar atau jumlah sampel terpenuhi.
6.  Sistem kini mengenali wajah Anda.

### Tahap 2: Konfigurasi Koneksi
1.  Scroll ke bagian pengaturan di bawah canvas kamera.
2.  **Cloudflare URL:** Masukkan URL Worker Anda (Contoh: `https://api-iot.nama.workers.dev/simpan`).
3.  **Wemos IP:** Masukkan IP Address Wemos (Contoh: `http://192.168.1.11`).

### Tahap 3: Mulai Deteksi
1.  Klik tombol **Start**.
2.  Arahkan wajah ke kamera.
3.  Jika wajah dikenali (Verified):
    * Kotak akan berwarna hijau.
    * Lampu pada Wemos akan menyala (jika IP benar).
    * Data log akan terkirim ke Cloudflare D1.

---

## 📝 Dokumentasi API (Cloudflare Worker)

Backend worker menyediakan dua endpoint utama:

### 1. Mengambil Data Log (`GET`)
Mengambil daftar riwayat orang yang terdeteksi.

* **Endpoint:** `/data`
* **Method:** `GET`
* **Response:**
    ```json
    [
      {
        "id": 1,
        "nilai": 1,
        "keterangan": "Sismadi",
        "waktu": "2026-01-22 16:01:05"
      },
      ...
    ]
    ```

### 2. Menyimpan Log (`POST`)
Mencatat data baru ke database saat wajah dikenali.

* **Endpoint:** `/simpan`
* **Method:** `POST`
* **Content-Type:** `application/json`
* **Body Request:**
    ```json
    {
      "nilai": 1,
      "keterangan": "Nama User"
    }
    ```
* **Response:**
    ```json
    {
      "status": "Berhasil disimpan"
    }
    ```

---

## ⚠️ Catatan Penting
* **Mixed Content:** Jika Anda membuka website via HTTPS (GitHub Pages) dan Wemos via HTTP (IP Lokal), browser mungkin memblokir request ke Wemos. Solusinya:
    * Izinkan "Insecure Content" di pengaturan situs browser.
    * Atau jalankan website secara lokal (localhost).
* **Pencahayaan:** Pastikan pencahayaan cukup terang agar akurasi pengenalan wajah maksimal.

---
**Dibuat oleh [Dani Ijlal Fitriansyah](https://github.com/difnsy)**
