# IoT Face Recognition + Cloudflare D1 Logger

Project ini adalah sistem **Absensi/Keamanan Berbasis Wajah** yang terintegrasi dengan IoT dan Cloud Database. Sistem ini berjalan sepenuhnya di browser (Client-Side) menggunakan OpenCV.js, namun tetap memiliki kemampuan logging data ke server (Serverless) dan kontrol perangkat keras lokal.

🔗 **Live Demo:** [https://difnsy.github.io/iot/](https://difnsy.github.io/iot/)

---

## 📺 Demo Video
Tonton cara kerja sistem ini di YouTube:

[![Tonton Video Demo](https://img.youtube.com/shorts/oq3I72Nmh1A?si=qqqamRNTMOFmsyBv.jpg)](https://youtube.com/shorts/oq3I72Nmh1A?si=qqqamRNTMOFmsyBv)

*(Klik gambar di atas untuk memutar video)*

---

## 🚀 Fitur Utama

1.  **Face Recognition di Browser**: Menggunakan **OpenCV.js** (LBP Cascade + Cosine Similarity) untuk mendeteksi dan mengenali wajah secara *real-time* tanpa perlu backend berat.
2.  **Cloud Logging (Serverless)**: Setiap kali wajah dikenali, data dikirim ke **Cloudflare D1 Database** melalui **Cloudflare Workers**.
3.  **IoT Hardware Control**: Terintegrasi dengan **Wemos D1 Mini (ESP8266)** untuk menyalakan indikator (LED/Relay) saat wajah terverifikasi.
4.  **Dual Mode**: Bisa berjalan offline (hanya deteksi) atau online (dengan logging & IoT).

## 🛠️ Teknologi yang Digunakan

* **Frontend**: HTML5, CSS3, JavaScript (Vanilla).
* **AI/CV Library**: OpenCV.js (WebAssembly).
* **Backend API**: Cloudflare Workers.
* **Database**: Cloudflare D1 (SQLite Serverless).
* **Hardware**: Wemos D1 Mini / ESP8266.

## 🏗️ Arsitektur Sistem

```mermaid
graph LR
    A[Browser / Kamera] -->|Deteksi Wajah| B(OpenCV.js)
    B -->|Verified| C{Action}
    C -->|Log Data| D[Cloudflare Worker]
    D -->|Insert| E[(Database D1)]
    C -->|Trigger| F[Wemos / ESP8266]
    F -->|Output| G[Lampu / Door Lock]
