# C2GRAPH Bounded Emergency Authorization (BEA v2) — Rilis v1.2.0

> **Status rilis:** BEA v2 termasuk dalam C2GRAPH v1.2.0. Mekanisme ini dibangun di atas exact-write contract
> v1.1.0 tanpa mengubah klaim historis byte rilis v1.1.0 secara retrospektif.

## Tujuan

C2GRAPH sudah memerlukan semantic scope, `ACTUAL_WRITE_SET` canonical dari PHASE 3.W, rollback/write authority
per path/action, serta freshness tepat sebelum setiap action. BEA menambahkan overlay approval untuk respons insiden
yang tetap bounded tanpa membuat jalur mutation authority kedua.

Aturan utamanya:

> Emergency memperpendek prosedur menuju write canonical yang sudah authorized; emergency tidak pernah mengubah
> write yang unauthorized menjadi authorized.

## Entry dan authority

Agent boleh mendeteksi insiden dan otomatis mengurangi effective authority dengan menghentikan action terdampak yang
tidak aman. User/operator juga boleh mendeklarasikan insiden. Detection maupun declaration tidak memberi BEA. Agent
melakukan analisis read-only yang bounded, PHASE 3.W menghitung `ACTUAL_WRITE_SET` normal, rollback dibuktikan,
lalu assertion approval manusia yang dimediasi host dapat menghasilkan grant BEA berumur pendek.

`severity` hanya memengaruhi urgensi, bukan scope authority. Wildcard dilarang.

## Binding exact

Proposal BEA v2 mengikat incident, run, session epoch, fingerprint target root, digest forward write plan canonical,
semantic-scope attestation, exact action ID, target prestate, rollback basis, issuance, dan expiry. Grant tidak dapat
dipindahkan lintas session epoch. Arbitrary MODIFY/DELETE yang tidak memiliki distribution restore juga membutuhkan
rollback snapshot nyata yang non-empty, regular, bukan symlink, dan berada di luar target repository; SHA-256,
resolved-path fingerprint, serta byte size dibind pada proposal. Perluasan plan atau material binding drift selalu
memerlukan proposal dan approval baru.

Sebelum setiap forward action, preflight C2GRAPH yang sudah ada kembali memverifikasi path exact dan expected
pre-state. Freshness failure, mismatch plan/target/session/run, snapshot/prestate stale, grant expired/future-issued, atau incident
yang tidak aktif akan fail closed. BEA v2 hanya mengotorisasi exact forward action. Eksekusi rollback tetap berada pada
jalur normal rollback freshness/effect authority dan tidak pernah diwarisi dari emergency grant.

## Batas kepercayaan host

Repo tidak dapat membuktikan secara kriptografis bahwa manusia benar-benar memberikan approval. `--human-approved`,
`--approver`, `--identity-assurance`, dan `--incident-state` adalah assertion dari host. Script memvalidasi struktur,
integrity, dan binding exact; host produksi wajib mengautentikasi operator serta mencegah worker yang tidak dipercaya
memalsukan assertion pada interface approval/check. Digest SHA-256 adalah bukti integrity/binding, bukan signature.

BEA tidak memiliki incident registry atau persistent revocation service independen. Grant menjadi tidak dapat dipakai
ketika TTL habis, exact binding menjadi stale, session berubah, atau host menyatakan incident tidak lagi aktif.

## Prosedur

```text
DETECT atau DECLARE
  -> SAFE HOLD
  -> bounded read-only analysis
  -> PHASE 3 / PHASE 3.W normal
  -> forward ACTUAL_WRITE_SET canonical + semantic-scope attestation
  -> rollback readiness; external snapshot bila diperlukan
  -> proposal BEA v2 exact
  -> assertion approval manusia melalui host
  -> BEA check per action + freshness check yang sudah ada
  -> normal serialization/effective-route tooling
  -> exact forward canonical mutation
  -> effect verification
  -> grant tidak dapat dipakai setelah completion/expiry/drift/session change
  -> post-incident reconciliation
```

## CLI

Gunakan `scripts/emergency/authority.py propose`, `approve`, dan `check`. Approval sengaja dipisahkan dari proposal.
Proposal wajib memakai plan exact dan semantic-scope attestation. `approve` membutuhkan `--human-approved` serta
assertion approver. `check` wajib menerima run, session epoch, incident state, target, dan action ID exact saat effect.

## Non-kapabilitas yang disengaja

ACRP, cache-hit telemetry, severity, serta requested/effective model identity bersifat non-authoritative; model failover boleh rebind tooling/cache/reasoning tetapi tidak pernah memperluas scope BEA. BEA bukan unrestricted break-glass, tidak memberi authority otomatis ke cloud/database/payment/deployment di luar
kontrak repository, tidak mengautentikasi operator dengan sendirinya, bukan distributed consensus, dan tidak mengklaim
atomicity universal terhadap arbitrary filesystem atau power loss. BEA juga bukan rollback executor pengganti.
