# MEMORIA E CONTESTO AI — Riepilogo del 9 maggio 2026

*Sessione pomeridiana del 9 maggio 2026, dalle 13:00 alle 18:00 circa.*
*Continua da: MEMORIA E CONTESTO AI RIEPILOGO 09_05_2026.md (mattina, ore 12:03).*

---

## Stato attuale

L'infrastruttura Contesto AI è confermata operativa e robusta dal lavoro mattutino di Cowork. La sessione pomeridiana ha aperto una **fase di sperimentazione e formazione su AI-JSON-DOCUMENTI IPERTESTUALI**, con tre risultati principali:

1. **Hub Documentale è stato esteso** per accogliere file `.md` (whitelist aggiornata su tutti e tre gli script indexer).
2. **Service Layer SAP è stato verificato in produzione** (via estrazione concreta di una fattura completa via Postman/Code).
3. **Il pattern del "framework intermedio"** descritto da Vincenzo ad aprile è stato riconosciuto come obiettivo del progetto 390 FastAPI, e tutti i pezzi necessari risultano già a disposizione.

Inoltre, due interventi tecnici di emergenza sono stati gestiti durante la sessione: rigenerazione del token GitHub `Viceconti Hub` scaduto oggi, e diagnosi di un comportamento di cache di sessione del fetch tool che era già documentato ma è stato osservato empiricamente.

---

## Lavoro svolto in questa sessione

### 1. Verifica esito sync Cowork e diagnosi cache

Confermato col contenuto incollato dall'utente che il repo contesto-ai era pulito e coerente (1 voce SINTESI, 12 RIEPILOGHI, 4 DOC con solo l'ultimo REPORT). Discrepanza iniziale con la mia versione fetchata era dovuta a snapshot di 27 minuti prima.

**Test empirico sulla cache di sessione del fetch tool**: rifornire lo stesso URL nella stessa chat NON aggira la cache. Il fetch tool restituisce identica versione cached. Risultato del test riproducibile: pixel-perfect identico. Confermato come limite strutturale del canale, non bug.

**Distinzione dei due livelli di cache**:

| | Cache CDN GitHub | Cache sessione fetch |
|---|---|---|
| Dove vive | Server (nodi distribuiti) | Client (sessione Claude) |
| Durata tipica | 5–30 min | Tutta la chat |
| Cosa la invalida | TTL o purge | Solo nuova chat |
| Controllabile | Sì (cambiando backend) | No |

### 2. Esplorazione strutturale di Hub Documentale come dispositivo navigabile

Analisi del file `catalogo.json` (16 MB, 24.728 file totali, 8 sezioni, profondità albero fino a 7 livelli). Confronto strutturale con `index.json` di contesto-ai:

- **index.json contesto-ai**: piatto a 3 array (sintesi, riepiloghi, doc), record uniformi, navigazione "lista"
- **catalogo.json hub-documentale**: albero ricorsivo profondo, gerarchia path = significato semantico, navigazione "discesa"

Stessi mattoni JSON, due forme diverse a seconda del dominio. Il pattern operativo per AI è identico: ogni nodo espone metadata sufficienti per decidere se scendere/fetchare, senza dover caricare l'intero contenuto.

Lettura del README INDICIZZATORI HUB DOCUMENTALE v3.0. Identificate 3 modalità di consumo del documento da parte di un'AI:
1. **Contesto in chat** — README pre-caricato
2. **Lookup on-demand** — fetch quando serve
3. **Istruzioni operative** — README come script in linguaggio naturale per AI agentica

### 3. Diagnosi e fix token GitHub scaduto

**Sintomo**: alle 16:29 lo script `indexer_rapido.py` ha restituito 404 al download del catalogo.

**Diagnosi**: il token fine-grained `Viceconti Hub` è scaduto oggi (9 maggio 2026). Il timing combacia: la scansione notturna era partita alle 00:00 col token ancora valido (chiusura ~07:49 UTC = 09:49 ora italiana, salvataggio copia locale del catalogo). Il rapido alle 16:29 ha trovato token scaduto.

