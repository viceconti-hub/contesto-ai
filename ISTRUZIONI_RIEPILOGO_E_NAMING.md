# Istruzioni per la creazione e gestione dei file di riepilogo

*Ultimo aggiornamento: 17/05/2026 — allineamento post Option B (Cap. V FastAPI) e a Mappatura Progetti Claude ↔ Cartelle Drive del 17/05/2026.*

Questo documento è lo standard operativo per la creazione dei file di riepilogo nei progetti Claude/Viceconti. Deve essere seguito da Claude in ogni sessione di lavoro che produce un riepilogo, e dalla procedura di sincronizzazione che raccoglie i riepiloghi nella cartella centralizzata.

---

## 1. Tipi di documento

Esistono due tipi di documento con naming dedicato:

### RIEPILOGO (riepilogo operativo di progetto)
Documento che fotografa lo stato di avanzamento di un singolo progetto alla data indicata. Contiene: cosa è stato fatto, cosa resta da fare, decisioni prese, blocchi aperti.

Viene prodotto da Claude alla fine di ogni sessione di lavoro significativa su un progetto.

### SINTESI (sintesi trasversale)
Documento che aggrega e sintetizza i riepiloghi di più progetti per dare una visione d'insieme. È riservato alla cartella SINTESI ECOSISTEMA VICECONTI e prodotto periodicamente.

---

## 2. Convenzione di naming

### Schema generale

```
[NOME PROGETTO] RIEPILOGO [GG_MM_AAAA].md
```

### Regole

- **Formato file:** sempre `.md` (markdown). Se serve una copia in altro formato (.docx, .pdf), si genera a parte ma il file master resta .md
- **Nome progetto:** deve corrispondere esattamente al nome della cartella del progetto in PROGETTI CLAUDE
- **Keyword:** `RIEPILOGO` (maiuscolo) per i riepiloghi operativi, `SINTESI` (maiuscolo) per le sintesi trasversali
- **Data:** formato `GG_MM_AAAA` con underscore come separatore (es. `17_05_2026`)
- **La data si riferisce** alla data della sessione di lavoro, non alla data di creazione del file
- **Separatore** tra le parti del nome: spazio semplice (non underscore, non trattino)
- **Nessun altro testo** tra il nome del progetto e la keyword (no "operativo", no "aggiornamento", no "ripresa")

### Esempi conformi

```
SAP SERVICE LAYER RIEPILOGO 17_05_2026.md
AI HUMAN LAB RIEPILOGO 15_05_2026.md
DATABASE CENTRALIZZATO RIEPILOGO 28_04_2026.md
STRUMENTI DI CATTURA VOCALE RIEPILOGO 15_05_2026.md
AUDIT INFRASTRUTTURA RIEPILOGO 16_05_2026.md
FastAPI RIEPILOGO 17_05_2026.md
n8n RIEPILOGO 17_02_2026.md
SINTESI ECOSISTEMA VICECONTI SINTESI 14_05_2026.md
```

### Esempi NON conformi (da evitare)

```
SAP SERVICE LAYER Riepilogo_Operativo_15-05-2026.md     → keyword non maiuscola, "Operativo" superfluo, trattini nella data
AI HUMAN LAB Riepilogo operativo 15_05_2026.txt          → formato .txt, "operativo" superfluo, keyword non maiuscola
RIEPILOGO OPERATIVO DATABASE CENTRALIZZATO RIPRESA.txt   → keyword in testa, "OPERATIVO" e "RIPRESA" superflui, formato .txt
Contesto_Manutenzione_Infrastruttura.md                  → manca la keyword RIEPILOGO, manca la data
n8n stato progetto al 17_02_2026.txt                     → manca la keyword, formato .txt
```

---

## 3. Struttura interna del file di riepilogo

Ogni file RIEPILOGO deve contenere almeno queste sezioni:

```markdown
# [Nome Progetto] — Riepilogo del [data leggibile]

## Stato attuale
[Descrizione sintetica di dove si trova il progetto]

## Lavoro svolto in questa sessione
[Elenco delle attività completate]

## Decisioni prese
[Scelte fatte durante la sessione, con motivazione]

## Prossimi passi
[Cosa resta da fare, in ordine di priorità]

## Blocchi o dipendenze
[Eventuali ostacoli, attese da terzi, prerequisiti non ancora risolti]
```

Le sezioni "Decisioni prese" e "Blocchi o dipendenze" possono essere omesse se non applicabili alla sessione.

---

## 4. Istruzioni per Claude

Quando lavori su un progetto in PROGETTI CLAUDE e la sessione produce risultati significativi:

1. **Alla fine della sessione**, proponi la creazione di un file di riepilogo
2. **Usa esattamente** il naming descritto sopra: `[NOME CARTELLA PROGETTO] RIEPILOGO [GG_MM_AAAA].md`
3. **Salva il file** nella cartella del progetto corrispondente
4. **Non rinominare** i riepiloghi precedenti: ogni sessione produce un file nuovo con la sua data
5. **Formato .md** sempre, senza eccezioni per il file master

---

## 5. Regole per la sincronizzazione nella cartella centralizzata

La cartella `510.SINTESI_RIEPILOGHI_COWORK` raccoglie una copia del riepilogo più recente di ogni progetto attivo.

### Logica di selezione
Per ogni cartella in PROGETTI CLAUDE (esclusa 510 stessa):
1. Cerca file che contengono la keyword `RIEPILOGO` o `SINTESI` nel nome
2. Tra quelli trovati, prendi quello con la data più recente nel nome (formato GG_MM_AAAA)
3. Se non esiste nessun file con la keyword, segnala la cartella nel report come "senza riepilogo"
4. Se la cartella è vuota, segnala nel report come "cartella vuota"

### Comportamento della copia
- Il file viene copiato in `510.SINTESI_RIEPILOGHI_COWORK` con il nome SENZA la data: `[NOME PROGETTO] RIEPILOGO.md`
- Se nella destinazione esiste già un file dello stesso progetto, il vecchio viene sostituito dal nuovo
- Ad ogni esecuzione viene generato un file `REPORT_SINCRONIZZAZIONE_[GG_MM_AAAA].md` (vedi sezione 6 per la gestione)

---

## 6. Cartella di lavoro del sistema sync (520)

La cartella `520.SINTESI RIEPILOGHI COWORK CARTELLA DI LAVORO` è la **cartella di lavoro/master** del sistema di sincronizzazione stesso. Si comporta come una cartella di progetto, con alcune specificità.

### Contenuto e regole di propagazione verso 510

| File / pattern | Propagazione in 510 | Note |
|---|---|---|
| `ISTRUZIONI RIEPILOGO E NAMING.md` | Sì, come `ISTRUZIONI_RIEPILOGO_E_NAMING.md` (invariato nel contenuto) | Documento operativo master |
| `REPORT_SINCRONIZZAZIONE_GG_MM_AAAA.md` (più recente) | Sì, **mantenendo la data nel nome** | Solo l'ultimo viene pubblicato; i precedenti restano qui come archivio |
| `BLOCCHI_PROMPT_PROGETTI.md` | No, privato in 520 | Documento di lavoro |
| `TODO_INTERVENTI_MANUALI.md` | No, privato in 520 | Documento di lavoro |
| `00x.REPORT_SINCRONIZZAZIONE_*.md` (con prefisso numerico) | No, archivio storico | File con prefisso numerico = archivio, non propagati |

### Generazione del REPORT_SINCRONIZZAZIONE

- Il task schedulato `sync-riepiloghi-push-github` **scrive il nuovo REPORT direttamente in 520** (la fonte), non in 510.
- Subito dopo, lo stesso task **propaga il REPORT più recente in 510** mantenendo la data nel nome.
- Il REPORT precedente in 510 (con data diversa) viene rimosso al passo di sync delete.

