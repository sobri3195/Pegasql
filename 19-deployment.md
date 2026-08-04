# Deployment dan Release

## Model distribusi

Pegasql adalah CLI yang didistribusikan sebagai artifact/package immutable. “Deployment” tidak berarti memberi service pusat akses bebas ke jaringan pelanggan. Instalasi dan scan adalah dua approval terpisah.

## Prasyarat release

- Versi semantik, dependency lock/checksum, license inventory, SBOM.
- CI dari commit/tag bersih; QA dan Security gate lulus.
- Artifact ditandatangani dan memiliki provenance/checksum.
- Changelog, compatibility/migration, known issues, dan rollback tersedia.
- Installer/self-update legacy tidak digunakan.

## Konfigurasi runtime

Scope manifest dan approval berada di luar package, permission minimal, dan memiliki expiry. Workspace default berada di user data directory dengan mode restriktif. Proxy/TLS/limits dikonfigurasi eksplisit; secret melalui environment/secret store, tidak melalui argumen CLI bila dapat terlihat di process list.

## Rollout

1. Smoke test offline.
2. Lab fixture berizin.
3. Canary pada scope internal kecil.
4. Observasi deny/error/rate/cancel metrics.
5. Promosi bertahap atau rollback artifact/config.

## Operasional

Preflight memvalidasi disk, waktu, config/schema, izin workspace, dan scope tanpa network. Kill switch harus menghentikan penjadwalan request baru. Retention default pendek; deletion terverifikasi dan audit metadata minimum dipertahankan sesuai kebijakan.

## Rollback

Hentikan run, arsipkan audit teredaksi, kembalikan artifact dan config kompatibel, verifikasi checksum, jalankan smoke test, lalu dokumentasikan penyebab. Jangan rollback dengan updater yang mengunduh dan mengeksekusi source mentah.
