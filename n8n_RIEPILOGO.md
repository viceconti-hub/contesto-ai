# n8n — Riepilogo del 22 aprile 2026

## Stato attuale

n8n è installato, configurato e operativo in produzione sul PC Lauria con avvio automatico. Tre workflow sono attivi in produzione. Il progetto è passato in meno di tre settimane (4-22 aprile 2026) da "software non installato" a tre workflow operativi che coprono cattura vocale, archiviazione note e sincronizzazione calendari SAP.

---

## Workflow in produzione

### Workflow 1 — AudioPen → Telegram
```
AudioPen (nota vocale)
    ↓ webhook (ritardo ~4 min)
ngrok → localhost:5678
    ↓
n8n Webhook (POST)
    └→ Telegram: messaggio formattato → gruppo NOTE PERSONALI
```
Ramo Google Drive **disattivato** (nodi presenti ma deactivated) — la funzione è coperta dal Workflow 2.

---

### Workflow 2 — NOTE PERSONALI → Google Drive
```
Telegram Trigger (tutti i messaggi del gruppo NOTE PERSONALI)
    ↓
Code JS (genera timestamp + slug dalle prime 5 parole + file .md)
    ↓
Google Drive Upload → cartella NOTE PERSONALI DA AUDIOPEN
```
Cattura qualsiasi messaggio nel gruppo — AudioPen (inoltrato), microfono nativo Claude, testo digitato. I messaggi diretti del bot (Workflow 1) non vengono duplicati per limitazione API Telegram (i bot non ricevono i propri messaggi).

**Nota:** token Google OAuth riconfigurato il 12 aprile — aggiunto redirect URI ngrok in Google Cloud Console e `vicecontip@gmail.com` come utente tester.

---

### Workflow 3 — sync-calendar (SAP → Google Calendar)
```
Webhook POST /sync-calendar
    ↓
Split Out (spacca array attivita[])
    ↓
Code JS (prepara payload Calendar: titolo, orari, location, descrizione, routing calendarId)
    ↓
Google Calendar → Create Event
    (ASSISTENZA TECNICA o CONSEGNE in base ad ActivityType)
```
Innesco: Prospero seleziona manualmente attività dall'interfaccia Service Layer (`viceconti-attivita-v01.html`) e clicca "Sincronizza su Calendar". Semiautomatico per scelta — nessuna sincronizzazione indiscriminata.

**Routing calendari:**
- `ActivityType = ASSISTENZA` → calendario ASSISTENZA TECNICA (`vicecontisnc@gmail.com`)
- `ActivityType = CONSEGNE` → calendario CONSEGNE (`iur82rio0kb0545l3h2puertqg@group.calendar.google.com`)

---

## Lavoro svolto per periodo

