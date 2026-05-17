# SINTESI ECOSISTEMA VICECONTI
## Documento di visione e stato operativo

*Aggiornato al 14 maggio 2026 sera — sostituisce versione del 10 maggio 2026*

---

## 1. CHI È VICECONTI S.N.C.

**Viceconti s.n.c.** — commercio e assistenza tecnica di attrezzature professionali per la ristorazione e l'hotellerie, con sede operativa a Lauria (Basilicata).

**Soci:** Prospero Viceconti e Antonio Viceconti (quota 50/50).

**Aree di attività** (con distribuzione fatturato storica):

| Area | Quota fatturato | Note |
|------|----------------|------|
| Progettazione | ~40% | Cantieri complessi, risorsa dedicata, software CAD |
| Vendita | ~40% | Fornitura attrezzature, anche multi-macchina senza progetto |
| Assistenza Tecnica | ~20% | 2 tecnici, anche logistica consegne |
| E-commerce | ~5% | PrestaShop, stessi processi della vendita, canale diverso |

**Risorse umane:** Gianni Limongi e Biagio Rizzo (tecnici), destinati al passaggio nella nuova struttura.

**Contesto strategico:** Prospero è un imprenditore che sfrutta le nuove competenze AI per i propri progetti — non consulente per altri. La s.n.c. è in liquidazione volontaria (obiettivo luglio 2026). Nuova s.r.l. unipersonale di Prospero: avvio operativo settembre 2026.

**Vincolo principale:** Viceconti tradizionale non è ancora autonoma da Prospero. Non è un problema tecnico: richiede una decisione strutturale (risorsa umana, riduzione perimetro, o accettazione dei limiti).

Per il dettaglio della transizione societaria si rimanda al progetto NUOVA STRATEGIA SETTEMBRE 2026.

---

## 2. CORNICE STRATEGICA

> *Claude è il livello cognitivo dell'azienda. SAP è la rappresentazione strutturata della realtà aziendale. HTML First era il sostrato di interscambio iniziale; la maturazione architetturale ha rivelato che il principio sovraordinato è **API First** — la coerenza tra destinatari si garantisce a livello di fonte unica (Service Layer SAP), non di formato unificato. Da lì si proiettano formati specializzati per famiglia di consumer: PDF per umani in cantiere, JSON/XML per macchine, HTML come specializzazione situazionale.*

> *Investire in ciascun emisfero (Claude e SAP) nella propria autonomia è investire nell'integrazione futura. Ogni workflow che migliora SAP-stand-alone migliorerà anche SAP-integrato. Ogni decisione che organizza Claude-stand-alone migliorerà anche Claude-integrato.*

### SAP come system of record operativo

Riconoscimento del 05/05/2026 consolidato: le automazioni costruite per ottimizzare l'assistenza tecnica hanno prodotto un effetto laterale. Il modulo Attività di SAP è diventato l'ingresso a maggior efficienza per registrare task operativi, perché l'output è automatizzato (Calendar, Viceconti Hub, in prospettiva Telegram e AI).

**SAP sta evolvendo da "system of record passivo" (registra il passato) a "system of record operativo" (orchestra il presente).** L'evoluzione successiva consolidata questa settimana è **SAP come punto di scambio attivo nel flusso operativo** — non più applicazione da aprire/chiudere, ma nodo di scambio sempre disponibile via canali multipli (Interfaccia SL, voce, messaggistica, AI conversazionale).

### I tre livelli di integrazione Claude-SAP

| Livello | Funzione | Stato | Modalità |
|---------|----------|-------|----------|
| **L1 — Pagine pubbliche AI-readable** | Interrogazioni standard ricorrenti, dati strutturati, accesso da Claude.web senza VPN | ✅ Operativo | Piattaforma FastAPI su Render; Viceconti Hub (portale) — convergenza dentro FastAPI decisa 10/05 |
| **L2 — Claude Code da desktop** | Interrogazioni libere, esplorazioni, accesso diretto al Service Layer | ✅ Operativo | Postazione di lavoro, VPN |
| **L3 — Claude Code da mobile** | Capability scoperta 13/05/2026: Code gira nativamente da smartphone, accesso a repo `viceconti-hub`, contesto 1M token | ✅ Operativo | Smartphone, niente più Dispatch come passaggio obbligato |

### I tre nodi di propagazione fondamentali

Riconoscimento retrospettivo della struttura emergente (11-12/05/2026):

```
1. Ordine cliente (esplicito, contratto a priori)       → area vendita
2. Attività SAP (ordine di servizio implicito)          → area assistenza  ← ✅ già coperto
3. Ordine d'acquisto (bilaterale)                       → area acquisti
```

