# Nusa Wallet Extension

**Nusa Wallet** adalah ekstensi dompet **Nusacoin (NUX)** untuk browser. Dengan ekstensi ini Anda dapat melihat saldo, mengirim NUX, dan masuk ke aplikasi web Nusachain (dApps) tanpa memasukkan private key atau seed phrase ke situs mana pun. Setiap transaksi harus Anda setujui sendiri lewat jendela konfirmasi.

Tersedia untuk **Google Chrome** dan browser berbasis Chromium (Brave, Microsoft Edge), serta **Mozilla Firefox**.

---

## ✨ Fitur Utama

* **Dompet NUX di browser:** Buat wallet baru atau impor private key yang sudah ada, lalu pantau saldo langsung dari popup ekstensi.
* **Kirim NUX:** Masukkan alamat tujuan dan jumlah, periksa rincian beserta fee, lalu setujui. Transaksi ditandatangani secara lokal di browser Anda.
* **Koneksi dApps:** Situs dapat meminta alamat Anda lewat `window.nux`. Setelah Anda mengizinkan koneksi, situs tetap tidak bisa memindahkan dana tanpa persetujuan Anda untuk setiap transaksi.
* **Tanda tangan pesan:** Buktikan kepemilikan alamat tanpa memindahkan dana.
* **Catat data di blockchain:** Tulis data singkat (maksimal 80 byte) ke blockchain melalui OP_RETURN, berguna untuk pencatatan seperti hash dokumen.
* **Kunci otomatis:** Wallet terkunci kembali setelah tidak digunakan beberapa saat (bawaan 15 menit).

---

## 🔒 Keamanan

* Private key atau seed phrase dienkripsi dengan password Anda (PBKDF2 + AES-GCM) dan hanya tersimpan di browser ini.
* **Password tidak dapat dipulihkan.** Simpan cadangan private key Anda di tempat yang aman dan offline.
* Tim Nusa Wallet **tidak pernah** meminta password atau private key Anda dalam kondisi apa pun.
* Periksa alamat tujuan dengan teliti sebelum menyetujui transaksi. Transaksi blockchain tidak dapat dibatalkan.
* Unduh file rilis hanya dari halaman [**Releases**](../../releases) di repositori resmi ini.

---

## 📥 Cara Instalasi

Unduh file rilis terbaru sesuai browser Anda dari halaman [**GitHub Releases**](../../releases).

### 1. Google Chrome / Brave / Microsoft Edge

1. Unduh **`nusa-wallet-chrome.zip`** dari halaman [Releases](../../releases).
2. Ekstrak file ZIP tersebut ke sebuah folder. Jangan hapus folder ini setelah pemasangan, karena browser memuat ekstensi langsung dari sana.
3. Buka halaman ekstensi browser:
   * Chrome: `chrome://extensions/`
   * Brave: `brave://extensions/`
   * Edge: `edge://extensions/`
4. Aktifkan **Developer mode** (Mode pengembang) di pojok kanan atas.
5. Klik **Load unpacked** (Muat yang belum dikemas), lalu pilih folder hasil ekstrak tadi.
6. Ikon **Nusa Wallet** muncul di toolbar. Sematkan (*pin*) agar mudah diakses.

### 2. Mozilla Firefox

1. Unduh **`nusa-wallet-firefox.zip`** dari halaman [Releases](../../releases).
2. Buka `about:debugging#/runtime/this-firefox` di Firefox.
3. Klik **Load Temporary Add-on…** (Muat Pengaya Sementara…).
4. Pilih file **`nusa-wallet-firefox.zip`**, atau file `manifest.json` di dalam folder hasil ekstrak.

> **Catatan:** Add-on yang dimuat dengan cara ini bersifat sementara dan akan hilang saat Firefox ditutup. Pemasangan permanen memerlukan paket yang sudah ditandatangani Mozilla.

---

## 🧩 Untuk Pengembang dApps

Ekstensi menyediakan objek `window.nux` di setiap halaman web. Tunggu event `nux#initialized` atau periksa `window.nux` sebelum memakainya.

```js
if (window.nux) {
  const { address } = await nux.connect();          // minta izin koneksi
  const saldo = await nux.getBalance();             // saldo dalam NUX

  const { txid } = await nux.sendTransaction({      // kirim NUX
    to: 'ALAMAT_TUJUAN',
    amount: 1.5
  });

  const sig = await nux.signMessage('Halo Nusa');   // tanda tangan pesan
  const rec = await nux.sendData({ data: 'hash-dokumen' }); // OP_RETURN, maks 80 byte
}
```

| Method | Keterangan |
|---|---|
| `connect()` | Meminta izin koneksi, mengembalikan `{ address, publicKey }` |
| `disconnect()` | Mencabut izin koneksi situs |
| `getAddress()` | Mengambil alamat (situs harus sudah terhubung) |
| `getBalance()` | Mengambil saldo dalam NUX |
| `signMessage(message)` | Menandatangani pesan teks |
| `sendTransaction({ to, amount })` | Mengirim NUX, jumlah dalam satuan NUX |
| `sendData({ data, encoding })` | Menulis data ke blockchain, `encoding`: `'utf8'` (bawaan) atau `'hex'` |

Setiap permintaan yang menyentuh dana atau tanda tangan memerlukan persetujuan pengguna. Kode penolakan mengikuti konvensi umum: `4001` berarti pengguna menolak, `4100` berarti situs belum terhubung.

---

## 🤝 Kontribusi & Laporan Bug

Menemukan bug atau punya usulan fitur? Silakan buat laporan di tab [**Issues**](../../issues).

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).