### 4-6 aprile — Installazione e primi workflow
- Installazione n8n v2.14.2 via npm globale (Node.js v24.14.0)
- Workflow AudioPen → Telegram + Google Drive (poi sdoppiato)
- Configurazione ngrok dominio statico, credenziali Telegram, Google Drive, Dropbox
- Avvio automatico via Task Scheduler (due batch in `C:\Viceconti\n8n\`)
- Risolti: blocco fs/child_process in v2, tunnel flag deprecato, password Task Scheduler

### 7 aprile — Scenario B (Telegram Trigger → Drive)
- Creato Workflow 2 con Telegram Trigger
- Privacy mode bot disabilitata via BotFather
- Bot rimosso e riaggiunto al gruppo NOTE PERSONALI
- Configurato `WEBHOOK_URL` in start-n8n.bat per risolvere errore HTTPS webhook
- Naming file con slug dalle prime 5 parole del testo

### 12 aprile — Fix OAuth Google Drive
- Token Google OAuth scaduto — credenziale ricreata da zero
- Aggiunto redirect URI ngrok in Google Cloud Console
- Configurato `N8N_EDITOR_BASE_URL=http://localhost:5678` per bypassare blocco OAuth su ngrok

### 22 aprile — Workflow sync-calendar (progetto CALENDAR)
- Creato Workflow 3: Webhook → Split Out → Code → Google Calendar
- Aggiornata query SQL `attivita_assistenza` con campi `BeginTime`, `EndTime`, `Tipo`
- Modificata interfaccia `viceconti-attivita-v01.html`: checkbox, bottone Sincronizza, campi data/ora editabili con salvataggio PATCH su Service Layer
- Risolto problema fuso orario: ora fine calcolata come stringa senza `toISOString()` (evita sottrazione UTC+2)
- Aggiunto `vicecontisnc@gmail.com` come utente tester in Google Cloud `n8n-viceconti`
- Abilitata Google Calendar API nel progetto

---

## Decisioni prese

- **n8n sul PC Lauria** via npm globale — non Docker, non server SAP, non cloud
- **Task Scheduler resta sul server SAP** — coesistenza pulita, territori distinti
- **Google Drive personale** per note AudioPen — cartella NOTE PERSONALI DA AUDIOPEN
- **Sincronizzazione Calendar semiautomatica** — selezione manuale delle attività, non trigger automatico
- **Durata default eventi** +1 ora se `EndTime` non valorizzato in SAP
- **Campo Oggetto (Subject) SAP** non editabile da interfaccia — SAP richiede intero, non testo libero
- **Payload verso n8n include `ActivityCode`** — prerequisito per caso d'uso 2 (esito intervento)

---

## Prossimi passi

1. **Caso d'uso 2 Calendar** — tecnico aggiorna descrizione evento → n8n intercetta modifica → scrive `U_Esito` in SAP. Prerequisito: salvare `EventId` ↔ `ActivityCode` via `extendedProperties` Calendar
2. **Prevenire duplicati Calendar** — prima di Create, verificare se EventId già associato ad ActivityCode → fare Update invece di Create
3. **Foto in Telegram** — aggiungere ramo Switch nel Workflow 2 per gestire `msg.photo` (file_id → getFile → download → Drive)
4. **Local File Trigger** — workflow che monitora cartella locale e triggera script Python/PowerShell (watchdog come alternativa senza n8n)
5. **Gmail Trigger** — per automazione registrazione bonifici in uscita (CCN → viceconti.amministrazione@gmail.com → n8n → API Anthropic → Service Layer) e altri casi d'uso email
6. **PLAUD Note Pro** — integrazione da definire dopo verifica modalità export (webhook, API, email o app manuale)
7. **Script PowerShell push GitHub** — automatizzare caricamento riepiloghi su repo contesto-ai
8. **Export workflow JSON** — backup dei tre workflow per migrazione futura
9. **Token GitHub** — scade 9 maggio 2026, rigenerare prima della scadenza

---

## Blocchi e dipendenze

- **Duplicati Calendar**: workflow attuale esegue sempre Create — evitare di cliccare Sincronizza più volte sulla stessa attività finché non è implementato il caso d'uso 2
- **Ngrok URL variabile**: a ogni riavvio senza dominio statico l'URL cambia. Il dominio statico `karlene-apsidal-ruminantly.ngrok-free.dev` è configurato e permanente — verificare che start-ngrok.bat lo usi sempre
- **Token Google OAuth**: app in "fase di test" su Google Cloud — funziona solo per utenti tester registrati
- **VPN per accesso remoto Service Layer**: necessario per accesso futuro tecnici da tablet — da chiarire con Vincenzo Strazzullo
- **Local File Trigger**: in n8n v2 potrebbe essere disabilitato (variabile `NODES_EXCLUDE` da verificare)

---

## Componenti tecnici di riferimento

| Componente | Versione/Dettaglio |
|---|---|
| n8n | v2.14.2, self-hosted, npm globale |
| Node.js | v24.14.0 |
| ngrok | v3.37.3, piano Free, regione EU, dominio statico |
| ngrok domain | karlene-apsidal-ruminantly.ngrok-free.dev |
| Webhook AudioPen | 26673b83-ea85-4edc-b4c4-2e091302c53a |
| Webhook Calendar | sync-calendar |
| Telegram Bot | @Chiamate_di_servizio_bot (privacy mode disabilitata) |
| Telegram gruppo | NOTE PERSONALI |
| Google Cloud Project | n8n-viceconti |
| Google Drive Folder ID | 131iWrhFJ6G_XbLnjuskRe-iIc0h_GvqS (NOTE PERSONALI DA AUDIOPEN) |
| Google Calendar ASSISTENZA | vicecontisnc@gmail.com |
| Google Calendar CONSEGNE | iur82rio0kb0545l3h2puertqg@group.calendar.google.com |
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
set WEBHOOK_URL=https://karlene-apsidal-ruminantly.ngrok-free.dev
n8n start
```

Terminale 2:
```powershell
ngrok http --domain=karlene-apsidal-ruminantly.ngrok-free.dev 5678
```

---

*Aggiornato al 22 aprile 2026 — consolida sessioni 4-6 aprile, 7 aprile, 12 aprile, 22 aprile 2026*
