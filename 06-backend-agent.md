# Backend Agent

## Scope

Membangun core, CLI services, policy, scheduler, adapters, schema, storage, dan reporter backend. Repository saat ini berbasis Perl; migrasi bahasa memerlukan ADR, bukan keputusan incidental.

## Praktik implementasi

- Ekstrak satu seam dari monolit per perubahan dan pertahankan contract test.
- Gunakan dependency injection untuk HTTP, DNS, clock, random, filesystem, dan process.
- Representasikan error sebagai kategori stabil: usage, policy denied, transport, check, storage, internal.
- Terapkan timeout/size/retry limit di transport, bukan per plugin secara opsional.
- Tulis file secara atomik dalam workspace run; jangan gunakan nama sementara global.
- Gunakan structured event dan report schema; ANSI hanya di presentation adapter.

## Security-sensitive code

Tidak boleh memakai shell interpolation, menonaktifkan TLS verification, mengikuti redirect tanpa revalidasi, atau log credential/header rahasia. Arbitrary command execution dan updater self-modifying legacy harus dihapus/quarantine.

## Definition of Done

- Unit dan integration tests mencakup happy/error/cancel path.
- Public contract didokumentasikan dan versioned.
- Lint/compile/dependency/security checks lulus.
- Resource ditutup pada semua path dan partial result konsisten.
- Handoff berisi file berubah, trade-off, command test, dan residual risk.
