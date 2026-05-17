# Report Sincronizzazione — 17/05/2026

Esecuzione del task **sync-riepiloghi-push-github** (run pomeridiano, scheduled task automatico).

## Riepilogo operazioni

| Operazione                               | Numero |
|------------------------------------------|--------|
| File copiati in SINTESI (510)            | 4      |
| File creati su GitHub                    | 3      |
| File aggiornati su GitHub                | 4      |
| File invariati su GitHub                 | 15     |
| File eliminati da GitHub                 | 0      |
| File eliminati da 510 (obsoleti locali)  | 0      |
| Errori                                   | 0 (1 warning non bloccante) |

## Passo 0 — Stato riepiloghi (analisi iniziale)

### Allineati (12)

- 100.STRATEGIA ORGANIZZAZIONE E FINANZA — `NUOVA STRATEGIA SETTEMBRE 2026 RIEPILOGO 10_04_2026.md`
- 110.AUDIT INFRASTRUTTURA — `AUDIT INFRASTRUTTURA RIEPILOGO 04_04_2026.md`
- 200.AI HUMAN LAB — `AI_HUMAN_LAB_RIEPILOGO_10_05_2026.md`
- 210.NAMING VICECONTI — `NAMING VICECONTI RIEPILOGO 25_02_2026.md`
- 310.SAP ACADEMY — `SAP ACADEMY RIEPILOGO 13_02_2026.md`
- 311.SAP SERVICE LAYER — `SAP SERVICE LAYER RIEPILOGO 05_05_2026.md`
- 320.STRUMENTI DI CATTURA VOCALE — `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO_10_05_2026.md`
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md`
- 360.CALENDAR — `CALENDAR RIEPILOGO 22_04_2026.md`
- 390.FastAPI — `FastAPI RIEPILOGO 17_05_2026.md`
- 401.SINTESI ECOSISTEMA VICECONTI — `SINTESI ECOSISTEMA VICECONTI RIEPILOGO 14_05_2026.md`
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md`

### Sincronizzati in questa esecuzione (4)

- **410.MEMORIA E CONTESTO AI** — `MEMORIA E CONTESTO AI RIEPILOGO 16_05_2026.md` → `MEMORIA E CONTESTO AI RIEPILOGO.md` (contenuto aggiornato; precedente in 510 era versione del 09_05)
- **430.HUB DOCUMENTALE** — `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` → `HUB DOCUMENTALE RIEPILOGO.md` (creato; assente in 510 prima di questo run)
- **440.VICECONTI HUB** — `RIEPILOGO SESSIONE 22_03_2026.md` → `VICECONTI HUB RIEPILOGO.md` (creato; assente in 510 prima di questo run)
- **480.ECOSISTEMA TEAM SYSTEM** — `DIRECTORY_ANALYZER_RIEPILOGO_PROGETTO_04_05_2026.md` → `ECOSISTEMA TEAM SYSTEM RIEPILOGO.md` (creato; assente in 510 prima di questo run)

### Naming non conforme (segnalazioni — non bloccanti)

- 005.ORGANIZZAZIONE PROGETTI CLAUDE — `PUSH_ISTRUZIONI_17_05_2026_Wave2_ConRiepilogo.md` / `Wave3_SenzaRiepilogo.md` (file di istruzioni di push, NON riepiloghi di progetto; keyword "RIEPILOGO" presente solo come substring di "ConRiepilogo"). **NON sincronizzato** — cartella di metodo, non progetto attivo.
- 200.AI HUMAN LAB — `AI_HUMAN_LAB_RIEPILOGO_10_05_2026.md` (underscore invece di spazi)
- 310.SAP ACADEMY — `SAP SERVICE LAYER RIEPILOGO CHAT COMPLETA 15_02_2026.txt` (nome progetto errato + parola superflua + .txt). Sincronizzato comunque il file conforme `SAP ACADEMY RIEPILOGO 13_02_2026.md`.
- 311.SAP SERVICE LAYER — molti file `Riepilogo_Operativo_*.md` con date trattino (vedi Sezione 2 di ISTRUZIONI)
- 320.STRUMENTI DI CATTURA VOCALE — `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO_10_05_2026.md` (underscore)
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md` (underscore)
- 390.FastAPI — diversi file `RIEPILOGO_CAP_*_PER_*_16_05_2026.md` (sotto-riepiloghi di capitolo, naming non standard)
- 401.SINTESI ECOSISTEMA VICECONTI — `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_*_2026.md` (underscore)
- 410.MEMORIA E CONTESTO AI — `MEMORIA E CONTESTO AI RIEPILOGO 16_05_2026.md` (file più recente conforme — sincronizzato)
- 430.HUB DOCUMENTALE — `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` (underscore + parola "Operativo" superflua + keyword non maiuscola). **Sincronizzato comunque** in questo run perché unico candidato.
- 440.VICECONTI HUB — `RIEPILOGO SESSIONE 22_03_2026.md` (manca il nome progetto). **Sincronizzato comunque** in questo run perché unico candidato.
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md` (underscore)
- 480.ECOSISTEMA TEAM SYSTEM — `DIRECTORY_ANALYZER_RIEPILOGO_PROGETTO_04_05_2026.md` (nome progetto errato: DIRECTORY_ANALYZER è un sotto-componente). **Sincronizzato comunque** in questo run perché unico candidato.

