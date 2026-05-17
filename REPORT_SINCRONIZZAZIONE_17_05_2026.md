# Report Sincronizzazione — 17/05/2026

Esecuzione del task **sync-riepiloghi-push-github** con interventi addizionali manuali (refactor `index.json` arricchito + nuovo Passo 1ter nelle ISTRUZIONI).

## Riepilogo operazioni

| Operazione                               | Numero |
|------------------------------------------|--------|
| File copiati in SINTESI (510)            | 5      |
| File creati su GitHub                    | 1      |
| File aggiornati su GitHub                | 6      |
| File invariati su GitHub                 | 11     |
| File eliminati da GitHub                 | 1      |
| File eliminati da 510 (obsoleti locali)  | 2      |
| Errori                                   | 0 (1 warning non bloccante) |

## Passo 0 — Stato riepiloghi (analisi iniziale)

### Allineati (7)

- 100.STRATEGIA ORGANIZZAZIONE E FINANZA — `NUOVA STRATEGIA SETTEMBRE 2026 RIEPILOGO 10_04_2026.md`
- 110.AUDIT INFRASTRUTTURA — `AUDIT INFRASTRUTTURA RIEPILOGO 04_04_2026.md`
- 210.NAMING VICECONTI — `NAMING VICECONTI RIEPILOGO 25_02_2026.md`
- 310.SAP ACADEMY — `SAP ACADEMY RIEPILOGO 13_02_2026.md`
- 311.SAP SERVICE LAYER — `SAP SERVICE LAYER RIEPILOGO 05_05_2026.md`
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md`
- 360.CALENDAR — `CALENDAR RIEPILOGO 22_04_2026.md`
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md`

### Sincronizzati in questa esecuzione (5)

- **200.AI HUMAN LAB** — `AI_HUMAN_LAB_RIEPILOGO_10_05_2026.md` → `AI HUMAN LAB RIEPILOGO.md` (contenuto aggiornato)
- **320.STRUMENTI DI CATTURA VOCALE** — `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO_10_05_2026.md` → `STRUMENTI DI CATTURA VOCALE RIEPILOGO.md` (contenuto aggiornato)
- **390.FastAPI** — `FastAPI RIEPILOGO 07_05_2026.md` → `FastAPI RIEPILOGO.md` (contenuto aggiornato)
- **401.SINTESI ECOSISTEMA VICECONTI** — `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_10_05_2026.md` → `SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md` (contenuto aggiornato)
- **410.CONTESTO AI** — `MEMORIA E CONTESTO AI RIEPILOGO 09_05_2026.md` → `MEMORIA E CONTESTO AI RIEPILOGO.md` (contenuto aggiornato; precedente in 510 era versione `.txt` del 06_04)

### Naming non conforme (segnalazioni — non bloccanti)

- 200.AI HUMAN LAB — `AI_HUMAN_LAB_RIEPILOGO_10_05_2026.md` (underscore invece di spazi)
- 310.SAP ACADEMY — `SAP SERVICE LAYER RIEPILOGO CHAT COMPLETA 15_02_2026.txt` (nome progetto errato + parola superflua + .txt). Sincronizzato comunque il file conforme `SAP ACADEMY RIEPILOGO 13_02_2026.md`, già allineato.
- 320.STRUMENTI DI CATTURA VOCALE — `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO_10_05_2026.md` (underscore)
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md` (underscore)
- 401.SINTESI ECOSISTEMA VICECONTI — `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_10_05_2026.md` (underscore)
- 430.HUB DOCUMENTALE — `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` (underscore + parola "Operativo" superflua + keyword non maiuscola). **NON sincronizzato**: il file in 510 era già un placeholder (`SITI WEB RIEPILOGO.md` non più presente).
- 440.VICECONTI HUB — `RIEPILOGO SESSIONE 22_03_2026.md` (manca il nome progetto). **NON sincronizzato**.
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md` (underscore)
- 480.ECOSISTEMA TEAM SYSTEM — `DIRECTORY_ANALYZER_RIEPILOGO_PROGETTO_04_05_2026.md` (nome progetto errato). **NON sincronizzato**.

### Cartelle senza riepilogo conforme (10)

- 1000 SIMBOLI
- 220.FORMAZIONE IT
- 370.TELEGRAM
- 400.MANUALE DELLE AUTOMAZIONI
- 420.ASSISTENTE AMMINISTRATIVA
- 421.ASSISTENTE DI PROGETTO
- 450.INTERFACCIA SERVICE LAYER
- 470.ASSET PRODOTTI
- 490.PIPELINE PRESTASHOP (solo placeholder in 510)
- ORGANIZZAZIONE PROGETTI CLAUDE (cartella di riferimento/metodo)

## Passo 1bis — Propagazione master da 520

