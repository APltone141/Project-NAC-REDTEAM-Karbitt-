# Project NAC — REDTEAM Karbitt 

Selamat datang di repositori Project NAC Redteam 4 Karbitt.
bukankah ini My-team

---

**Tim**

- **Captain:** Muhamad Ridwan (Akademik)
- **Members:**
	- M. Thoriqul Fadli (Akademik)
	- Raihan Faiz Ramadhan (Medinfo)
	- Ibnu Fajar (Medinfo)
	- Arsenyius Mastry Naibaho (Medinfo)
	- Muhammad Sahl (Medinfo)

---

Contributions, issues, dan dokumentasi disimpan di folder terkait — lihat daftar file di repo untuk memulai.

---

## Analisis Tema & Kesamaan Flag 1-4(milestone 1)

Berdasarkan analisis terhadap ke-4 Proof of Concept yang berhasil dieksekusi, ditemukan kesamaan fundamental yang mengikat semuanya dalam satu tema besar:

### Ringkasan 4 Challenge

| # | Challenge | Kategori | Flag |
|---|-----------|----------|------|
| 1 | Missing Encoding | Broken Access Control / Input Validation | `80770861e37b8f3408d8825ee3bc7463b0487073` |
| 2 | Christmas Special | Broken Access Control (IDOR) / Sensitive Data Exposure | `929646db81fdde9492b64f2d3c5fa0a3da182ad7` |
| 3 | Multiple Likes | Broken Anti Automation / Race Condition | `e6334118280c7b75de8e2ef6a11bb172b2e34f13` |
| 4 | CSRF | Cross-Site Request Forgery | `7f2af07e0e0ac2c54e8ae0665b4aa68762ba63af` |

---

### Tema Umum: **Server-Side Validation & Authorization Bypass**

Meskipun ke-4 challenge memiliki kategori keamanan yang berbeda (Encoding, IDOR, Race Condition, CSRF), semuanya mengikuti pola serangan yang sama:

#### **1. Kesamaan Teknis**

| Aspek | Penjelasan |
|------|-----------|
| **Input Manipulation** | Keempat serangan memanfaatkan manipulasi request client yang dikirim ke server tanpa validasi yang cukup |
| **Server Trust Issues** | Server mempercayai input dari user/client tanpa melakukan pengecekan yang proper |
| **Validation Bypass** | Semua challenge mengandalkan celah pada mekanisme validasi server-side |
| **Request Exploitation** | Menggunakan tools seperti Burp Suite, Turbo Intruder, atau Direct URL modification |

#### **2. Pola Serangan yang Sama**

```
User Request → Client Manipulation → Server Processing (TANPA VALIDASI CUKUP) → Exploit Success
```

**Penjelasan per challenge:**
- **Missing Encoding:** URL fragment (#) dimanipulasi → Server tidak memvalidasi encoding → Akses file tersembunyi
- **Christmas Special:** Product ID dimanipulasi → Server tidak validasi status produk active → IDOR berhasil
- **Multiple Likes:** Multiple requests dikirimi secara concurrent → Server tidak validasi race condition → Multiple likes berhasil
- **CSRF:** Origin header dimanipulasi → Server tidak validasi origin dengan proper → Profile berhasil diubah

---

### Kategori Serangan yang Sama: **Authorization & Access Control Flaws**

Semua 4 challenge berbeda dalam **mekanisme serangan** tetapi sama dalam **tujuan akhir**:

1. **Bypass Authorization Check** - Melakukan aksi yang seharusnya tidak diizinkan
2. **Manipulate Protected Resources** - Mengakses atau memodifikasi resource yang seharusnya protected
3. **Exploit Trust Mechanism** - Memanfaatkan kepercayaan server terhadap komponen request

### Root Cause yang Sama

| Root Cause | Challenge yang Terdampak |
|-----------|------------------------|
| **Incomplete Input Validation** | Missing Encoding, Christmas Special, CSRF |
| **Lack of Concurrency Control** | Multiple Likes |
| **No Proper Access Control** | Christmas Special, Missing Encoding |
| **Insufficient Origin/Context Verification** | CSRF, Multiple Likes |

---

### Kesimpulan

**Tema Overarching:** Semua 4 challenge merupakan manifestasi dari **Authorization & Access Control Bypass Attacks** dengan berbagai vektor serangan. Kerentanan fundamental yang menghalangi adalah:

**Kurangnya server-side validation yang proper terhadap user input dan request context**

Dengan kata lain, semua challenge dapat diringkas dalam satu kategori besar:


Have fun & keep learning! 