**Perché 404 invece di 401**: il repo `viceconti-hub/hub-documentale` è privato. Token scaduto → richiesta non autenticata → GitHub nasconde l'esistenza del repo con 404 invece di rivelarla con 401.

**Inventario token confermato dalla pagina Fine-grained PAT**:

| Token | Scadenza | Repo | Uso |
|---|---|---|---|
| viceconti-hub push 2026-2027 | 2 mag 2027 | contesto-ai + piattaforma-ai | Push piattaforma AI-readable |
| push-contesto-ai | 6 apr 2027 | contesto-ai | Possibile duplicato del precedente, da verificare |
| Viceconti Hub | scaduto oggi → 9 mag 2027 | hub-documentale | Indexer su PC Lauria |
| SAP Query Engine | scade oggi → 9 mag 2027 | portale | Query Engine su server SAP |

**Azione**: "Regenerate token" su `Viceconti Hub` eseguito da utente. Token sostituito in `C:\Viceconti\archivio-indicizzatore\.env`. Test rapido alle 16:55-17:00 ha confermato funzionamento (174 file in FILE TEMPORANEI). Anche `SAP Query Engine` è stato rigenerato (entrambi ora scadono il 9 maggio 2027).

### 4. Estensione whitelist .md su Hub Documentale

**Problema osservato**: dopo aver caricato `510.SINTESI_RIEPILOGHI_COWORK` in `FILE TEMPORANEI` (17 file), solo 4 file passavano nel catalogo (index.html, index.json, settings.local.json di .claude, e un altro). Tutti i `.md` filtrati silenziosamente.

**Causa**: la whitelist `ESTENSIONI_AMMESSE` negli script `indexer.py / indexer_sezione.py / indexer_rapido.py` non includeva `.md`. Scelta originale ragionevole quando l'Hub serviva solo documenti business (PDF, DOC, XLSX), ma blocker quando si vuole canale per documenti AI-readable.

**Modifica applicata**: aggiunto `'.md'` al set `ESTENSIONI_AMMESSE` in tutti e tre i file (devono restare allineati per coerenza fra scansione completa, manuale, rapida).

**Esito post-modifica**: 191 file in FILE TEMPORANEI (era 170 prima della cartella, 174 dopo cartella senza .md). I 17 .md ora indicizzati e visibili nell'Hub.

### 5. Verifica empirica AI-readability del canale Dropbox

Test su tre AI mainstream chiedendo di recuperare il contenuto di un `.md` dal link Dropbox condiviso esposto dal catalogo:

| AI | Comportamento | Esito |
|---|---|---|
| Claude (con tool bash) | `web_fetch` bloccato da robots.txt → bypass con curl | ✅ recupera |
| ChatGPT | Tenta, fallisce, dichiara apertamente il limite | ❌ ma onesto |
| Gemini | Ignora il link, cerca solo in Google Drive/Gmail | ❌ con errore di interpretazione |

**Diagnosi**: Dropbox espone shared link via path `/scl/fi/...` ma blocca i crawler con robots.txt. AI rispettose dello standard (web_fetch puro) sono bloccate. Solo AI con tool di esecuzione (bash + curl) possono bypassare. Gemini ha un comportamento più rigido: non guarda nemmeno fuori dal proprio ecosistema Google.

**Implicazione strategica**: GitHub raw URL è il canale di scelta per AI-readability universale, perché:
- Robots.txt permissivo
- HTTP standard senza preview/redirect
- Lavora con tutti i `web_fetch` mainstream senza richiedere bypass

Dropbox via Hub funziona per consumo umano. Per AI universale, pointing a `raw.githubusercontent.com/viceconti-hub/contesto-ai/...`.

**Bug minore identificato**: l'URL Dropbox generato da `indexer.py` ha query string malformata: `?rlkey=...&dl=0?raw=1` invece di `?rlkey=...&dl=0&raw=1`. Funziona per tolleranza di Dropbox, ma è formalmente errato. Da fixare quando ci sarà occasione.

### 6. Estrazione fattura via Service Layer e trasformazione semantica

**Correzione su informazione datata**: avevo creduto inizialmente che il Service Layer fosse ancora bloccato (basandomi sulla Sintesi Ecosistema 8 marzo, file di progetto). L'utente ha corretto: "Service Layer è sbloccato da molte settimane, è uno dei pilastri principali in produzione". Aggiornamento dello stato preso.