### Differenza chiave rispetto agli altri progetti

Per i riepiloghi di progetto: il file in 510 ha SEMPRE lo stesso nome (`[NOME PROGETTO] RIEPILOGO.md`, senza data) e il contenuto cambia ad ogni sync.
Per i REPORT di sincronizzazione: ogni esecuzione genera un nuovo nome con data, e in 510 si mantiene **solo l'ultimo**.

---

## 7. Elenco cartelle progetto attive

Aggiornato al 17/05/2026 sulla base della Mappatura Progetti Claude ↔ Cartelle Drive (file `Mappatura_Progetti_Claude_vs_Cartelle_Drive_17_05_2026.xlsx`).

### Progetti attivi (cartelle PROGETTI CLAUDE)

| Codice | Cartella | Slug contesto-ai | Pubblicato | Note |
|---|---|---|---|---|
| 100 | STRATEGIA ORGANIZZAZIONE E FINANZA | `strategia_settembre_2026` | Sì | Cartella rinominata; valutare se aggiornare slug a uno più aderente al nome corrente |
| 110 | AUDIT INFRASTRUTTURA | — | No | Da creare quando una sessione produrrà un riepilogo conforme |
| 200 | AI HUMAN LAB | — | No | Da creare |
| 210 | NAMING VICECONTI | `naming_viceconti` | Sì (documento operativo) | Documento operativo, non riepilogo evolutivo |
| 220 | FORMAZIONE IT | — | No | Da creare |
| 310 | SAP ACADEMY | `sap_academy` | Sì | |
| 311 | SAP SERVICE LAYER | — | No | Da creare |
| 320 | STRUMENTI DI CATTURA VOCALE | `strumenti_cattura_vocale` | Sì | |
| 350 | n8n | `n8n` | Sì | |
| 360 | CALENDAR | — | No | Da creare |
| 370 | TELEGRAM | — | No | Da creare |
| 390 | FastAPI | `fastapi` | Sì (dal 17/05/2026) | Verificare uniformità naming cartella ↔ progetto Claude (consigliato `FastAPI` ovunque) |
| 400 | MANUALE DELLE AUTOMAZIONI | — | No | Da creare |
| 401 | SINTESI ECOSISTEMA VICECONTI | `sintesi` | Sì | Sintesi trasversale, non riepilogo operativo |
| 410 | MEMORIA E CONTESTO AI | `contesto_ai` | Sì | |
| 420 | ASSISTENTE AMMINISTRATIVA | — | No | Da creare |
| 421 | ASSISTENTE DI PROGETTO | — | No | Da creare |
| 430 | HUB DOCUMENTALE | — | No | Da creare |
| 440 | VICECONTI HUB | — | No | Da creare |
| 450 | INTERFACCIA SERVICE LAYER | — | No | Da creare |
| 460 | DATABASE CENTRALIZZATO | — | No | Da creare |
| 470 | ASSET PRODOTTI | — | No | Da creare |
| 480 | ECOSISTEMA TEAM SYSTEM | — | No | Da creare |
| 490 | PIPELINE PRESTASHOP | — | No | Da creare |

### Cartelle di sistema (non progetti Claude)

| Codice | Cartella | Funzione |
|---|---|---|
| 510 | SINTESI_RIEPILOGHI_COWORK | Cartella centralizzata destinazione sync (vedi Sezione 5) |
| 520 | SINTESI RIEPILOGHI COWORK CARTELLA DI LAVORO | Cartella di lavoro/master del sistema sync (vedi Sezione 6) |
| 999 | ARCHIVIO | Archivio storico, escluso dalla sincronizzazione |

### Documenti operativi pubblicati (non riepiloghi di progetto)

