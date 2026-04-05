# RIEPILOGO OPERATIVO — Sessione 3-4 Marzo 2026
## Per ripresa in nuova chat

*Generato il 04/03/2026*

---

## CONTESTO RAPIDO

Ecosistema IT Viceconti: SAP Business One → Query Engine → Viceconti Hub (dati SAP) + Hub Documentale (documenti Dropbox) → GitHub Pages → tecnici da smartphone. Tutti i componenti seguono pattern Jamstack: sorgente dati → script estrazione → JSON → sito statico.

---

## STATO ATTUALE HUB DOCUMENTALE

### Hub online — COMPLETO ✅

L'Hub Documentale è **completo e operativo** con tutte le 8 sezioni e 24.079 documenti. Prima scansione v3.0 completata con successo nella notte del 4 marzo 2026 (00:00-11:02, 11h 2m).

| Sezione | File | Stato |
|---------|------|-------|
| CLIENTI | 6.922 | ✅ |
| FORNITORI | 4.321 | ✅ |
| VENDITE | 5.495 | ✅ |
| ACQUISTI | 3.757 | ✅ |
| BANCHE E PAGAMENTI | 2.288 | ✅ |
| CREDITI E DEBITI | 333 | ✅ |
| VICECONTI SNC | 44 | ✅ |
| FILE TEMPORANEI | 919 | ✅ |
| **TOTALE** | **24.079** | **8 sezioni** |

**URL:** `https://viceconti-hub.github.io/hub-documentale/`

### Script — tutti alla v3.0

| Script | Versione | Funzione | Stato |
|--------|----------|----------|-------|
| `indexer.py` | 3.0 | Scansione completa notturna | ✅ Testato in produzione |
| `indexer_rapido.py` | 3.0 | Aggiornamento FILE TEMPORANEI | ✅ Aggiornato |
| `indexer_sezione.py` | 3.0 | Scansione singola sezione | ✅ Aggiornato |

---

## FIX IMPLEMENTATI IN v3.0

### Fix 1: Preservazione sezioni fallite (bug critico risolto)
**Problema:** quando una sezione crashava, il checkpoint successivo sovrascriveva il catalogo su GitHub senza quella sezione, cancellando dati buoni.
**Soluzione:** all'avvio, lo script scarica il catalogo da GitHub come `catalogo_base`. In scansione completa, pre-popola il catalogo con le sezioni esistenti. Se una sezione crasha, i dati vecchi restano al loro posto. Riepilogo finale con sezioni OK / preservate / fallite.

### Fix 2: Retry su errori di connessione
**Problema:** errori di rete (DNS failure, timeout, SSL EOF, connection reset) facevano crashare intere sezioni senza recupero.
**Soluzione:** funzione `_richiesta_con_retry()` applicata a tutte le chiamate HTTP (Dropbox e GitHub). Cattura `ConnectTimeout`, `ReadTimeout`, `ConnectionError`, `SSLError` con 3 tentativi e pause crescenti (15s, 30s, 60s).

### Fix 3: SHA refresh prima di ogni checkpoint
**Problema:** `indexer.py` salvava lo SHA del catalogo GitHub all'avvio. Quando `indexer_rapido.py` modificava il catalogo, lo SHA cambiava e i checkpoint successivi fallivano con errore 409.
**Soluzione:** `_github_get_sha()` è ora una funzione separata, chiamata immediatamente prima di ogni upload. Ogni script ottiene lo SHA fresco.

### Fix 4: Log rotation
**Problema:** file di log unico che cresceva indefinitamente, difficile da condividere e analizzare.
**Soluzione:** ogni esecuzione scrive in file separato con timestamp:
```
logs/indexer_2026-03-04_000002.log
logs/rapido/rapido_2026-03-04_090000.log
logs/sezione/sezione_CLIENTI_2026-03-04_143000.log
```

---

## AUTOMAZIONI ATTIVE

| Dove | Task | Script | Frequenza | Stato |
|------|------|--------|-----------|-------|
| Server SAP | SAP Query Engine | QueryEngine.ps1 | Ogni ora 08-20 | ✅ Funzionante |
| Server SAP | Update PrestaShop | (sync prezzi/qtà) | Ogni ora lun-ven | ✅ Funzionante |
| PC Lauria | Hub Doc Completa | indexer.py v3.0 | Ogni notte 00:00 | ✅ Funzionante |
| PC Lauria | Hub Doc Rapido | indexer_rapido.py v3.0 | Ogni 5 min 09:00-21:00 | ✅ Funzionante |

---

## LAVORO COMPLETATO IN QUESTA SESSIONE (3-4 marzo 2026)

- ✅ Analisi scansione notturna 03/03 (crash 4 sezioni su 5 per instabilità rete)
- ✅ Diagnosi definitiva dei 3 bug strutturali (preservazione, retry, SHA)
- ✅ **indexer.py v3.0** — riscrittura con tutti i fix
- ✅ **indexer_rapido.py v3.0** — aggiornato con retry, SHA refresh, log rotation
- ✅ **indexer_sezione.py v3.0** — aggiornato con retry, SHA refresh, log rotation
- ✅ Sostituzione script su PC Lauria
- ✅ Modifica orari Task Scheduler (indexer.py alle 00:00, rapido dalle 09:00)
- ✅ Prima scansione completa v3.0: 8/8 sezioni, 24.079 file, zero errori
- ✅ README Indicizzatori aggiornato a v3.0
- ✅ Riepilogo operativo aggiornato

