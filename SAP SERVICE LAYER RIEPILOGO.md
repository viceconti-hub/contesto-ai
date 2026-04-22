# SAP Service Layer — Riepilogo del 22 aprile 2026

## Stato attuale

Il Service Layer è operativo in produzione. L'interfaccia `viceconti-attivita-v01.html` è il punto centrale di sviluppo attivo: gestisce le Attività SAP in lettura/scrittura e ora è anche il punto di innesco del workflow SAP → Google Calendar, operativo dal 22/04/2026.

---

## Architettura corrente

### Infrastruttura
- **Service Layer:** `https://192.168.122.99:50000/b1s/v2/` — operativo
- **Server:** SQLPRD0303 (192.168.122.99) — stabilizzato dopo riavvio del 03/04/2026 (RAM al 63%, era al 92%)
- **Proxy locale:** `proxy.py` su localhost:8080 per sviluppo da PC esterno (gestisce certificato self-signed)
- **Credenziali SAP:** manager / viceconti

### Interfaccia `viceconti-attivita-v01.html` — stato al 22/04/2026

Funzionalità operative:
- Lettura attività da Service Layer (OCLG) con filtro data e stato
- Editing inline: `U_Esito`, `Notes`, `StartDate`, `StartTime` con salvataggio PATCH su SAP
- Chiusura/riapertura attività via API (il client SAP blocca la riapertura, il Service Layer la esegue)
- Creazione nuova attività con form completo incluso `PersonalFlag`
- **Selezione multipla con checkbox + bottone "📅 Sincronizza su Calendar"** — invia le attività selezionate all'n8n webhook per creazione eventi Google Calendar
- Badge "N selezionate", conferma visiva post-invio (bordo verde su righe), banner esito

### Campi editabili inline
| Campo SAP | Comportamento |
|---|---|
| `U_Esito` | Dropdown, salvataggio PATCH immediato |
| `Notes` | Testo libero, salvataggio su `onblur` |
| `StartDate` | Data, salvataggio su `onblur` |
| `StartTime` | Ora, salvataggio su `onblur` |

**Nota:** campo `Subject` (Oggetto) non editabile — SAP richiede intero (codice lista), non testo libero.

---

## Workflow SAP → Google Calendar (n8n)

### Architettura
```
viceconti-attivita-v01.html
→ selezione checkbox attività
→ POST webhook n8n (sync-calendar)
→ Split Out → Code → Google Calendar Create Event

Routing per tipo:
• ActivityType = ASSISTENZA → calendario ASSISTENZA TECNICA (vicecontisnc@gmail.com)
• ActivityType = CONSEGNE → calendario CONSEGNE (iur82rio0kb0545l3h2puertqg@group.calendar.google.com)
```

### Mapping campi SAP → Calendar
| Campo Calendar | Fonte SAP |
|---|---|
| Titolo | `CardName + Details` |
| Data/ora inizio | `StartDate + StartTime` |
| Data/ora fine | `EndTime` se presente, altrimenti `StartTime + 1h` |
| Location | `City` |
| Descrizione | `TecName + ActivityType + N. Attività (ActivityCode)` |

### Configurazione n8n
- **URL webhook:** `https://karlene-apsidal-ruminantly.ngrok-free.dev/webhook/sync-calendar`
- **Fuso orario n8n:** `Europe/Rome`
- **Ora fine:** calcolata come stringa per evitare conversione UTC che sottraeva 2 ore
- **Google OAuth:** `vicecontisnc@gmail.com`, progetto Google Cloud `n8n-viceconti`

### Variabile nell'HTML
```javascript
const N8N_WEBHOOK_URL = 'https://karlene-apsidal-ruminantly.ngrok-free.dev/webhook/sync-calendar';
```

---

## Prossimi sviluppi previsti (priorità)

### Caso d'uso 2 — Calendar → SAP (U_Esito)
Il tecnico aggiorna la descrizione dell'evento Google Calendar a fine intervento → n8n intercetta la modifica → scrive il testo in `U_Esito` dell'Attività SAP corrispondente.

**Prerequisito:** salvare `EventId` ↔ `ActivityCode` al momento della creazione evento. Meccanismo: `extendedProperties` di Google Calendar per memorizzare l'`ActivityCode`, oppure log su file/database.

### Prevenzione duplicati
Il workflow attuale esegue sempre `Create`. Implementare logica: prima di creare, verificare se esiste già un evento con `ActivityCode` nelle `extendedProperties`. Se esiste → fare `Update` invece di `Create`.

### Task Scheduler per `crea_moduli_vuoti.py`
Script operativo ma non ancora schedulato sul PC Lauria — da configurare con Task Scheduler.

---

## Conoscenza tecnica consolidata Service Layer

| Aspetto | Dettaglio |
|---|---|
| Autenticazione | Cookie `B1SESSION` via `POST /Login`, gestito automaticamente da `requests.Session()` |
| Riapertura attività | `PATCH /Activities(code)` con `Closed: "tNO"` — bypass del blocco client SAP |
| Chiusura attività | `PATCH /Activities(code)` con `Closed: "tYES"` |
| Editing parziale | `PATCH` con solo i campi da modificare — SAP aggiorna solo quelli |
| Fatture servizi | `POST /PurchaseInvoices` con `DocType: "dDocument_Service"` + `VatDate` obbligatorio |
| Storno fattura | `POST /PurchaseInvoices(DocEntry)/Cancel` — unica operazione disponibile |
| DELETE fattura | Bloccato anche via API (errore -5006) |
| Bozze/Drafts | `POST /Drafts` con `DocObjectCode` — comportamento anomalo non risolto, registra invece di parcheggiare |
| DocEntry vs DocNum | Service Layer usa sempre DocEntry (chiave interna DB), non DocNum (numero visibile) |

---

## Domande aperte per Vincenzo Strazzullo
(email inviata 11/04/2026, risposta in attesa)

1. Payload corretto per documento parcheggiato via `/Drafts`
2. B1iF — installato? Esperienza di configurazione?
3. Tool PDF SAP — funzionamento, costi, cartella destinazione configurabile
4. Esposizione Service Layer senza VPN per tablet tecnici

---

## Credenziali e scadenze urgenti

| Credenziale | Scadenza | Priorità |
|---|---|---|
| **Token GitHub** | **9 maggio 2026** | **🔴 Rigenerare subito** |
| Google OAuth n8n | 2027 | — |
| Token Dropbox OAuth2 | Nessuna | — |

---

## File e path rilevanti

| File | Path | Stato |
|---|---|---|
| `viceconti-attivita-v01.html` | GitHub Pages viceconti-hub/portale | Produzione |
| `registra_fattura.py` | C:\Viceconti\viceconti-hub | Operativo (incompleto) |
| `Indice_Fornitori_SAP.xlsx` | C:\Users\PC\Dropbox\HUB DOCUMENTALE\ACQUISTI\FATTURE DA FORNITORE | 27 fornitori mappati |
| `proxy.py` | localhost:8080 | Dev da PC esterno |

---

*Sessione del 22 aprile 2026 — Viceconti s.n.c.*
