# n8n — Riepilogo del 5 aprile 2026

## Stato attuale

n8n è installato, configurato e operativo in produzione sul PC Lauria. Il primo workflow è pubblicato e funzionante end-to-end: riceve note vocali da AudioPen via webhook, le invia su un gruppo Telegram personale (NOTE PERSONALI) e le salva come file markdown su Google Drive personale. L'esposizione del webhook a internet è gestita tramite ngrok con dominio statico permanente.

Nella sessione del 5 aprile è stato inoltre validato il pattern GitHub Pages per il contesto AI: Claude legge automaticamente file .md pubblicati su GitHub, da istruzione nel prompt di sistema. Il sito https://viceconti-hub.github.io/contesto-ai/ è operativo come indice navigabile di tutti i riepiloghi.

---

## Lavoro svolto nelle sessioni del 4-5 aprile 2026

### Sessione 4 aprile — Installazione e primo workflow

- Installazione n8n v2.14.2 via npm globale su PC Lauria (Node.js v24.14.0)
- Creazione account locale n8n
- Costruzione primo workflow: AudioPen → Webhook → Telegram + Google Drive
- Configurazione credenziali: Telegram Bot, Dropbox (Access Token), Google Drive (OAuth2 via Google Cloud Console, progetto `n8n-viceconti`)
- Installazione e configurazione ngrok per esporre il webhook a internet
- Migrazione del ramo salvataggio da Dropbox aziendale a Google Drive personale (ragioni di privacy)
- Creazione gruppo Telegram NOTE PERSONALI come destinazione dei messaggi AudioPen
- Test end-to-end con nota vocale reale: voce → AudioPen → webhook → n8n → Telegram + file .md su Google Drive ✅

### Sessione 5 aprile — Stabilizzazione e contesto AI

- Configurazione dominio statico ngrok (`karlene-apsidal-ruminantly.ngrok-free.dev`) — l'URL non cambia più ad ogni riavvio
- Validazione del pattern GitHub Pages per contesto AI:
  - File .md caricati sul repo `viceconti-hub/contesto-ai`
  - Test fetch da Claude: URL raw di GitHub letto automaticamente con contenuto completo e accurato ✅
  - Test fetch da ChatGPT: funziona su URL raw e standard ✅
  - Test fetch da Gemini: funziona solo su URL standard GitHub (non raw) ✅
  - Test fetch automatico da prompt di sistema Claude: confermato ✅ — il contesto viene caricato senza azione dell'utente
- Creazione sito indice `contesto-ai` con index.html: lista navigabile di tutti i riepiloghi con link GitHub, Raw, e bottone copia link
- GitHub Pages abilitato sul repo

### Problemi incontrati e risolti

