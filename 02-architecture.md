# Arsitektur

## Prinsip

- Functional core, imperative shell: parsing dan networking di boundary; policy/domain sebisa mungkin pure.
- Semua outbound request melewati satu transport dan satu policy gate.
- Capability eksplisit; plugin tidak memperoleh filesystem/network/process access secara implisit.
- Workspace per-run dan output schema versioned.
- Runtime scanning deterministik; agent hanya mengelola lifecycle perubahan software.

## Target logical architecture

```text
CLI -> Config/Scope Loader -> Policy Engine -> Orchestrator/Scheduler
                                      |              |
                                      |        Check Registry
                                      |              |
                                      +-------- Transport ----> authorized target
                                                     |
                        Artifact Store <--- Event Bus +-- Finding Store
                                                     |
                                                  Reporters
```

## Layer dan dependency direction

1. **Domain:** Run, Target, Finding, PolicyDecision; tidak bergantung pada I/O.
2. **Application:** use case, scheduler, cancellation, lifecycle.
3. **Ports:** interface transport, clock, resolver, store, reporter.
4. **Adapters:** HTTP/socket, filesystem, CLI, JSON, provider discovery.

Dependency hanya mengarah ke dalam. Adapter boleh bergantung pada port; domain tidak boleh mengimpor adapter.

## Runtime lifecycle

`CREATED -> VALIDATED -> APPROVED -> RUNNING -> {COMPLETED|PARTIAL|CANCELLED|FAILED}`. Transisi dicatat. Tidak ada request sebelum `APPROVED`. Cancellation menghentikan penjadwalan baru, menutup request aktif sesuai deadline, lalu mem-flush hasil.

## Boundary keamanan

- **Input boundary:** normalisasi Unicode, URL, hostname, IP, path, dan batas ukuran.
- **Scope boundary:** validasi host/IP sebelum DNS dan setelah resolution/redirect.
- **Network boundary:** TLS verification, timeout, maximum bytes, redirect count, rate limit.
- **Process boundary:** target-state tidak menyediakan arbitrary command execution.
- **Storage boundary:** path relatif tervalidasi, permission minimal, redaction dan retention.

## Strategi migrasi

Pertama karakterisasi perilaku aman lama; kedua ekstrak domain dan policy; ketiga centralize transport; keempat migrasikan modul pasif; terakhir quarantine/hapus fitur berisiko. Entrypoint lama dapat menjadi compatibility shim sementara dengan warning dan pemetaan opsi yang terdokumentasi.

## Architecture Decision Records

Keputusan irreversible atau lintas modul dicatat sebagai ADR: konteks, opsi, keputusan, konsekuensi, security impact, dan rollback. Architect Agent memiliki template; persetujuan tetap milik manusia.