Tutti e tre i nodi propagano sequenze operative a valle. Tutto il lavoro investito nell'area assistenza negli ultimi mesi (Interfaccia SL, sync Calendar, modulo tecnico, archivio documenti SAP) è già **copertura completa del nodo 2**. Il programma futuro estende il pattern ai nodi 1 e 3.

I tre nodi convergono in **documenti omogenei a valle** (DDT come documento di chiusura, fattura come documento contabile), indipendentemente dalla natura del nodo iniziale. L'eterogeneità degli input converge nell'omogeneità dell'output.

### Nota strategica — Pattern unificato interno/B2B

Riconoscimento del 05/05/2026: il lavoro sui flussi interni (esposizione delle attività SAP al web, scrittura via FastAPI, autenticazione e controllo accessi) è strutturalmente identico al lavoro che servirebbe per un B2B esterno (clienti rivenditori). **Investimento unico, due rendimenti.**

---

## 3. STATO OPERATIVO — Maggio 2026

### Componenti in produzione

| Componente | Stato | Note |
|-----------|-------|------|
| **SAP Business One 10.0 + Service Layer** | ✅ Operativo | Attivo dal 29/03/2026 |
| **SAP Query Engine v2.5** | ✅ Operativo | Specializzato come produttore di snapshot storici |
| **Piattaforma AI-readable (FastAPI)** | ✅ Prima pietra in produzione | Render, 19.476 articoli Morini |
| **n8n v2.14.2** | ✅ Operativo | PC Lauria, ngrok dominio statico |
| **Viceconti Hub + Hub Documentale** | ✅ Operativi | GitHub Pages; convergenza FastAPI decisa 10/05 |
| **Contesto AI su GitHub Pages** | ✅ Operativo | repo `contesto-ai`, doppio binario HTML+JSON |
| **`archivio_documenti_sap.py` v2** | ✅ In produzione | Multi-tipo, 5 tipi mappati + 1 in workspace |
| **`registra_fattura.py`** | ✅ Operativo | Pipeline completa dopo riattivazione Directory Analyzer |
| **`crea_offerta.py` / `crea_moduli_vuoti.py`** | ✅ Operativi | Batch mode, dry-run, archivio automatico |
| **Cartella SAP Attachments riconfigurata** | ✅ Operativa | `C:\Users\PC\Dropbox\HUB DOCUMENTALE\DOCUMENTI_SAP\` |
| **Directory Analyzer TS Digital** | ✅ Riattivato (settimana 04-08/05) | Scarica XML ciclo attivo e passivo |
| **UDF SAP `RifFile`** | ✅ Operativo | **Scoperta 13/05: si propaga nativamente dai documenti padre ai figli SAP** |
| **Pipeline link in descrizione attività SAP → Calendar/Hub/SAP client** | ✅ Validata 10/05/2026 | Test end-to-end conclusivo |
| **ASSISTENTE AMMINISTRATIVA** | ✅ Avviata | Operativa dal 13/04/2026 |
| **Apple Reminders** | ✅ Componente adottata | Layer di task management |

### Capability scoperte questa settimana

- **Claude Code mobile (Opus 4.7 1M)** — gira nativamente da smartphone, accesso ai repo `viceconti-hub`, contesto 1M token. Capability presente, design d'uso ancora da definire.
- **Propagazione gerarchica nativa SAP** — UDF su documenti padre si propagano automaticamente ai documenti figli generati nella catena commerciale (ordine cliente → ordini d'acquisto → consegna → fattura). Effetto leva moltiplicato.
- **Campo `Stato documento` enumerato** — 5 valori canonici (TRASMESSO, CONFERMATO, in attesa pagamento bonifico, in attesa di assegno, DA COMPLETARE) — struttura già presente, non vista prima negli script di analisi.

### Evento esterno strategicamente significativo

**13/05/2026: SAP investe in n8n** (valuation 5,2 mld $), n8n viene integrato nativamente in Joule Studio. Validazione esterna della traiettoria architetturale Viceconti: la combinazione "Service Layer SAP + n8n + AI agentica + FastAPI custom" coincide con la direzione strategica del mercato enterprise. Lettura per la decisione tool Strazzullo: il calcolo dell'standby consapevole è rafforzato — i tool SDK B1iF custom rischiano di diventare legacy.

---

## 4. PRINCIPI ARCHITETTURALI CONSOLIDATI

### Principi storici (consolidati prima del 14/05)

| Principio | Descrizione |
|-----------|-------------|
| **Destinatario First** | Decidi consapevolmente quale destinatario è primario e costruisci a partire da lì |
| **API First (riformulazione di HTML First)** | La coerenza tra destinatari si garantisce a livello di fonte unica (Service Layer), non di formato. PDF, JSON, HTML sono specializzazioni equipollenti |
| **HTML come trinità** | Quando convergono umano e macchina sullo stesso destinatario: interfaccia + documento + dato strutturato in un file solo |
| **L'apparato del consumer guida la scelta del formato** | Il formato si valuta sull'apparato concreto che il consumer userà, non in astratto |
| **Duplicazione consapevole per destinatari divergenti** | Quando i destinatari hanno esigenze divergenti, conviene tenere contenitori distinti anche al costo di duplicare |
| **La struttura del Hub Documentale rispecchia le famiglie di consumer** | Tre cartelle parallele per tipo: `<TIPO>` umani, `<TIPO> XML/JSON` macchine, `<TIPO> WORKSPACE` operativo |
| **Dispositivo che cambia retroattivamente il significato del dato** | L'esistenza di un dispositivo AI cambia retroattivamente cosa ha senso produrre |
| **Scalabilità progressiva** | Non costruire mai a scala completa quando puoi validare il pattern a scala ridotta |
| **SAP genera documenti come effetto collaterale dell'anteprima** | L'azione "anteprima" deposita il PDF nella cartella configurata — comportamento esistente convertito in risorsa |
| **Archivio vs coda di lavoro come pattern strutturale** | Verbi attivi per code, nomi statici per archivi |
| **Separazione archivio-workspace** | Archivio per consultazione, workspace per task |
| **Una sola copia per chiave naturale in archivio** | Cleanup-by-DocNum sulla destinazione, mai sulla fonte |
| **Archivio canonico ≠ output SAP** | Per documenti bilaterali, il canone è esterno; il PDF SAP è bozza |
| **Completezza come condizione necessaria per archivi AI-consumati** | L'assenza è interpretata come "non esiste"; per costruzione, non per disciplina |
| **Effetto leva nelle proiezioni multiple** | Arricchire un singolo campo della fonte si propaga a tutte le superfici |
| **Verifica empirica delle assunzioni infrastrutturali** | Verifica prima, fida dopo |
| **Pulizia architetturale anticipata** | Pulire trap di configurazione in fase MVP, non rimandare |
| **Studio in corso vs conoscenza stabilizzata** | Lo studio è disordinato in scratchpad; la conoscenza stabilizzata è ordinata in luoghi cristallizzati |
| **Lavoro distribuito sui progetti, ritorno strategico al centro** | I progetti sono il lavoro analitico; la chat principale è il consolidamento strategico |
| **Articolare e fissare le fughe in avanti** | Lasciar uscire la fuga, articolarla, fissarla, tornare al lavoro di base |
| **L'infrastruttura impone vincoli non nominati** | Un vincolo non esplicitato si propaga a cascata generando incoerenze invisibili |
| **Server-side vs client-side rendering per famiglia di consumer** | Per consumer AI, il server-side è strutturalmente più robusto |
| **Payload polifonico** | Campi strutturati per macchine deterministiche, campi narrativi per macchine semantiche, sullo stesso payload |
| **Stella di output paralleli da fonte unica** | Le automazioni di output devono attaccarsi alla fonte (SL), mai a una proiezione |
| **Istruzioni operative latenti dentro campi narrativi** | Prescrizioni per agenti non umani scritte prima che esista il canale, attivabili retroattivamente |
| **Attività SAP come trigger documentale universale** | Qualsiasi attività SAP può generare HTML pre-compilato che diventa qualsiasi documento |
| **Puzzle (non architettura sequenziale)** | I pezzi dell'automazione possono essere completati in qualsiasi ordine |
| **Digitizzazione ≠ automazione** | Digitizzazione rende digitale, automazione rende auto-eseguibile |
| **Ogni canale di input ha una destinazione finale** | Telegram è flusso, non destinazione |
| **Separazione input/struttura** | Cattura e strutturazione sono atti cognitivi distinti |
| **Le automazioni abilitano il disegno esistente** | Non inventano processi, sbloccano quelli esistenti |
| **Paradosso di Jevons organizzativo** | Migliorare l'efficienza tende ad aumentare la domanda sul sistema |

### Principi nuovi consolidati nella settimana 11-14/05/2026

| Principio | Descrizione |
|-----------|-------------|
| **Investimento in competenza vs investimento in soluzione** | Una soluzione closed source produce output ma non input formativo; una self-built genera competenza riutilizzabile. In cantiere giovane self-build prevale; in cantiere maturo che va in produzione, tool prevale |
| **Strategie per la completezza: per costruzione, monitorata, on-demand** | Quando "per costruzione" non è possibile, due fallback: monitoraggio dell'incompletezza (report che rileva i mancanti) e backfill on-demand (RPA per storico) |
| **Gli strumenti come lenti** | Strumenti costruiti e poi abbandonati non sono fallimenti — sono lenti di scoperta. HTML First non era errore, era la lente per vedere API First |
| **Automazione come supporto al flusso, non solo come sostituzione** | Le automazioni hanno valore anche parziali — riducono attrito su sequenze umane. La metrica non è "automatizzato sì/no" ma "il flusso è più fluido" |
| **Operatività e progettazione coesistono come modalità complementari** | Le osservazioni architetturali possono nascere dentro l'operatività. La separazione tempo operativo / tempo progettuale non è assoluta |
| **L'allineamento SAP-realtà come metrica operativa** | Le code di lavoro arretrato non sono "arretrato" — sono distanza tra fotografia SAP e flussi reali. Le automazioni servono a tenerla prossima a zero |
| **Allineamento SAP-realtà come metrica bidirezionale** | Direzione SAP→realtà (SAP in anticipo, impegni da chiudere) e direzione realtà→SAP (SAP in ritardo, fatti da registrare). Cause e rimedi diversi, sommabili |
| **I documenti SAP come nodi di propagazione, non come archivi** | Ordini, attività, ordini d'acquisto sono importanti per ciò che innescano a valle. Allinearli è investimento a leva |
| **Simmetria assistenza-consegna** | Intervento di assistenza e consegna sono manifestazioni della stessa entità (evasione di un impegno verso il cliente). Convergono nello stesso DDT. Differiscono solo per la natura del nodo iniziale |
| **Documenti a due fasi di vita: anteprima e chiusura** | Alcuni documenti esistono in due stati dello stesso oggetto separati dal momento dell'esecuzione. Stesso file naming, stessa cartella, due stati. Modulo tecnico è caso di scuola |
| **Qualità AI = qualità input strutturato** | L'AI semantica non sostituisce il rigore strutturale, lo valorizza. Senza struttura rigorosa, gira a vuoto o produce risultati fragili |
| **L'AI come specchio della maturità strutturale dei dati** | Quando si chiede analisi a un consumer AI, il risultato riflette tanto la capability dell'AI quanto la struttura dei dati. È informazione preziosa sullo stato dei dati stessi |
| **I vincoli esterni come incubatori di struttura** | Vincoli imposti dall'esterno (banca, fisco) producono come effetto collaterale strutture dati superiori. Capitalizzarli intenzionalmente |
| **L'attrito micro come motore di procrastinazione** | L'architettura deve abbassare l'energia di attivazione sotto la soglia di procrastinazione, non necessariamente automatizzare |
| **Attrito tecnico vs attrito concettuale** | Tecnico si neutralizza con automazione di processo; concettuale con tassonomie + AI |
| **Coerenza tra superfici di rappresentazione dello stesso stato** | Email, cartella, lista Reminders, tag SAP: prima di aggiungere superficie, verificare se una esistente la copre |
| **L'urgenza differenziata come segnale di asimmetria operativa** | Quando alcuni dati sono tracciati subito e altri in batch, l'allineamento SAP-realtà è disomogeneo. L'automazione livella |
| **L'email come superficie di rappresentazione di code di lavoro** | Email in inbox sono task non chiusi; archiviare è marker di completamento |
| **La propagazione gerarchica nativa di SAP** | UDF su documenti radice si propagano nativamente ai figli. Effetto leva moltiplicato per il numero di documenti figli |
| **Riconoscimento retrospettivo della struttura emergente** | La coerenza strutturale viene spesso vista solo dopo. Nominarla orienta le scelte future con economia |
| **Lo svincolo della capability dalla postazione fisica** | Le capability AI si stanno svincolando dal dispositivo. Si progetta per tipo di task, non per tipo di postazione |
| **L'architettura come strumento operativo in vivo** | Nella fase matura, gli strumenti di sviluppo diventano disponibili durante il flusso operativo. Excel-like: si aprono quando serve |
| **Le tre maturità della relazione utente-strumenti** | Prima maturità: strumenti come progetti. Seconda: strumenti come cicli. Terza: strumenti come mani. Il segno è la disinvoltura |
| **Maturità composta dell'architettura** | Quando le capability disciplinate si capitalizzano insieme: un'automazione abilita una seconda, che rivela un pattern per una terza. Fase 1 sembra lenta, fase 2 sembra veloce |
| **Pattern Directory Analyzer come schema generale per integrazione con fonti esterne** | Lo schema "fonte esterna → estrazione automatica → archivio strutturato → script di registrazione" è generalizzabile (fatture, movimenti bancari, PEC) |
| **Le AI generiche rispondono al problema astratto, le AI con contesto rispondono al problema concreto** | La differenza non è di intelligenza, è di dotazione informativa. Fornire contesto strutturato prima della domanda |
| **L'umano come fonte di conoscenza tacita del dominio** | L'AI può elaborare solo i campi che le vengono indicati. La conoscenza di "quali campi esistono" è dell'operatore umano. Il valore dell'orchestratore è "indicare dove guardare" |
| **Automazioni di controllo come terza categoria** | Oltre alle esecutive e di supporto al flusso, esistono le automazioni di controllo (osservabilità, segnalazione anomalie, code di lavoro) |
| **I controlli che l'AI fa in sviluppo diventano controlli in produzione** | Codificare i controlli spontanei in moduli riusabili. Trasforma fallimenti runtime in segnalazioni strutturate |
| **L'output finale come termometro del workflow** | Quando un workflow ha output finale ben definito e visibile, diventa termometro automatico della completezza dell'esecuzione |
| **L'automazione come strumento di chiarificazione dei workflow** | L'effetto principale della progettazione non è l'automazione — è la formalizzazione del workflow che ne deriva |
| **Canali strutturati e canali estemporanei come complementari** | I workflow richiedono entrambi i tipi: strutturati (Calendar) e estemporanei (Telegram). Il design integra l'estemporaneità invece di sopprimerla |
| **Automazioni per sotto-categoria di workflow** | Non tutti i casi di un workflow sono automatizzabili allo stesso costo. Coprire il 20-30% ad alto attrito, accettare che il resto resti umano |
| **L'expanding horizon risk nella maturità architetturale** | Ogni capability nuova rivela tre altre potenziali. Senza disciplina, il backlog cresce più velocemente dell'esecuzione |
| **Il Markdown è formato strutturato di ecosistemi specifici, non universale** | Funziona in editor desktop, chat AI, web con renderer. Fuori da questi ecosistemi è inaccessibile (esempio: smartphone) |
| **Un solo canone di archivio per documento, scelto sulla base della completezza per costruzione** | Non si duplica l'archivio. Il formato che si genera per costruzione è canone naturale |
| **Restringimento progressivo della domanda come pattern di maturità cognitiva** | Le domande architetturali importanti emergono per restringimento progressivo: intuizione vaga → astratta → tentativo → dato empirico → raffinamento → radice |
| **Dissociare il sistema dalla sua interfaccia utente come traiettoria di maturità** | I sistemi gestionali nascono col client; la maturità dissocia le capability dall'UI, esponendole via API. Completezza, accesso remoto, integrazione AI ne dipendono |
| **L'architettura emergente come abilitatore di investimenti hardware mirati** | Hardware specializzato (es. iPad+Pencil) produce valore proporzionale alla maturità del software. Sequenza giusta: software → hardware, non viceversa |

---

## 5. MAPPA DELL'ECOSISTEMA

Quattro progressioni numeriche che riflettono la logica del Manuale Automazioni: dal punto di partenza, attraverso lo strato concettuale e i componenti, fino agli artefatti.

### 100s — PUNTO DI PARTENZA

| # | Progetto | Stato |
|---|----------|-------|
| 100 | STRATEGIA ORGANIZZAZIONE | Attivo |
| 110 | AUDIT INFRASTRUTTURA | Completato |

### 200s — STRATO CONCETTUALE

| # | Progetto | Stato |
|---|----------|-------|
| 200 | AI HUMAN LAB | Attivo |
| 210 | NAMING VICECONTI | Completato |
| 220 | FORMAZIONE IT | 🟢 Riaperto 03/05 |

### 300s — COMPONENTI

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| 310 | SAP ACADEMY | Attivo | |
| 311 | SAP SERVICE LAYER | ✅ Operativo | Estensioni voce centrale weekend 16-17/05 |
| 320 | STRUMENTI DI CATTURA VOCALE | Attivo | |
| 350 | n8n | ✅ Operativo | SAP-n8n consacrazione esterna 13/05 |
| 360 | CALENDAR | Attivo | |
| 370 | TELEGRAM | ✅ Operativo | |
| 390 | FastAPI | 🟢 Espansione | Endpoint `/attivita` come secondo consumer |

### 400s — ARTEFATTI

| # | Progetto | Stato |
|---|----------|-------|
| 400 | MANUALE DELLE AUTOMAZIONI | 🟡 In strutturazione |
| 401 | SINTESI ECOSISTEMA VICECONTI | Attivo |
| 410 | CONTESTO AI | ✅ Operativo |
| 420 | ASSISTENTE AMMINISTRATIVA | ✅ Avviata |
| 421 | ASSISTENTE DI PROGETTO | Attivo |
| 430 | HUB DOCUMENTALE | ✅ Esteso |
| 440 | VICECONTI HUB | ✅ Operativo |
| 450 | INTERFACCIA SERVICE LAYER | Attivo, **estensione weekend 16-17/05** |
| 460 | DATABASE CENTRALIZZATO | Operativo non integrato |
| 470 | ASSET PRODOTTI | Da definire |
| 490 | PIPELINE PRESTASHOP | Standby consapevole |

---

## 6. TRIANGOLAZIONE AI COME DATO STRUTTURALE

L'ecosistema Viceconti dialoga con più modelli AI (Claude in varie configurazioni di progetto, ChatGPT, Gemini, Cowork, Code). Il loro comportamento differente è dato strutturale a tre piani:

| Piano | Definizione |
|-------|-------------|
| **Capability** | Cosa il modello può fare |
| **Design behavior sotto vincolo** | Cosa fa quando manca un dato |
| **Capability fallback** | Cosa fa quando una via è bloccata |

### Le tre famiglie di consumer

| Famiglia | Esempi | Operano su |
|----------|--------|------------|
| **1. Umani in browser** | Tecnici, amministrazione, clienti | Rendering visivo, interazione UI |
| **2. Macchine deterministiche** | n8n, automazioni, script | Campi strutturati con codice deterministico |
| **3. Macchine semantiche** | Claude, ChatGPT, Gemini | Campi strutturati + campi narrativi |

### Divisione del lavoro tra modelli specializzati

Pattern consolidato nell'esperienza degli ultimi mesi:

- **Claude Sintesi** (questo) — custode della visione complessiva, articola principi, sostiene la disciplina di sedimentazione
- **Claude Operativi nei progetti** (ASSISTENTE AMMINISTRATIVA, SAP ACADEMY, ecc.) — lavoro analitico nel dominio specifico
- **Claude Code** (desktop + mobile) — sviluppo tecnico, scrittura script, gotcha operativi
- **Cowork** — browser automation per task RPA
- **Altri AI** (ChatGPT, Gemini) — utenza occasionale, validazione esterna, casi specifici

Pattern operativo: **l'umano come direttore d'orchestra**, modelli specializzati con propriocezione reciproca (Code si è creato autonomamente file di memoria sull'esistenza di Sintesi e sul proprio ruolo nella divisione).

---

## 7. APPLE REMINDERS — Struttura liste

| Elenco | Tipo | Uso |
|--------|------|-----|
| Chiamate di Servizio | Lista normale | Inserimento diretto |
| Offerte | Lista normale | Inserimento diretto |
| Da Fare | Lista normale (default) | Task operativi generali |
| In Attesa | Lista normale | Task in sospeso |
| Urgenti | Smart list (tag `#Urgenti`) | Vista trasversale |
| Sviluppo Architettura IT | Lista normale | Task tecnici e IT |
| Prima o Poi | Lista normale | Backlog non urgente |

