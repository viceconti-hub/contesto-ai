# Push GitHub - Sincronizzazione .md su GitHub

Script PowerShell per pushare automaticamente i file `.md` dalla cartella locale
al repository GitHub `viceconti-hub/contesto-ai` via REST API.

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
| Errore di rete | `ERRORE` + retry | fino a 3 tentativi con pausa crescente |

## Log

I log vengono salvati in `logs/push_github_YYYY-MM-DD_HHMMSS.log`.

## Exit code

- `0` = tutti i file processati con successo
- `1` = almeno un file ha avuto errori, oppure token non configurato
