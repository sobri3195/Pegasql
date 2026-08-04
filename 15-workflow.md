# Workflow Pengembangan

## Alur standar

1. **Intake:** definisikan masalah, scope, non-goals, dan acceptance criteria.
2. **Triage:** baca aturan, klasifikasikan risiko, inspeksi repo.
3. **Plan:** pecah work item dan test plan.
4. **Design:** Architect review bila boundary/contract berubah; Security review bila sensitif.
5. **Approval:** manusia menyetujui perubahan high/critical risk.
6. **Implement:** perubahan kecil, regression test lebih dahulu untuk bug.
7. **Verify:** jalankan checks relevan dan catat command aktual.
8. **Review:** QA, Security, code review, docs/compatibility review.
9. **Integrate:** commit atomik dan PR lengkap.
10. **Release/observe:** artifact terverifikasi, rollout bertahap, monitor, rollback bila perlu.

## Risk tiers

| Tier | Contoh | Gate |
|---|---|---|
| Low | docs, typo | normal review |
| Medium | pure core, output format additive | QA |
| High | network, policy, storage, dependency | QA + Security + human |
| Critical | auth/scope bypass, process execution, production release | explicit owner approval; default block |

## Branch/commit

Mulai dari working tree bersih, jangan mengubah pekerjaan orang lain, inspeksi diff sebelum commit, dan gunakan pesan imperatif. Generated/runtime files tidak dikomit. Rebase/merge mengikuti kebijakan maintainer; history rewriting bukan default.

## Failure workflow

Saat test gagal, simpan reproduksi, klasifikasikan product vs environment, dan jangan lanjut release. Saat blocker eksternal muncul, teruskan pekerjaan aman yang independen; eskalasi dengan satu keputusan konkret yang dibutuhkan.

## Emergency change

Minimalkan patch, wajib peer/security review secepatnya, sediakan rollback langsung, dan buat postmortem serta follow-up test. Emergency tidak menghapus audit trail.
