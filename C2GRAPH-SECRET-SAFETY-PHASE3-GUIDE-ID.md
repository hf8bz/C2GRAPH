# Panduan Secret Safety C2GRAPH

**English:** [C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md)

> Companion Bahasa Indonesia untuk secret-aware discovery dan Phase 3 safety. Tujuannya mencegah secret-prone bytes diperlakukan seperti ordinary text evidence.

## Prinsip

Secret path/content memerlukan classification dan structural/redacted inspection yang sesuai. Agent tidak boleh membaca atau mempublikasikan raw secret hanya karena file berada di repository.

## Discovery

Gunakan bounded inventory untuk mengenali secret-prone names/patterns sebelum content read. Discovery tidak boleh mengeksekusi tooling candidate atau command berisiko untuk “mencari tahu” tanpa contract yang membolehkan.

## Read policy

Untuk secret-prone surface, gunakan redacted/structural reader yang ditetapkan. Canonical bounded-read CLI dapat mengembalikan `SECRET_PRONE_REFUSED` ketika direct text read tidak sesuai.

## Reporting

Jangan memasukkan secret value ke log, provenance, handoff, failure memory, output, atau documentation. Report existence/type/location secara minimum yang diperlukan, bukan nilai rahasianya.

## Mutation

Perubahan secret/config sensitive tetap membutuhkan normal semantic scope, exact write plan, rollback, fresh pre-state, dan verification. “Secret fix” tidak menjadi blanket authority.

## Incident

Jika exposure terdeteksi, utamakan containment, rotation/revocation sesuai sistem yang relevan, minimization evidence, dan audit trail tanpa memperbanyak secret copies.
