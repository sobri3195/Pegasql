# Aturan Proyek

## Prioritas aturan

1. Hukum, izin pemilik sistem, dan kebijakan organisasi.
2. Instruksi repository (`AGENTS.md` jika kelak ditambahkan) dan dokumen ini.
3. PRD, arsitektur, serta standar testing/deployment.
4. Instruksi task yang tidak bertentangan dengan aturan di atas.

## Aturan keselamatan wajib

- Gunakan Pegasql hanya pada aset milik sendiri atau dengan izin tertulis yang masih berlaku.
- Scope bersifat deny-by-default. Redirect, DNS resolution, subdomain, dan IP hasil resolusi harus tetap berada dalam scope.
- Dilarang menambah credential brute force, shell upload/execution, persistence, destructive payload, evasion, atau mass internet scanning.
- Network test otomatis hanya menuju fixture lokal/terisolasi. Test CI tidak boleh menyentuh internet publik.
- Jangan menaruh token, password, cookie, PII, response body sensitif, atau target pelanggan dalam source, fixture, log, commit, dan PR.
- Perubahan pada policy, transport, authentication, updater, command execution, atau active checks membutuhkan review Security Agent dan persetujuan manusia.
- Harus tersedia dry-run, timeout, request budget, rate/concurrency limits, cancellation, dan audit trail sebelum active networking dirilis.

## Aturan perubahan

- Pahami alur terkait sebelum mengedit; bedakan fakta **as-is** dengan usulan **target-state**.
- Buat perubahan kecil dan atomik. Satu PR harus memiliki tujuan, risiko, test evidence, rollback, dan dokumentasi yang koheren.
- Jangan melakukan refactor tak terkait, mengganti kontrak CLI diam-diam, atau mengedit artefak pengguna.
- Semua bug fix diawali regression test bila memungkinkan.
- Dependency baru harus memiliki alasan, versi terkunci, lisensi kompatibel, dan pemeriksaan kerentanan.
- Jangan membungkus import dalam `try/catch`; dependency wajib harus gagal cepat dan jelas.

## Definition of Done

- Acceptance criteria terpenuhi dan traceable.
- Formatter/linter, compile check, unit/integration/security tests relevan lulus.
- Dokumentasi, schema, changelog, dan contoh diperbarui bila kontrak berubah.
- Tidak ada secret atau artefak runtime dalam diff.
- QA dan Security gate lulus; reviewer manusia menyetujui perubahan berisiko.
- Rollback/recovery telah dijelaskan dan commit dapat direproduksi.

## Git dan review

- Gunakan commit message imperatif dan spesifik.
- Jangan force-push shared branch atau mengubah history tanpa instruksi eksplisit.
- PR menjelaskan `why`, `what`, test aktual, risiko, kompatibilitas, dan rollback.
- Status “lulus” hanya boleh diberikan berdasarkan command/evidence yang benar-benar dijalankan.

## Konflik dan eskalasi

Jika scope, otorisasi, atau dampak tidak jelas: hentikan aksi jaringan/berisiko, dokumentasikan asumsi, dan minta keputusan manusia. Agent boleh terus melakukan analisis statis atau menyiapkan test lokal yang aman.
