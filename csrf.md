# Proof of Concept: CSRF Challenge

---

## Informasi Challenge

| Nama Challenge | Deskripsi / Hint |
| --- | --- |
| **CSRF** | Kau tidak bisa mengubah nama targetmu secara langsung di dalam benteng ini. Pergilah ke tanah asing (origin lain), pasanglah jebakan di sana, dan pancing korbanmu untuk mengubah identitasnya sendiri tanpa sadar saat ia berkunjung ke tempatmu. |

---

## Analisis Kerentanan

**Cross-Site Request Forgery (CSRF)** adalah sebuah kerentanan keamanan web di mana penyerang dapat memanipulasi browser korban untuk melakukan request yang tidak diinginkan ke situs lain di mana korban telah terautentikasi.

Dalam tantangan ini, mekanisme pengubahan nama user menjadi target eksploitasi. Peserta diminta untuk mengubah nama tanpa berinteraksi langsung dengan platform utama (direct interaction).

### Mekanisme Pengubahan Nama

Setelah dilakukan observasi pada platform, ditemukan bahwa fungsi pengubahan nama menggunakan metode **HTTP POST** dengan rincian sebagai berikut:

* **Endpoint:** `/update-profile` (atau endpoint relevan pada target).
* **Payload:** `username=[input_nama]`.
* **Parameter:** Hanya memerlukan satu variabel yaitu `username`.

---

## Langkah Penyelesaian

### 1. Persiapan Payload

Berdasarkan temuan pada mekanisme POST, dibuat sebuah form HTML mandiri yang mensimulasikan request tersebut. Form ini dirancang agar dapat mengirimkan data ke server target dari "tanah asing" (domain/file lokal).

### 2. Skenario Eksploitasi

Syarat mutlak keberhasilan serangan ini adalah:

* Korban harus memiliki sesi aktif (**Active Session/Cookie**) pada browser yang sama.
* Korban harus mengakses halaman yang telah dipasangi jebakan oleh penyerang.

Dalam simulasi ini, form dibuat untuk melakukan submisi otomatis menggunakan JavaScript agar tercipta kondisi *zero-click* atau *one-click*.

```html
    <form action="http://localhost:3000/profile" method="POST">
      <input type="hidden" name="username"/>
      <button type="submit">submit</button>
    </form>
```

### 3. Kendala dan Penyesuaian (Header Origin)

Pada percobaan pertama, serangan menghasilkan *false positive* (atau *true negative*), di mana metode sudah benar namun flag tidak muncul. Hal ini disebabkan oleh proteksi platform yang melakukan pengecekan terhadap header **Origin**.

Ditemukan bahwa:

* Request harus berasal dari origin yang telah ditentukan oleh platform.
* Terdapat kendala teknis (error *refused to connect to localhost*) saat mencoba menjalankan form secara eksternal.

### 4. Manipulasi Request menggunakan Burp Suite

Untuk mengatasi batasan origin tersebut, dilakukan manipulasi request menggunakan tool **Burp Suite**:

1. Menangkap (intercept) request POST pengubahan nama.
2. Mengubah nilai pada header `Origin` agar sesuai dengan yang diminta atau diizinkan oleh platform.
3. Meneruskan (forward) request yang telah dimanipulasi.

---

## Hasil Akhir

Setelah header `Origin` dimanipulasi dan request dikirimkan, server menerima permintaan tersebut sebagai permintaan yang valid dari sumber terpercaya. Nama berhasil diubah dan sistem memberikan output berupa **Flag**.

