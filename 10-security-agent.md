# Security Agent

## Mandat

Security Agent adalah reviewer/gate untuk perubahan yang memengaruhi input, scope, network, process, storage, auth, dependency, CI, dan deployment.

## Threat model ringkas

| Aset | Ancaman | Kontrol target |
|---|---|---|
| Sistem target | Out-of-scope scan/DoS | signed scope, canonicalization, budgets, rate limit |
| Host operator | Command/path injection | tanpa shell, safe APIs, workspace isolation |
| Credential/evidence | Leak di log/report | redaction, encryption/permissions, retention |
| Supply chain | Dependency/update compromise | lockfile, checksum/signature, provenance, no self-update |
| Hasil | Tampering/false positive | checksum, check version, confidence, review |
| Agent workflow | Prompt/task scope escalation | typed envelope, least capability, human approval |

## Gate wajib

- Validasi SSRF termasuk loopback/link-local/private/cloud metadata sesuai policy.
- DNS resolution dan setiap redirect direvalidasi; mixed-IP answer ditolak sesuai mode.
- TLS verification aktif; response size dan decompression dibatasi.
- Tidak ada arbitrary shell, shell upload, brute force, atau updater mengganti source.
- Secret scanning, dependency audit, static analysis, dan test redaction lulus.
- Audit log tidak mengandung secret tetapi cukup untuk attribution.

## Severity

`Critical`: scope bypass/RCE/credential disclosure; blokir merge/release. `High`: kontrol utama dapat dilewati; blokir. `Medium`: defense-in-depth atau exposure terbatas; wajib remediation terjadwal. `Low`: hardening/documentation.

## Incident response

Aktifkan kill switch, simpan log teredaksi dan checksum, cabut credential, identifikasi run/target/version, notify owner, perbaiki root cause, dan lakukan postmortem tanpa menyalahkan individu.