### Sessioni precedenti (riepilogo)
- **25-26/02:** Whitelist estensioni, filtro cartelle software, checkpoint per sezione, convenzioni naming
- **28/02-01/03:** Diagnosi bug preservazione, scoperta flusso iPad, proposta log rotation e Cowork
- **02/03:** Upload manuale catalogo 6 sezioni, disattivazione temporanea indexer_rapido

---

## DOMANDE TECNICHE DISCUSSE

### Task Scheduler — dipendenze tra script
Task Scheduler di Windows non ha dipendenze native tra task. Soluzione pratica: file di lock creato da `indexer.py` all'avvio e controllato da `indexer_rapido.py`. Per ora risolto spostando l'avvio del rapido alle 09:00. Soluzione definitiva futura: n8n.

### Terminale invisibile per script schedulati
Task Scheduler con "Esegui anche se l'utente non è connesso" non mostra finestra. Per debug: cambiare temporaneamente in "Esegui solo se l'utente è connesso" oppure lanciare manualmente da terminale.

### Trasportabilità degli script
Principio: mai percorsi assoluti hardcoded. Usare `SCRIPT_DIR = Path(__file__).parent.resolve()` e riferire tutto relativamente. I nostri script già usano percorsi relativi; l'unica dipendenza è il campo "Inizio in" di Task Scheduler. Da applicare esplicitamente quando si riorganizzeranno le cartelle.

---

## VERSIONI E FILE

### PC Lauria — C:\Viceconti\archivio-indicizzatore\

| File | Versione | Stato |
|------|----------|-------|
| `indexer.py` | 3.0 | ✅ Testato in produzione |
| `indexer_sezione.py` | 3.0 | ✅ Aggiornato |
| `indexer_rapido.py` | 3.0 | ✅ Aggiornato |
| `.env` | — | `DROPBOX_SEZIONI_ATTIVE` vuoto (scansione completa) |
| `catalogo.json` | locale | 8 sezioni, 24.079 file |
| `logs/` | — | Log rotation attiva |

### GitHub — viceconti-hub/hub-documentale

| File | Stato |
|------|-------|
| `index.html` | ✅ Aggiornato con navbar |
| `viewer.html` | ✅ Vista isolata per condivisione |
| `catalogo.json` | ✅ Completo — 8 sezioni, 24.079 file |

### GitHub — viceconti-hub/portale

| File | Stato |
|------|-------|
| `index.html` | ✅ Aggiornato con navbar |
| `config.json` + `*.json` | ✅ Aggiornati dal Query Engine ogni ora |

---

## CREDENZIALI E SCADENZE

| Credenziale | Scadenza | Note |
|-------------|----------|------|
| Token GitHub | **9 maggio 2026** | Da rigenerare prima della scadenza |
| Token Dropbox (refresh) | Nessuna | OAuth2 permanente |
| App Secret Dropbox | Nessuna | Esposto durante config — rigenerare |

---

## ON THE HORIZON

### A breve termine
- **Lock file tra script** — meccanismo per evitare sovrapposizione indexer.py / indexer_rapido.py (ora mitigato dagli orari diversi, ma non risolto strutturalmente)
- **Pulizia automatica log** — script o logica per rimuovere log più vecchi di N giorni
- **Riorganizzazione cartelle script** — consolidare tutti gli script sotto `C:\Viceconti\` con percorsi relativi espliciti (`SCRIPT_DIR`)

### A medio termine
- **Cowork come supervisore** — Claude Desktop analizza log ogni mattina e produce report diagnostico
- **Prototipo tool AI per catalogo** — interrogazione intelligente di catalogo.json in linguaggio naturale (test completato con successo il 24/02)
- **Integrazione Telegram** — tecnico manda messaggio al bot, assistente risponde con documenti trovati
- **Service Layer SAP** — sblocca scrittura su SAP (in attesa S-user + sessione consulente)
- **n8n** — sostituzione Task Scheduler con flussi intelligenti

---

## LEZIONI APPRESE (aggiornate)

1. **Mai sovrascrivere dati buoni con assenza di dati** — principio fondamentale, ora implementato in indexer.py v3.0
2. **Quattro tipi di errore di rete diversi in una notte** — DNS failure, connection reset, read timeout, SSL EOF. Serve gestirli tutti, non solo il rate limit 429
3. **SHA refresh prima di ogni upload** — lo SHA salvato all'avvio diventa stale in poche ore quando altri script modificano lo stesso file
4. **Log rotation è prerequisito per diagnostica automatica** — file singoli per esecuzione abilitano analisi, condivisione, e futura automazione con Cowork
5. **L'infrastruttura abilita flussi non previsti** — Hub Documentale + Dropbox Markup + iPad = workflow di annotazione sul campo senza una riga di codice aggiuntiva
6. **Trasportabilità degli script** — percorsi relativi + `SCRIPT_DIR` rendono gli script indipendenti dalla posizione sul filesystem

---

*Usare questo documento come contesto operativo nella prossima chat dedicata all'ecosistema IT Viceconti.*
