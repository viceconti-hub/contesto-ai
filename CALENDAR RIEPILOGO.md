# CALENDAR — Riepilogo del 22 aprile 2026

## Stato attuale

Il workflow SAP → Google Calendar è operativo in produzione. Dalla selezione delle attività nel Service Layer, con un click le attività vengono sincronizzate negli appositi calendari Google (ASSISTENZA TECNICA o CONSEGNE) con tutti i dati rilevanti. Il workflow è attivo su n8n con webhook di produzione.

---

## Lavoro svolto in questa sessione

### Analisi e progettazione del workflow

- Definito il workflow semiautomatico SAP → Calendar: Prospero seleziona manualmente le attività da sincronizzare dal Service Layer, senza automazione indiscriminata.
- Identificato Viceconti Hub (interfaccia Service Layer `viceconti-attivita-v01.html`) come punto di innesco — dati live da SAP, senza dipendenza dal ciclo orario del Query Engine.
- Definito il mapping dei campi: `CardName + Details` → titolo evento, `StartDate + StartTime` → inizio, `City` → location, `TecName + ActivityType + ActivityCode` → descrizione.

### Aggiornamento query SQL (Query Engine)

- Aggiunti i campi `Ora Inizio` (`BeginTime`), `Ora Fine` (`EndTime`) e `Tipo` (calcolato da `Details`: CONSEGNA/ASSISTENZA) alla query `attivita_assistenza` in `queries.json`.
- Verificato il nuovo JSON prodotto dal Query Engine con i tre campi aggiuntivi.

### Modifica interfaccia Service Layer (`viceconti-attivita-v01.html`)

- Aggiunta colonna checkbox con select-all nell'header della tabella.
- Aggiunto badge "N selezionate" e bottone "📅 Sincronizza su Calendar" nella toolbar.
- Il bottone è disabilitato finché non si seleziona almeno un'attività.
- Dopo l'invio: righe marchiate con bordo verde, checkbox disabilitati, banner di esito.
- Resi editabili i campi Data di inizio e Ora di inizio con salvataggio via SAP Service Layer (PATCH).
- Corretto il comportamento dei campi editabili: `onchange` sostituito con `onblur` per evitare salvataggio prematuro durante la digitazione.
- Aggiunti attributi `title` a tutti gli span editabili per preservare il valore originale all'apertura dell'editor.
- Rimossa l'editabilità del campo Oggetto (Subject) — SAP lo tratta come intero (codice lista), non testo libero.
- Configurato `N8N_WEBHOOK_URL` con l'URL di produzione ngrok.

### Configurazione n8n — workflow sync-calendar

- Creato workflow con 4 nodi: Webhook → Split Out → Code → Google Calendar (Create Event).
- **Webhook**: POST, path `sync-calendar`, attivo in produzione.
- **Split Out**: spacca l'array `body.attivita` in item singoli.
- **Code**: prepara il payload Calendar con routing calendarId (ASSISTENZA TECNICA vs CONSEGNE), calcolo ora fine +1h senza conversione UTC, descrizione strutturata con N. Attività SAP.
- **Google Calendar**: credenziale OAuth `vicecontisnc@gmail.com` configurata, Calendar by ID da expression, tutti i campi mappati.
- Risolto problema fuso orario: ora fine calcolata come stringa senza passaggio per `Date.toISOString()` che sottraeva 2 ore (UTC+2).
- Configurato `N8N_EDITOR_BASE_URL=http://localhost:5678` per bypassare il blocco OAuth su ngrok.
- Aggiunto `vicecontisnc@gmail.com` come utente di test nel progetto Google Cloud `n8n-viceconti`.
- Abilitata Google Calendar API nel progetto `n8n-viceconti`.
- Risolto problema modalità Code: cambiato da "Run Once for All Items" a "Run Once for Each Item".
- Timezone n8n impostato su `Europe/Rome`.

### Routing calendari

- Attività con `ActivityType = ASSISTENZA` → calendario **ASSISTENZA TECNICA** (`vicecontisnc@gmail.com`)
- Attività con `ActivityType = CONSEGNE` → calendario **CONSEGNE** (`iur82rio0kb0545l3h2puertqg@group.calendar.google.com`)
- Testato con attività reali per entrambi i tipi — eventi creati correttamente.

---

## Decisioni prese

- **Interfaccia di innesco**: Service Layer (`viceconti-attivita-v01.html`) anziché portale SQL — dati live, `ActivityCode` disponibile direttamente per il caso d'uso 2.
- **Durata default**: +1 ora rispetto all'ora di inizio se `EndTime` non è valorizzato. Se `EndTime` è presente in SAP viene usato.
- **Routing per tipo**: due calendari distinti (ASSISTENZA TECNICA / CONSEGNE) allineati alla struttura esistente di Google Calendar di `vicecontisnc@gmail.com`.
- **Salvataggio data/ora**: `onblur` invece di `onchange` per evitare salvataggi prematuri durante la digitazione.
- **Campo Oggetto (Subject)**: non editabile da interfaccia — SAP richiede un intero (codice lista), non testo libero.
- **Payload verso n8n**: include `ActivityCode` — prerequisito essenziale per il caso d'uso 2 (esito intervento → `U_Esito` SAP).

---

## Prossimi passi

1. **Caso d'uso 2** — il tecnico aggiorna la descrizione dell'evento Calendar a fine intervento → n8n intercetta la modifica → scrive il testo in `U_Esito` dell'Attività SAP corrispondente. Prerequisito: salvare `EventId` ↔ `ActivityCode` al momento della creazione evento (via `extendedProperties` Calendar).
2. **Prevenire duplicati** — prima di creare un evento, verificare se esiste già un `EventId` associato all'`ActivityCode`. Se esiste: fare `Update` invece di `Create`.
3. **Durata default da SAP** — verificare dove si configura la durata predefinita delle attività nel client SAP B1 (Impostazioni generali o Impostazioni utente).
4. **Ngrok URL statico** — l'URL ngrok cambia a ogni riavvio. Valutare un URL fisso (ngrok plan a pagamento) o alternativa locale per rendere il webhook stabile senza dover aggiornare il file HTML.
5. **Token GitHub** — scade il 9 maggio 2026. Rigenerare prima della scadenza.

---

## Blocchi o dipendenze

- **Ngrok URL variabile**: ogni riavvio di ngrok genera un nuovo URL. Il webhook di produzione e la variabile `N8N_WEBHOOK_URL` nell'HTML devono essere aggiornati manualmente. URL attuale: `https://karlene-apsidal-ruminantly.ngrok-free.dev`.
- **Duplicati**: il workflow attuale esegue sempre `Create` — non controlla se l'evento esiste già. Finché non è implementato il caso d'uso 2 (con salvataggio EventId), evitare di cliccare Sincronizza più volte sulla stessa attività.
- **VPN per accesso remoto Service Layer**: Vincenzo Strazzullo da consultare per esposizione Service Layer senza VPN — necessario per accesso futuro dei tecnici da tablet in campo.

---

*Sessione del 22 aprile 2026 — Viceconti s.n.c.*