- `ISTRUZIONI RIEPILOGO E NAMING.md` → `ISTRUZIONI_RIEPILOGO_E_NAMING.md` in 510: **AGGIORNATO** (aggiunto nuovo Passo 1ter sulla generazione di `index.json`, su istruzione dell'utente).
- Back-fill in 520 del `REPORT_SINCRONIZZAZIONE_09_05_2026.md` (il run precedente non aveva salvato il proprio report in 520 — corretta la lacuna copiandolo da 510).

## Passo 1ter — Generazione e mantenimento di index.json

Aggiornata struttura arricchita di `index.json` con i seguenti campi:

- Per ogni entry: `slug` (con shortening per nomi lunghi: `sintesi`, `contesto_ai`, `strategia_settembre_2026`, `strumenti_cattura_vocale`)
- `riepiloghi_progetti` + `sintesi_trasversale`: aggiunto `fastapi_url` (`<base>/contesto/<slug>/latest`)
- `documenti_operativi`: aggiunto `esposto_fastapi` (true|false) e `fastapi_url` quando esposto
  - `ISTRUZIONI_RIEPILOGO_E_NAMING.md` → esposto via FastAPI
  - `NAMING_VICECONTI.md` → esposto via FastAPI
  - `README_push_github.md` → non esposto
  - `REPORT_SINCRONIZZAZIONE_*.md` → non esposto
- Aggiunta entry `README PUSH GITHUB` come documento operativo
- 12 progetti totali in `riepiloghi_progetti` (escluso SINTESI che è in `sintesi_trasversale` e NAMING/ISTRUZIONI/README/REPORT che sono in `documenti_operativi`)

## Passo 2 — Pulizia file obsoleti in 510

File rimossi da 510:

- `REPORT_SINCRONIZZAZIONE_22_04_2026.md` (obsoleto, era stato erroneamente propagato e ora superato dal 09_05)
- `_last_push_tmp.json` (residuo temporaneo dello script v2.1 — vedi warning sotto)

## Passo 3 — Push GitHub Pages

Eseguito `push_github.py` v2.1 dalla sandbox Linux:

- **Repo**: viceconti-hub/contesto-ai (branch `main`)
- **File creati su GitHub**: 1 (`SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md`)
- **File aggiornati su GitHub**: 6
  - `AI_HUMAN_LAB_RIEPILOGO.md`
  - `FastAPI_RIEPILOGO.md`
  - `ISTRUZIONI_RIEPILOGO_E_NAMING.md`
  - `MEMORIA_E_CONTESTO_AI_RIEPILOGO.md`
  - `STRUMENTI_DI_CATTURA_VOCALE_RIEPILOGO.md`
  - `index.html`
- **File invariati su GitHub**: 11
- **File eliminati da GitHub**: 1 (`SINTESI_2026_05_14.md` — versione manifest precedente, ora sostituita da `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md` con nome stabile)
- **Errori**: 0 bloccanti

### Secondo push manuale dopo lo script

Lo script v2.1 genera un `index.json` con schema minimale (solo nome/file/url/tipo/data). Per mantenere lo schema arricchito definito al Passo 1ter (slug, fastapi_url, esposto_fastapi), è stato eseguito un secondo push manuale via REST API GitHub che sovrascrive `index.json` con la versione completa.

- `index.json` arricchito → ripushato su GitHub (commit `42686d83`)

### Warning non bloccante

Lo script v2.1 ha terminato con un `PermissionError` su `_last_push_tmp.json.unlink()` (eredità del sandbox Linux che non supporta rimozione di file su cowork-mount senza autorizzazione esplicita). Tutti gli step funzionali erano già completati al momento dell'errore. Il file temporaneo è stato rimosso manualmente. Non risulta aggiornato `last_push.json` (lo script falliva nel rename → cleanup); valutare patch dello script per gestire il fallback.

## Passo 4 — Generazione report finale

- Report salvato in **520** come `REPORT_SINCRONIZZAZIONE_17_05_2026.md` (questo file).
- Report propagato in **510** con stesso nome.
- Secondo push limitato al solo nuovo REPORT.

## Cartelle senza riepilogo conforme (da creare manualmente)

Per le cartelle elencate al Passo 0 nella sezione "Cartelle senza riepilogo conforme" (10 cartelle), va valutata la creazione di un primo riepilogo conforme al naming standard `[NOME PROGETTO] RIEPILOGO GG_MM_AAAA.md`.

## Note operative

- Lo script `push_github.py` v2.1 attualmente genera un `index.json` minimale. Per riallineare lo script al nuovo schema arricchito (Passo 1ter), serve patch della funzione `build_index_json()` in `push_github.py`.
- I file con naming non conforme nelle cartelle di progetto andrebbero gradualmente rinominati per allinearsi alla convenzione (vedi Sezione 2 di ISTRUZIONI). Il sync attuale gestisce la divergenza ma è meglio normalizzare alla fonte.
- La cartella `1000 SIMBOLI` non rispetta il pattern di esclusione `1000.*` perché non ha il punto: valutare se aggiungerla agli esclusi o rinominarla in `1000.SIMBOLI`.
