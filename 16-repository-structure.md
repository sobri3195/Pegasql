# Struktur Repository

## Aktual

```text
Pegasql/
├── README.md       # deskripsi dua baris
├── pegasql.pl      # aplikasi Perl monolitik
└── pegasql.bac     # installer shell legacy
```

Skrip menghasilkan `output/` serta file kerja seperti `aTdorks.txt`, `aTmotors.txt`, `aTsearch.txt`, `aTexploits.txt`, `aTmails.txt`, `pegasearch.txt`, dan `pegasql.txt`. Semua harus dianggap runtime artifacts dan tidak menjadi kontrak permanen.

## Target bertahap

```text
bin/pegasql
lib/Pegasql/
  Domain/
  Application/
  Ports/
  Adapters/
  Checks/
t/unit/
t/contract/
t/integration/
t/fixtures/
config/schema/
docs/adr/
docs/contracts/
docs/runbooks/
scripts/
```

## Ownership

- `Domain`, contracts, ADR: Architect + Backend review.
- `Policy`, `Transport`, security config: Backend + Security mandatory.
- CLI/presentation: Frontend/CLI owner.
- `t/`: code owner + QA review.
- CI/release/scripts: DevOps + Security.
- Product/standards: product owner/maintainers.

## Konvensi

- Source tidak menulis ke repository root saat runtime.
- Fixture kecil, sintetis, tanpa data pelanggan.
- Satu module memiliki satu tanggung jawab dan test pasangan.
- Generated report/build/cache masuk ignore dan workspace per-run.
- Folder baru hanya ditambahkan saat ada implementasi; struktur target bukan alasan membuat placeholder kosong.
