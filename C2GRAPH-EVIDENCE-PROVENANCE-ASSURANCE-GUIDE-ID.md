# Panduan Evidence, Provenance, dan Assurance C2GRAPH

**English:** [C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md](C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md)

> Scope: C2GRAPH v1.2.0. Dokumen ini menjelaskan cara evidence mendukung requirement, decision, compliance mapping, verification, dan completion claim. Ia tidak membuat write authority.

## 1. Evidence-first

C2GRAPH membedakan fakta yang terlihat, inference, requirement, claim, dan keputusan. Confidence tidak mengubah inference menjadi evidence.

Contoh batas klaim:

```text
file count           != token saving
cache telemetry      != reasoning quality
validator PASS       != universal correctness
graph citation       != semantic truth
synthetic simulation != production behavior
```

## 2. Requirement provenance

Requirement harus mempertahankan asalnya, misalnya extracted, inferred, ambiguous, test-derived, atau document-derived. Tujuannya agar “agent menyimpulkan” tidak berubah diam-diam menjadi “repository mewajibkan”.

## 3. Provenance

Public surface menyatakan provenance dapat merekam model, provider, source, run, phase, decision, human acceptance, dan digest lineage. Ledger dapat diekspor ke W3C PROV-O JSON-LD. Digest chaining adalah integrity evidence, bukan bukti cryptographic authorship/identity.

## 4. Test-to-workflow dan evidence contract

Test harus dipetakan ke workflow/claim yang benar. `PASS` tidak otomatis membuktikan semua hal; vacuous pass harus dibedakan dari pemeriksaan yang benar-benar memiliki objek bermakna.

## 5. Compliance evidence mapping

C2GRAPH dapat memetakan evidence terhadap struktur kontrol seperti SOC 2, PCI DSS 4.0, dan EU AI Act. Ini **evidence mapping**, bukan sertifikasi dan bukan jaminan compliance.

## 6. Verification classes

Bedakan:

- deterministic registered checks;
- declared finite-domain enumeration;
- randomized exercises;
- fuzz/adversarial harnesses;
- package/manifest integrity;
- process conformance;
- external/live observation.

Kata “exhaustive” hanya sah jika finite domain yang dimaksud disebutkan. Ia bukan klaim tentang semua kondisi real-world.

## 7. Completion claim

Completion harus dibatasi oleh evidence yang benar-benar tersedia. Unknown/unverified area harus tetap terlihat. Reporting yang kuat bukan reporting yang menghapus uncertainty, tetapi yang membuat boundary uncertainty dapat diaudit.

## 8. Authority non-interference

Evidence, provenance, compliance status, cache state, ACRP mode, CEA containment, severity, atau liveness classification dapat mengubah strategy/diagnosis. Semuanya tidak boleh memperluas exact write predicate.
