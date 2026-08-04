# Standar Coding

## Umum

- Utamakan kejelasan, fungsi kecil, nama domain, dan control flow eksplisit.
- Pisahkan parsing, policy, I/O, dan formatting.
- Hindari global mutable state; dependency dan context run diteruskan eksplisit.
- Jangan membungkus import dalam `try/catch`; dependency wajib gagal cepat.
- Komentar menjelaskan alasan/invariant, bukan mengulang syntax.
- Semua input eksternal memiliki validasi tipe, ukuran, format, dan canonicalization.

## Perl

- Wajib `use strict; use warnings;`; tetapkan versi Perl minimum setelah compatibility audit.
- Satu package per file, lexical variables (`my`), tiga-argumen `open`, lexical filehandles, dan cek error.
- Hindari indirect object syntax, bareword filehandle, magic globals, dan `system STRING`.
- Untuk process, gunakan list-form API tanpa shell; lebih baik hapus capability process dari runtime.
- Dependency dicatat dalam manifest dan dipin melalui release process.
- Gunakan formatter/linter yang disepakati (`perltidy`, `perlcritic`) setelah config dikomit.

## Error dan logging

- Error memiliki code stabil, message aman, cause, dan retryability.
- Library tidak memanggil `exit`; entrypoint memetakan result ke exit code.
- Structured log menyertakan run/check ID dan tidak menyertakan secret/body mentah.
- Jangan mengabaikan return value operasi file/network.

## Network dan concurrency

Transport tunggal mengelola TLS, proxy, redirect, timeout, retries, bytes, rate, dan cancellation. Jangan menggunakan sleep/random tersembunyi dalam domain. Shared state harus immutable atau tersinkronisasi dan workspace unik.

## Review checklist

- [ ] Boundary dan dependency direction benar.
- [ ] Tidak ada injection/path traversal/SSRF bypass.
- [ ] Failure dan cleanup diuji.
- [ ] API/schema/docs konsisten.
- [ ] Tidak ada dead code, duplicate dispatch, atau typo binding opsi.
