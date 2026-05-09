# SINTESI ECOSISTEMA VICECONTI
## Documento di visione e stato operativo

*Aggiornato al 5 maggio 2026 sera — sostituisce versione del 3 maggio 2026*

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

> *Claude è il livello cognitivo dell'azienda. SAP è la rappresentazione strutturata della realtà aziendale. HTML First è il sostrato di interscambio tra i due. Il Manuale Automazioni è l'evoluzione organizzata di HTML First a scala di sistema.*

> *Investire in ciascun emisfero (Claude e SAP) nella propria autonomia è investire nell'integrazione futura. Ogni workflow che migliora SAP-stand-alone migliorerà anche SAP-integrato. Ogni decisione che organizza Claude-stand-alone migliorerà anche Claude-integrato.*

Questa formulazione è il filtro decisionale dell'ecosistema. Ogni scelta architetturale viene valutata in coerenza con questa direzione.

### Aggiornamento 05/05/2026 — SAP come system of record operativo emergente

Riconoscimento retrospettivo: le automazioni costruite per ottimizzare l'assistenza tecnica hanno prodotto un effetto laterale non programmato. Il modulo Attività di SAP è diventato l'ingresso a maggior efficienza per registrare task operativi, perché l'output è ormai automatizzato (Calendar, Viceconti Hub, in prospettiva Telegram e AI).

**SAP sta evolvendo dal ruolo di "rappresentazione strutturata della realtà aziendale" (registra il passato) verso il ruolo di "system of record operativo" (orchestra il presente).** L'estensione del pattern dall'assistenza alle consegne, e ora alle attività personali, consolida questa evoluzione. Non sostituisce la formulazione originaria — la specifica e la arricchisce.

### I tre livelli di integrazione Claude-SAP

L'integrazione non è un evento futuro — è già articolata su tre livelli operativi:

| Livello | Funzione | Stato | Modalità |
|---------|----------|-------|----------|
| **L1 — Pagine pubbliche AI-readable** | Interrogazioni standard ricorrenti, dati strutturati, accesso da Claude.web senza VPN | ✅ Operativo | **Due dispositivi:** Piattaforma FastAPI su Render (costruita coscientemente come tale, prima pietra 02/05); Viceconti Hub (portale) — dispositivo emerso retrospettivamente il 03/05, a un passo dalla soglia |
| **L2 — Claude Code da desktop** | Interrogazioni libere, esplorazioni, accesso diretto al Service Layer | ✅ Operativo | Postazione di lavoro, VPN |
| **L3 — Cowork via Dispatch da mobile** | Interrogazioni libere quando non al desktop | ✅ Operativo | Smartphone → Cowork → Claude Code |

**Dinamica tra i livelli:** alcune interrogazioni che oggi vivono al Livello 2 o 3 saranno "promosse" al Livello 1 quando si dimostreranno ricorrenti. Il Livello 1 cresce per evoluzione, non per pianificazione a priori. Il dispositivo agentico (L2 e L3) resta sempre operativo per la coda lunga delle domande nuove — basso volume ma alto valore.

**Due tipi di dispositivi al Livello 1.** Distinzione emersa il 03/05/2026:
- **Dispositivi costruiti coscientemente** (Piattaforma FastAPI): nascono già come Livello 1, server-side rendering, ottimizzati per consumer AI come scelta deliberata
- **Dispositivi emersi retrospettivamente** (Viceconti Hub portale): erano già lì, già pubblici, con dato strutturato già accessibile via HTTPS — invisibili alla famiglia AI per effetto collaterale del rendering client-side, non per intenzione di design. Spostare il punto di consegna di uno strato (servire JSON a URL stabili invece di passarlo solo al client JavaScript) li converte in dispositivi pieni del Livello 1 con investimento incrementale, non rifacimento

**Evoluzione del Livello 1 da canale di lettura a canale di interazione.** Riconoscimento del 05/05/2026: FastAPI può inoltrare operazioni di scrittura su SAP via Service Layer, aggiungendo strati di autenticazione, validazione, idempotenza e tracciabilità. Il Livello 1 evolve quindi da canale di lettura a canale di interazione potenziale — le tre famiglie di consumer diventano potenzialmente agenti capaci di azione, non solo lettori. Le quattro questioni che la scrittura impone (auth, validazione, idempotenza, tracciabilità) sono lavoro di design da affrontare con scrupolo, non ostacoli bloccanti. Stato attuale: capability nominata, non ancora costruita.

### Nota strategica — Pattern unificato interno/B2B