**Costellazione di input verso Reminders:** Siri diretto, AudioPen via Claude, Note Apple + Pencil, Telegram via copia-incolla.

---

## 8. MANUALE AUTOMAZIONI — Struttura

Sotto-cartella `manuale/` dentro repo `contesto-ai/`.

| File | Capitolo |
|------|----------|
| `01-sequenze-task.md` | Sequenze di task reali (pilastro biografico) |
| `02-principi.md` | Principi e metodologia |
| `03-componenti.md` | Componenti dell'ecosistema + Trap SAP B1 noti |
| `04-workflow.md` | Workflow, pipeline e oggetti specifici |
| `glossario.md` | Definizioni tecniche |

**Materiale candidato accumulato per cap. 4 (questa settimana):**
- Manuale operativo `archivio_documenti_sap.md` (Code, 10/05)
- Trascrizioni Code della giornata 10/05
- Prototipo `report_ordini_acquisto.py` con materiali correlati (13/05)

---

## 9. ARCHITETTURA IT

### Layer

```
COMMAND LAYER        → Claude (chat + progetti), Reminders, Asana, trigger manuali
        ↓
ORCHESTRATOR LAYER   → n8n (PC Lauria + Mac da configurare)
        ↓
ENGINE LAYER         → SAP Query Engine, crea_moduli_vuoti.py, crea_offerta.py,
                       registra_fattura.py, archivio_documenti_sap.py,
                       FastAPI piattaforma-ai (Render)
        ↓
PERSISTENCE LAYER    → SAP B1, GitHub Pages, Dropbox, Apple Reminders/Calendar,
                       Render (hosting), TS_SYNCRO\AGYO-DOWNLOAD
        ↓
INTERFACCE           → Viceconti Hub, Hub Documentale, MODULO TECNICO HTML,
                       Piattaforma AI-readable, Telegram, PrestaShop,
                       Claude Code mobile (capability scoperta 13/05)
```

