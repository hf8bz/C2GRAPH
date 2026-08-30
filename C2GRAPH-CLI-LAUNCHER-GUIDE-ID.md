# Panduan CLI Launcher C2GRAPH

**English:** [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md)

> Companion Indonesia untuk notasi `<C2PY>` dan portability launcher Python.

## Mengapa `<C2PY>` digunakan

Dokumentasi C2GRAPH tidak mengasumsikan semua host memakai command Python yang sama. `<C2PY>` adalah placeholder dokumentasi untuk launcher Python yang sudah di-resolve pada host, misalnya sesuai platform/environment yang valid.

## Aturan

- Resolve launcher melalui environment/tooling yang tersedia dan aman.
- Jangan mengubah script path atau arguments saat mengganti `<C2PY>`.
- Jangan menganggap `python`, `python3`, `py`, venv binary, atau wrapper tertentu tersedia tanpa evidence.
- Launcher resolution bukan alasan untuk menjalankan arbitrary discovered executable.

Contoh dokumentasi:

```text
<C2PY> scripts/read/core.py --path <PATH>
<C2PY> scripts/verify/all.py
<C2PY> scripts/verify/counts.py --json
```

## Batas

Notasi launcher hanya menyelesaikan portability command. Ia tidak mengganti entry mode, scope, authority, secret safety, atau verification semantics.