Riconoscimento del 05/05/2026: il lavoro sui flussi interni (esposizione delle attività SAP al web, scrittura via FastAPI, autenticazione e controllo accessi) è strutturalmente identico al lavoro che servirebbe per un B2B esterno (clienti rivenditori). **Investimento unico, due rendimenti.** È la traiettoria FastAPI che si rivela centrale, non collaterale. Non costituisce cantiere prossimo — orienta le scelte di lungo periodo.

---

## 3. STATO OPERATIVO — Maggio 2026

### Componenti in produzione

| Componente | Stato | Note |
|-----------|-------|------|
| **SAP Business One 10.0 + Service Layer** | ✅ Operativo | Attivo dal 29/03/2026. 111 fatture registrate automaticamente. Server SQLPRD0303 stabilizzato |
| **SAP Query Engine v2.5** | ✅ Operativo | Estrazioni orarie, JSON su GitHub Pages via API |
| **Piattaforma AI-readable (FastAPI)** | ✅ Prima pietra in produzione | Deployata su Render 02/05/2026. URL `piattaforma-ai.onrender.com`. 19.476 articoli Morini, endpoint JSON+HTML+Swagger. Validata su Claude Opus, Claude Sonnet, ChatGPT. **Dispositivo Livello 1 costruito coscientemente come tale** |
| **n8n v2.14.2** | ✅ Operativo | PC Lauria, ngrok dominio statico, Task Scheduler auto-start. Workflow: AudioPen → Telegram NOTE PERSONALI + Google Drive |
| **Viceconti Hub + Hub Documentale** | ✅ Operativi | GitHub Pages. 3.068 file, 257 clienti. **Riconoscimento 03/05/2026: il portale è dispositivo Livello 1 emerso retrospettivamente, a un passo dalla soglia. JSON SAP estratti orariamente esistono già, sono pubblici, accessibili via HTTPS — invisibili alla famiglia AI per effetto del rendering client-side, non per scelta deliberata** |
| **Contesto AI su GitHub Pages** | ✅ Operativo | repo contesto-ai, file .md a nome fisso, fetch validato su Claude/ChatGPT/Gemini |
| **`RIAPRI E MODIFICA MODULO TECNICO.html`** | ✅ Beta produzione | v4: auto-idratazione, modulo vuoto, campi header editabili, sovrascrittura stesso nome file |
| **`crea_moduli_vuoti.py`** | ✅ Operativo | Legge JSON da GitHub, genera HTML pre-compilati, non sovrascrive file esistenti. 94 moduli in Dropbox |
| **`crea_offerta.py`** | ✅ Operativo | Versione batch: file multipli, --cartella, --dry-run, sposta in PROCESSATI. 29 offerte create (260080-260108) |
| **`registra_fattura.py`** | ✅ Operativo (incompleto) | 111 fatture registrate, da completare |
| **ASSISTENTE AMMINISTRATIVA** | ✅ Avviata | Progetto Claude operativo da 13 aprile 2026 |
| **Apple Reminders** | ✅ Componente adottata | Layer di task management tra cattura grezza e sistemi di destinazione |
| **Interfaccia Service Layer + sincronizzazione Calendar** | ✅ Risolto 04-05/05/2026 | Sincronizzazione attività SAP su Google Calendar (tre calendari: Assistenza / Consegne / Sopralluoghi) tornata operativa. Workflow n8n ripristinato |
| **Pipeline voce → SAP → Service Layer → JSON → AI** | ✅ Testata end-to-end 05/05/2026 | Sette passaggi (manuali a metà), principio verificato: messaggio AudioPen registrato → copiato in attività SAP → sincronizzato via Service Layer → esportato come JSON → letto da AI. Materiale per orientamento futuro |

### Workflow modulo tecnico — flusso completo in produzione beta

```
SAP Query Engine (server, ogni ora)
→ Attivita_Assistenza.json → push GitHub

crea_moduli_vuoti.py (PC Lauria, da schedulare)
→ legge JSON da GitHub
→ genera MODULO_*.html per ogni attività aperta
→ salva in Dropbox\MODULI ATTIVITA' APERTE\

Tecnico apre RIAPRI E MODIFICA MODULO TECNICO.html
→ trascina il MODULO_*.html corrispondente
→ compila Descrizione Lavori + ricambi
→ salva → stesso nome file

Prospero lancia crea_offerta.py --cartella .
→ crea Offerta di Vendita SAP
→ sposta file in MODULI_PROCESSATI

Prospero chiude attività in SAP
→ al prossimo ciclo il modulo non viene rigenerato
```

---

## 4. PRINCIPI ARCHITETTURALI CONSOLIDATI

