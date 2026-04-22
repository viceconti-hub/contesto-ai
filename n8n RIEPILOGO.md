# n8n — Riepilogo del 6 aprile 2026

## Stato attuale

n8n è installato, configurato e operativo in produzione sul PC Lauria con avvio automatico. Il workflow AudioPen → Telegram + Google Drive è pubblicato e funzionante end-to-end. L'infrastruttura è stabile: n8n e ngrok si avviano automaticamente all'accensione del PC tramite Task Scheduler, il dominio ngrok è statico e permanente.

Il progetto è passato in tre sessioni (4-6 aprile 2026) da "software non installato" a "workflow in produzione con avvio automatico".

---

## Lavoro svolto nel periodo 4-6 aprile 2026

### Sessione 4 aprile — Installazione e primo workflow

- Installazione n8n v2.14.2 via npm globale su PC Lauria (Node.js v24.14.0)
- Costruzione primo workflow: AudioPen → Webhook → Telegram + Google Drive
- Configurazione credenziali: Telegram Bot, Dropbox (Access Token), Google Drive (OAuth2 via Google Cloud Console, progetto `n8n-viceconti`)
- Installazione e configurazione ngrok per esporre il webhook a internet
- Migrazione del ramo salvataggio da Dropbox aziendale a Google Drive personale (ragioni di privacy)
- Creazione gruppo Telegram NOTE PERSONALI come destinazione dei messaggi AudioPen
- Test end-to-end con nota vocale reale ✅

### Sessione 5 aprile — Stabilizzazione e contesto AI

- Configurazione dominio statico ngrok (`karlene-apsidal-ruminantly.ngrok-free.dev`)
- Validazione del pattern GitHub Pages per contesto AI (documentato nel progetto MEMORIA E CONTESTO AI)
- Creazione sito indice `contesto-ai` su GitHub Pages

### Sessione 6 aprile — Avvio automatico e rifinitura

- Configurazione n8n e ngrok come servizi automatici via Task Scheduler:
  - Due task creati: "n8n Start" (ritardo 30s all'avvio) e "ngrok Start" (ritardo 60s all'avvio)
  - Script batch in `C:\Viceconti\n8n\` (`start-n8n.bat` e `start-ngrok.bat`)
  - Modalità: "Esegui solo se l'utente è connesso" (terminali visibili per monitoraggio)
  - Problema password con "Esegui anche se l'utente non è connesso" — risolto cambiando modalità
- Test riavvio PC: n8n e ngrok partono automaticamente, workflow operativo senza intervento manuale ✅
- Configurazione cartella dedicata su Google Drive: file AudioPen ora salvati in "NOTE PERSONALI DA AUDIOPEN" (Folder ID: `131iWrhFJ6G_XbLnjuskRe-iIc0h_GvqS`)
- Rimozione attribuzione n8n dai messaggi Telegram (Append Attribution → off)

### Problemi incontrati e risolti nel periodo

- **Scrittura file su disco locale (n8n v2.14):** moduli `fs` e `child_process` bloccati, nodo Write File con restrizioni. Risolto con approccio cloud (upload via API Google Drive)
- **Permessi Dropbox:** scope `files.content.write` mancante — risolto
- **Google OAuth in fase di test:** richiesta aggiunta utente come tester — risolto
- **Tunnel n8n v2:** flag `--tunnel` non supportato — risolto con ngrok esterno
- **Task Scheduler e password:** task con "Esegui anche se l'utente non è connesso" non partiva senza password salvata — risolto con modalità "Esegui solo se l'utente è connesso"
- **Vicolo cieco AI su scrittura filesystem:** identificato pattern di sunk cost cognitivo. Soluzione trovata tramite triangolazione con Gemini e ChatGPT (approccio cloud API)

---

## Decisioni prese

- **n8n installato sul PC Lauria** via npm globale — non Docker, non server SAP, non cloud
- **Task Scheduler resta sul server SAP** — coesistenza pulita con n8n
- **Google Drive personale** come destinazione file AudioPen — cartella "NOTE PERSONALI DA AUDIOPEN"
- **Gruppo Telegram NOTE PERSONALI** come destinazione messaggi AudioPen
- **ngrok con dominio statico** permanente per webhook
- **Avvio automatico** via Task Scheduler con terminali visibili per monitoraggio
- **GitHub repo pubblico `contesto-ai`** per file di contesto AI (rischio accettato)

---

## Architettura del workflow in produzione

```
AudioPen (nota vocale)
    ↓ (webhook, ritardo 4 min)
