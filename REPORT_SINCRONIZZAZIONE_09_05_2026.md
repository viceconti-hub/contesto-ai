# Report Sincronizzazione — 09/05/2026

Esecuzione automatica del task **sync-riepiloghi-push-github**.

## Riepilogo operazioni

| Operazione                               | Numero |
|------------------------------------------|--------|
| File copiati in SINTESI                  | 4      |
| File creati su GitHub                    | 1      |
| File aggiornati su GitHub                | 4      |
| File invariati su GitHub                 | 18     |
| File eliminati da GitHub                 | 0      |
| Errori                                   | 0      |

## Passo 0 — Stato riepiloghi (analisi iniziale)

### Allineati (10)

- 100.STRATEGIA ORGANIZZAZIONE E FINANZA — `NUOVA STRATEGIA SETTEMBRE 2026 RIEPILOGO 10_04_2026.md`
- 110.AUDIT INFRASTRUTTURA — `AUDIT INFRASTRUTTURA RIEPILOGO 04_04_2026.md`
- 200.AI HUMAN LAB — `AI HUMAN LAB RIEPILOGO 03_04_2026.md`
- 210.NAMING VICECONTI — `NAMING VICECONTI RIEPILOGO 25_02_2026.md`
- 310.SAP ACADEMY — `SAP ACADEMY RIEPILOGO 13_02_2026.md`
- 320.STRUMENTI DI CATTURA VOCALE — `STRUMENTI DI CATTURA VOCALE RIEPILOGO 26_03_2026.md`
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md`
- 360.CALENDAR — `CALENDAR RIEPILOGO 22_04_2026.md`
- 410.CONTESTO AI — `MEMORIA E CONTESTO AI RIEPILOGO 06_04_2026.txt`
- 430.HUB DOCUMENTALE — `SITI WEB RIEPILOGO 04_03_2026.md` (già allineato in SINTESI)
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md`

### Sincronizzati in questa esecuzione (4)

- **311.SAP SERVICE LAYER** — `SAP SERVICE LAYER RIEPILOGO 05_05_2026.md` → `SAP SERVICE LAYER RIEPILOGO.md` (contenuto aggiornato)
- **390.FastAPI** — `FastAPI RIEPILOGO 05_05_2026.md` → `FastAPI RIEPILOGO.md` (NUOVO file in SINTESI)
- **401.SINTESI ECOSISTEMA VICECONTI** — `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_05_05_2026.md` → `SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md` (contenuto aggiornato)
- **520.PROGETTO COWORK SINTESI RIEPILOGO** — `ISTRUZIONI RIEPILOGO E NAMING.md` → `ISTRUZIONI_RIEPILOGO_E_NAMING.md` (contenuto aggiornato dal master)

### Naming non conforme (segnalazioni — non bloccanti)

I file seguenti sono il riepilogo più recente nelle rispettive cartelle ma non rispettano la convenzione `[NOME PROGETTO] RIEPILOGO GG_MM_AAAA.md`:

- 310.SAP ACADEMY — `SAP SERVICE LAYER RIEPILOGO CHAT COMPLETA 15_02_2026.txt` (nome progetto errato + parola superflua + .txt). Sincronizzato comunque il file conforme `SAP ACADEMY RIEPILOGO 13_02_2026.md`, già allineato.
- 350.n8n — `n8n_RIEPILOGO_22_04_2026.md` (underscore invece di spazi)
- 401.SINTESI ECOSISTEMA VICECONTI — `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_05_05_2026.md` (underscore invece di spazi)
- 410.CONTESTO AI — `MEMORIA E CONTESTO AI RIEPILOGO 06_04_2026.txt` (formato .txt invece di .md)
- 430.HUB DOCUMENTALE — `HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md` (underscore + parola "Operativo" superflua + keyword non maiuscola)
- 460.DATABASE CENTRALIZZATO — `DATABASE_CENTRALIZZATO_RIEPILOGO_19_04_2026.md` (underscore invece di spazi)
- 440.VICECONTI HUB — `RIEPILOGO SESSIONE 22_03_2026.md` (manca il nome progetto, keyword in testa, parola "SESSIONE" superflua). **NON sincronizzato**: senza nome progetto nel file non è possibile derivare il nome SINTESI con sicurezza.
- 480.ECOSISTEMA TEAM SYSTEM — `DIRECTORY_ANALYZER_RIEPILOGO_PROGETTO_04_05_2026.md` (nome progetto errato + parola "PROGETTO" superflua). **NON sincronizzato**: stesso motivo.

### Cartelle senza riepilogo (9)

Nessun file con keyword `RIEPILOGO` o `SINTESI` (.md o .txt) trovato:

- 1000 SIMBOLI
- 220.FORMAZIONE IT
- 370.TELEGRAM
- 400.MANUALE DELLE AUTOMAZIONI
- 420.ASSISTENTE AMMINISTRATIVA
- 421.ASSISTENTE DI PROGETTO
- 450.INTERFACCIA SERVICE LAYER
- 470.ASSET PRODOTTI
- 490.PIPELINE PRESTASHOP (esistono solo .docx in cartella; in SINTESI è presente un file placeholder)

## Passo 2 — File potenzialmente obsoleti in SINTESI

I file seguenti esistono in SINTESI ma non corrispondono ad alcuna cartella progetto attiva. **Non sono stati rimossi** (richiede conferma utente, assente nell'esecuzione automatica):

- `ASSISTENTI INTELLIGENTI RIEPILOGO.md` — nessuna cartella progetto con questo nome (potrebbe essere correlato a 420 / 421)
- `SINTESI ECOSISTEMA VICECONTI SINTESI.md` — duplicato di `SINTESI ECOSISTEMA VICECONTI RIEPILOGO.md` (contenuto era identico prima della sincronizzazione corrente)

## Passo 3 — Push GitHub Pages

Esecuzione equivalente di `push_github.ps1` (port Python in sandbox Linux, stessa logica REST API):

- **Repo**: viceconti-hub/contesto-ai (branch main)
- **File creati**: 1 (`FastAPI_RIEPILOGO.md`)
- **File aggiornati**: 4 (`ISTRUZIONI_RIEPILOGO_E_NAMING.md`, `SAP_SERVICE_LAYER_RIEPILOGO.md`, `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md`, `last_push.json`)
- **File invariati**: 18
- **File eliminati**: 0
- **Errori**: 0
- **Log**: `logs/push_github_2026-05-09_063112.log`

## Note

- L'esecuzione PowerShell nativa non è stata possibile dal sandbox Linux dell'agent. È stata eseguita una replica fedele in Python che usa la stessa REST API GitHub e produce lo stesso log nella cartella `logs/`. L'esito netto sul repo è equivalente.
- Si consiglia di intervenire manualmente sui naming non conformi (specialmente 440.VICECONTI HUB e 480.ECOSISTEMA TEAM SYSTEM, dove manca il nome progetto nel file e la sincronizzazione è quindi stata saltata).
- Si consiglia di valutare la rimozione dei due file potenzialmente obsoleti in SINTESI elencati al Passo 2.
