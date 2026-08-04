# Komunikasi Antar-Agent

## Prinsip

Pesan harus typed, ringkas, traceable, idempotent bila mungkin, dan tidak membawa secret. Artifact besar disimpan sebagai file aman; pesan hanya menyertakan referensi dan checksum.

## Task envelope

```yaml
schema_version: 1
task_id: TASK-123
correlation_id: RUN-...
sender: orchestrator
recipient: backend
objective: "..."
in_scope: ["path/or/contract"]
out_of_scope: ["..."]
acceptance_criteria: ["..."]
constraints: ["offline tests only"]
dependencies: []
risk: low|medium|high|critical
approval_ref: null
artifacts: []
```

## Result envelope

```yaml
schema_version: 1
task_id: TASK-123
status: completed|partial|blocked|failed
summary: "..."
changes: []
evidence: [{command: "...", outcome: pass}]
decisions: []
risks: []
follow_ups: []
```

## Semantik status

`completed` berarti semua acceptance criteria terpenuhi. `partial` berarti ada hasil berguna namun pekerjaan tersisa. `blocked` membutuhkan dependency/keputusan eksternal yang spesifik. `failed` berarti percobaan berakhir karena error. Hanya Orchestrator yang mengubah status workflow global.

## Konflik dan ownership

Satu file/kontrak memiliki satu writer pada satu waktu. Konflik kontrak dieskalasi ke Architect; konflik requirement ke Planner/product owner; security ambiguity ke Security/human approver. Agent tidak diam-diam mengubah output agent lain.

## Data handling

Gunakan klasifikasi `public`, `internal`, `confidential`, `restricted`. Prompt/log default `internal`; secret dan raw evidence adalah `restricted` dan tidak boleh masuk kanal agent.