ngrok (dominio statico → localhost:5678)
    ↓
n8n Webhook (POST)
    ├→ Telegram: messaggio formattato → gruppo NOTE PERSONALI (senza attribuzione n8n)
    └→ Code JS (genera .md + binary) → Google Drive Upload → cartella NOTE PERSONALI DA AUDIOPEN
```

---

## Approfondimenti concettuali prodotti nelle sessioni

- **Ruolo di n8n vs Task Scheduler vs Cowork:** n8n per meccanica event-driven, Task Scheduler per cron sul server SAP, Cowork per compiti che richiedono giudizio
- **Stato persistente e elaborazioni lunghe:** n8n orchestra ma non esegue lavoro pesante — script restano nell'engine layer
- **Limiti di n8n v2 su filesystem:** restrizioni di sicurezza → preferire API cloud
- **Pattern vicolo cieco AI:** sunk cost cognitivo nei modelli. Soluzione: triangolazione con altra AI
- **n8n locale vs cloud vs DMZ:** analisi delle opzioni architetturali. PC locale è la scelta corretta per l'accesso al filesystem; il tunnel è il pattern standard per webhook da servizi cloud
- **Coesistenza n8n e Task Scheduler:** n8n non sostituisce Task Scheduler sul server SAP — sono due strumenti complementari con territori diversi
- **Documentazione per migrazione macchina:** best practice identificate — README per componente, export workflow JSON, file credenziali separato, script di installazione

---

## Prossimi passi

1. **Local File Trigger** — implementare un workflow n8n con il nodo nativo che monitora una cartella e si attiva quando un file viene creato o modificato. Prossima funzionalità da implementare
2. **Script PowerShell push GitHub** — automatizzare il caricamento dei riepiloghi su GitHub repo `contesto-ai`. Parametrizzabile per qualsiasi contesto futuro
3. **Smistamento condizionale Telegram** — nodo Switch basato sui tag AudioPen per inviare a gruppi diversi
4. **Export workflow JSON** — backup del workflow n8n per migrazione futura
5. **README n8n** — documentazione completa per reinstallazione su nuova macchina
6. **Rinominare il workflow** — da "My workflow" a "AudioPen → Telegram + Google Drive"

---

## Blocchi o dipendenze

- **Local File Trigger:** in n8n v2 potrebbe essere disabilitato di default (da verificare con variabile `NODES_EXCLUDE`)
- **Push GitHub manuale:** funziona ma non scala. Script PowerShell da sviluppare
- **Token Google OAuth:** app in "fase di test" su Google Cloud — funziona solo per utenti tester
- **PC Lauria acceso:** n8n + ngrok dipendono dalla disponibilità della macchina (già operativa 24/7)

---

## Componenti tecnici di riferimento

| Componente | Versione/Dettaglio |
|---|---|
| n8n | v2.14.2, self-hosted, npm globale |
| Node.js | v24.14.0 |
| ngrok | v3.37.3, piano Free, regione EU, dominio statico |
| ngrok domain | karlene-apsidal-ruminantly.ngrok-free.dev |
| Webhook path | 26673b83-ea85-4edc-b4c4-2e091302c53a |
| Telegram Bot | @Chiamate_di_servizio_bot |
| Telegram gruppo | NOTE PERSONALI |
| Google Cloud Project | n8n-viceconti |
| Google Drive Folder ID | 131iWrhFJ6G_XbLnjuskRe-iIc0h_GvqS |
| Dropbox App | ID 5964611 (scope files.content.write abilitato) |
| GitHub repo contesto AI | viceconti-hub/contesto-ai |
| Sito contesto AI | https://viceconti-hub.github.io/contesto-ai/ |
| Task Scheduler | n8n Start (30s delay) + ngrok Start (60s delay) |
| Script batch | C:\Viceconti\n8n\start-n8n.bat, start-ngrok.bat |

---

## Procedura di avvio dopo riavvio PC

Automatica via Task Scheduler. Se necessario avvio manuale:

Terminale 1:
```powershell
n8n start
```

Terminale 2:
```powershell
ngrok http --domain=karlene-apsidal-ruminantly.ngrok-free.dev 5678
```

---

*Sessioni del 4-5-6 aprile 2026 — da zero a workflow in produzione con avvio automatico*
