# ASSISTENTE AMMINISTRATIVA — Riepilogo del 23 maggio 2026

## Stato attuale

Progetto avviato il 13 aprile 2026. Dopo circa sei settimane di operatività, l'assistente è integrato nel flusso quotidiano di Prospero con funzioni di cattura task, gestione promemoria, produzione documenti e supporto operativo. Il layer di task management si è stabilizzato su **Apple Reminders** con integrazione MCP dall'app Claude iOS. I principali limiti dello strumento sono documentati e conosciuti.

---

## Lavoro svolto in questa sessione (sintesi trasversale)

### Struttura operativa giornaliera
- Definita la bozza degli slot giornalieri: Controllo finanziario (8:00–8:30), Programmazione tecnici (8:30–9:00), Programmazione Prospero (9:00–9:30), Operativo (9:30→).
- Identificata automazione immediata: apertura automatica home banking + Excel + SAP via batch Windows / Task Scheduler.

### Gestione task — evoluzione strumento
- Percorso documentato: Google Keep → Promemoria Apple → Google Tasks (problema accesso desktop Windows) → **Apple Reminders via iCloud** (soluzione adottata).
- Struttura liste Reminders consolidata:

| Lista | Tipo | Uso |
|---|---|---|
| Chiamate di Servizio | Lista normale | Inserimento diretto |
| Offerte | Lista normale | Inserimento diretto |
| Da Fare | Lista normale (default) | Task operativi generali |
| In Attesa | Lista normale | Task in sospeso |
| Urgenti | Smart list (tag `#Urgenti`) | Vista trasversale |
| Sviluppo Architettura IT | Lista normale | Task tecnici e IT |
| Prima o Poi | Lista normale | Backlog |

### Test MCP Reminders — mappa completa
| Piattaforma | Disponibilità |
|---|---|
| App Claude iOS | ✅ Funziona |
| Browser desktop | ❌ Non disponibile |
| Claude Desktop MacBook Air | ❌ Non disponibile |

**Limiti confermati:** tag non leggibili né scrivibili via MCP. Il tag `#Urgenti` va aggiunto manualmente dall'app dopo la creazione del task. Le smart list non sono accessibili come liste normali.

### Integrazione Gmail MCP
- Standard filtri Gmail per verifica registrazione documenti SAP definito per 5 fornitori:

| Fornitore | Mittente | Tipo |
|---|---|---|
| Fimar | noreply@fimargroup.it | Conferme ordine + DDT |
| Sirman | noreply@sirman.com | Solo DDT |
| Morini | noreply@morinionline.it | Fatture + DDT (casella mista) |
| Klimaitalia | no-reply@klimaitalia.it | Ordini + Conferme ritiro |
| Sambonet Paderno | noreply@sambonet.it | Fatture + DDT + Distinte (casella mista) |

- Logica di controllo: `in:inbox = 0 risultati` → tutti i documenti del periodo registrati in SAP.
- Documento brandizzato prodotto: **Standard_Gmail_Filtri_Fornitori.docx** (A4 landscape, logo Viceconti).

### Apple Notes MCP
- Estensione attivata su MacBook Air (v0.1.7).
- Limite identificato: note scritte con Apple Pencil → immagine PNG base64, non testo leggibile.
- Solo le note scritte da tastiera sono leggibili come testo.
- Flusso confermato: Apple Pencil → AudioPen → Claude (passaggio AudioPen resta necessario).

### Ricerca e test canali fetch
Sessione di testing sistematico dei canali di consegna URL a Claude:

| Canale | web_fetch | curl via bash_tool |
|---|---|---|
| URL incollato in chat | ✅ | ✅ |
| URL da Reminders | ❌ (permissions) | ✅ |
| URL da Gmail | ❌ (permissions) | ✅ (se dominio consentito) |
| GitHub raw | ❌ (cache stantia) | ✅ (contenuto fresco) |
| piattaforma-ai.onrender.com | N/A | ✅ |
| Siti consumer (es. nationalgeographic.it) | ❌ | ❌ (egress policy) |

**Conclusione architettonica:** `curl` via `bash_tool` è il mattone fondamentale. I canali (Reminders, Gmail, prompt) sono solo modi diversi di consegnare URL a curl. La cache opaca di `web_fetch` di Anthropic è confermata empiricamente (5+ giorni di staleness su GitHub raw).

