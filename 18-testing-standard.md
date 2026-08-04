# Standar Testing

## Prinsip

Test harus deterministic, isolated, repeatable, self-validating, dan cepat. Tidak ada test otomatis yang memindai internet publik atau aset pihak ketiga.

## Tingkatan

- **Unit:** normalization, scope rules, parsers, state transitions, finding builders.
- **Contract:** transport/store/reporter port, JSON schema, CLI exit code.
- **Integration:** proses CLI dengan HTTP/DNS/socket fixture lokal.
- **Security:** SSRF/redirect/DNS rebinding simulation, command/path injection, redaction, decompression/size limit.
- **Packaging smoke:** artifact bersih pada container/VM ephemeral.

## Coverage dan mutation

Target minimal 80% statement untuk core/policy dan 100% branch pada keputusan allow/deny kritis. Coverage bukan pengganti assertions. Mutation/property-based tests direkomendasikan untuk URL/IP canonicalization dan policy.

## Fixture

Gunakan hostname/IP dokumentasi atau loopback terisolasi; response sintetis dan kecil. Clock/random/DNS di-inject. Port dinamis dialokasikan test harness. Cleanup tetap berjalan pada failure/cancellation.

## Naming

Nama test menjelaskan kondisi dan hasil: `rejects_redirect_to_out_of_scope_ip`. Struktur Arrange–Act–Assert dan satu alasan utama kegagalan per test.

## CI gates

Formatter/linter/compile, unit, contract/integration, schema validation, secret scan, dependency audit, dan security regression. Flaky test dikarantina hanya dengan issue, owner, expiry; tidak boleh diabaikan permanen.

## Evidence

Catat exact command, exit code, environment penting, dan hasil. `NOT RUN` bukan `PASS`; environment limitation dilabeli warning dengan alasan.