- **Scrittura file su disco locale (n8n v2.14):** moduli `fs` e `child_process` bloccati nel nodo Code, nodo Write File con restrizioni di accesso. Risolto con approccio cloud: upload via API Dropbox/Google Drive
- **Permessi Dropbox:** scope `files.content.write` mancante — risolto abilitando il permesso e rigenerando il token
- **Google OAuth in fase di test:** richiesta aggiunta manuale utente come tester — risolto
- **Tunnel n8n v2:** flag `--tunnel` non più supportato — risolto con ngrok esterno
- **Vicolo cieco AI:** identificato il pattern di sunk cost cognitivo nei modelli linguistici. Soluzione: triangolazione tra AI (Gemini e ChatGPT hanno suggerito l'approccio cloud che ha sbloccato il problema)

---

## Decisioni prese

- **n8n installato sul PC Lauria** via npm globale — non Docker, non server SAP, non cloud
- **Task Scheduler resta sul server SAP** — coesistenza pulita, non sostituzione
- **Google Drive personale** come destinazione dei file AudioPen — separazione netta tra dati personali e Dropbox aziendale
- **Gruppo Telegram NOTE PERSONALI** come destinazione messaggi AudioPen
- **ngrok con dominio statico** per esposizione webhook — permanente e stabile
- **GitHub repo pubblico `contesto-ai`** per i file di contesto AI — rischio di esposizione accettato in fase di sperimentazione
- **Due livelli di contesto per Claude:** Sintesi Ecosistema (visione d'insieme, in ogni progetto) + Riepilogo specifico (dettaglio, nel singolo progetto)

---

## Architettura del workflow in produzione

```
AudioPen (nota vocale)
    ↓ (webhook, ritardo 4 min)
ngrok (dominio statico → localhost:5678)
    ↓
n8n Webhook (POST)
    ├→ Telegram: messaggio formattato → gruppo NOTE PERSONALI
    └→ Code JS (genera .md + binary) → Google Drive Upload (Drive personale)
```

## Architettura contesto AI

```
Riepiloghi progetto (.md)
    → cartella SINTESI_RIEPILOGHI_COWORK (aggregazione Cowork)
    → upload su GitHub repo contesto-ai (manuale, da automatizzare)
    → GitHub Pages (sito indice) + URL raw (fetch diretto)
    → Claude/ChatGPT/Gemini leggono il contesto aggiornato
```

---

## Approfondimenti concettuali prodotti nelle sessioni

- **Ruolo di n8n vs Task Scheduler vs Cowork:** confini chiari definiti — n8n per meccanica event-driven, Task Scheduler per cron sul server SAP, Cowork per compiti che richiedono giudizio
- **Stato persistente e elaborazioni lunghe:** n8n orchestra ma non esegue lavoro pesante — script Python/PowerShell restano nell'engine layer
- **Consolidamento prima dell'orchestrazione:** principio confermato e applicato
- **Limiti di n8n v2 su filesystem:** le restrizioni di sicurezza cambiano l'approccio — preferire API cloud
- **Pattern vicolo cieco AI:** sunk cost cognitivo nei modelli. Soluzione: triangolazione con altra AI o chat nuova
- **Contesto AI — architettura a due livelli:** Sintesi globale (sfondo) + Riepilogo specifico (dettaglio). Evita sia la diluizione del contesto che la perdita di collegamenti trasversali
- **GitHub come ponte AI-universale:** URL standard GitHub funziona con tutte e tre le AI (Claude, ChatGPT, Gemini); URL raw funziona con Claude e ChatGPT ma non Gemini
- **Indice testuale come ponte per file binari:** per PDF e file non testuali, Claude può leggere un indice che descrive cosa è disponibile, poi il file specifico viene caricato in chat quando serve

---

## Prossimi passi

1. **Automatizzare il push su GitHub** — script PowerShell parametrizzabile (cartella sorgente → repo GitHub) che replica il pattern del SAP Query Engine. Priorità: è il pezzo mancante per rendere il contesto AI sempre aggiornato senza intervento manuale
2. **Configurare n8n come servizio Windows** — evitare che la chiusura del terminale fermi il workflow
3. **Smistamento condizionale Telegram** — nodo Switch basato sui tag AudioPen per inviare a gruppi diversi (NOTE PERSONALI default, CHIAMATE DI SERVIZIO se tag `#tecnici`)
4. **Aggiornare i riepiloghi su GitHub** — il file n8n attualmente su GitHub è la versione pre-installazione, va sostituito con questo riepilogo
5. **Creare cartella AudioPen su Google Drive** — spostare i file dalla root a una cartella dedicata
6. **Rinominare il workflow n8n** — da "My workflow" a "AudioPen → Telegram + Google Drive"
7. **Estensione pattern contesto AI ad altri ambiti** — cantieri (con indice testuale dei file PDF), altri progetti. Da pianificare

---

## Blocchi o dipendenze

- **n8n in terminale:** se il terminale viene chiuso, n8n si ferma. Da configurare come servizio Windows per funzionamento 24/7
- **Token Google OAuth:** app in "fase di test" su Google Cloud — funziona solo per utenti aggiunti come tester. Non è un problema finché l'unico utente è Prospero
- **Aggiornamento GitHub manuale:** i file di contesto AI richiedono upload manuale su GitHub. Script di automazione da sviluppare
- **PC Lauria acceso:** n8n + ngrok dipendono dalla disponibilità della macchina (già operativa 24/7 per gli indicizzatori)
- **File non testuali (PDF):** Claude non può fare fetch di PDF da URL. Per cantieri e contesti con file binari serve il pattern dell'indice testuale

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
| Dropbox App | ID 5964611 (scope files.content.write abilitato) |
| GitHub repo contesto AI | viceconti-hub/contesto-ai |
| Sito contesto AI | https://viceconti-hub.github.io/contesto-ai/ |

---

## Procedura di avvio dopo riavvio PC

Terminale 1:
```powershell
n8n start
```

Terminale 2:
```powershell
ngrok http --domain=karlene-apsidal-ruminantly.ngrok-free.dev 5678
```

---

*Sessioni del 4-5 aprile 2026 — da zero a workflow in produzione + contesto AI validato*
