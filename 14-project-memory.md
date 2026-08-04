# Memori Proyek

## Tujuan

Project memory menyimpan fakta dan keputusan yang tahan lama agar agent tidak mengulang analisis atau mengandalkan konteks percakapan sementara. Git-tracked docs adalah source-of-truth; chat bukan source-of-truth.

## Kategori

- **Facts:** bahasa, entrypoint, dependency, struktur aktual.
- **Decisions:** ADR dengan status proposed/accepted/superseded.
- **Contracts:** CLI, schema, interface, exit codes.
- **Operations:** build/test/release/runbook.
- **Known debt:** risiko, owner, prioritas, target milestone.

## Aturan pembaruan

- Catat hanya hal yang diverifikasi; beri tanggal/commit untuk fakta yang mudah berubah.
- Jangan simpan secret, data target, PII, atau evidence mentah.
- Keputusan yang berubah tidak dihapus: tandai superseded dan tautkan pengganti.
- Dokumentasi diperbarui dalam PR yang sama dengan perubahan kontrak.
- Reporter memeriksa drift antara memory dan source.

## Suggested layout

```text
docs/
  adr/
  contracts/
  runbooks/
  threat-model/
  decisions-index.md
```

## Baseline terverifikasi saat dokumen dibuat

- Entrypoint: `pegasql.pl`.
- Installer/backup shell legacy: `pegasql.bac`.
- Tidak ada test suite, CI, manifest dependency, atau struktur module terpisah.
- Agent architecture dalam dokumen ini masih target-state.

## Retensi dan hygiene

Review memory setiap release; hapus cache generated, bukan history keputusan. Link checker dan documentation review menjadi bagian CI. Informasi operasional sensitif tinggal di sistem akses-terkontrol dan hanya direferensikan dengan ID.
