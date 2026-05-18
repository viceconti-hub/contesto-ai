# Report Sincronizzazione — 18/05/2026

Esecuzione automatica del task `sync-riepiloghi-push-github` da agent Cowork.

---

## 1. Analisi stato riepiloghi (Passo 0)

| Stato | Conteggio |
|---|---|
| ✅ Allineati | 15 |
| 🔄 Da sincronizzare | 1 |
| ⚠️ Naming non conforme | 1 |
| ❌ Mancanti (senza riepilogo conforme) | 8 |

### ✅ Allineati (15)

| Progetto | File sorgente |
|---|---|
| 100.STRATEGIA ORGANIZZAZIONE E FINANZA | `NUOVA STRATEGIA SETTEMBRE 2026 RIEPILOGO 10_04_2026.md` |
| 110.AUDIT INFRASTRUTTURA | `AUDIT INFRASTRUTTURA RIEPILOGO 04_04_2026.md` |
| 210.NAMING VICECONTI | `NAMING VICECONTI 25 02 2026.md` |
| 310.SAP ACADEMY | `SAP ACADEMY RIEPILOGO 13_02_2026.md` |
| 311.SAP SERVICE LAYER | `SAP SERVICE LAYER RIEPILOGO 05_05_2026.md` |
| 320.STRUMENTI DI CATTURA VOCALE | `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO_10_05_2026.md` |
| 350.n8n | `n8n_RIEPILOGO_22_04_2026.md` |
| 360.CALENDAR | `CALENDAR RIEPILOGO 22_04_2026.md` |
| 390.FastAPI | `FastAPI RIEPILOGO 17_05_2026.md` |
| 401.SINTESI ECOSISTEMA VICECONTI | `SINTESI ECOSISTEMA VICECONTI RIEPILOGO 14_05_2026.md` |
| 410.MEMORIA E CONTESTO AI | `MEMORIA E CONTESTO AI RIEPILOGO 16_05_2026.md` |
| 430.HUB DOCUMENTALE | `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` |
| 440.VICECONTI HUB | `RIEPILOGO SESSIONE 22_03_2026.md` |
| 460.DATABASE CENTRALIZZATO | `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md` |
| 480.ECOSISTEMA TEAM SYSTEM | `DIRECTORY_ANALYZER_RIEPILOGO_PROGETTO_04_05_2026.md` (naming sorgente non standard, contenuto allineato a 510) |

### 🔄 Sincronizzati in questa esecuzione (1)

| Progetto | File sorgente | Destinazione 510 |
|---|---|---|
| 200.AI HUMAN LAB | `AI HUMAN LAB RIEPILOGO 18_05_2026.md` | `AI HUMAN LAB RIEPILOGO.md` (sovrascritto) |

### ⚠️ Naming non conforme (1, NON sincronizzato)

| Progetto | File sorgente | Note |
|---|---|---|
| 005.ORGANIZZAZIONE PROGETTI CLAUDE | `MATERIALE_PER_SINTESI_v1.6_sessione_17_05_2026.md` | Il core name del file sorgente non corrisponde alla convenzione `[NOME PROGETTO] RIEPILOGO`. Verifica manuale richiesta: il file sembra essere materiale di preparazione, non un riepilogo finale. |

### ❌ Cartelle senza riepilogo conforme (8) — Da creare manualmente

| Progetto | Note |
|---|---|
| 220.FORMAZIONE IT | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 370.TELEGRAM | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 400.MANUALE DELLE AUTOMAZIONI | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 420.ASSISTENTE AMMINISTRATIVA | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 421.ASSISTENTE DI PROGETTO | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 450.INTERFACCIA SERVICE LAYER | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 470.ASSET PRODOTTI | Nessun file `RIEPILOGO`/`SINTESI` trovato |
| 490.PIPELINE PRESTASHOP | Nessun file `.md` di riepilogo nella cartella; in 510 esiste `PIPELINE PRESTASHOP RIEPILOGO.md` (storico, 456 byte) — non aggiornato in questa esecuzione |

---

## 2. Propagazione documenti master da 520 (Passo 1bis)

| File 520 | Azione | Esito |
|---|---|---|
| `ISTRUZIONI RIEPILOGO E NAMING.md` | Propagazione a `510/ISTRUZIONI_RIEPILOGO_E_NAMING.md` | Contenuto già allineato (skip) |
| `REPORT_SINCRONIZZAZIONE_17_05_2026.md` | Propagazione (era ultimo) | Già presente in 510, invariato |
| `BLOCCHI_PROMPT_PROGETTI.md` | Non propagare (privato in 520) | OK |
| `TODO_INTERVENTI_MANUALI.md` | Non propagare (privato in 520) | OK |
| `SCHEDULED_TASK_INSTRUCTIONS.md` | Non propagare (privato in 520) | OK |
| `00x.REPORT_SINCRONIZZAZIONE_*.md` (con prefisso) | Non propagare (archivio storico) | OK |

Nota tecnica: il file system mount FUSE della sandbox Linux ha mostrato un'anomalia di accesso ai file in 520 (visibili in `os.scandir` ma `open()` fallisce con ENOENT). La verifica del contenuto è stata effettuata via Read tool (path Windows) — i file in 510 risultano funzionalmente identici a quelli in 520, quindi la propagazione era già conclusa nel sync precedente del 17/05/2026.

---

## 3. Pulizia file obsoleti in 510 (Passo 2)

Scansione `.md` in `510.SINTESI_RIEPILOGHI_COWORK`: nessun file orfano trovato. Tutti i 20 file mappano a:
- Progetti attivi (17 riepiloghi di progetto incluso `NUOVA STRATEGIA SETTEMBRE 2026` per progetto 100)
- Documenti operativi: `ISTRUZIONI_RIEPILOGO_E_NAMING.md`, `NAMING VICECONTI.md`, `README_push_github.md`
- Ultimo REPORT: `REPORT_SINCRONIZZAZIONE_17_05_2026.md` (verrà sostituito dal report odierno dopo il push)

---

## 4. Push su GitHub Pages (Passo 3)

Script eseguito: `push_github.py` (replica Python di v2.1).
Repo: `viceconti-hub/contesto-ai` (branch `main`).

| Esito | Conteggio | File |
|---|---|---|
| 🆕 Creati | 0 | — |
| 🔄 Aggiornati | 1 | `AI HUMAN LAB RIEPILOGO.md` → `AI_HUMAN_LAB_RIEPILOGO.md` |
| ➖ Invariati (skip) | 19 | tutti gli altri .md |
| 🗑️ Eliminati | 0 | — |
| ❌ Errori push file | 0 | — |
| 📑 Indici aggiornati | 2 | `index.json`, `index.html` |
| ⚠️ Errori finali | 1 | Cleanup locale di `_last_push_tmp.json` fallito (Operation not permitted sul mount FUSE). Il file è già stato pushato e copiato in `last_push.json` correttamente; il `.tmp` resta locale ma non impatta il repo. |

`last_push.json` aggiornato a `18/05/2026 20:01`.

---

## 5. Note di esecuzione

- Esecuzione su sandbox Linux dell'agent Cowork — usata la replica Python `push_github.py` (PowerShell non disponibile).
- L'unica anomalia (mount FUSE per la cartella 520 e cleanup `_last_push_tmp.json`) non ha impatto sull'output GitHub: il push è riuscito.
- Il file orfano locale `_last_push_tmp.json` può essere rimosso manualmente in una sessione successiva o in Windows.