| Principio | Descrizione |
|-----------|-------------|
| **Destinatario First** | Decidi consapevolmente quale destinatario è primario e costruisci a partire da lì. **HTML First** (manufatto persistente è HTML con JSON embedded, dato canonico nel file, per workflow file-based) e **JSON First** (manufatto persistente è JSON nel sistema, HTML come vista derivata, per workflow API-based) sono due specializzazioni dello stesso principio sovraordinato |
| **HTML come trinità** | Quando convergono umano e macchina sullo stesso destinatario: interfaccia + documento + dato strutturato in un file solo |
| **Duplicazione consapevole per destinatari divergenti** | Quando i destinatari hanno esigenze divergenti, conviene tenere contenitori distinti anche al costo di duplicare. Esempio canonico: due ecosistemi paralleli di siti pubblici Viceconti — umano-facing (Store, B2B) e AI-facing (Hub, Hub Documentale, contesto-ai, piattaforma AI-readable) |
| **Dispositivo che cambia retroattivamente il significato del dato** | L'esistenza di un dispositivo che renda interrogabili dati aziendali da AI cambia retroattivamente cosa ha senso produrre. Esempio: depositare contesto nel campo "osservazioni" delle anagrafiche SAP ha senso *adesso* che esiste l'orizzonte della piattaforma AI-readable |
| **Scalabilità progressiva** | Non costruire mai a scala completa quando puoi validare il pattern a scala ridotta. Esempio: 19.476 articoli Morini invece di 155.000 totali — abbastanza per testare il pattern alla scala vera, senza la complessità multi-fornitore |
| **Studio in corso vs conoscenza stabilizzata** | Lo studio è disordinato per natura — vive in scratchpad (FORMAZIONE IT, AI Human Lab, chat esplorative). La conoscenza stabilizzata è ordinata per costruzione — vive in luoghi cristallizzati (Manuale Automazioni, riepiloghi consolidati). I due piani non si sovrappongono né si chiudono in sequenza: procedono in parallelo, con promozione occasionale dal primo al secondo |
| **Lavoro distribuito sui progetti, ritorno strategico al centro** | I progetti specifici sono il luogo del lavoro analitico, operativo, diagnostico. La chat principale (Sintesi Ecosistema) è il luogo del confronto strategico e del consolidamento. Il ritorno deve riportare non solo le decisioni operative ma anche le valutazioni strategiche emerse — altrimenti il piano operativo cresce e quello strategico si impoverisce, perché le valutazioni più ricche tendono a maturare proprio nel lavoro analitico. Standard di riepilogo aggiornato 03/05/2026 con sezione "Valutazioni di possibile valore strategico" |
| **Articolare e fissare le fughe in avanti** | Le visioni che generano "fughe in avanti" nascono in parte dal bisogno di semplificare passaggi complessi. Hanno valore strategico, ma il loro fascino dipende dalla loro distanza dai dettagli. Pratica: lasciar uscire la fuga, articolarla con disciplina, smontarla, riconoscerla come parentesi, fissare quello che ha valore in un punto stabile (Sintesi), tornare al lavoro di base senza che la visione abbia generato cantiere prematuro |
| **L'infrastruttura impone vincoli non nominati** | Rovescio del principio "le automazioni abilitano il disegno esistente". Quando un vincolo strutturale di un servizio (es. GitHub Pages free funziona solo con repo pubblici) condiziona un'architettura senza essere esplicitato, si propaga a cascata generando incoerenze invisibili. Va nominato per essere disinnescato |
| **Server-side vs client-side rendering come scelta di leggibilità per famiglia di consumer** | Il rendering client-side serve bene umani in browser, ma rende invisibili i dati alle AI con tool tipo `web_fetch` (che ricevono solo lo scheletro HTML). Per dispositivi destinati a consumer AI, il server-side è strutturalmente più robusto. È correzione su asse diverso dalla visibilità del repo: visibilità del repo è proprietà del **canale del sorgente**, leggibilità del runtime è proprietà del **canale del runtime**. Due correzioni potenziali su due assi distinti |
| **Payload polifonico** | Un payload JSON ben progettato è polifonico: campi strutturati come spina dorsale per macchine deterministiche (n8n, automazioni), campi narrativi come tessuto interpretativo per macchine semantiche (AI), sullo stesso payload. Le due dimensioni convivono **senza contropartite** — la famiglia deterministica ignora i campi narrativi che non le servono, la famiglia semantica li valorizza. Aggiunta senza penalizzazione, raro nel design |
| **Stella di output paralleli da fonte unica** | Le automazioni di output devono attaccarsi alla **fonte di verità** (Service Layer), mai a una **proiezione** (Calendar, Hub). Mantiene canali indipendenti e fa invecchiare bene l'architettura. SAP è la fonte; Calendar, Viceconti Hub, Telegram (in costruzione) sono raggi paralleli; l'AI come consumer sarà ennesimo raggio. Architettura event-driven sui dati operativi che emerge per via naturale dal lavoro |
| **Istruzioni operative latenti dentro campi narrativi** | I campi narrativi possono contenere prescrizioni destinate ad agenti non umani, scritte prima ancora che esista il canale per eseguirle, attivabili retroattivamente quando il canale esisterà. Esempio: "PER COWORK: CREARE CARTELLA IN HUB DOCUMENTALE" trovato in U_Esito di un'attività SAP. I campi narrativi non sono solo arricchimento descrittivo — sono anche canale di delega operativa latente |
| **Attività SAP come trigger documentale universale** | Qualsiasi tipo di attività SAP può generare un HTML pre-compilato che uno script trasforma in qualsiasi documento SAP. Assistenza → offerta/DDT è il primo caso. Offerte, ordini, consegne replicano lo stesso pattern |
| **Puzzle (non architettura sequenziale)** | I pezzi dell'automazione possono essere completati in qualsiasi ordine senza dipendenze sequenziali obbligatorie |
| **Digitizzazione ≠ automazione** | Digitizzazione rende le cose digitali. Automazione le rende auto-eseguibili. Confonderle porta a aspettative irrealistiche |
| **Ogni canale di input ha una destinazione finale** | Telegram non è mai la destinazione. Il flusso deve uscire verso il sistema giusto prima che l'informazione diventi difficile da recuperare |
| **Separazione input/struttura** | Cattura e strutturazione sono atti cognitivi distinti. Il modello a 2 step li separa deliberatamente: prima cattura libera, poi strutturazione per destinazione |
| **Le automazioni abilitano il disegno esistente** | Il disegno operativo era già buono — sedimentato in anni di pratica. Le automazioni non inventano nuovi processi, sbloccano quelli esistenti |
| **Paradosso di Jevons organizzativo** | Migliorare l'efficienza di un sistema tende ad aumentare la domanda su quel sistema. L'esecuzione dei task è il capitolo che viene dopo l'organizzazione ed è quello più grande |

