# Product Requirements Document — Pegasql

## 1. Ringkasan

Pegasql saat ini adalah aplikasi CLI Perl monolitik untuk mengumpulkan kandidat URL dari mesin pencari lalu menjalankan pemeriksaan HTTP, CMS, injeksi, port, dan utilitas encoding. Produk modernisasi yang dituju adalah **alat audit keamanan defensif** untuk aset yang dimiliki atau memiliki izin tertulis, dengan eksekusi terkontrol, bukti yang dapat diaudit, dan pemisahan modul yang dapat diuji.

Dokumen bernomor `00`–`20` adalah spesifikasi kerja untuk target tersebut. Deskripsi tentang agent/orchestrator adalah **arsitektur tujuan**, bukan fitur yang sudah ada. Kondisi aktual hanya `pegasql.pl`, `pegasql.bac`, dan README singkat.

## 2. Masalah

- Seluruh alur, state global, I/O jaringan, formatting terminal, dan dispatch berada dalam satu berkas besar.
- Target dan artefak kerja dipertukarkan lewat berkas sementara di root (`aTsearch.txt`, `aTdorks.txt`, `pegasql.txt`, dan lainnya).
- Fitur berisiko tinggi—brute force, pemanggilan shell, updater yang mengganti kode, dan pemindaian jaringan—tidak memiliki policy gate yang memadai.
- Tidak ada dependency manifest, suite pengujian, CI, format keluaran terstruktur, atau release process yang terdokumentasi.
- Integrasi mesin pencari dan endpoint eksternal menggunakan URL/pola lama dan parsing HTML rapuh.

## 3. Tujuan produk

1. Menyediakan inventarisasi dan pemeriksaan keamanan **non-destruktif** yang repeatable untuk scope berizin.
2. Menolak target di luar allowlist dan merekam identitas operator, persetujuan, waktu, dan konfigurasi run.
3. Menghasilkan temuan terstruktur dengan severity, confidence, evidence yang disanitasi, dan remediation.
4. Memecah monolit menjadi CLI, domain/core, adapter jaringan, policy engine, dan reporter.
5. Mendukung koordinasi agent untuk perencanaan dan perubahan kode, bukan otonomi serangan.

## 4. Non-goals

- Eksploitasi, persistence, pengunggahan shell, credential attack, atau bypass pembatasan target.
- Menjamin bahwa pola respons adalah kerentanan tanpa verifikasi manusia.
- Crawling internet secara massal atau penggunaan search-engine scraping tanpa persetujuan penyedia.
- Menyimpan password, cookie autentikasi, payload sensitif, atau response body mentah secara default.

## 5. Persona dan kebutuhan

| Persona | Kebutuhan |
|---|---|
| Security engineer | Menjalankan pemeriksaan berizin dengan konfigurasi konsisten dan evidence aman. |
| Maintainer | Mengubah modul kecil, menjalankan test lokal, dan melacak keputusan. |
| Reviewer/approver | Menyetujui scope dan aksi aktif sebelum run. |
| Auditor | Membuktikan siapa menjalankan apa, terhadap scope mana, dan versi tool apa. |

## 6. Kebutuhan fungsional

### P0 — fondasi aman

- CLI memuat konfigurasi, memvalidasi kombinasi opsi, dan menampilkan dry-run.
- Scope manifest wajib berisi target, pemilik/approver, masa berlaku, dan kategori pemeriksaan.
- Policy engine melakukan canonicalization host/IP, pemeriksaan allowlist, rate limit, concurrency limit, timeout, dan kill switch.
- Modul pasif: validasi HTTP, metadata/TLS, fingerprint dengan confidence, dan ekstraksi link/email yang dibatasi.
- Reporter menghasilkan JSON versioned dan ringkasan manusia; secret dan data pribadi direduksi.
- Run ID unik, audit log append-only, dan exit code stabil.

### P1 — kualitas dan migrasi

- Pisahkan discovery, transport, checks, storage, dan reporting dari entrypoint.
- Adapter discovery resmi/configurable; tidak bergantung pada scraping HTML hard-coded.
- Unit, integration, contract, dan golden tests berjalan di CI tanpa menyentuh internet publik.
- Konversi artefak sementara ke workspace per-run dengan cleanup atomik.

### P2 — orkestrasi agent

- Planner memecah permintaan; Architect menetapkan boundary; implementer bekerja pada ownership berbeda; QA/Security memberi gate; Reporter merangkum hasil.
- Semua handoff menggunakan envelope yang dispesifikasikan di `13-agent-communication.md`.
- Perubahan berisiko memerlukan approval manusia; agent tidak boleh memperluas scope.

## 7. Kebutuhan nonfungsional

- **Safety:** deny-by-default, least privilege, tanpa shell interpolation, tanpa aksi destruktif.
- **Reliability:** timeout wajib; retry terbatas dengan jitter; run dapat dibatalkan; partial result dipertahankan.
- **Performance:** batas concurrency dan request/second eksplisit per target.
- **Portability:** Linux utama; dependency terkunci; tidak bergantung pada service manager tertentu.
- **Observability:** structured logs, correlation/run ID, metrik durasi/error/rate-limit.
- **Compatibility:** selama migrasi, opsi aman lama dipetakan atau menghasilkan pesan deprecation yang jelas.

## 8. Model data minimum

`Run` menyimpan `run_id`, versi tool/config, operator, scope hash, waktu, status, dan summary. `Target` menyimpan nilai canonical serta hasil policy. `Finding` menyimpan check ID/version, target, severity, confidence, evidence teredaksi, remediation, dan timestamps. `Artifact` menyimpan tipe, path relatif, checksum, klasifikasi, dan retention.

## 9. Metrik sukses

- 100% request jaringan melewati policy dan transport terpusat.
- 0 eksekusi terhadap target di luar scope pada acceptance tests.
- ≥80% statement coverage pada core/policy; 100% branch coverage untuk keputusan allow/deny kritis.
- Semua temuan memiliki check version, confidence, dan remediation.
- Run yang dibatalkan berhenti mengirim request baru dalam 2 detik.
- Tidak ada secret pada log fixture dan report hasil automated scanning.

## 10. Acceptance criteria rilis aman pertama

1. `dry-run` membuktikan target canonical, checks, budget request, dan keputusan policy tanpa jaringan.
2. Fixture server lokal dapat diperiksa end-to-end dan menghasilkan JSON sesuai schema.
3. IP/hostname di luar manifest ditolak sebelum DNS/connect.
4. Retry, timeout, redirect, dan cancellation diuji deterministik.
5. Fitur legacy berisiko dinonaktifkan sampai didesain ulang dan disetujui.

## 11. Risiko dan asumsi

Asumsi utama adalah operator dapat menyediakan bukti otorisasi dan target stabil. Risiko terbesar adalah penyalahgunaan capability lama, false positive, kebocoran evidence, perubahan layanan eksternal, dan refactor besar tanpa baseline test. Mitigasinya adalah quarantine fitur aktif, fixture lokal, schema versioning, review security wajib, dan migrasi bertahap.

## 12. Referensi

- Aturan wajib: `00-project-rules.md`
- Gambaran aktual/tujuan: `01-system-overview.md`, `02-architecture.md`
- Workflow dan roadmap: `15-workflow.md`, `20-roadmap.md`
