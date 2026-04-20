---
name: trainer-candidate-reviewer-v2
description: >
  Evaluasi kandidat Trainer/Tutor/Speaker terhadap silabus pelatihan dan hasil asesmen video teaching. Dilengkapi Company Prestige Modifier: kandidat dari perusahaan besar/terkenal (BUMN, multinasional, unicorn, Big 4) mendapat +3 poin; perusahaan tidak dikenal/sangat kecil mendapat −2 poin; modifier diterapkan setelah Match Score. Gunakan saat user melampirkan profil kandidat (CV/PDF), silabus/outline pelatihan, dan hasil observasi video — lalu meminta penilaian kecocokan. Trigger: "review kandidat trainer", "cek kandidat ini", "cocok ngga buat training ini", "buatin report trainer". Output: .docx per kandidat siap dilampirkan di dashboard rekrutmen.
---

# Trainer Candidate Reviewer

## Skala Skor (digunakan di semua dimensi)

| Skor | Kriteria |
|------|----------|
| 5 | Sangat baik — konsisten dan menonjol |
| 4 | Baik — solid dengan minor gap |
| 3 | Cukup — ada potensi, perlu pengembangan |
| 2 | Kurang — gap signifikan |
| 1 | Tidak terlihat / tidak tersedia |

---

## Step 1 — Verifikasi Input

Pastikan tiga input tersedia sebelum lanjut:
1. CV/resume kandidat (PDF)
2. Silabus atau outline pelatihan
3. Hasil observasi video teaching dari user (teks bebas)

Jika ada yang kurang, minta ke user sebelum melanjutkan.

---

## Step 2 — Baca Profil Kandidat

Ekstrak dari CV: nama, role saat ini, pengalaman mengajar/melatih, domain expertise, sertifikasi (TOT, BNSP, dll.), dan topik yang pernah diajarkan.

---

## Step 3 — Dimensi A: Teaching Skill (bobot 50%)

Gunakan hasil observasi video dari user. Jangan analisis video secara mandiri.

4 aspek, bobot equal (25% masing-masing):
- Making Concepts Easy to Understand
- Smooth and Structured Explanation
- Encouraging Student Participation
- Verbal & Non-Verbal Engagement

Jika skor tidak diberikan eksplisit, inferensikan dari kata-kata user. Jika ambigu, konfirmasi.

**Teaching Skill Score** = Σ(bobot × skor/5) × 100%

---

## Step 4 — Dimensi B: Profile–Silabus Fit (bobot 50%)

Bandingkan profil kandidat dengan silabus. 4 aspek, bobot equal (25% masing-masing):

| Aspek | Deskripsi |
|-------|-----------|
| Domain Expertise | Background kandidat relevan dengan topik silabus? |
| Pengalaman Mengajar Topik Serupa | Pernah mengajar materi sejenis? |
| Level Audience Fit | Pengalaman sesuai target peserta di silabus? |
| Kredensial & Sertifikasi | Ada sertifikasi yang mendukung topik? |

Bukti harus eksplisit dari profil. Jika tidak ada → `"Tidak tersedia — perlu konfirmasi"`, skor 1–2.

**Profile–Silabus Fit Score** = Σ(bobot × skor/5) × 100%

---

## Step 5 — Match Score & Verdict

**Match Score** = rata-rata Teaching Skill Score dan Profile–Silabus Fit Score

## Step 5b — Company Prestige Modifier

Terapkan modifier setelah Match Score dihitung:

| Tier | Kriteria | Modifier |
|------|----------|----------|
| A | BUMN/Persero, multinasional, unicorn/decacorn (Gojek, Tokopedia, Grab, dll.), Big 4, Fortune 500, institusi nasional terkemuka | **+3 poin** |
| B | Perusahaan swasta mid-size yang dikenal di industri/regional, startup Series A+, lembaga regional terkemuka | **0 (netral)** |
| C | Perusahaan tidak dikenal/sangat kecil, atau sole proprietor tanpa klien notable yang teridentifikasi dari profil | **−2 poin** |

Aturan: (1) Kenali dari training knowledge → tier langsung. (2) Tidak dikenal tapi ada sinyal (Tbk., Persero, klien notable, employee count besar) → tier dari sinyal. (3) Tidak bisa diverifikasi → Tier B, catat *"tidak dapat diverifikasi — diasumsikan netral"*. (4) Freelance dengan klien Tier A/B → ikuti tier klien.

**Final Score** = Match Score + Modifier (capped 0–100%)
Tampilkan eksplisit: *"78% + 3 poin prestige → Final: 81%"*

| Final Score | Verdict |
|-------------|---------|
| ≥ 85% | ✅ HIGHLY RECOMMENDED |
| 70–84% | 🟡 RECOMMENDED WITH NOTES |
| < 70% | 🔴 NOT RECOMMENDED |

Verdict menggunakan **Final Score** (bukan raw Match Score).

---

## Step 6 — Generate .docx

Baca docx skill: `view /mnt/skills/public/docx/SKILL.md`

Struktur dokumen:
1. Header — nama kandidat, role, tanggal review, nama pelatihan
2. Ringkasan Profil — 3–5 kalimat
3. Tabel Dimensi A: Teaching Skill — 4 aspek, skor, observasi user
4. Tabel Dimensi B: Profile–Silabus Fit — 4 aspek, skor, bukti dari profil
5. Match Score & Verdict — ditampilkan besar, warna sesuai verdict
6. Kekuatan Kandidat — 3–5 poin
7. Gap / Catatan — poin yang perlu diperhatikan atau dikonfirmasi
8. Rekomendasi Tindak Lanjut

Warna skor:
- 4–5 → `E2F0D9` / `375623`
- 3 → `FFF2CC` / `7F6000`
- 1–2 → `FCE4D6` / `C00000`

Warna verdict:
- HIGHLY RECOMMENDED → `1C7C3A`
- RECOMMENDED WITH NOTES → `7F6000`
- NOT RECOMMENDED → `C00000`

Nama file: `trainer_review_[slug_nama_kandidat].docx`

```bash
python /mnt/skills/public/docx/scripts/office/validate.py trainer_review_[slug].docx
cp trainer_review_[slug].docx /mnt/user-data/outputs/
```

Gunakan `present_files` untuk share ke user.

---

## Multiple Kandidat

Generate satu .docx per kandidat. Setelah semua selesai, tampilkan tabel ringkasan in-chat: nama, match score, verdict semua kandidat berdampingan.

---

## Edge Cases

| Situasi | Handling |
|---------|----------|
| Skor video ambigu | Inferensikan dari teks, konfirmasi jika tidak yakin |
| Profil sangat minim | Skor 1–2, catat "perlu konfirmasi" |
| Bahasa Indonesia | Output penuh Bahasa Indonesia |
| Bahasa Inggris | Output penuh Bahasa Inggris |
