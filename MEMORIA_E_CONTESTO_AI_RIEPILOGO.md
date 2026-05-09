```
MEMORIA E CONTESTO AI RIEPILOGO 06_04_2026.md
```

---

# MEMORIA E CONTESTO AI — Riepilogo del 6 aprile 2026

## Stato attuale

Il progetto è passato dalla fase di sperimentazione alla fase operativa. L'infrastruttura GitHub Pages è validata, lo script di push automatico è scritto e testato, il flusso operativo è definito. La catena completa — dalla produzione dei riepiloghi alla disponibilità del contesto per le AI — funziona end-to-end.

---

## Lavoro svolto in questa sessione

### Analisi e approfondimento

- Confronto diretto GitHub Pages vs API Memory Tool sul problema strutturale della memoria — GitHub Pages risolve il problema logistico, non quello qualitativo; API Memory Tool è la risposta strutturale per gli assistenti AI concreti
- Analisi critica con approccio adversarial: bias cognitivi identificati (effetto IKEA, entusiasmo da convergenza), premesse non dimostrate (fetch automatico non testato, qualità del contenuto aggregato, robustezza della catena, risoluzione del problema reale)
- Approfondimento della questione sicurezza: soluzione "URL oscuro" (security through obscurity) come protezione adeguata per la fase attuale; Cloudflare Access come evoluzione quando il contenuto lo richiede
- Chiarimento del ruolo di Cowork, Script PowerShell e Claude Code nel flusso: Cowork prepara, Script pusha, Claude Code mantiene

### Sviluppi operativi

- Verifica sito GitHub Pages aggiornato al 05/04/2026 con nomi file fissi senza data — allineamento completato
- Identificazione problema cache CDN GitHub Pages: il browser riceve la versione aggiornata immediatamente, il fetch delle AI passa per i nodi CDN con ritardo variabile (10-15 minuti). Limite strutturale, non risolvibile
- Script `push_github.ps1` scritto da Claude Code, testato con successo: 15/15 file processati, confronto SHA corretto (skip se invariato), retry automatico, log su file con timestamp
- Token GitHub fine-grained generato per repo `contesto-ai`, scadenza 6 aprile 2027
- Aggiunta funzionalità `last_push.json` + indicatore "Ultimo sync" nell'header del sito — si aggiorna ad ogni esecuzione dello script

### Evoluzioni concettuali

- La seconda direttrice (interrogazione conversazionale) non è solo ricerca documenti ma interrogazione su qualsiasi dato strutturato rappresentabile in forma testuale — confermato dallo screenshot degli articoli Aristarco per cantiere
- GitHub Pages non è solo memoria di contesto per i progetti Claude — diventa il layer di accesso universale ai dati operativi Viceconti per qualsiasi AI
- Connessione con lo scambio Vincenzo/Gemini sull'archiviazione automatica documenti SAP in HTML/JSON su Dropbox: se i documenti SAP vengono archiviati automaticamente, il confine tra dati SAP e dati interrogabili dall'AI scompare
- Il sito è nativamente buono sia per le AI che per gli umani — non è un compromesso ma una convergenza

---

## Decisioni prese

**Flusso operativo definitivo (senza Task Scheduler):**
- Flusso normale: Cowork sincronizza riepiloghi → lancia push_github.ps1
- Flusso manuale: aggiornamento file → lancio script manuale

**Nomi file su GitHub senza data** — link permanenti nell'index.html, contenuto sovrascritto ad ogni aggiornamento. Le date restano nei nomi dei file nelle cartelle di progetto locali.

**Sicurezza per oscurità** — accettata come protezione adeguata per la fase attuale. Evoluzione futura: Cloudflare Access quando il contenuto lo richiede.

**Percorso strategico confermato:** GitHub Pages adesso → API Memory Tool quando l'infrastruttura regge (Service Layer SAP + n8n operativi).

**Due direttrici in parallelo sull'Hub Documentale:**
- Consultazione strutturata → Hub Documentale attuale (feedback positivo dai tecnici confermato)
- Interrogazione conversazionale → GitHub Pages con indici testuali come implementazione dell'Assistente Hub Documentale, senza backend

---

## Nuove funzionalità dell'ecosistema Claude rilevanti

### Cowork Dispatch (da mobile)
Cowork è ora utilizzabile da smartphone tramite la funzione Dispatch. Il thread è unico e persistente tra desktop e mobile — stesso contesto, stessa conversazione. Da telefono si possono assegnare task che vengono eseguiti sul desktop, con notifica push al completamento. Il desktop deve essere acceso con Claude Desktop aperto.

### Cowork + Claude Code integrati
Quando Cowork riceve un task, valuta autonomamente il tipo di lavoro e apre una sessione Claude Code se il task richiede esecuzione di codice o script. Questo significa che schedulare Cowork equivale a schedulare l'intera catena, inclusa l'esecuzione di script PowerShell. Il push_github.ps1 rientra esattamente in questa categoria.

### Task schedulati in Cowork
Cowork supporta task schedulati ricorrenti configurabili una volta sola. Implicazione diretta: la catena completa (sincronizzazione riepiloghi → push GitHub Pages) può essere schedulata come task mattutino automatico, senza intervento manuale e con notifica di completamento su telefono.

**Catena completa a regime:**
```
Smartphone (messaggio o schedule)
    → Cowork Desktop
    → Claude Code (push_github.ps1)
    → GitHub Pages aggiornato
    → Notifica su telefono
```

---

## Prossimi passi

1. **Configurare task schedulato in Cowork** — sincronizzazione riepiloghi + push GitHub ogni mattina
2. **Generare primo indice testuale per categoria** — probabilmente CLIENTI, come primo caso d'uso dell'Assistente Hub Documentale su GitHub Pages
3. **Aggiungere istruzione fetch nei prompt di tutti i progetti Claude attivi** — Sintesi Ecosistema come contesto universale, riepilogo specifico nel progetto corrispondente
4. **Script PowerShell per generazione indici testuali da catalogo.json** — prerequisito per l'Assistente Hub Documentale su GitHub Pages
5. **Valutare repo separato per collaboratori** — versione filtrata del contesto con informazioni operative (non strategiche), accessibile ai tecnici via AI personale
6. **Rigenerare token GitHub esistente** — scade 9 maggio 2026, meno di 5 settimane

## Blocchi o dipendenze

- **Cache CDN GitHub Pages** — ritardo di 10-15 minuti tra push e disponibilità per fetch AI. Limite strutturale da tenere presente nell'uso operativo
- **Indici testuali non ancora generati** — il pattern dell'Assistente Hub Documentale su GitHub Pages è definito ma non implementato. Dipende dalla scrittura dello script di generazione da catalogo.json
- **API Memory Tool** — soluzione strutturale per gli assistenti AI concreti, non ancora accessibile senza backend. Prerequisiti: Service Layer SAP + n8n operativi

---

*Sessione del 6 aprile 2026 — Completamento infrastruttura push automatico e definizione evoluzioni*
*Continua da: MEMORIA E CONTESTO AI RIEPILOGO 05_04_2026.md*

— Domenica 06 aprile 2026, 11:44