# Push GitHub - Sincronizzazione .md su GitHub

Script per sincronizzare automaticamente i file `.md` dalla cartella locale al
repository GitHub `viceconti-hub/contesto-ai` via REST API e generare gli
indici (`index.html` per umani, `index.json` per AI).

Lo script non si limita a pushare: tiene il repo allineato al locale,
eliminando da GitHub i file `.md` che sono stati rimossi dalla cartella
sorgente, e ricostruisce a ogni run gli indici a partire dai file presenti.

## Versione corrente

**v2.0 (Python)** — `push_github.py`. Sostituisce lo script PowerShell
`push_github.ps1` v1.1, che resta in cartella come riferimento/backup.

Differenze rispetto a v1.1:
- Implementazione in Python (più portabile, meno boilerplate Windows-only).
- Generazione automatica di `index.html` (UI per umani) e `index.json`
  (indice strutturato per agenti AI).
- I file generati sostituiscono ogni volta `index.html` locale: i contenuti
  sono ricavati interamente dai filename presenti.

## Prerequisiti

- Python 3.10+ (testato con 3.14 su Windows)
- Token GitHub fine-grained con permesso **Contents: Read and Write** sul
  repo target. Generalo da:
  GitHub > Settings > Developer settings > Fine-grained tokens

Lo script usa solo la libreria standard (`urllib`, `json`, `base64`,
`pathlib`, `datetime`). Nessuna installazione `pip` necessaria.

## Configurazione

1. Apri `push_github.env`
2. Inserisci il token in `GitHubToken = ...`
3. (Opzionale) Modifica `SourceFolder` se i file `.md` sono in un'altra
   cartella

Variabili supportate:

```
GitHubToken  = github_pat_...
RepoOwner    = viceconti-hub
RepoName     = contesto-ai
Branch       = main
SourceFolder = (default: cartella dello script)
MaxRetries   = 3
```

## Lancio manuale

```powershell
# Dalla cartella dello script
python push_github.py
```

## Schedulare con Task Scheduler

1. Apri **Task Scheduler** (Utilità di pianificazione)
2. Crea attività di base:
   - **Nome**: Push GitHub Contesto AI
   - **Trigger**: Giornaliero, ore 08:00 (o a piacere)
   - **Azione**: Avvia programma
     - Programma: `python.exe` (path completo, es.
       `C:\Users\PC\AppData\Local\Programs\Python\Python314\python.exe`)
     - Argomenti: `"C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\510.SINTESI_RIEPILOGHI_COWORK\push_github.py"`
   - **Condizioni**: spunta "Avvia solo se il computer è connesso alla rete"

## Comportamento

| Scenario | Log | Azione API |
|---|---|---|
| File nuovo (non esiste su GitHub) | `CREATO` | PUT (create) |
| File modificato (contenuto diverso) | `AGGIORNATO` | GET SHA + PUT (update) |
| File invariato (contenuto identico) | `INVARIATO` | GET solo (nessun PUT) |
| File rimosso dal locale | `[DELETE] ... rimosso` | DELETE |
| Errore di rete | `ERRORE` + retry | fino a 3 tentativi con pausa 2s/4s/6s |

### Naming locale -> GitHub

Per ogni `.md` lo script converte gli **spazi in underscore** quando
costruisce il nome remoto. Il file locale resta invariato.

- Locale: `SAP SERVICE LAYER RIEPILOGO.md`
- GitHub: `SAP_SERVICE_LAYER_RIEPILOGO.md`

La conversione **non** si applica ai file protetti: `index.html`,
`index.json`, `last_push.json`, `README_push_github.md`.

### Sincronizzazione (sync delete)

Dopo aver pushato i file locali, lo script chiama
`GET /repos/{owner}/{repo}/contents/` e confronta i `.md` presenti sul repo
con la lista dei nomi remoti attesi (calcolati dai file locali). I `.md` su
GitHub che non corrispondono a nessun file locale vengono eliminati con
`DELETE /repos/{owner}/{repo}/contents/{path}` usando lo SHA restituito
dall'API.

