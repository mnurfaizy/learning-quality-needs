---
name: ppt-bmri-generator
description: Generate slide content (PPT) untuk modul pelatihan berbasis kompetensi Bank Mandiri. Gunakan skill ini setiap kali user meminta generate slides, membuat konten PPT, atau menyusun materi slide untuk Elemen Kompetensi (EK) — termasuk saat user menyebut "bikin slides", "generate PPT", "susunkan slide untuk EK ini", atau melampirkan modul/materi dan meminta slide-nya. Output berupa dua format: isi slide siap copy-paste ke Gamma App, dan tabel rekomendasi ilustrasi.
---

# PPT Generator — Bank Mandiri Training Modules

## Batas Kata per Slide
- Slide konten biasa: **maks 155 kata**
- Slide tabel: **maks 160 kata**
- Ini batas atas, bukan target — kalau konten memang ringkas, biarkan ringkas. Jangan padding.

---

## Gaya Penulisan
- To the point: lebih pendek dari modul, tidak terlalu disingkat hingga kehilangan konteks
- Bahasa Indonesia; istilah teknis/asing tetap Inggris dan **wajib ditulis italic**: *secure by design*, *real-time*, *proof of concept*, *eavesdropping*, *spoofing*, *likelihood*, *impact*, dll.
- Tanpa em dash; tanpa struktur "Bukan X, melainkan Y"
- Label standar dalam bullet: **Fokus:**, **Output:**, **Tools:**, **Contoh:**
- Setiap bullet point **wajib menggunakan tanda `-`** di awal baris — bukan hanya baris baru tanpa penanda

---

## Struktur Slide

### Cover
- Tidak di-generate oleh Claude — langsung mulai dari slide 1 materi
- Cover tidak dihitung dalam kuota slide (9/18/27)

### Kuota & Wajib
- 1 JP = maks 9 slides
- Setiap 9 slides: **wajib minimal 1** slide Mini Activity **atau** Pertanyaan Refleksi
- Tanya user mana yang dipilih **sebelum** generate

---

## Helicopter View — Dua Tipe

### Slide 1: Helicopter View Awal (selalu ada)

**Struktur wajib slide 1:**
Selalu dimulai dengan **paragraf definisi** (1–3 kalimat) yang menjelaskan apa itu materi ini. Baru setelah itu diikuti preview sub-materi atau poin kunci dalam bentuk label/heading terstruktur.

Contoh heading yang dipakai: "Tiga Faktor Kunci", "2 Pilar Utama", "6 Instrumen Utama" — bukan narasi seperti "Materi ini mencakup:" yang merupakan bahasa spoken/narrator dan tidak boleh muncul di slide.

**Skenario A — Slot terbatas / materi padat:**
Setelah paragraf definisi, slide 1 langsung mention semua poin dari seluruh sub-materi dalam bentuk label terstruktur. Tidak ada helicopter view tengah. Sisa slot dipakai untuk slide penjelasan + mini activity/refleksi.

Gunakan skenario ini ketika: jumlah sub-materi × estimasi slide penjelasan + 1 slide activity = memenuhi/melebihi kuota slot.

**Skenario B — Slot masih cukup:**
Setelah paragraf definisi, slide 1 hanya berisi konteks umum atau alasan mengapa materi ini penting — tanpa mention semua poin. Helicopter view tengah muncul di sub-materi yang poinnya banyak dan dalam.

Gunakan skenario ini ketika: masih ada ruang untuk helicopter view tengah + slide penjelasan + activity dalam kuota slot.

> Hitung dulu sebelum generate: estimasi total slide → tentukan skenario → baru mulai.

### Helicopter View Tengah (kondisional, hanya Skenario B)
- Muncul sebelum sub-materi yang poinnya banyak dan penjelasannya dalam
- Isi: judul sub-materi + daftar poin (label saja atau 1 kalimat singkat)
- Selalu sertakan ilustrasi (lihat bagian Ilustrasi)
- Tidak diperlukan jika tiap poin hanya butuh 1–2 kalimat → gabung dalam 1 slide

---

## Mini Activity & Pertanyaan Refleksi

**Mini Activity:**
- Narasi "Kamu adalah [role] di [konteks]. [Situasi/tantangan]."
- Skenario: 3 kartu kondisi
- 2 pertanyaan diskusi di bawahnya

**Pertanyaan Refleksi:**
- Intro singkat 1 kalimat di bawah judul
- 3 pertanyaan dalam kartu quote (tanda petik besar)
- Pertanyaan mengacu pada pengalaman nyata peserta di organisasinya

---

## Ilustrasi — Kapan & Kategori

**Dipakai di:**
1. Helicopter view tengah (selalu)
2. Slide konten dengan poin ≤6 dan penjelasan per poin pendek (1–2 kalimat)

**Tidak dipakai di:**
- Slide 1 (helicopter view awal)
- Slide konten yang sudah padat

**Kategori:**

| Kategori | Kapan | Contoh Visual |
|---|---|---|
| Hierarki/Level | Poin bertingkat/berlevel | Tangga, Piramid |
| Alur Proses | Poin prosedural/berurutan | Panah linear |
| Timeline | Alur proses + variabel waktu | Garis waktu |
| Comparison | Poin dibedakan berdasarkan aspek; >2 poin → tabel | Timbangan (2 poin) |
| Cycle/Siklus | Prosedural, poin terakhir kembali ke awal | Panah melingkar |
| Parts of Whole | Poin adalah satu kesatuan tak terpisahkan | Block puzzle |
| Card | Poin biasa tanpa pola khusus | Kotak kartu |
| `-` | Tidak masuk kategori manapun | — |

---

## Output Format

Kedua format diberikan langsung di chat (bukan file terpisah).

### Format 1 — Isi Slide (copy-paste ke Gamma App)
```
[Judul Slide]
[Isi poin 1]
[Isi poin 2]
dst.

---

[Judul Slide berikutnya]
[Isi poin 1]
```

### Format 2 — Rekomendasi Ilustrasi (tabel di chat, setelah Format 1)
| No. Slide | Judul | Kategori Ilustrasi | Contoh Visual |
|---|---|---|---|
| 2 | Siklus Hidup TI | Alur Proses | Panah linear 6 tahap |
| 3 | ... | - | - |

---

## Alur Kerja Sebelum Generate

1. Terima materi/modul EK dari user
2. **Jika user belum menyebutkan jumlah JP atau maks slide — tanya dulu sebelum lanjut**
3. Hitung estimasi total slide yang dibutuhkan
4. Tentukan: Skenario A atau B untuk slide 1
5. Tanya user: Mini Activity atau Pertanyaan Refleksi?
6. Generate Format 1 (isi slide) + Format 2 (tabel ilustrasi) di chat