### Cartelle senza riepilogo conforme (9)

- 005.ORGANIZZAZIONE PROGETTI CLAUDE (cartella di metodo; vedi sopra)
- 220.FORMAZIONE IT
- 370.TELEGRAM
- 400.MANUALE DELLE AUTOMAZIONI
- 420.ASSISTENTE AMMINISTRATIVA
- 421.ASSISTENTE DI PROGETTO
- 450.INTERFACCIA SERVICE LAYER
- 470.ASSET PRODOTTI
- 490.PIPELINE PRESTASHOP (solo placeholder esistente in 510)

## Passo 1bis — Propagazione master da 520

- `ISTRUZIONI RIEPILOGO E NAMING.md` → `ISTRUZIONI_RIEPILOGO_E_NAMING.md` in 510: **INVARIATO** (contenuto identico al precedente run del 17/05/2026 mattina, già propagato).
- `REPORT_SINCRONIZZAZIONE_17_05_2026.md`: il REPORT del run precedente del 17/05 era già propagato in 510. Questo run sovrascrive il REPORT in 520 e in 510 con il contenuto aggiornato del run pomeridiano.

**Nota tecnica**: in questo run la cartella 520 è risultata non leggibile via shell sandbox Linux (l'ambiente bash non riesce ad aprire i file della cartella 520, pur essendo visibili in `ls`). Il controllo del contenuto è stato eseguito tramite il file-API (Read tool), che usa un canale di accesso diverso. La scrittura del nuovo REPORT in 520 funziona regolarmente.

## Passo 2 — Pulizia file obsoleti in 510

Nessun file orfano rilevato. Tutti i `.md` in 510 corrispondono a:
- un progetto attivo (riepilogo o placeholder)
- un documento propagato da 520 (ISTRUZIONI, REPORT)
- un file di sistema (README_push_github, index.html, index.json, last_push.json, push_github.*)

Nessun `REPORT_SINCRONIZZAZIONE_*.md` obsoleto da rimuovere (in 510 era già presente solo il `17_05_2026`, stessa data del nuovo REPORT).

## Passo 3 — Push GitHub Pages

Eseguito `push_github.py` v2.1 dalla sandbox Linux:

- **Repo**: viceconti-hub/contesto-ai (branch `main`)
- **File creati su GitHub**: 3
  - `ECOSISTEMA_TEAM_SYSTEM_RIEPILOGO.md`
  - `HUB_DOCUMENTALE_RIEPILOGO.md`
  - `VICECONTI_HUB_RIEPILOGO.md`
- **File aggiornati su GitHub**: 4
  - `MEMORIA_E_CONTESTO_AI_RIEPILOGO.md`
  - `index.json`
  - `index.html`
  - `last_push.json`
- **File invariati su GitHub**: 15
- **File eliminati da GitHub**: 0
- **Errori bloccanti**: 0

### Warning non bloccante (ricorrente)

Lo script v2.1 termina con `PermissionError` su `_last_push_tmp.json.unlink()` (la sandbox Linux non permette `unlink` su mount cowork). Tutti gli step funzionali risultano già completati al momento dell'errore. Il file `_last_push_tmp.json` resta in 510 anche dopo questo run (non rimovibile via bash; va eliminato manualmente dall'utente o da uno script che gira fuori sandbox). Da valutare patch dello script per gestire il fallback (es. try/except con log, o spostamento del temp file in `/tmp`).

## Passo 4 — Generazione report finale

- Report salvato in **520** come `REPORT_SINCRONIZZAZIONE_17_05_2026.md` (questo file — sovrascrive la versione mattutina dello stesso giorno).
- Report propagato in **510** con stesso nome.
- Secondo push limitato al solo nuovo REPORT.

## Note operative

- I 3 nuovi riepiloghi pubblicati (430 HUB DOCUMENTALE, 440 VICECONTI HUB, 480 ECOSISTEMA TEAM SYSTEM) sono stati sincronizzati nonostante il loro naming non conforme, perché erano l'unico candidato disponibile. Per allinearsi alla convenzione di Sezione 2 di ISTRUZIONI, andrebbero rinominati alla fonte:
  - 430: `HUB DOCUMENTALE RIEPILOGO 04_03_2026.md`
  - 440: `VICECONTI HUB RIEPILOGO 22_03_2026.md`
  - 480: `ECOSISTEMA TEAM SYSTEM RIEPILOGO 04_05_2026.md` (o nome aderente al sotto-componente effettivamente coperto)
- Le 9 cartelle senza riepilogo conforme (vedi Passo 0) attendono un primo riepilogo conforme creato in una sessione operativa.
- Residuo `_last_push_tmp.json` in 510 e file di test `_write_test_*.txt` in 520 lasciati dal sandbox a causa del limite `Operation not permitted` su `unl