---

## 5. MAPPA DELL'ECOSISTEMA

L'ecosistema è organizzato in **quattro progressioni numeriche** che riflettono la stessa logica generativa del Manuale Automazioni: dal punto di partenza, attraverso lo strato concettuale e i componenti, fino agli artefatti prodotti dall'incontro tra le tre cose precedenti. Gli spazi liberi nella numerazione (es. 130, 230) sono volutamente lasciati per inserzioni future. Allineamento con le cartelle Drive di pari numerazione.

### 100s — PUNTO DI PARTENZA
*Realtà storica dell'organizzazione, funzioni aziendali tradizionali, infrastruttura come la trovi*

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 100 | STRATEGIA ORGANIZZAZIONE | Attivo | Transizione s.n.c. → s.r.l. unipersonale (luglio liquidazione SNC, settembre nuova società). Per dettaglio vedi NUOVA STRATEGIA SETTEMBRE 2026 |
| 110 | AUDIT INFRASTRUTTURA | Completato | Server SQLPRD0303 stabilizzato dopo crisi marzo 2026. Manutenzione periodica con 3W Sistemi |

### 200s — STRATO CONCETTUALE
*Principi, teoria, formazione che informa il progetto*

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 200 | AI HUMAN LAB | Attivo | Sapere personale che sedimenta lentamente. Riflessione sul rapporto AI-soggetto |
| 210 | NAMING VICECONTI | Completato | Convenzioni di naming file e cartelle. Riferimento operativo |
| 220 | FORMAZIONE IT | 🟢 Riaperto 03/05 | Scratchpad dello studio in corso. Materiale FastAPI/REST/JSON. Promozione occasionale al Manuale (cap. 3 e glossario) |

### 300s — COMPONENTI
*Tutto ciò che è ereditato dal mondo, esiste indipendentemente dall'incontro con Prospero*

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 310 | SAP ACADEMY | Attivo | Documentazione tecnica SAP B1 e Service Layer |
| 311 | SAP SERVICE LAYER | ✅ Operativo | Attivo dal 29/03/2026. 111 fatture registrate via API |
| 320 | STRUMENTI DI CATTURA VOCALE | Attivo | AudioPen Prime, microfono nativo Claude, Jamie Live, valutazione PLAUD |
| 350 | n8n | ✅ Operativo | v2.14.2 su PC Lauria. Workflow AudioPen → Telegram + Google Drive |
| 360 | CALENDAR | Attivo | Google Calendar. 18 calendari mappati via MCP |
| 370 | TELEGRAM | ✅ Operativo | Notifiche + canale NOTE PERSONALI |
| 390 | FastAPI | 🟢 Nuovo | Framework Python per API REST. Deployata prima pietra su Render |