I documenti operativi sono pubblicati in `contesto-ai` come parte della sezione `documenti_operativi` dell'`index.json`, non come riepiloghi di progetto. Stato al 17/05/2026:

| Documento | Slug | Esposto via FastAPI |
|---|---|---|
| ISTRUZIONI RIEPILOGO E NAMING | `istruzioni` | Sì |
| NAMING VICECONTI | `naming_viceconti` | Sì |
| README push_github | `readme_push_github` | No |
| REPORT SINCRONIZZAZIONE (più recente) | — | No |

---

## 8. Lettura dei documenti da parte di consumer AI

I documenti pubblicati in `contesto-ai` sono letti dai consumer AI (Claude nei progetti Claude.ai) attraverso una formula specifica iniettata nei prompt sistema di ciascun progetto. La sezione che segue documenta il principio e i prerequisiti; la forma esatta della formula è mantenuta centralmente nel progetto Cowork di allineamento dei prompt sistema.

### Principio

Per fetchare documenti dell'ecosistema, il consumer AI deve usare `curl` via `bash_tool`, NON `web_fetch`.

Motivo: `web_fetch` ha una cache opaca infrastrutturale lato Anthropic che restituisce contenuto stantio (fino a settimane), ignorando l'header `Cache-Control: no-store` del server. `curl` via `bash_tool` passa per un proxy di rete diverso senza quella cache. La scoperta è stata fatta empiricamente il 17/05/2026 nel Cap. V FastAPI e risolta a livello di tool (cambio di tool) anziché di architettura server.

### Prerequisito

Il toggle "Code Execution and File Creation" deve essere abilitato in ogni progetto Claude perché `bash_tool` sia disponibile. È configurazione una tantum per progetto.

### Endpoint FastAPI esposti

Base: `https://piattaforma-ai.onrender.com`

| Risorsa | Endpoint |
|---|---|
| Sintesi trasversale | `/contesto/sintesi/latest` |
| Riepilogo di progetto | `/contesto/<slug>/latest` |
| Indice completo | `/contesto/index` |
| Documenti operativi esposti | `/contesto/<slug>` (es. `/contesto/naming_viceconti`, `/contesto/istruzioni`) |

### Varianti della formula

La formula da inserire nei prompt sistema esiste in tre varianti:

- **Variante A** (standard): progetti con un proprio riepilogo in contesto-ai. Fetcha Sintesi + riepilogo specifico al primo messaggio della chat.
- **Variante B** (sintesi-only): progetti senza riepilogo proprio. Fetcha solo Sintesi al primo messaggio.
- **Variante speciale** (401.SINTESI ECOSISTEMA VICECONTI): il progetto che produce la Sintesi. Non fetcha la Sintesi; al primo messaggio recupera l'indice per orientamento e i riepiloghi altrui su richiesta.

### Fallback

Se `bash_tool` non è disponibile o `curl` fallisce, il consumer AI deve:

1. Informare l'utente del problema
2. Chiedere all'utente di abilitare il toggle Code Execution, oppure
3. Chiedere all'utente di incollare manualmente il contenuto del documento (pattern umano-courier)

Il raw GitHub resta canale alternativo per `curl`:
`curl -s https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/<NOME_FILE>.md`

---

## Passo 1ter — Generazione e mantenimento di index.json

Dopo Passi 1 e 1bis, genera o aggiorna `index.json` in 510 secondo la struttura sottostante. L'`index.json` serve sia da indice per la pagina HTML pubblica (`viceconti-hub.github.io/contesto-ai/`) sia da configurazione per gli endpoint FastAPI di `piattaforma-ai.onrender.com/contesto/*`. Mantenere lo schema corretto è quindi essenziale per il funzionamento della piattaforma AI.

### Struttura

Quattro sezioni:

- `meta`: metadati globali
- `sintesi_trasversale`: array con un solo elemento (la Sintesi)
- `riepiloghi_progetti`: array dei riepiloghi specifici di progetto
- `documenti_operativi`: array dei documenti operativi

