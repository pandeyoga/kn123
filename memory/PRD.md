# PRD — Kain Nusantara (KN) · sesi audit-perbaikan 2026-09-05

## Pernyataan masalah (asli)
Lanjutkan development repo `https://github.com/snakissb/KN`: clone, verifikasi catatan audit
(`INSTRUKSI_PERBAIKAN_2026-09.md`, `audit_temuan_2026_09.py`, patch) — jika valid perbaiki, tanpa
merusak yang sudah benar; validasi cermat. Pemilih: kerjakan 11 temuan sesuai urutan gelombang;
pemisahan per temuan cukup di laporan; setuju mengubah default env (CORS/cookie/seed).

## Arsitektur
FastAPI (`backend/`, 122 router · 189 service · MongoDB via Motor) + React CRA (`frontend/`) +
guardrail statik/runtime (`scripts/gate.sh`, `scripts/guardrails/*`). Data demo `seed_realistic.py`,
fondasi `bootstrap.run_bootstrap()` saat backend start. Lingkungan dipulihkan `bash .restore_env.sh`.
Login demo: `admin@kainnusantara.id / demo12345` (lihat `memory/test_credentials.md`).

## Persona
Admin · Manajer · Admin Sales · Finance · Sales · Gudang · Desainer · Sopir · MD · Admin Gudang (ERP tekstil multi-PT).

## Yang dikerjakan 2026-09-05 (rincian + bukti: `memory/LAPORAN_PERBAIKAN_2026-09.md`)
- T-05 runner korpus permanen `scripts/run_corpus.py` + `scripts/triase_korpus.py` → `memory/TRIASE_KORPUS_2026-09.md` (220 skrip: 84 lulus · 58 lingkungan · 2 uji basi · 76 tidak tahu); pemeriksa basi `INV-GL-REV-01` diperbaiki.
- T-06 `scripts/install_hooks.sh`, `.github/workflows/gate.yml` (belum pernah dijalankan di GitHub).
- T-02 CORS gagal-berisik + cookie Secure dari env + `backend/.env.example`.
- T-08 `SEED_DEMO_ENABLED` bawaan false (dev `.env` = true).
- T-07 `scripts/gen_codebase_map.py` + guard `INV-DOC-01`; `CODEBASE_MAP.md` digenerate.
- T-11 `approve_order` idempoten; T-09 saran reorder sadar `rnd.lifecycle_enforcement` (+lencana FE); T-10 `/ar-receipts` 403 sebelum 422.
- T-03 Lapis 1–3: filter `movement_type` ke query (COLLSCAN→IXSCAN, hasil identik) + guard `INV-PERF-01` (ratchet).
- T-04 tabel triase `memory/TRIASE_NPLUS1_2026-09.md` (214 kandidat, nol perbaikan).
- T-01 Langkah 1 klaim atomik `resolve-escalation` outbound (+probe ulang-jalan) · Langkah 2 `memory/INVENTARIS_MULTI_KOLEKSI_2026-09.md` (87 endpoint).

## Backlog (prioritas)
- P0 Keputusan pemilik (§7 laporan): T-01 Opsi A/B · 55 skrip URL-hardcoded · 2 uji basi SoD · `warehouse_id` saran reorder · seed 42 koleksi sisa & akun bootstrap.
- P1 T-05: baca 76 skrip `TIDAK TAHU`; T-03 Lapis 4 paginasi 3 router; T-04 perbaiki 2 lokasi `PERBAIKI`; `inbound_receiving` resolve-escalation (pola T-01).
- P2 5 gate `--quick` merah pra-eksisting (INV-UI-01/UI-10/UX-01/i18n); T-10 daftar saudara 459 endpoint; jalankan CI di GitHub.