### 400s — ARTEFATTI
*Prodotti dell'incontro tra punto di partenza, strato concettuale e componenti — non esistono al di fuori dell'incontro con Prospero*

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 400 | MANUALE DELLE AUTOMAZIONI | 🟡 In strutturazione | 5 file su `contesto-ai/manuale/` (decisione 03/05). Pilastri: cap. 1 sequenze task + cap. 4 workflow |
| 401 | SINTESI ECOSISTEMA VICECONTI | Attivo | Questo documento. Mappa di stato e visione complessiva |
| 410 | CONTESTO AI | ✅ Operativo | Repo `contesto-ai` su GitHub Pages. Layer di accesso universale ai riepiloghi per qualsiasi AI |
| 420 | ASSISTENTE AMMINISTRATIVA | ✅ Avviata | Operativa dal 13/04/2026. Prima settimana completata |
| 421 | ASSISTENTE DI PROGETTO | Attivo | Assistente per gestione progetti |
| 430 | HUB DOCUMENTALE | ✅ Operativo | GitHub Pages. 3.068 file, 257 clienti. Interfaccia Dropbox |
| 440 | VICECONTI HUB | ✅ Operativo | Estrazione dati SAP via SQL su GitHub Pages |
| 450 | INTERFACCIA SERVICE LAYER | Attivo | Interfaccia per scrittura SAP via Service Layer (HTML statico + chiamate dirette) |
| 460 | DATABASE CENTRALIZZATO | Operativo non integrato | SQLite con 9.785 prodotti Morini. Da connettere a FastAPI |
| 470 | ASSET PRODOTTI | Da definire | — |
| 490 | PIPELINE PRESTASHOP | Standby consapevole | Da riprendere H2 2026 |

---

## 6. TRIANGOLAZIONE AI COME DATO STRUTTURALE

L'ecosistema Viceconti dialoga con più modelli AI (Claude, ChatGPT, Gemini). Il loro comportamento differente non è "AI con fetch / AI senza fetch" — è un dato strutturale a tre piani:

| Piano | Definizione | Esempio (caso Bormioli, validazione 02/05) |
|-------|-------------|--------------------------------------------|
| **Capability** | Cosa il modello può fare | Fetch HTTP, code execution, multimodale |
| **Design behavior sotto vincolo** | Cosa fa quando manca un dato | Claude chiede o pivota; Gemini riempie con plausibilità; ChatGPT con prompt operativo esegue |
| **Capability fallback** | Cosa fa quando una via è bloccata | Claude pivota su capability alternative (es. da web_fetch a curl in container) |

**Conseguenza:** un'AI con capability ricche ma design behavior sbagliato è più pericolosa di un'AI con poche capability ma design coerente.

**Conseguenza strategica:** il Livello 1 (Piattaforma AI-readable) è ottimizzato per **Claude come consumer primario per scelta consapevole**, non per accidente. ChatGPT come consumer secondario funziona se si sa chiedere bene. Gemini come non-consumer è dato strutturale, non difetto da risolvere.

### Aggiornamento 05/05/2026 — Le tre famiglie di consumer

Riformulazione introdotta nella sessione FastAPI del 05/05/2026: la famiglia 2 (precedentemente "macchine non intelligenti") è riformulata come **macchine deterministiche**. Descrive *come operano* (eseguono codice deterministico sui campi strutturati) invece di *cosa gli manca*.

| Famiglia | Esempi | Operano su |
|----------|--------|------------|
| **1. Umani in browser** | Tecnici, amministrazione, clienti | Rendering visivo, interazione UI |
| **2. Macchine deterministiche** | n8n, automazioni, script | Campi strutturati con codice deterministico |
| **3. Macchine semantiche** | Claude, ChatGPT, Gemini | Campi strutturati + campi narrativi (interpretazione contestuale) |

La distinzione è la base del principio del **payload polifonico** (sezione 4): un singolo payload può servire le tre famiglie senza compromessi, perché ogni famiglia consuma quello che le serve e ignora il resto.

---

## 7. APPLE REMINDERS — Struttura liste

| Elenco | Tipo | Uso |
|--------|------|-----|
| Chiamate di Servizio | Lista normale | Inserimento diretto |
| Offerte | Lista normale | Inserimento diretto |
| Da Fare | Lista normale (default) | Task operativi generali |
| In Attesa | Lista normale | Task in sospeso |
| Urgenti | Smart list (tag `#Urgenti`) | Vista trasversale — non si scrive qui |
| Sviluppo Architettura IT | Lista normale | Task tecnici e IT |
| Prima o Poi | Lista normale | Backlog non urgente |

