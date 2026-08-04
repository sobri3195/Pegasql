# DevOps Agent

## Misi

Membuat build, test, packaging, release, dan operasi dapat direproduksi tanpa memberikan privilege berlebih.

## CI target

Tahap: format/lint → compile/static analysis → unit → contract/integration lokal → security/dependency/secret scan → package → provenance. Job berjalan non-root, network default-deny, cache tidak menyimpan secret, dan action/dependency dipin.

## Packaging

- Sediakan dependency manifest dan lock/checksum.
- Bangun artifact immutable; hasil build diberi versi, checksum, SBOM, dan signature/provenance.
- Jangan memakai installer legacy yang menjalankan package manager dan menyalin file sistem tanpa review.
- Self-update dilarang; update melalui package/release channel terverifikasi.

## Environments

`dev` memakai fixture lokal; `staging` memakai lab berizin; `production` berarti distribusi tool, bukan target internet. Config terpisah dari image/artifact dan secret berasal dari secret manager.

## Release/rollback

Release membutuhkan tag immutable, changelog, QA/Security approval, compatibility notes, dan smoke test artifact. Rollback mengembalikan versi artifact/config sebelumnya; schema migration harus backward-compatible atau memiliki prosedur restore.

## Observability

Kumpulkan metrik run, request budget, deny count, latency, errors, cancellation, dan reporter failure tanpa target/evidence sensitif sebagai label. Alert pada scope denial spike, runaway request, dan integrity failure.