**Lezione meta**: ho ricreato esattamente il problema della "memoria datata" che il sistema CONTESTO AI è stato costruito per evitare, perché ho usato i file di progetto allegati invece di fetchare i riepiloghi correnti su GitHub. È esattamente il pattern descritto nella nota AudioPen del 31 marzo. Da annotare nei prompt di sistema dei progetti: "in caso di contraddizione fra file allegati e riepiloghi su GitHub, prevale il riepilogo su GitHub".

**Estrazione concreta**: l'utente ha estratto via Postman/Code la fattura 2600097 (BRUTIUM SRL, 8 maggio 2026, 9.579,83 €) ottenendo il JSON pieno del Service Layer.

**Caratteristiche del JSON SAP completo**:
- ~250 campi a livello header
- ~120 campi per ogni riga (8 righe nella fattura)
- Totale: ~1.210 campi
- Maggioranza `null` o `tNO` (campi per funzionalità inerti: GST indiana, withholding USA, transito brasiliano, eDoc Cina, ecc.)

**Trasformazione semantica eseguita**: riduzione manuale a JSON essenziale di ~30 campi totali, mantenendo solo informazione operativa (testata, righe con codice/desc/qta/prezzo/sconto/totale, totali finali, pagamento). Riduzione del 97% senza perdita informativa per il dominio Viceconti.

### 7. Riconoscimento del punto 4 dell'email Vincenzo

L'utente ha ripreso l'email di Vincenzo Strazzullo del 24 aprile 2026, dove al punto 4 descriveva: *"si può creare un framework intermedio che aspetta una chiamata REST API e che restituisce il documento (indicando tipo e numero) in formato JSON"*.

**Riconoscimento condiviso**: quello che è stato fatto manualmente in questa sessione (Postman → Service Layer → JSON pieno → JSON essenziale → uso umano/AI) è la **prova di concept manuale del framework intermedio**. Il progetto 390 PIATTAFORMA AI-READABLE / FastAPI già in roadmap è esattamente questo framework, automatizzato.

| Strato del framework | Cosa fa | Cosa è stato fatto a mano oggi |
|---|---|---|
| Endpoint HTTP | Riceve chiamata REST | Postman call |
| Backend | Auth + fetch da Service Layer | Auth Postman + GET Invoice |
| Trasformatore | JSON pieno → JSON essenziale | Riduzione manuale 97% |
| Risposta | Restituisce JSON essenziale | Visualizzazione in chat |

I quattro strati esistono come operazioni distinte oggi. Il salto a FastAPI è "metterli in pipeline automatica con un endpoint solo".

### 8. Decisione di pulizia file di progetto Claude.ai

L'utente ha proposto di eliminare i file di progetto datati. Conferma: i file di progetto sono fotografie statiche, mentre il sistema CONTESTO AI ha disciplina di aggiornamento. Mantenere entrambi crea drift.

**Categorizzazione**:
- **Categoria A — Riepiloghi datati con equivalente su GitHub**: eliminabili senza pensieri (Sintesi 08/03, HUB 04/03, n8n 17/02, MAPPA 12/03, SAP_SERVICE_LAYER 15/03, segretaria-artificiale gennaio, REPORT 17/03, ISTRUZIONI vecchio, file .txt vuoti)
- **Categoria B — Fonti grezze non sintetizzate altrove**: meritano destino diverso (TRASCRIZIONE_GEMINI_LIVE, TRASCRIZIONE_AUDIOPEN_31_MARZO). Decidere se archiviare in cartella tipo `530.TRACCE_E_FONTI_GREZZE` su Drive, citare nei riepiloghi correlati, o eliminare.

---

## Decisioni prese

1. **Whitelist `.md` aggiunta** a `indexer.py`, `indexer_sezione.py`, `indexer_rapido.py`. Cambia il dominio coperto dall'Hub Documentale: ora include anche documenti AI-readable, non solo business.

