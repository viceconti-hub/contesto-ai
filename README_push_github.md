# Push GitHub - Sincronizzazione .md su GitHub

Script PowerShell per sincronizzare automaticamente i file `.md` dalla cartella
locale al repository GitHub `viceconti-hub/contesto-ai` via REST API.

Lo script non si limita a pushare: tiene il repo allineato al locale, eliminando
da GitHub i file `.md` che sono stati rimossi dalla cartella sorgente.

## Prerequisiti

- PowerShell 5.1+ (incluso in Windows 10/11)
- Token GitHub fine-grained con permesso **Contents: Read and Write** sul repo target
  - Generalo da: GitHub > Settings > Developer settings > Fine-grained tokens

## Configurazione

1. Apri `push_github.env`
2. Sostituisci `ghp_YOUR_TOKEN_HERE` con il tuo token GitHub
3. (Opzionale) Modifica `SourceFolder` se i file .md sono in un'altra cartella

## Lancio manuale

```powershell
# Dalla cartella dello script
.\push_github.ps1
```

## Schedulare con Task Scheduler

1. Apri **Task Scheduler** (Utilità di pianificazione)
2. Crea attività di base:
   - **Nome**: Push GitHub Contesto AI
   - **Trigger**: Giornaliero, ore 08:00 (o a piacere)
   - **Azione**: Avvia programma
     - Programma: `powershell.exe`
     - Argomenti: `-ExecutionPolicy Bypass -File "C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\010.SINTESI_RIEPILOGHI_COWORK\push_github.ps1"`
   - **Condizioni**: spunta "Avvia solo se il computer è connesso alla rete"

## Comportamento

| Scenario | Log | Azione API |
|---|---|---|
| File nuovo (non esiste su GitHub) | `CREATO` | PUT (create) |
| File modificato (contenuto diverso) | `AGGIORNATO` | GET SHA + PUT (update) |
| File invariato (contenuto identico) | `INVARIATO` | GET solo (nessun PUT) |
| File rimosso dal locale | `[DELETE] ... rimosso` | DELETE |
| Errore di rete | `ERRORE` + retry | fino a 3 tentativi con pausa crescente |

### Naming locale -> GitHub

Per ogni file `.md` lo script converte gli **spazi in underscore** quando
costruisce il nome remoto. Il file locale resta invariato.

- Locale: `SAP SERVICE LAYER RIEPILOGO.md`
- GitHub: `SAP_SERVICE_LAYER_RIEPILOGO.md`

La conversione **non** si applica ai file "protetti": `index.html`,
`last_push.json`, `README_push_github.md`.

### Sincronizzazione (sync delete)

Dopo aver pushato i file locali, lo script chiama
`GET /repos/{owner}/{repo}/contents/` e confronta i `.md` presenti sul repo con
la lista dei nomi remoti attesi (calcolati dai file locali). I `.md` su GitHub
che non corrispondono a nessun file locale vengono eliminati con
`DELETE /repos/{owner}/{repo}/contents/{path}` usando lo SHA restituito
dall'API.

Non vengono mai eliminati:
- i file protetti (`index.html`, `last_push.json`, `README_push_github.md`),
- i file non-`.md` presenti nella root del repo,
- i file in sottocartelle (es. `manuale/`).

## Log

I log vengono salvati in `logs/push_github_YYYY-MM-DD_HHMMSS.log`.

Il riepilogo finale riporta i contatori:

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

- **v1.0** (06/04/2026): versione iniziale. Push di file `.md` + `index.html`
  via REST API, con confronto SHA per evitare PUT inutili e retry su errori
  di rete.
- **v1.1** (09/05/2026): conversione spazi -> underscore nei nomi remoti su
  GitHub (file locale invariato); aggiunta logica di **sync delete**: i `.md`
  rimossi dal locale vengono ora eliminati anche dal repo. Aggiunto contatore
  `Eliminati` nel riepilogo.