Non vengono mai eliminati:
- i file protetti (`index.html`, `index.json`, `last_push.json`,
  `README_push_github.md`),
- i file non-`.md` presenti nella root del repo,
- i file in sottocartelle (es. `manuale/`).

### Generazione `index.html`

Generato a ogni run dai filename in cartella, classificati in tre sezioni:

- **Sintesi trasversale** — `.md` con `SINTESI` nel nome
- **Riepiloghi progetti** — `.md` con `RIEPILOGO` nel nome (se non già
  classificato come SINTESI)
- **Documenti operativi** — tutti gli altri `.md`

Il display name è ricavato dal filename rimuovendo l'estensione `.md` e
l'eventuale suffisso ` RIEPILOGO` o ` SINTESI`. Non sono previsti override
manuali nella v2.0.

**Eccezioni di classificazione (`DOC_EXCEPTIONS`):** alcuni file con
keyword nel nome devono essere classificati come DOC anche se contengono
`RIEPILOGO`/`SINTESI`. Attualmente:

- `ISTRUZIONI_RIEPILOGO_E_NAMING.md`
- `README_push_github.md`

**Filtro REPORT log:** i file con pattern
`REPORT_SINCRONIZZAZIONE_GG_MM_AAAA.md` sono log di esecuzione, non
riepiloghi permanenti. Lo script tiene solo quello con la data più recente
nel nome; gli altri vengono loggati come `[SKIP-REPORT]` ed esclusi dal
push. Non essendo più nella lista dei file attesi, vengono rimossi dal
repo dalla normale sync-delete.

L'header contiene un link visibile a `index.json` per i lettori AI.

### Generazione `index.json`

Documento JSON strutturato con `meta` (repo, ultimo aggiornamento,
istruzioni, base raw URL) e tre liste (`sintesi_trasversale`,
`riepiloghi_progetti`, `documenti_operativi`). Ogni voce ha `nome`, `file`,
`raw_url`, `github_url`, `tipo`, `ultimo_aggiornamento` (data del commit
più recente che ha toccato il file, in formato `YYYY-MM-DD`).

## Log

Salvati in `logs/push_github_YYYY-MM-DD_HHMMSS.log`. Il riepilogo finale
riporta i contatori:

```
RIEPILOGO:
  Creati     : N
  Aggiornati : N
  Invariati  : N
  Eliminati  : N
  Errori     : N
```

## Exit code

- `0` = tutti i file processati con successo
- `1` = almeno un file ha avuto errori, oppure token non configurato

## Changelog

- **v1.0** (06/04/2026): versione iniziale (PowerShell). Push di file `.md`
  + `index.html` via REST API, con confronto SHA per evitare PUT inutili e
  retry su errori di rete.
- **v1.1** (09/05/2026, PowerShell): conversione spazi -> underscore nei
  nomi remoti su GitHub (file locale invariato); logica di **sync delete**:
  i `.md` rimossi dal locale vengono eliminati dal repo. Aggiunto contatore
  `Eliminati` nel riepilogo.
- **v2.0** (09/05/2026, Python): riscrittura in Python come `push_github.py`,
  punto di ingresso ufficiale. Stessa logica di push/sync di v1.1 +
  generazione automatica di `index.html` e `index.json` a ogni run, derivati
  dai filename. Aggiunto `index.json` ai file protetti. `last_push.json` ora
  contiene sia `last_push` sia `last_sync` (compatibilità).
  `push_github.ps1` v1.1 resta in cartella come backup.
- **v2.1** (09/05/2026, Python): aggiunto `DOC_EXCEPTIONS`
  (`ISTRUZIONI_RIEPILOGO_E_NAMING.md`, `README_push_github.md`) per forzare
  la categoria DOC a prescindere dalle keyword nel nome. Aggiunto filtro
  REPORT: dei file `REPORT_SINCRONIZZAZIONE_GG_MM_AAAA.md` viene tenuto solo
  il più recente; gli altri sono esclusi dal push (log `[SKIP-REPORT]`) e
  rimossi dal repo via sync-delete.