2. **Token separati per dominio funzionale confermati come pattern**: `Viceconti Hub` (indexer), `SAP Query Engine` (Query Engine sul server SAP), `viceconti-hub push 2026-2027` (push piattaforma AI-readable), `push-contesto-ai` (verificare se sia duplicato). Naming dei token con anno di scadenza (es. "2026-2027") adottato come convenzione.

3. **GitHub raw è il canale principale per AI-readability**, non Dropbox. I `.md` dei riepiloghi continuano a vivere in tre posti (Drive locale, contesto-ai su GitHub, Hub Documentale via Dropbox). Per fetch automatico nei prompt di sistema delle AI: usare sempre `raw.githubusercontent.com`.

4. **File di progetto Claude.ai datati (Categoria A) da eliminare**. Il blocco "CONTESTO AGGIORNATO — FETCH AUTOMATICO" già garantisce contesto fresco per ogni chat — i file allegati sono ridondanti e fonte di drift.

5. **Schema JSON essenziale come pattern di trasformazione**. Per ogni tipo di documento SAP (offerta, ordine, consegna, fattura) andrà definito uno schema essenziale del dominio Viceconti, separato dal payload pieno SAP. La trasformazione tra i due è il cuore del framework intermedio (futuro FastAPI).

6. **Service Layer SAP confermato in produzione, è un pilastro**. Da aggiornare nelle Sintesi e nei riepiloghi: "stato Service Layer = operativo, in produzione" (correzione rispetto alle versioni datate marzo).

---

## Prossimi passi

### Pulizia e messa a regime (urgenti, basso impegno)

1. **Eliminare file di progetto Claude.ai Categoria A** in tutti i progetti dove sono ancora presenti
2. **Decidere destino Categoria B** (trascrizioni AudioPen + Gemini Live)
3. **Aggiornare riepilogo progetto SAP SERVICE LAYER** con stato "in produzione" (correzione di info datata)
4. **Annotare nei riepiloghi MEMORIA E CONTESTO AI**: cache di sessione del fetch tool come pattern noto, non bug
5. **Aggiungere nota nei prompt di sistema dei progetti**: "in caso di contraddizione fra file allegati e riepiloghi su GitHub, prevale il riepilogo su GitHub"

### Manutenzione tecnica (medio termine)

6. **Annotare scadenza nuovi token (9 maggio 2027)** in posto visibile. Quando Calendar (progetto 360) sarà attivo: evento ricorrente con anticipo di 14 giorni per ogni token gestito
7. **Verificare se `push-contesto-ai` è duplicato** di `viceconti-hub push 2026-2027` ed eventualmente eliminarlo
8. **Fixare bug query string** in `indexer.py` (URL Dropbox malformata: `dl=0?raw=1` → `dl=0&raw=1`)
9. **Considerare aggiunta di `.claude` alle cartelle escluse di sistema** negli script indexer
10. **Decidere strategicamente**: `.md` AI-readable nell'Hub Documentale insieme ai documenti business, oppure sezione dedicata? L'esperimento di oggi fornisce materiale empirico per decidere

### Sviluppo strategico (progetto dedicato)

11. **Progetto 390 PIATTAFORMA AI-READABLE / FastAPI**: il framework intermedio descritto da Vincenzo ad aprile, riconosciuto oggi come obiettivo concreto. Pezzi disponibili:
    - Service Layer operativo (auth + GET su documenti)
    - Schema essenziale identificato (riduzione 97%)
    - Pattern di consegna a AI/umani validato (GitHub raw)
    - Email Vincenzo come specifica funzionale di partenza
    
    Il prossimo punto del programma utente sarà portato avanti in questo progetto dedicato.

---

## Blocchi o dipendenze

- **Cache di sessione del fetch tool**: limite strutturale del canale, non controllabile dall'utente. Mitigazioni: incollare contenuto direttamente, oppure aprire chat nuova. Da documentare come comportamento noto, non come bug.

- **Token SAP Query Engine**: anche questo scadeva oggi. Andrebbe rigenerato e il nuovo valore aggiornato nel `queries.json` sul server SAP. Se non fatto entro la sessione, prossima esecuzione del Query Engine fallirà.