### Blocco `meta`

```json
{
  "repo": "viceconti-hub/contesto-ai",
  "base_raw": "https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/",
  "fastapi_base": "https://piattaforma-ai.onrender.com",
  "ultimo_aggiornamento": "<timestamp ISO 8601 del sync corrente>"
}
```

### Campi comuni a ogni entry

- `nome`: nome leggibile in maiuscolo con spazi
- `slug`: identificatore lowercase con underscore (vedi regole sotto)
- `tipo`: "SINTESI" | "RIEPILOGO" | "DOC"
- `file`: nome del file in 510 (con spazi)
- `raw_url`: `base_raw` + filename in versione underscore
- `github_url`: equivalente blob GitHub
- `ultimo_aggiornamento`: data ISO YYYY-MM-DD dell'ultima modifica del file sorgente

### Regole di derivazione dello slug

Default: nome in lowercase, spazi → underscore.

Eccezioni esplicite per nomi lunghi (shortening):

| Nome originale | Slug |
|---|---|
| SINTESI ECOSISTEMA VICECONTI | `sintesi` |
| MEMORIA E CONTESTO AI | `contesto_ai` |
| NUOVA STRATEGIA SETTEMBRE 2026 / STRATEGIA ORGANIZZAZIONE E FINANZA | `strategia_settembre_2026` |
| STRUMENTI DI CATTURA VOCALE | `strumenti_cattura_vocale` |
| ISTRUZIONI RIEPILOGO E NAMING | `istruzioni` |
| NAMING VICECONTI | `naming_viceconti` |
| README push_github | `readme_push_github` |
| FastAPI | `fastapi` |

### Campi specifici per `sintesi_trasversale`

- `fastapi_url`: `<fastapi_base>/contesto/sintesi/latest`

Il `file` è il file con **nome stabile** (`SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md`). La Sintesi segue il **pattern proxy uniforme** come tutti gli altri riepiloghi: ogni nuova versione sovrascrive il file precedente, l'endpoint FastAPI fa proxy del contenuto corrente.

**Nota architettonica**: la versione precedente di queste istruzioni prevedeva un pattern manifest+redirect con filename versionato (`SINTESI_2026_05_14.md`) e campo `current_version`. La decisione di **Option B** del 17/05/2026 (Cap. V FastAPI) ha abbandonato il versioning della Sintesi per uniformità con gli altri riepiloghi e per coerenza con il workflow Cowork → `push_github` (i nomi stabili sono richiesti dalla logica sync-delete con whitelist). Il problema di freschezza per consumer AI è gestito a livello di tool (`curl` via `bash_tool`, vedi Sezione 8), non a livello di architettura URL.

### Campi specifici per `riepiloghi_progetti`

- `fastapi_url`: `<fastapi_base>/contesto/<slug>/latest`

I riepiloghi di progetto seguono il pattern proxy senza versioning: il file ha nome stabile (es. `SAP_ACADEMY_RIEPILOGO.md`), e ogni aggiornamento sovrascrive il file precedente.

### Campi specifici per `documenti_operativi`

- `esposto_fastapi`: true | false
- Se `esposto_fastapi == true`: aggiungere `fastapi_url`: `<fastapi_base>/contesto/<slug>`

Default per `esposto_fastapi`:

| File | esposto_fastapi |
|---|---|
| `ISTRUZIONI_RIEPILOGO_E_NAMING.md` | true |
| `NAMING_VICECONTI.md` | true |
| `README_push_github.md` | false |
| `REPORT_SINCRONIZZAZIONE_*.md` | false |
| Altri file futuri | false (default conservativo) |

### File esclusi da `index.json`

I file privati elencati in Passo 1bis come "NON propagare" non arrivano in 510 e quindi non compaiono in `index.json`. Se in futuro venisse aggiunta una cartella `archivio/`, i suoi file non vanno indicizzati.