**Limite MCP:** lettura/scrittura tag non disponibile. Il tag `#Urgenti` va aggiunto manualmente dall'app.

**Costellazione di input verso Reminders:**

```
Siri → Reminders diretto (velocità, zero frizione)
AudioPen → Claude/Assistente → Reminders (struttura, contesto ricco)
Note Apple + Pencil → rilettura AudioPen → Claude → Reminders
Telegram → copia-incolla → Reminders (condivisione collaboratori)
```

---

## 8. MANUALE AUTOMAZIONI — Struttura consolidata

**Decisione 03/05/2026:** sotto-cartella `manuale/` dentro repo `contesto-ai/`, con un file per capitolo.

### Struttura

| File | Capitolo | Natura |
|------|----------|--------|
| `01-sequenze-task.md` | **Cap. 1 — Sequenze di task reali** | Pilastro biografico — punto di partenza |
| `02-principi.md` | **Cap. 2 — Principi e metodologia** | Astrazioni dell'ecosistema (i principi della sezione 4 di questa Sintesi) |
| `03-componenti.md` | **Cap. 3 — Componenti dell'ecosistema** | Tutto ciò descrivibile indipendentemente dall'incontro con Prospero (SAP, Claude, FastAPI, n8n, ecc.) |
| `04-workflow.md` | **Cap. 4 — Workflow, pipeline e oggetti specifici** | Pilastro — tutto ciò che non esiste al di fuori dell'incontro con Prospero |
| `glossario.md` | **Glossario** | Definizioni tecniche di base trasversali (REST, JSON, endpoint, codici HTTP, ecc.) |

**Criterio biografico cap. 3 vs cap. 4:** "esiste indipendentemente dall'incontro con Prospero" → cap. 3. "Non esiste al di fuori dell'incontro con Prospero" → cap. 4.

**Pilastri:** cap. 1 e cap. 4. Cap. 2 e cap. 3 si approfondiscono in proporzione all'utilità per le automazioni.

### Pattern studio → consolidamento

Il Manuale è il luogo della conoscenza stabilizzata. Lo studio in corso vive nel progetto **FORMAZIONE IT** come scratchpad: ogni esplorazione lì può non lasciare traccia, oppure può consolidarsi in un paragrafo del cap. 3 (componenti) o nel glossario, occasionalmente come principio nel cap. 2.

---

## 9. ARCHITETTURA IT

### Layer

```
COMMAND LAYER        → Claude (chat + progetti), Reminders, Asana, trigger manuali
        ↓
ORCHESTRATOR LAYER   → n8n (PC Lauria + Mac da configurare)
        ↓
ENGINE LAYER         → SAP Query Engine v2.5, crea_moduli_vuoti.py,
                       crea_offerta.py, registra_fattura.py, aggiorna_attivita.py,
                       FastAPI piattaforma-ai (Render)
        ↓
PERSISTENCE LAYER    → SAP B1, GitHub Pages, Dropbox, Apple Reminders/Calendar,
                       Render (hosting piattaforma-ai)
        ↓
INTERFACCE           → Viceconti Hub, Hub Documentale, MODULO TECNICO HTML,
                       Piattaforma AI-readable (JSON+HTML+Swagger), Telegram, PrestaShop
```

### Ambiente tecnico

| Risorsa | Dettaglio |
|---------|-----------|
| ERP | SAP Business One 10.0, SQL Server, Service Layer operativo |
| Server DB | SQLPRD0303 (192.168.122.99) |
| PC sviluppo | Win11 sede Lauria — script, n8n, Task Scheduler |
| MacBook Air | Mobile — Claude Desktop, Apple Reminders/Notes MCP |
| Script path | C:\Viceconti\viceconti-hub |
| Piattaforma AI-readable | C:\Viceconti\piattaforma-ai\prima-pietra (locale), Render (cloud) |
| Dropbox moduli | C:\Users\PC\Dropbox\HUB DOCUMENTALE\FILE TEMPORANEI\MODULI ATTIVITA' APERTE |
| GitHub org | viceconti-hub (repos: portale, hub-documentale, contesto-ai, **piattaforma-ai privato**) |

### Visibilità repo GitHub

| Repo | Usa GitHub Pages? | Sensibile? | Stato | Coerenza |
|------|-------------------|-----------|-------|----------|
| `contesto-ai` | Sì | No | Pubblico | ✅ |
| `portale` | Sì | Sì (JSON SAP) | Pubblico | ⚠️ |
| `hub-documentale` | Sì | Sì (catalogo 257 clienti) | Pubblico | ⚠️ |
| `piattaforma-ai` | No (Render) | Sì (listino Morini) | Privato | ✅ |