### Ambiente tecnico

| Risorsa | Dettaglio |
|---------|-----------|
| ERP | SAP Business One 10.0, SQL Server, Service Layer operativo |
| Server DB | SQLPRD0303 (192.168.122.99) |
| PC sviluppo | Win11 sede Lauria — script, n8n, Task Scheduler |
| MacBook Air | Mobile — Claude Desktop, Apple Reminders/Notes MCP |
| Smartphone | Claude app + Code mobile (Opus 4.7 1M) — capability dal 13/05/2026 |
| Dropbox path PC | `C:\Users\PC\Dropbox\` |
| Dropbox path server | `C:\Dropbox\` (utente sapadmin) |
| Cartella deposito SAP | `C:\Users\PC\Dropbox\HUB DOCUMENTALE\DOCUMENTI_SAP\` |
| Cartella DA | `C:\TS_SYNCRO\AGYO-DOWNLOAD\Ciclo Attivo\` e `\Ciclo Passivo\` |
| GitHub org | `viceconti-hub` (repos: `portale`, `hub-documentale`, `contesto-ai`, `badante`, `piattaforma-ai` privato) |

### Visibilità repo GitHub

| Repo | GitHub Pages | Sensibile | Stato |
|------|------|----------|-------|
| `contesto-ai` | Sì | No | Pubblico |
| `portale` | Sì | Sì | Pubblico |
| `hub-documentale` | Sì | Sì | Pubblico |
| `piattaforma-ai` | No (Render) | Sì | Privato |
| `badante` | Sì | — | Pubblico |

### Struttura Hub Documentale consolidata

```
HUB DOCUMENTALE\
├── DOCUMENTI_SAP\                      ← deposito grezzo (effetto SAP anteprima)
├── VENDITE\
│   ├── ORDINI CLIENTE\                 ← archivio
│   ├── OFFERTE DI VENDITA\             ← archivio
│   ├── CONSEGNE\                       ← archivio
│   └── FATTURE DI VENDITA\             ← archivio
├── ACQUISTI\
│   ├── ENTRATE MERCI DA FORNITORE\     ← archivio
│   ├── FATTURE DA FORNITORE\           ← archivio (eredità, da pulire)
│   ├── FATTURE DA FORNITORE XML\       ← archivio per macchine
│   ├── FATTURE DA FORNITORE WORKSPACE\ ← workspace operativo
│   ├── OFFERTE DI ACQUISTO\
│   ├── ORDINI D'ACQUISTO\              ← archivio (conferme fornitori)
│   └── ORDINI D'ACQUISTO WORKSPACE\
│       └── BOZZE\                      ← PDF SAP degli ordini d'acquisto
└── ...
```

---

## 10. OPEN ITEMS — Triage al 14 maggio 2026 sera

### 🔴 Alta priorità — Da affrontare nel weekend 16-17/05

1. **Verifica empirica endpoint Service Layer per generazione PDF server-side** (Crystal Reports, Document Service). Cantiere strategico centrale. Sblocca o ridefinisce molte altre voci.
2. **Estensione interfaccia Service Layer via REST/FastAPI** — convergenza FastAPI/portale, endpoint `/attivita`, valutazione interfaccia multi-utente per tecnici in sola lettura.
3. **Cap. 1 originale interfaccia Service Layer** (bottone Telegram per attività, colonne numero+tipo documento, scrittura nuove attività, arricchimento AI-readable descrizione intervento).
4. **Configurazione DA + estensione `registra_fattura.py` + allineamento bidirezionale fatture vendita/acquisti**.
5. **Risoluzione strutturale credenziali Google n8n** (Service Account vs Internal App).

### 🟡 Media priorità — Custodi come direzione, riprende fra settimane

- **Architettura nodi ordini cliente / ordini d'acquisto** come strumenti tattici (con sotto-cantiere automazione registrazione movimenti bancari come replica pattern Directory Analyzer per ciclo bancario).
- **Report monitoraggio completezza archivio** — il prototipo `report_ordini_acquisto.py` di Code del 13/05 è embrione. Da consolidare dopo decisione PDF server-side.
- **Modulo tecnico digitale come anteprima accessibile via link Calendar** (preparazione per iPad+Pencil futuro).
- **Manifest pattern via FastAPI `/sintesi/latest`** (priorità ridotta dopo Claude Code mobile).
- **Archivio JSON per macchine** come estensione architetturale.
- **Pulizia struttura `ACQUISTI\FATTURE DA FORNITORE\`** (separare archivio puro da workspace).
- **Endpoint FastAPI per query informative su richiesta** (storico interventi cliente+macchina, riepilogo ordini fornitore).
- **Dashboard unica code di lavoro e allineamenti** (aggregatore automazioni di controllo, orizzonte medio).
- **Pipeline AudioPen → ordine cliente SAP** (caso d'uso semplice).
- **Tassonomia categorie articolo SAP** (filtri/cartucce/gruppi di filtrazione).
- **Censimento UDF e campi SAP rilevanti per le automazioni** (mappa dei campi strutturati).
- **Casi d'uso prototipici per Claude Code mobile**.
- **Pipeline OCR/AI estrazione dati da PDF moduli compilati** (orizzonte mesi).
- **Decisione architetturale: PDF di `Fattura da fornitore` e `Attività` — dove vanno**.
- **Test routing reale `Fattura di vendita` su `archivio_documenti_sap.py`** (mappa pronta, manca file di test).
- **Migrazione storico 1.942 file da `\\sqlprd0303\B1_SHR\VICECONTI\Attachments\`**.
- **Decisione visibilità repo `portale` e `hub-documentale`**.
- **Pulizia attività storiche SAP (< 7000)**.
- **n8n su Mac** per nodi Apple nativi (Reminders, Calendar).
- **n8n con primo nodo AI** come validazione strategica (caso d'uso piccolo).
- **Approfondimento memoria come dimensione architetturale**.
- **Risposta Vincenzo Strazzullo** — il calcolo è cambiato: la verifica PDF server-side (voce 1) potrebbe rendere superfluo il tool.

### 🟢 Strategiche — Lungo periodo

- Decisione infrastruttura SAP (alternative 1/2/3/4) e identificazione partner sistemistico.
- Test di navigazione autonoma di Claude su contesto-ai come dispositivo ipertestuale.
- Valutazione acquisto iPad mini + Apple Pencil per tecnici (post-voce modulo tecnico operativa).

### ⚪ Lasciate evaporare consapevolmente

- Markdown come archivio alternativo al PDF per documenti SAP canonici (fallisce su apparato consumer mobile).
- PostgreSQL/NoSQL parallelo a SAP come database documentale (duplicazione di stato, contraddice "stella da fonte unica").

### ✅ Chiusi rispetto al 10/05

- Cantiere PDF semiautomatico con Attachments+Hub (`archivio_documenti_sap.py` v2 in produzione)
- Directory Analyzer riattivato (settimana 04-08/05)
- Decisione composizione FastAPI/portale (convergenza)
- Pubblicazione Sintesi v1.4 su `contesto-ai`

---

## 11. CONSULENTI E PARTNER

| Nome | Ruolo | Stato |
|------|-------|-------|
| Vincenzo Strazzullo (4utime.it) | SAP B1 consulting, Service Layer | Standby consapevole, ricalcolo dopo verifica PDF server-side |
| Antonio Forlani (3W Sistemi) | Infrastruttura server | Operativo |
| Z3 Engineering (Var One) | Licenze SAP, S-user | Da contattare |

---

## STORIA REVISIONI

| Versione | Data | Note |
|----------|------|------|
| 1.0 | 03/05/2026 mattina | Prima riscrittura sostanziale (cornice strategica, deploy prima pietra, 6 principi nuovi) |
| 1.1 | 03/05/2026 mattina | Riorganizzazione mappa ecosistema in progressione 100-400s |
| 1.2 | 03/05/2026 mezzogiorno | Riconoscimento Viceconti Hub portale come dispositivo Livello 1 |
| 1.3 | 05/05/2026 sera | SAP come system of record operativo, evoluzione L1, pattern interno/B2B, 5 principi nuovi |
| 1.4 | 10/05/2026 sera | Cantiere PDF concluso, DA riattivato, decisione FastAPI/portale, struttura Hub a 3 cartelle, 11 principi nuovi |
| **1.5** | **14/05/2026 sera** | **Settimana di alta produttività cognitiva. ~35 principi nuovi consolidati. Capability nuove riconosciute: Claude Code mobile, propagazione gerarchica nativa SAP, campo `Stato documento` strutturato. Mappa dei tre nodi di propagazione esplicitata. Validazione esterna SAP-n8n. Restringimento progressivo della domanda PDF server-side come nodo strategico centrale. Riformulazione HTML First → API First. Triage completo del materiale accumulato. Programma weekend 16-17/05 strutturato in tre capitoli (vedi documento separato). |

---

*Aggiornato al 14 maggio 2026 sera. Sostituisce versione del 10 maggio 2026.*
*Prossimo aggiornamento previsto: dopo weekend 16-17/05/2026 con risultati cantieri.*