### Fix contesto AI — URL GitHub
- Problema risolto: URL nei prompt usavano `/refs/heads/main/` → corretto in `/main/`.
- Bozza Gmail creata come canale alternativo per consegna contesto (subject: `[CONTESTO AI] SINTESI ECOSISTEMA VICECONTI`).
- Download allegati Gmail: **non supportato** dal Gmail MCP (confermato).

### Attività operative registrate in sessione
Decine di task inseriti in Reminders nelle liste appropriate:
- Chiamate di Servizio: Bar 7 Praia (mantecatore), Sirtaki (friggitrice), Central Pausa (produttore ghiaccio + armadio combinato), Chiacchio Mansueto (affettatrice casa), e altri.
- Da Fare / Urgenti: task operativi quotidiani (fornitori, clienti, banca, SAP).
- Sviluppo Architettura IT: PLAUD NotePin S, Gmail MCP, Calendar, SAP Service Layer, Nuova Strategia, Progetto Anthropic.

### Documenti prodotti
| Documento | Formato | Contenuto |
|---|---|---|
| Offerta Sammartino Isacco | .docx brandizzato | ICOOL 401 WHT/BLK + NORA 380 |
| Standard Gmail Filtri Fornitori | .docx brandizzato A4 landscape | 5 fornitori, 10 filtri, logica SAP |
| Fimar_Importazione_SAP | .xlsx | Articoli con prefisso FIM. |
| Articoli REPA + IDEAM import | .xlsx | 9 articoli da 2 fornitori |
| Check chiusura giornata | React artifact | Tool interattivo con checkbox e note |

### Analisi clienti e documenti
- Estratti articoli SIR. (Sirman) da storico cliente Happy Moments (13 righe).
- Riepilogo potenza gas 3 macchine Zanussi EVO900 (friggitrice 42 kW, cucina 44 kW, cuocipasta 16,5 kW — totale 102,5 kW / 88.150 kcal/h).
- Lettura DDT/ordini da PDF e email per import SAP.

---

## Decisioni prese

- **Apple Reminders** adottato come layer principale di task management (iCloud risolve il problema accesso desktop).
- **Smart list Urgenti** mantenuta come vista trasversale su tag, non come lista separata (per evitare che i task spariscano dalla loro categoria).
- **Check chiusura giornata** standardizzato come artifact React interattivo con checkbox; screenshot come canale di feedback a Claude.
- **curl via bash_tool** adottato come strumento di fetch primario al posto di web_fetch per i documenti del contesto AI.
- **Reminders e Gmail** riconosciuti come canali di delivery di URL verso curl, non come canali di fetch autonomi.

---

## Prossimi passi

1. **Aggiornare le istruzioni di tutti i progetti** con URL GitHub corretti (`/main/` invece di `/refs/heads/main/`)
2. **Caricare ASSISTENTE AMMINISTRATIVA RIEPILOGO su GitHub** (contesto-ai) per fetch automatico
3. **Testare Gmail MCP da app Claude iOS** per confermare disponibilità (finora testato solo da browser)
4. **PLAUD NotePin S** — approfondire modalità di acquisto (task già in Sviluppo Architettura IT)
5. **Automazione apertura mattutina** — script batch Windows per home banking + Excel + SAP
6. **n8n su Mac** — prerequisito per nodi Apple nativi (Reminders, Calendar) e integrazione bidirezionale
7. **Allineamento Reminders ↔ SAP Activities** — esplorare n8n come ponte (Caso d'uso 1)
8. **Definire prefisso standard task** per Chiamate di Servizio (es. `[CS]`) per riconoscimento da parte di Claude senza tag

---

## Blocchi o dipendenze

- **MCP Reminders disponibile solo da app Claude iOS** — limita l'inserimento programmatico a quel client; da browser e Claude Desktop non funziona.
- **Tag non accessibili via MCP** — il tag `#Urgenti` richiede aggiunta manuale dall'app dopo ogni inserimento.
- **Download allegati Gmail non supportato** — il MCP restituisce metadati ma non il contenuto degli allegati; alternativa: salvataggio in Drive e accesso via Drive MCP.
- **Cache web_fetch Anthropic** — TTL apparente > 5 giorni; soluzione adottata: curl via bash_tool. Richiede toggle "Code Execution" abilitato nel progetto.
- **n8n non ancora su Mac** — prerequisito per automazioni con nodi Apple nativi.