Le incoerenze su `portale` e `hub-documentale` derivano dal vincolo strutturale GitHub Pages free (Pages funziona solo con repo pubblici). Tre opzioni future: status quo, upgrade GitHub Pro/Team, migrazione a Cloudflare Pages. **Decisione rinviata.**

### Credenziali e scadenze

| Credenziale | Scadenza | Stato |
|-------------|----------|-------|
| Token GitHub | 2 maggio 2027 | ✅ Rigenerato 02/05/2026 |
| Token Dropbox OAuth2 | Nessuna | — |

---

## 10. OPEN ITEMS — Priorità aggiornate al 5 maggio 2026

| Priorità | Attività | Progetto |
|----------|---------|---------|
| 🔴 Alta | **Weekend 9-10/05/2026 — Chiusura automazione fatture passive di servizio.** Lo script di smistamento Python è l'unico pezzo nuovo da scrivere. Pipeline completa: Directory Analyzer → cartella locale A_DISPOSIZIONE → script smistamento → Dropbox/FATTURE DA FORNITORE/APERTE → registra_fattura.py → SAP via Service Layer. Perimetro: solo fatture servizi | SAP SERVICE LAYER |
| 🔴 Alta | **Scelta architetturale: come comporre traiettoria FastAPI server-side e portale client-side esistente.** Tre strade possibili da nominare e valutare consapevolmente alla luce del riconoscimento del 03/05/2026 sul portale come dispositivo Livello 1 emerso retrospettivamente | Trasversale (PIATTAFORMA AI-READABLE / VICECONTI HUB) |
| 🔴 Alta | Riorganizzazione progetti Claude (creare MANUALE AUTOMAZIONI, PIATTAFORMA AI-READABLE, FORMAZIONE IT riaperto; archiviare candidati inattivi) | Trasversale |
| 🔴 Alta | Fix fetch contesto-ai (diagnostica problema spazi nei nomi file) | CONTESTO AI |
| 🔴 Alta | Setup struttura Manuale (scaffold dei 5 file) | MANUALE AUTOMAZIONI |
| 🔴 Alta | Task Scheduler per `crea_moduli_vuoti.py` sul PC Lauria | SAP SERVICE LAYER |
| 🔴 Alta | Completamento `registra_fattura.py` | SAP SERVICE LAYER |
| 🟡 Media | **Indice contesto-ai come JSON statico su GitHub Pages** (Variante A: manutenzione manuale per il primo passo, senza FastAPI). Decisione 05/05/2026 | CONTESTO AI |
| 🟡 Media | Connessione FastAPI → Database Centralizzato SQLite | PIATTAFORMA AI-READABLE |
| 🟡 Media | Aggiunta entità anagrafica clienti alla piattaforma | PIATTAFORMA AI-READABLE |
| 🟡 Media | Endpoint `/marche` (discovery prefissi) | PIATTAFORMA AI-READABLE |
| 🟡 Media | INDICE_SCRIPT.md come prima mossa documentale sul nodo script | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 1 Manuale (sequenze task reali) | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 2 Manuale (principi, materiale già maturo) | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 4 Manuale (workflow, format derivato da `registra_fattura.py`) | MANUALE AUTOMAZIONI |
| 🟡 Media | Visibilità repo `portale` e `hub-documentale` (decisione rinviata) | SITI WEB |
| 🟡 Media | Approfondimento ordini d'acquisto come coda di lavoro (priorità per migliorare la registrazione fatture merci a valle) | SAP SERVICE LAYER |
| 🟡 Media | n8n su Mac (per nodi Apple nativi: Reminders, Calendar) | n8n |
| 🟡 Media | Allineamento Reminders ↔ SAP Activities via n8n | n8n |
| 🟡 Media | Smistamento AudioPen per tag via n8n | n8n |
| 🟡 Media | Aggiustamenti Hub Documentale, Viceconti Hub | Trasversale |
| 🟡 Media | Definizione istruzioni workflow moduli tecnici | SAP SERVICE LAYER |
| 🟡 Media | Risposta Vincenzo Strazzullo (email 11/04 — tool PDF, B1iF, XML, esposizione SL) — verosimilmente dopo decisione infrastrutturale | SAP SERVICE LAYER |
| 🟡 Media | Contattare TeamSystem per riattivazione DA | NUOVA STRATEGIA |
| 🟡 Media | Pulizia attività storiche SAP (< 7000) | SAP |
| 🟢 Strategica | Decisione infrastruttura SAP (alternative 1/2/3/4) e identificazione partner sistemistico — non urgente, ma importante | AUDIT INFRASTRUTTURA |
| 🟢 Strategica | Test di navigazione autonoma di Claude su contesto-ai come dispositivo ipertestuale di Livello 1 (dopo fix fetch + decisione forma indice) | CONTESTO AI |
| 🔵 Bassa | Esplorazione Slack | Trasversale |
| 🔵 Bassa | n8n MCP server (quando arriverà workflow significativo da costruire) | n8n |
| 🔵 Bassa | n8n — problema AudioPen → Telegram → Drive da diagnosticare | n8n |
| 🔵 Bassa | Eventuale quaderno GitHub formativo | Da definire |