- **Repo `viceconti-hub/portale`**: probabilmente privato come `hub-documentale`. Da verificare per eventuale fetch di JSON da parte di AI esterne (anche se in pratica per i JSON business il canale principale è il Service Layer via FastAPI futuro).

---

## Considerazioni strategiche emerse

### Pattern del payload polifonico applicato ai .md

Lo stesso file `.md` ora vive in tre posti contemporaneamente con tre "facce" diverse:
- **Drive locale** (`510.SINTESI_RIEPILOGHI_COWORK`) — formato editor, nomi con spazi
- **GitHub raw** (`viceconti-hub/contesto-ai`) — formato raw, nomi con underscore, ottimo per AI fetch
- **Hub Documentale via Dropbox** — vetrina human-readable, nomi con spazi, link condiviso

Stesso contenuto, tre canali di accesso, ognuno ottimizzato per un modo di consumo. È il pattern del payload polifonico (Sintesi Ecosistema sez. 4) applicato non al singolo file JSON ma alla **distribuzione di un documento**.

### Pattern della trasformazione semantica

Il payload SAP Service Layer è "ricco e generale" (250+ campi, copre ogni caso d'uso globale). Il payload essenziale Viceconti è "stretto e contestuale" (30 campi, copre il caso d'uso reale). La pipeline che trasforma il primo nel secondo è il cuore del framework intermedio.

Questa stessa logica vale per qualsiasi consumo di API esterne ricche: Dropbox API, GitHub API, PrestaShop API, eccetera. Sempre lo stesso pattern: dominio generale → dominio specifico → consumo applicativo. Riconosciuto in chiave architetturale.

### Hub Documentale come dispositivo "accidentalmente" AI-readable

L'aggiunta di `.md` alla whitelist non era prevista nel design originale dell'Hub Documentale (nato per documenti business). Eppure il sistema esistente ha rivelato di avere già tutti i mattoni necessari per accogliere il nuovo dominio:
- Indice (catalogo.json)
- URL dereferenziabili (link Dropbox)
- Presentazione in browser (testo grezzo per i .md)
- Catalogazione strutturata (path, sezione, dimensione)

Una micro-modifica chirurgica (una riga in tre script) ha attivato funzionalità che erano latenti. È esattamente il caso del "costruire sopra l'esistente" che l'utente voleva esplorare ad inizio sessione.

### La memoria datata vissuta in prima persona

L'errore sul Service Layer "ancora bloccato" è stato un caso di studio vivo del problema che il sistema CONTESTO AI è stato disegnato per risolvere. L'AI ha letto i file di progetto allegati (datati 8 marzo) invece di fetchare i riepiloghi correnti su GitHub. Il sistema esiste già per evitarlo, ma richiede disciplina di consumo da parte dei Claude futuri.

**Dato osservativo**: la presenza di file di progetto datati in claude.ai non è una "comodità in più", è una fonte attiva di drift. La pulizia di Categoria A non è cosmetica, è igiene del sistema.

### Chiusura del giro

L'utente ha aperto la sessione con: *"voglio capire bene quello che ho già costruito, perché magari significa costruire sopra qualcosa di esistente, con evidenti vantaggi"*.

L'ha chiusa con: il riconoscimento che tutti i pezzi del framework intermedio descritto da Vincenzo ad aprile sono già a disposizione, e che il prossimo passo (FastAPI) non è "costruire da zero" ma "mettere in pipeline pezzi che già esistono ed esibiscono il comportamento giusto in isolamento".

Questa è la conclusione naturale del giro di ricognizione. Il sistema sapeva già fare quello che doveva fare; serviva una sessione di "vederlo insieme" per renderlo visibile a chi lo aveva costruito un pezzo alla volta nel tempo.

---

*Sessione del 9 maggio 2026, pomeriggio — Sperimentazione Hub Documentale come canale AI-readable, token GitHub rigenerati, Service Layer verificato in produzione, riconoscimento del punto 4 dell'email Vincenzo come obiettivo del progetto FastAPI.*
*Continua da: MEMORIA E CONTESTO AI RIEPILOGO 09_05_2026.md (mattina)*
*Prossima sessione: progetto dedicato 390 PIATTAFORMA AI-READABLE / FastAPI*
