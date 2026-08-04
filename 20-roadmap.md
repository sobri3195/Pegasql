# Roadmap

## Prinsip prioritas

Safety dan testability mendahului penambahan capability. Milestone berbasis exit criteria, bukan tanggal atau jumlah fitur.

## M0 — Baseline dan containment

- Dokumentasikan perilaku aktual, dependency, opsi, files, dan network boundaries.
- Tambahkan license/dependency manifest, ignore runtime artifacts, compile/lint baseline.
- Nonaktifkan/quarantine self-update, arbitrary command, brute force, shell/RFI, dan aksi berisiko.
- Buat characterization tests untuk CLI/utilitas aman.

**Exit:** baseline repeatable; tidak ada test publik; capability berisiko tidak dapat dipanggil tanpa build/policy khusus.

## M1 — Core dan policy

- Ekstrak config, target normalization, scope manifest, policy decision, run lifecycle.
- Tambahkan dry-run, typed errors, exit codes, cancellation, dan request budgets.
- Fixture server/DNS lokal dan unit/security tests.

**Exit:** semua target ditolak default dan acceptance test membuktikan tidak ada connect sebelum approval.

## M2 — Transport dan checks pasif

- Central HTTP transport dengan TLS/redirect/size/rate limits.
- Migrasi availability, headers/TLS metadata, dan fingerprint pasif.
- Finding schema versioned, artifact store per-run, JSON/human reporter.

**Exit:** end-to-end fixture menghasilkan report valid dan tidak membocorkan secret.

## M3 — Packaging dan operasi

- CI hermetik, locked dependencies, SBOM, signed artifacts/provenance.
- Observability, retention, runbook incident/rollback, compatibility shim terbatas.

**Exit:** release reproducible dari clean checkout dan canary lab berhasil.

## M4 — Agent-assisted development

- Implement typed task/result envelopes, project memory/ADR index, ownership, dan QA/Security gates.
- Evaluasi agent workflow pada perubahan docs/test sebelum code sensitif.

**Exit:** audit menunjukkan requirement-to-test traceability dan tidak ada autonomous network action.

## M5 — Evaluasi capability lanjutan

Hanya pertimbangkan checks aktif non-destruktif bila ada use case, threat model, legal approval, sandbox, rate budget, dan false-positive benchmark. Capability credential attack, arbitrary command, persistence, dan shell upload tetap non-goal.

## Backlog prioritas

1. Perbaiki binding `decode64`, duplicate validation, path/file overwrite, dan dispatch ambiguity dengan regression tests.
2. Ganti scraping provider rapuh dengan adapter resmi atau input inventory.
3. Pisahkan output ANSI dari data.
4. Tambahkan schema migration policy dan deprecation schedule.
5. Perluas dokumentasi pengguna setelah CLI aman stabil.
