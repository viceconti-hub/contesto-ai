# Report Sincronizzazione — 22/04/2026

**Eseguito:** mercoledì 22 aprile 2026, ore 19:46
**Tipo:** manuale + verifica fetch URL

---

## Passo 1 — Sincronizzazione riepiloghi

### File aggiornati

| Progetto | File sorgente | Esito |
|---|---|---|
| 400.SINTESI ECOSISTEMA VICECONTI | SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO 22_04_2026.md | **AGGIORNATO** |
| 451.SAP SERVICE LAYER | SAP SERVICE LAYER RIEPILOGO 22_04_2026.md | **AGGIORNATO** |
| 460.DATABASE CENTRALIZZATO | DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md | **AGGIORNATO** |
| 480.n8n | n8n RIEPILOGO 06_04_2026.md | **AGGIORNATO** |

### File invariati (9)

100 NUOVA STRATEGIA, 200 AI HUMAN LAB, 300 AUDIT INFRASTRUTTURA, 410 NAMING VICECONTI,
420 MEMORIA E CONTESTO AI, 430 STRUMENTI DI CATTURA VOCALE, 440 SITI WEB,
450 SAP ACADEMY, 500 ASSISTENTI INTELLIGENTI

### Cartelle senza riepilogo conforme (3)

| Cartella | Nota |
|---|---|
| 445.TELEGRAM | Cartella vuota |
| 470.ASSET PRODOTTI | Cartella vuota |
| 490.PIPELINE PRESTASHOP | Solo .docx — placeholder in SINTESI con nota |

### Anomalie rilevate e corrette

- **450.SAP ACADEMY**: file `SAP SERVICE LAYER RIEPILOGO CHAT COMPLETA 15_02_2026.txt` presente nella cartella ma di pertinenza del progetto 451. Ignorato, usato correttamente `SAP ACADEMY RIEPILOGO 13_02_2026.md`.
- **440.SITI WEB**: algoritmo aveva selezionato `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` (stessa data). Corretto manualmente con `SITI WEB RIEPILOGO 04_03_2026.md`.

### File extra nella cartella SINTESI (non mappati ai progetti attivi)

- `CALENDAR RIEPILOGO.md` — origine sconosciuta, già presente in SINTESI. Pushato su GitHub senza modifiche.
- `SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md` — duplicato rispetto a `SINTESI ECOSISTEMA VICECONTI SINTESI.md`. Entrambi presenti e pushati. Valutare pulizia.

---

## Passo 2 — Push su GitHub Pages

**Repo:** viceconti-hub/contesto-ai (branch: main)

| File | Esito |
|---|---|
| SINTESI ECOSISTEMA VICECONTI SINTESI.md | NUOVO su GitHub |
| CALENDAR RIEPILOGO.md | NUOVO su GitHub |
| DATABASE CENTRALIZZATO RIEPILOGO.md | AGGIORNATO |
| SAP SERVICE LAYER RIEPILOGO.md | AGGIORNATO |
| SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md | AGGIORNATO |
| n8n RIEPILOGO.md | AGGIORNATO |
| last_push.json | AGGIORNATO (22/04/2026 19:46) |
| tutti gli altri (13 file) | INVARIATO |

**Riepilogo:** 7 aggiornati/nuovi · 13 invariati · 0 errori

---

## Passo 3 — Verifica fetch URL

Fetch reale su `raw.githubusercontent.com` per ogni file:

**20/20 OK — nessun errore, nessun mismatch contenuto**

Tutti i file sono raggiungibili e il contenuto corrisponde al file locale.

---

## Stato complessivo

✅ Sincronizzazione: 4 aggiornati, 9 invariati, 3 mancanti  
✅ Push GitHub: 7 aggiornati/nuovi, 13 invariati, 0 errori  
✅ Verifica fetch: 20/20 URL raggiungibili, contenuto verificato  
⚠️ Da valutare: rimozione duplicato `SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md` e origine di `CALENDAR RIEPILOGO.md`
