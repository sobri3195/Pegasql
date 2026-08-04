# Frontend Agent

## Konteks

Saat ini tidak ada web frontend; antarmuka pengguna adalah terminal ANSI. “Frontend” mencakup CLI UX dan, hanya setelah disetujui roadmap, web dashboard read-only.

## Tanggung jawab CLI

- Help yang akurat, grouping opsi, contoh aman, pesan error actionable, dan exit code konsisten.
- Mode `--dry-run`, progress yang tidak merusak output machine-readable, serta `--format human|json`.
- Accessibility: output tetap bermakna tanpa warna, mendukung `NO_COLOR`, dan tidak bergantung pada bunyi terminal.
- Konfirmasi/approval eksplisit untuk aksi berisiko; non-interactive mode harus gagal jika approval tidak tersedia.

## Kontrak presentasi

UI hanya mengonsumsi application API/events; tidak melakukan HTTP target atau membaca state internal langsung. JSON ke stdout harus bebas banner/progress; diagnostik ke stderr. Data sensitif disamarkan secara default.

## Jika web UI ditambahkan

Gunakan CSP ketat, output encoding, CSRF protection, session aman, RBAC, dan tidak pernah merender evidence mentah sebagai HTML. UI tidak boleh menjadi jalan untuk memperluas scope run.

## Testing

Golden tests untuk help/output, snapshot dengan normalisasi waktu/ID, keyboard/accessibility test untuk web, dan end-to-end terhadap fixture lokal. Screenshot hanya diperlukan untuk perubahan visual yang nyata.
