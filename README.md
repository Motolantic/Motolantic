# Motolantic

**Arsip darurat proyek Motolantic** — perencanaan platform perantara impor suku cadang otomotif untuk Indonesia.

Pemilik proyek: **Tydi Humaini (Tedy)**.

Repo ini menyimpan dokumen dan checkpoint perencanaan untuk pemulihan darurat. Repo ini bukan database operasional atau aplikasi produksi.

## Navigasi

- [Checkpoint arsitektur — 12 Oktober 2026](archives/checkpoints/Motolantic_Locked_Architecture_Checkpoint_2026-10-12.md)
- [Indeks arsip](archives/README.md)
- [Panduan arsip dan pemulihan](docs/ARCHIVE_GUIDE.md)

## Model produk

Pencarian part → identifikasi kecocokan → perbandingan penawaran pemasok → estimasi biaya dan ETA → pemesanan → pengiriman internasional → pengiriman lokal Indonesia.

## Status perencanaan

| Kategori | Kandidat Status Kuat | Status |
|---|---:|---|
| Web Platform | 27 | Pencarian dikunci |
| AI Operations | 15 | Pencarian dikunci |
| Cybersecurity | 5 | Terbuka |
| Automotive Data | 1 | Pencarian sumber data ditunda |
| **Total** | **48** | |

Status Kuat berarti kandidat evaluasi, bukan instruksi memasang semua komponen. Pemilihan komponen MVP dan implementasi bertahap merupakan tahap berikutnya.

Arah arsitektur: satu platform bersama untuk Customer, Staff, Technician, dan Owner; Web Platform TypeScript/Node.js, AI Operations Python, serta PostgreSQL sebagai sumber kebenaran bisnis yang direncanakan. AI merekomendasikan; kode deterministik menghitung; izin ditegakkan di backend.

## Kebijakan arsip

Pertahankan checkpoint historis. Catat keputusan baru dalam dokumen bertanggal dan perbarui indeks. Jangan simpan data pelanggan, transaksi, dump database produksi, password, token, atau API key.

Belum ada lisensi penggunaan ulang yang ditetapkan untuk materi Motolantic. Repository pihak ketiga yang disebut dalam checkpoint tetap mengikuti lisensinya masing-masing.
