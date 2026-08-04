# Gambaran Sistem

## Kondisi saat ini (as-is)

Pegasql adalah skrip CLI Perl tunggal. Saat startup, skrip membersihkan terminal, membuat direktori `output`, mendaftarkan puluhan opsi melalui `Getopt::Long`, membangun satu `LWP::UserAgent`, kemudian melakukan validasi dan dispatch berdasarkan variabel global.

Kapabilitas yang terlihat meliputi discovery lewat Bing/Google/Ask/Yandex/Sogou, validasi respons, fingerprint CMS, pemeriksaan XSS/LFI, enumerasi admin/subdomain/email, port scan TCP/UDP, login guessing WordPress/Joomla, proxy/Tor, updater mandiri, command execution, MD5, dan Base64. Sebagian capability ini harus dianggap legacy/high-risk dan tidak boleh dipertahankan tanpa redesign.

## Komponen aktual

| Area | Implementasi |
|---|---|
| Entrypoint & CLI | top-level `pegasql.pl`, `Getopt::Long` |
| HTTP | global `LWP::UserAgent`, `HTTP::Request`, cookies |
| Socket | `IO::Socket::INET` |
| Orkestrasi | conditional dispatch dan pemanggilan subroutine langsung |
| State | variabel global dan berkas teks sementara |
| Output | ANSI terminal dan file teks |
| Update | unduh source lalu mengganti skrip lokal |
| Packaging | belum ada manifest/build/release automation |

`pegasql.bac` adalah shell installer legacy yang menyalin skrip dan menginstal package sistem. Ia bukan source-of-truth aplikasi dan perlu diaudit sebelum digunakan.

## Alur aktual ringkas

```text
argv -> parse opsi -> validasi kombinasi
     -> target/dork/IP range menjadi aTsearch.txt
     -> opsional discovery mesin pencari
     -> satu atau lebih subroutine pemeriksaan
     -> pegasql.txt/output terminal -> opsional --save
```

## Masalah struktural

- Side effect terjadi saat file dimuat; sulit di-test sebagai library.
- State file memakai nama global sehingga run paralel dapat bertabrakan.
- Banyak operasi jaringan dan file tersebar serta error handling tidak konsisten.
- Beberapa bug tampak dari inspeksi: `decode64` pernah diarahkan ke variabel encode, validasi dipanggil dua kali, pola validasi motor tidak di-anchor, dan beberapa file memakai path relatif.
- Updater dan `system($command)` menciptakan boundary eksekusi kode yang kritis.
- Tidak ada schema untuk finding, provenance, atau audit log.

## Target-state

Target-state adalah CLI tipis di atas core teruji, policy engine, scheduler, adapter, check plugins, artifact store per-run, dan reporter terstruktur. Orkestrasi agent membantu pengembangan repository; runtime scanner tetap deterministik dan tidak bergantung pada LLM.

## Batas sistem

Input: CLI/config, scope manifest, target list, dan approval. Output: report JSON/human-readable, audit log, serta artefak teredaksi. Sistem eksternal: DNS, target berizin, proxy opsional, dan discovery provider resmi. Database permanen, web UI, dan multi-tenant service belum termasuk scope rilis awal.