### Open items chiusi rispetto al 03/05/2026

- ✅ **Calendar in Interfaccia Service Layer** — risolto 04-05/05/2026. Workflow n8n ripristinato, sincronizzazione attività SAP su tre calendari (Assistenza/Consegne/Sopralluoghi) tornata operativa

---

## 11. CONSULENTI E PARTNER

| Nome | Ruolo | Stato |
|------|-------|-------|
| Vincenzo Strazzullo (4utime.it) | SAP B1 consulting, Service Layer | Email inviata 11/04/2026, risposta in attesa |
| Antonio Forlani (3W Sistemi) | Infrastruttura server | Operativo |
| Z3 Engineering (Var One) | Licenze SAP, S-user | Da contattare |

---

## STORIA REVISIONI

| Versione | Data | Note |
|----------|------|------|
| 1.0 | 03/05/2026 mattina | Prima riscrittura sostanziale che incorpora cornice strategica 28/04, deploy prima pietra 02/05, principi nuovi del weekend (Destinatario First, Duplicazione consapevole, Dispositivo retroattivo, Scalabilità progressiva, Studio vs conoscenza stabilizzata, Infrastruttura impone vincoli). Aggiunta sezione 6 (Triangolazione AI). Riapertura FORMAZIONE IT. |
| 1.1 | 03/05/2026 mattina | Riorganizzazione sezione 5 (Mappa Ecosistema) in progressione 100-400s, allineata a struttura progetti Claude e cartelle Drive. Sostituite 8 categorie tematiche A-H con 4 progressioni generative (punto di partenza → strato concettuale → componenti → artefatti). |
| 1.2 | 03/05/2026 mezzogiorno | Incorporato il riconoscimento retrospettivo dalla sessione progetto FastAPI: Viceconti Hub (portale) è dispositivo Livello 1 emerso retrospettivamente. Distinti due tipi di dispositivi al L1 (costruiti coscientemente / emersi retrospettivamente). Aggiunto principio "Server-side vs client-side rendering come scelta di leggibilità per famiglia di consumer". Nuovo open item 🔴 Alta sulla scelta architetturale di composizione FastAPI/portale. |
| 1.3 | 05/05/2026 sera | Incorporato il delta dei giorni 4-5 maggio. **Sezione 2 (Cornice strategica):** aggiornamento "SAP come system of record operativo emergente" (oltre a rappresentazione strutturata della realtà), evoluzione del L1 da canale di lettura a canale di interazione potenziale (FastAPI per scrittura via Service Layer), nota su pattern unificato interno/B2B. **Sezione 3:** Calendar Interfaccia Service Layer ✅ risolto (4-5/05); pipeline voce → SAP → SL → JSON → AI testata end-to-end (5/05). **Sezione 4:** aggiunti principi "Lavoro distribuito sui progetti / ritorno strategico al centro" (era rimasto fuori dalla 1.2), "Articolare e fissare le fughe in avanti", "Payload polifonico", "Stella di output paralleli da fonte unica", "Istruzioni operative latenti dentro campi narrativi". **Sezione 6:** famiglia 2 riformulata da "macchine non intelligenti" a "macchine deterministiche". **Sezione 10:** weekend 9-10/05 in testa (chiusura automazione fatture passive di servizio); Calendar chiuso; aggiunti item operativi per indice contesto-ai come JSON statico, cap. 2 Manuale, ordini d'acquisto come coda di lavoro; aggiunti due item strategici (decisione infrastruttura SAP, test navigazione autonoma su contesto-ai). Materiale lasciato per Cap. 2 Manuale a freddo: cattura vocale come quarta fonte di popolamento, lezione metodologica nomi campi legacy, convergenza inter-progetto come segno di maturità. |

---

*Aggiornato al 5 maggio 2026 sera. Sostituisce versione del 3 maggio 2026.*
*Prossimo aggiornamento previsto: dopo weekend 9-10/05/2026 (chiusura automazione fatture passive di servizio) o dopo decisione infrastrutturale.*
