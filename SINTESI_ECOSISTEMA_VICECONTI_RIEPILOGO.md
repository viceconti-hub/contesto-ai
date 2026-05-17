# SINTESI ECOSISTEMA VICECONTI
## Documento di visione e stato operativo

*Aggiornato al 10 maggio 2026 sera — sostituisce versione del 5 maggio 2026*

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

### SAP come system of record operativo emergente (formulazione 05/05/2026)

Riconoscimento retrospettivo: le automazioni costruite per ottimizzare l'assistenza tecnica hanno prodotto un effetto laterale non programmato. Il modulo Attività di SAP è diventato l'ingresso a maggior efficienza per registrare task operativi, perché l'output è ormai automatizzato (Calendar, Viceconti Hub, in prospettiva Telegram e AI).

**SAP sta evolvendo dal ruolo di "rappresentazione strutturata della realtà aziendale" (registra il passato) verso il ruolo di "system of record operativo" (orchestra il presente).** L'estensione del pattern dall'assistenza alle consegne, e ora alle attività personali, consolida questa evoluzione. Non sostituisce la formulazione originaria — la specifica e la arricchisce.

### I tre livelli di integrazione Claude-SAP

L'integrazione non è un evento futuro — è già articolata su tre livelli operativi:

| Livello | Funzione | Stato | Modalità |
|---------|----------|-------|----------|
| **L1 — Pagine pubbliche AI-readable** | Interrogazioni standard ricorrenti, dati strutturati, accesso da Claude.web senza VPN | ✅ Operativo | **Due dispositivi:** Piattaforma FastAPI su Render (costruita coscientemente come tale, prima pietra 02/05); Viceconti Hub (portale) — dispositivo emerso retrospettivamente il 03/05, a un passo dalla soglia |
| **L2 — Claude Code da desktop** | Interrogazioni libere, esplorazioni, accesso diretto al Service Layer | ✅ Operativo | Postazione di lavoro, VPN |
| **L3 — Cowork via Dispatch da mobile** | Interrogazioni libere quando non al desktop | ✅ Operativo | Smartphone → Cowork → Claude Code |

**Decisione 10/05/2026 sulla composizione FastAPI/portale (era open item 🔴 Alta della v1.3):** l'interfaccia portale converge dentro la traiettoria FastAPI come secondo consumer della Piattaforma AI-readable. Endpoint `/attivita` come caso d'uso 2 (dopo articoli Morini). Query Engine specializzato come produttore di snapshot storici, non più fonte primaria del portale. Coerente con principio "stella di output paralleli da fonte unica" e "pattern unificato interno/B2B". Cantiere stimato 1-2 sessioni medie sul progetto 390. Non urge — il portale GitHub funziona, non c'è blocco operativo. Ma è il cantiere che porta il maggior valore architetturale a parità di sforzo.

**Evoluzione del Livello 1 da canale di lettura a canale di interazione.** Riconoscimento del 05/05/2026: FastAPI può inoltrare operazioni di scrittura su SAP via Service Layer, aggiungendo strati di autenticazione, validazione, idempotenza e tracciabilità. Il Livello 1 evolve quindi da canale di lettura a canale di interazione potenziale — le tre famiglie di consumer diventano potenzialmente agenti capaci di azione, non solo lettori. Stato attuale: capability nominata, non ancora costruita.

### Nota strategica — Pattern unificato interno/B2B

Riconoscimento del 05/05/2026: il lavoro sui flussi interni (esposizione delle attività SAP al web, scrittura via FastAPI, autenticazione e controllo accessi) è strutturalmente identico al lavoro che servirebbe per un B2B esterno (clienti rivenditori). **Investimento unico, due rendimenti.** È la traiettoria FastAPI che si rivela centrale, non collaterale. Non costituisce cantiere prossimo — orienta le scelte di lungo periodo.

---

## 3. STATO OPERATIVO — Maggio 2026

### Componenti in produzione

| Componente | Stato | Note |
|-----------|-------|------|
| **SAP Business One 10.0 + Service Layer** | ✅ Operativo | Attivo dal 29/03/2026. Server SQLPRD0303 stabilizzato |
| **SAP Query Engine v2.5** | ✅ Operativo | Estrazioni orarie, JSON su GitHub Pages via API. Specializzato come produttore di snapshot storici (decisione 10/05) |
| **Piattaforma AI-readable (FastAPI)** | ✅ Prima pietra in produzione | Deployata su Render 02/05/2026. URL `piattaforma-ai.onrender.com`. 19.476 articoli Morini, endpoint JSON+HTML+Swagger. **Dispositivo Livello 1 costruito coscientemente come tale** |
| **n8n v2.14.2** | ✅ Operativo | PC Lauria, ngrok dominio statico, Task Scheduler auto-start |
| **Viceconti Hub + Hub Documentale** | ✅ Operativi | GitHub Pages. **Riconoscimento 03/05/2026: il portale è dispositivo Livello 1 emerso retrospettivamente, a un passo dalla soglia.** Decisione 10/05: convergerà dentro FastAPI come secondo consumer della piattaforma |
| **Contesto AI su GitHub Pages** | ✅ Operativo | repo contesto-ai, file .md a nome fisso. Cap. 3 doppio binario HTML+JSON completato 09/05/2026 |
| **Hub Documentale: estensione `.md`** | ✅ Operativo dal 09/05/2026 | Whitelist `.md` aggiunta su tutti e tre gli script indexer. L'Hub ora include documenti AI-readable, non solo business |
| **`RIAPRI E MODIFICA MODULO TECNICO.html`** | ✅ Beta produzione | v4: auto-idratazione, modulo vuoto, campi header editabili |
| **`crea_moduli_vuoti.py`** | ✅ Operativo | Legge JSON da GitHub, genera HTML pre-compilati. Non sovrascrive file esistenti |
| **`crea_offerta.py`** | ✅ Operativo | Versione batch: file multipli, --cartella, --dry-run, sposta in PROCESSATI. 29 offerte create |
| **`registra_fattura.py`** | ✅ Operativo | Pipeline ora completa con DA attivo (vedi sotto) |
| **`archivio_documenti_sap.py` v2** | ✅ **Nuovo (10/05/2026)** | Multi-tipo + archivio storico flat. Smista PDF da `DOCUMENTI_SAP\` deposito alle cartelle archivio per tipo. Idempotente, non distruttivo, cleanup-by-DocNum. 14 file processati nei test, 5 tipi mappati (Ordine cliente, Offerta, Consegna, Fattura vendita, Entrata merci) + 1 in workspace (Ordine d'acquisto come bozza interna) |
| **Cartella SAP Attachments riconfigurata** | ✅ **Nuovo (10/05/2026)** | Path SAP "Cartella per allegati file" spostato da `\\sqlprd0303\B1_SHR\VICECONTI\Attachments\` a `C:\Users\PC\Dropbox\HUB DOCUMENTALE\DOCUMENTI_SAP\`. Tutto dentro Dropbox, sync automatico, niente più SMB |
| **Directory Analyzer TS Digital** | ✅ **Riattivato (settimana 04-08/05/2026)** | Era fermo dal settembre 2023 per credenziali expired. Ora scarica automaticamente XML ciclo attivo (`C:\TS_SYNCRO\AGYO-DOWNLOAD\Ciclo Attivo\...\INVIATO\`) e ciclo passivo (`C:\TS_SYNCRO\AGYO-DOWNLOAD\Ciclo Passivo\...\A_DISPOSIZIONE\`). Pipeline `registra_fattura.py` ora alimentata in modo completo |
| **UDF SAP `RifFile`** | ✅ **Nuovo (10/05/2026)** | Campo definito utente sui Document header (Ordini, Offerte, Consegne, Fatture, ecc.) per riferimento descrittivo nel naming archivio. Si propaga via Service Layer a tutte le superfici di consumo |
| **ASSISTENTE AMMINISTRATIVA** | ✅ Avviata | Progetto Claude operativo da 13 aprile 2026 |
| **Apple Reminders** | ✅ Componente adottata | Layer di task management tra cattura grezza e sistemi di destinazione |
| **Interfaccia Service Layer + sync Calendar** | ✅ Operativo | Sincronizzazione attività SAP su Google Calendar (Assistenza/Consegne/Sopralluoghi) |
| **Pipeline link in descrizione attività SAP → Calendar/Hub/SAP client** | ✅ **Validata 10/05/2026** | Link inserito in descrizione attività SL si propaga automaticamente a Calendar (mobile e desktop), portale Viceconti Hub (colonna Contenuto), client SAP nativo. Test 6 casi conclusivo |

### Workflow archivio documenti SAP — pipeline completa in produzione

```
SAP B1 anteprima di stampa
→ Salva PDF automaticamente in C:\Users\PC\Dropbox\HUB DOCUMENTALE\DOCUMENTI_SAP\
   con naming nativo: <TIPO>_<DOCNUM>_<YYYYMMDD>_<HHMMSS>.pdf
→ Sync automatico Dropbox visibile su tutti i dispositivi (anche mobile)

archivio_documenti_sap.py (PC Lauria, su lancio)
→ Legge file da DOCUMENTI_SAP\
→ Per ogni file: estrae TIPO+DocNum, lookup Service Layer per CardName + RifFile
→ Smista in cartella archivio per tipo:
   - VENDITE\ORDINI CLIENTE\
   - VENDITE\OFFERTE DI VENDITA\
   - VENDITE\CONSEGNE\
   - VENDITE\FATTURE DI VENDITA\
   - ACQUISTI\ENTRATE MERCI DA FORNITORE\
   - ACQUISTI\ORDINI D'ACQUISTO WORKSPACE\BOZZE\  (PO è bilaterale, va in workspace)
→ Naming arricchito: <CardName> <NamingSAP>[ RIF. <RifFile>].pdf
→ Cleanup-by-DocNum: una sola versione attiva per ordine in archivio
→ Sorgente DOCUMENTI_SAP intatta (non distruttivo)

(Manuale, opzionale) Link condivisione del PDF Dropbox
→ Inserito in descrizione attività SAP correlata via Interfaccia SL
→ Si propaga automaticamente a Calendar, portale Hub, client SAP
→ Tecnico in cantiere apre link da Calendar mobile → vede PDF in app Dropbox
```

### Workflow registrazione fatture passive — pipeline ora completa

```
Fornitore → SDI → AGYO/TS Digital
↓
Directory Analyzer scarica automaticamente XML in
C:\TS_SYNCRO\AGYO-DOWNLOAD\Ciclo Passivo\01601090762\SDIPR\A_DISPOSIZIONE\
↓
registra_fattura.py (PC Lauria)
→ Legge XML, registra fattura in SAP via Service Layer
→ Sposta XML processato in cartella REGISTRATE
```

---

## 4. PRINCIPI ARCHITETTURALI CONSOLIDATI

| Principio | Descrizione |
|-----------|-------------|
| **Destinatario First** | Decidi consapevolmente quale destinatario è primario e costruisci a partire da lì. **HTML First** (manufatto persistente è HTML con JSON embedded) e **JSON First** (manufatto persistente è JSON, HTML come vista derivata) sono due specializzazioni dello stesso principio sovraordinato |
| **HTML come trinità** | Quando convergono umano e macchina sullo stesso destinatario: interfaccia + documento + dato strutturato in un file solo |
| **L'apparato del consumer guida la scelta del formato** | *(Nuovo 10/05/2026)* Un formato sembra superiore quando lo si valuta in astratto (HTML è editabile, JSON è strutturato), ma la sua superiorità si misura sull'**apparato concreto** che il consumer userà per consumarlo. Apparato = dispositivo + software + competenze + connettività. PDF + Dropbox + iPad + Apple Pencil è un apparato maturo, familiare, mobile-native; HTML + browser-edit + sync-back-to-sap è un apparato meno maturo per il caso d'uso reale dei tecnici in cantiere |
| **Duplicazione consapevole per destinatari divergenti** | Quando i destinatari hanno esigenze divergenti, conviene tenere contenitori distinti anche al costo di duplicare. Esempio canonico: due ecosistemi paralleli di siti pubblici Viceconti — umano-facing (Store, B2B) e AI-facing (Hub, Hub Documentale, contesto-ai, piattaforma AI-readable) |
| **La struttura del Hub Documentale rispecchia le famiglie di consumer** | *(Nuovo 10/05/2026)* Per ogni tipo documento esistono al massimo tre cartelle parallele al primo livello: `<TIPO>\` per umani in browser, `<TIPO> XML\` (o JSON) per macchine, `<TIPO> WORKSPACE\` per workflow operativo. Non tutti i tipi richiedono tutte e tre — si aggiungono al bisogno. La struttura è incrementale: cresce con i bisogni reali, non per design a priori. Coerente con la sezione 6 sulle tre famiglie di consumer |
| **Dispositivo che cambia retroattivamente il significato del dato** | L'esistenza di un dispositivo che renda interrogabili dati aziendali da AI cambia retroattivamente cosa ha senso produrre |
| **Scalabilità progressiva** | Non costruire mai a scala completa quando puoi validare il pattern a scala ridotta |
| **SAP genera documenti come effetto collaterale dell'anteprima di stampa** | *(Nuovo 10/05/2026)* L'azione "anteprima" di SAP, oltre a mostrare il PDF a video, lo deposita nella cartella configurata in Parametrizzazione Generale. È un comportamento del sistema esistente che si converte in risorsa strutturale quando viene riconosciuto. Versione concreta del principio "le automazioni abilitano il disegno esistente" — non serve nessuna automazione nuova, serve solo nominare il pattern. **Importante**: la cartella di destinazione è configurabile, può essere ridiretta a percorsi Dropbox o cloud storage senza impattare il funzionamento SAP |
| **Archivio vs coda di lavoro come pattern strutturale** | *(Nuovo 10/05/2026)* Non tutte le cartelle hanno la stessa funzione. *Archivi*: persistenti, completi, organizzati per consultazione, contenuto si accumula. Il file resta indipendentemente dallo stato dell'entità. *Code di lavoro*: transitorie, parziali, organizzate per azione, contenuto si svuota. Il file esce quando l'azione è completata. Naming convention conseguente: verbi attivi per code (`FATTURE DA REGISTRARE`, `MODULI IN LAVORAZIONE`), nomi statici per archivi (`ORDINI CLIENTE`, `FATTURE DI VENDITA`) |
| **Separazione archivio-workspace** | *(Nuovo 10/05/2026)* L'archivio contiene manufatti finali organizzati per consultazione. Il workspace contiene strumenti di elaborazione, code di lavoro, documentazione operativa, dati di supporto, organizzati per task. Tenerli mescolati fa crescere la cartella in modo organico ma non scalabile. Tenerli separati richiede disciplina iniziale ma rende il sistema replicabile |
| **Una sola copia per chiave naturale in archivio** | *(Nuovo 10/05/2026)* Quando un archivio è derivato da una fonte automatica che genera versioni multiple della stessa entità (anteprime ripetute, regenerate), la regola dell'archivio canonico è: una versione attiva per chiave naturale, le altre rimosse. La fonte resta integra (storia delle versioni), l'archivio resta pulito (solo versione corrente). Pattern: clean-up-by-key sulla destinazione, mai sulla fonte |
| **Archivio canonico ≠ output SAP** | *(Nuovo 10/05/2026)* L'archivio canonico per un tipo documento non è automaticamente il PDF generato da SAP. Per documenti unilaterali (ordine cliente, offerta, consegna), SAP genera il documento canonico. Per documenti bilaterali (ordine d'acquisto, fattura), il documento canonico è quello esterno (conferma fornitore, fattura elettronica XML), e il PDF SAP è solo bozza/rappresentazione interna. La logica di smistamento deve distinguere per tipo dove va il PDF |
| **Completezza come condizione necessaria per archivi AI-consumati** | *(Nuovo 10/05/2026)* Un archivio destinato a consumer umani tollera lacune (l'umano integra con altri canali, ricorda, chiede). Un archivio destinato a consumer AI no: l'assenza di un documento è interpretata come "non esiste", non come "non è stato salvato". L'incompletezza distrugge la fiducia. Per archivi AI-consumati, il riempimento manuale è strutturalmente insufficiente — l'unica via è automazione per costruzione |
| **Effetto leva nelle proiezioni multiple** | *(Nuovo 10/05/2026)* Quando esiste già una stella di output paralleli da fonte unica (Calendar, Hub portale, client SAP tutti alimentati dal Service Layer), arricchire un singolo campo della fonte si propaga gratuitamente a tutte le superfici di consumo. L'investimento nella stella si capitalizza ad ogni nuovo arricchimento. Specializzazione del principio "Stella di output paralleli da fonte unica": la stella non porta solo *dati* dalla fonte ai raggi, porta anche **URL e arricchimenti narrativi** |
| **Verifica empirica delle assunzioni infrastrutturali** | *(Nuovo 10/05/2026)* Quando l'utente dichiara una configurazione di sistema (path, credenziali, presenza di un campo, struttura di un servizio), è prudente verificarla empiricamente prima di costruire sopra. Le assunzioni infrastrutturali sono particolarmente fragili perché sembrano oggettive — un path è un path — ma in realtà dipendono da scelte di setup. Pattern: "verifica prima, fida dopo". Vale tanto per Claude assistente architetturale quanto per Claude operativo |
| **Pulizia architetturale anticipata** | *(Nuovo 10/05/2026)* Quando un trap di configurazione (es. doppio prefisso UDF) viene scoperto in fase di MVP, il costo della pulizia è limitato ai dati di test. Posticipare significa accumulare uso reale che rende la pulizia sempre più costosa. Pulire subito è una forma di disciplina di sistema |
| **Studio in corso vs conoscenza stabilizzata** | Lo studio è disordinato per natura — vive in scratchpad. La conoscenza stabilizzata è ordinata per costruzione — vive in luoghi cristallizzati |
| **Lavoro distribuito sui progetti, ritorno strategico al centro** | I progetti specifici sono il luogo del lavoro analitico; la chat principale (Sintesi Ecosistema) è il luogo del consolidamento strategico. Il ritorno deve riportare anche le valutazioni strategiche emerse, non solo le decisioni operative |
| **Articolare e fissare le fughe in avanti** | Le visioni che generano "fughe in avanti" hanno valore strategico, ma il loro fascino dipende dalla loro distanza dai dettagli. Pratica: lasciar uscire la fuga, articolarla, riconoscerla come parentesi, fissare in punto stabile, tornare al lavoro di base |
| **L'infrastruttura impone vincoli non nominati** | Quando un vincolo strutturale di un servizio condiziona un'architettura senza essere esplicitato, si propaga a cascata generando incoerenze invisibili. Va nominato per essere disinnescato |
| **Server-side vs client-side rendering come scelta di leggibilità per famiglia di consumer** | Per dispositivi destinati a consumer AI, il server-side è strutturalmente più robusto del client-side |
| **Payload polifonico** | Un payload JSON ben progettato è polifonico: campi strutturati per macchine deterministiche, campi narrativi per macchine semantiche, sullo stesso payload, senza contropartite. La famiglia deterministica ignora i campi narrativi; la semantica li valorizza |
| **Stella di output paralleli da fonte unica** | Le automazioni di output devono attaccarsi alla **fonte di verità** (Service Layer), mai a una **proiezione** (Calendar, Hub). SAP è la fonte; Calendar, Viceconti Hub, Telegram, AI sono raggi paralleli |
| **Istruzioni operative latenti dentro campi narrativi** | I campi narrativi possono contenere prescrizioni destinate ad agenti non umani, scritte prima ancora che esista il canale per eseguirle, attivabili retroattivamente quando il canale esisterà |
| **Attività SAP come trigger documentale universale** | Qualsiasi tipo di attività SAP può generare un HTML pre-compilato che uno script trasforma in qualsiasi documento SAP |
| **Puzzle (non architettura sequenziale)** | I pezzi dell'automazione possono essere completati in qualsiasi ordine senza dipendenze sequenziali obbligatorie |
| **Digitizzazione ≠ automazione** | Digitizzazione rende le cose digitali. Automazione le rende auto-eseguibili. Confonderle porta ad aspettative irrealistiche |
| **Ogni canale di input ha una destinazione finale** | Telegram non è mai la destinazione. Il flusso deve uscire verso il sistema giusto prima che l'informazione diventi difficile da recuperare |
| **Separazione input/struttura** | Cattura e strutturazione sono atti cognitivi distinti. Il modello a 2 step li separa deliberatamente |
| **Le automazioni abilitano il disegno esistente** | Il disegno operativo era già buono — sedimentato in anni di pratica. Le automazioni non inventano nuovi processi, sbloccano quelli esistenti |
| **Paradosso di Jevons organizzativo** | Migliorare l'efficienza di un sistema tende ad aumentare la domanda su quel sistema |

---

## 5. MAPPA DELL'ECOSISTEMA

L'ecosistema è organizzato in **quattro progressioni numeriche** che riflettono la stessa logica generativa del Manuale Automazioni: dal punto di partenza, attraverso lo strato concettuale e i componenti, fino agli artefatti prodotti dall'incontro tra le tre cose precedenti.

### 100s — PUNTO DI PARTENZA

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 100 | STRATEGIA ORGANIZZAZIONE | Attivo | Transizione s.n.c. → s.r.l. unipersonale (luglio liquidazione SNC, settembre nuova società) |
| 110 | AUDIT INFRASTRUTTURA | Completato | Server SQLPRD0303 stabilizzato dopo crisi marzo 2026 |

### 200s — STRATO CONCETTUALE

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 200 | AI HUMAN LAB | Attivo | Sapere personale che sedimenta lentamente |
| 210 | NAMING VICECONTI | Completato | Convenzioni di naming file e cartelle |
| 220 | FORMAZIONE IT | 🟢 Riaperto 03/05 | Scratchpad dello studio in corso. Materiale FastAPI/REST/JSON |

### 300s — COMPONENTI

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 310 | SAP ACADEMY | Attivo | Documentazione tecnica SAP B1 e Service Layer |
| 311 | SAP SERVICE LAYER | ✅ Operativo | Attivo dal 29/03/2026. **10/05: nuovo script `archivio_documenti_sap.py` v2 in produzione (multi-tipo + archivio storico)** |
| 320 | STRUMENTI DI CATTURA VOCALE | Attivo | AudioPen Prime, microfono nativo Claude, Jamie Live, valutazione PLAUD |
| 350 | n8n | ✅ Operativo | v2.14.2 su PC Lauria |
| 360 | CALENDAR | Attivo | Google Calendar. 18 calendari mappati via MCP |
| 370 | TELEGRAM | ✅ Operativo | Notifiche + canale NOTE PERSONALI |
| 390 | FastAPI | 🟢 Espansione | Framework Python per API REST. Prima pietra Render. **10/05: decisione di estensione con endpoint `/attivita` come secondo consumer** |

### 400s — ARTEFATTI

| # | Progetto | Stato | Descrizione |
|---|----------|-------|-------------|
| 400 | MANUALE DELLE AUTOMAZIONI | 🟡 In strutturazione | 5 file su `contesto-ai/manuale/`. **10/05: candidato cap. 3 = "Trap SAP B1 noti", primo: doppio prefisso UDF** |
| 401 | SINTESI ECOSISTEMA VICECONTI | Attivo | Questo documento. Mappa di stato e visione complessiva |
| 410 | CONTESTO AI | ✅ Operativo | Repo `contesto-ai`. **09/05: doppio binario HTML+JSON completato. Whitelist `.md` su Hub Documentale aggiunta** |
| 420 | ASSISTENTE AMMINISTRATIVA | ✅ Avviata | Operativa dal 13/04/2026 |
| 421 | ASSISTENTE DI PROGETTO | Attivo | Assistente per gestione progetti |
| 430 | HUB DOCUMENTALE | ✅ **Esteso (10/05)** | GitHub Pages. **10/05: struttura cartelle a 3 livelli per famiglia consumer (`<TIPO>`, `<TIPO> XML`, `<TIPO> WORKSPACE`)** |
| 440 | VICECONTI HUB | ✅ Operativo | Estrazione dati SAP via SQL su GitHub Pages. **10/05: decisione di convergenza con FastAPI per latenza zero** |
| 450 | INTERFACCIA SERVICE LAYER | Attivo | Interfaccia per scrittura SAP via Service Layer. **10/05: pipeline link in descrizione attività validata end-to-end** |
| 460 | DATABASE CENTRALIZZATO | Operativo non integrato | SQLite con 9.785 prodotti Morini |
| 470 | ASSET PRODOTTI | Da definire | — |
| 490 | PIPELINE PRESTASHOP | Standby consapevole | Da riprendere H2 2026 |

---

## 6. TRIANGOLAZIONE AI COME DATO STRUTTURALE

L'ecosistema Viceconti dialoga con più modelli AI (Claude, ChatGPT, Gemini). Il loro comportamento differente non è "AI con fetch / AI senza fetch" — è un dato strutturale a tre piani:

| Piano | Definizione | Esempio (caso Bormioli, validazione 02/05) |
|-------|-------------|--------------------------------------------|
| **Capability** | Cosa il modello può fare | Fetch HTTP, code execution, multimodale |
| **Design behavior sotto vincolo** | Cosa fa quando manca un dato | Claude chiede o pivota; Gemini riempie con plausibilità; ChatGPT con prompt operativo esegue |
| **Capability fallback** | Cosa fa quando una via è bloccata | Claude pivota su capability alternative |

**Conseguenza:** un'AI con capability ricche ma design behavior sbagliato è più pericolosa di un'AI con poche capability ma design coerente.

### Le tre famiglie di consumer

| Famiglia | Esempi | Operano su |
|----------|--------|------------|
| **1. Umani in browser** | Tecnici, amministrazione, clienti | Rendering visivo, interazione UI |
| **2. Macchine deterministiche** | n8n, automazioni, script | Campi strutturati con codice deterministico |
| **3. Macchine semantiche** | Claude, ChatGPT, Gemini | Campi strutturati + campi narrativi |

La distinzione è la base del principio del **payload polifonico** e del nuovo principio (10/05) **la struttura del Hub rispecchia le famiglie di consumer**: per ogni tipo documento le tre cartelle parallele (`<TIPO>`, `<TIPO> XML`, `<TIPO> WORKSPACE`) servono in modo specializzato le tre famiglie + il workflow operativo.

### Pattern di consultazione AI documentale

Distinzione utile (emersa 09/05) sui modi di consumo dei documenti da parte di un'AI:

1. **Contesto in chat** — documento pre-caricato come allegato
2. **Lookup on-demand** — fetch quando serve (lettura selettiva via tool)
3. **Istruzioni operative** — documento come script in linguaggio naturale per AI agentica

Pattern operativo per trascrizioni Code e file di grandi dimensioni: GitHub raw + lettura selettiva via tool. Risparmia context window, permette grep/head/sed mirati. Dropbox via curl funziona per Claude ma non universalmente.

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

**Costellazione di input verso Reminders:**

```
Siri → Reminders diretto (velocità, zero frizione)
AudioPen → Claude/Assistente → Reminders (struttura, contesto ricco)
Note Apple + Pencil → rilettura AudioPen → Claude → Reminders
Telegram → copia-incolla → Reminders (condivisione collaboratori)
```

---

## 8. MANUALE AUTOMAZIONI — Struttura consolidata

Sotto-cartella `manuale/` dentro repo `contesto-ai/`, un file per capitolo.

| File | Capitolo | Natura |
|------|----------|--------|
| `01-sequenze-task.md` | **Cap. 1 — Sequenze di task reali** | Pilastro biografico |
| `02-principi.md` | **Cap. 2 — Principi e metodologia** | I principi della sezione 4 |
| `03-componenti.md` | **Cap. 3 — Componenti dell'ecosistema** | SAP, Claude, FastAPI, n8n, ecc. **+ "Trap SAP B1 noti" (10/05)** |
| `04-workflow.md` | **Cap. 4 — Workflow, pipeline e oggetti specifici** | Pilastro operativo |
| `glossario.md` | **Glossario** | Definizioni tecniche di base |

**Pattern studio → consolidamento:** il Manuale è il luogo della conoscenza stabilizzata. Lo studio in corso vive in **FORMAZIONE IT** come scratchpad. Promozione occasionale al Manuale.

**Nuovo materiale per Cap. 1 (10/05):** trascrizioni Claude Code come artefatti biografici. Salvataggio in `Drive\510.SINTESI_RIEPILOGHI_COWORK\trascrizioni-code\`, eventuale promozione su GitHub `viceconti-hub/contesto-ai/trascrizioni-code/` per consultazione AI. Decisione operativa rimandata.

**Nuovo materiale per Cap. 3 (10/05):** sezione "Trap SAP B1 noti" — primo trap raccolto: doppio prefisso UDF (nominare un UDF "U_RifFile" produce property `U_U_RifFile` su Service Layer perché SAP prefissa automaticamente; corretto è nominare "RifFile").

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
                       archivio_documenti_sap.py v2 (NUOVO 10/05),
                       FastAPI piattaforma-ai (Render)
        ↓
PERSISTENCE LAYER    → SAP B1, GitHub Pages, Dropbox, Apple Reminders/Calendar,
                       Render (hosting piattaforma-ai),
                       TS_SYNCRO\AGYO-DOWNLOAD (Directory Analyzer riattivato 04-08/05)
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
| Script path | `C:\Viceconti\viceconti-hub`, `C:\Users\PC\Dropbox\HUB DOCUMENTALE\ACQUISTI\FATTURE DA FORNITORE\` |
| Piattaforma AI-readable | `C:\Viceconti\piattaforma-ai\prima-pietra` (locale), Render (cloud) |
| Dropbox path locale | **`C:\Users\PC\Dropbox\` (correzione 10/05; non `C:\Dropbox\` come in v1.3)** |
| Cartella deposito SAP | `C:\Users\PC\Dropbox\HUB DOCUMENTALE\DOCUMENTI_SAP\` (configurata in Parametrizzazione SAP) |
| Cartella DA download | `C:\TS_SYNCRO\AGYO-DOWNLOAD\Ciclo Attivo\...\INVIATO\` (vendita) e `\Ciclo Passivo\...\A_DISPOSIZIONE\` (acquisto) |
| GitHub org | `viceconti-hub` (repos: `portale`, `hub-documentale`, `contesto-ai`, **`piattaforma-ai` privato**) |

### Visibilità repo GitHub

| Repo | Usa GitHub Pages? | Sensibile? | Stato | Coerenza |
|------|-------------------|-----------|-------|----------|
| `contesto-ai` | Sì | No | Pubblico | ✅ |
| `portale` | Sì | Sì (JSON SAP) | Pubblico | ⚠️ |
| `hub-documentale` | Sì | Sì (catalogo 257 clienti) | Pubblico | ⚠️ |
| `piattaforma-ai` | No (Render) | Sì (listino Morini) | Privato | ✅ |

### Credenziali e scadenze

| Credenziale | Scadenza | Stato |
|-------------|----------|-------|
| Token GitHub `viceconti-hub push` | 2 maggio 2027 | ✅ |
| Token GitHub `Viceconti Hub` | **9 maggio 2027** | ✅ Rigenerato 09/05/2026 |
| Token GitHub `SAP Query Engine` | **9 maggio 2027** | ✅ Rigenerato 09/05/2026 |
| Token Dropbox OAuth2 | Nessuna | — |

### Struttura Hub Documentale (consolidata 10/05/2026)

Pattern a 3 cartelle parallele per tipo documento:

```
HUB DOCUMENTALE\
├── DOCUMENTI_SAP\                          ← deposito grezzo (effetto SAP anteprima)
├── VENDITE\
│   ├── ORDINI CLIENTE\                     ← archivio (umani)
│   ├── OFFERTE DI VENDITA\                 ← archivio
│   ├── OFFERTE DI VENDITA WORKSPACE\       ← workspace
│   ├── CONSEGNE\                           ← archivio
│   └── FATTURE DI VENDITA\                 ← archivio
├── ACQUISTI\
│   ├── ENTRATE MERCI DA FORNITORE\         ← archivio
│   ├── FATTURE DA FORNITORE\               ← archivio (eredita organizzazione precedente, da pulire)
│   ├── FATTURE DA FORNITORE XML\           ← archivio per macchine
│   ├── FATTURE DA FORNITORE WORKSPACE\     ← workspace operativo
│   ├── OFFERTE DI ACQUISTO\
│   ├── ORDINI D'ACQUISTO\                  ← archivio (conferme fornitori, NON PDF SAP)
│   └── ORDINI D'ACQUISTO WORKSPACE\
│       └── BOZZE\                          ← qui finiscono i PDF SAP degli ordini d'acquisto
└── ... (altre cartelle storiche)
```

---

## 10. OPEN ITEMS — Priorità aggiornate al 10 maggio 2026

| Priorità | Attività | Progetto |
|----------|---------|---------|
| 🔴 Alta | **Pulizia struttura `ACQUISTI\FATTURE DA FORNITORE\`**. La cartella ha accumulato funzioni miste (archivio, code, script, doc, dati). Riorganizzare separando archivio puro da workspace operativo. Cantiere stimato 1-2 ore | Trasversale (HUB DOCUMENTALE / SAP SL) |
| 🔴 Alta | **Endpoint FastAPI `/attivita` come secondo consumer della Piattaforma AI-readable**. Decisione presa 10/05. Il portale Viceconti Hub passerà da JSON statico GitHub a JSON live via FastAPI → Service Layer. Cantiere 1-2 sessioni medie | PIATTAFORMA AI-READABLE |
| 🔴 Alta | Riorganizzazione progetti Claude (creare MANUALE AUTOMAZIONI, PIATTAFORMA AI-READABLE, FORMAZIONE IT riaperto; archiviare candidati inattivi) | Trasversale |
| 🔴 Alta | Setup struttura Manuale (scaffold dei 5 file) | MANUALE AUTOMAZIONI |
| 🔴 Alta | Task Scheduler per `crea_moduli_vuoti.py` e `archivio_documenti_sap.py` sul PC Lauria | SAP SERVICE LAYER |
| 🟡 Media | Decisione architetturale: PDF di `Fattura da fornitore` e `Attività` — dove vanno se vanno? | SAP SERVICE LAYER |
| 🟡 Media | **Test routing reale fattura di vendita su `archivio_documenti_sap.py`**. Mappa pronta, ma nessun file SAP nel pool dei test del 10/05. Validazione al primo file vero | SAP SERVICE LAYER |
| 🟡 Media | Eventuale generazione anche `fattura_di_vendita PDF` SAP nell'archivio (PDF affianca XML canonico). Decisione | SAP SERVICE LAYER |
| 🟡 Media | **Indice contesto-ai come JSON statico su GitHub Pages**. Variante A (manutenzione manuale prima fase) | CONTESTO AI |
| 🟡 Media | Connessione FastAPI → Database Centralizzato SQLite | PIATTAFORMA AI-READABLE |
| 🟡 Media | Aggiunta entità anagrafica clienti alla piattaforma | PIATTAFORMA AI-READABLE |
| 🟡 Media | Endpoint `/marche` (discovery prefissi) | PIATTAFORMA AI-READABLE |
| 🟡 Media | INDICE_SCRIPT.md come prima mossa documentale sul nodo script | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 1 Manuale (sequenze task reali + trascrizioni Code come materiale) | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 2 Manuale (principi, materiale già maturo — 30 principi censiti) | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 3 Manuale (componenti + sezione "Trap SAP B1 noti") | MANUALE AUTOMAZIONI |
| 🟡 Media | Strutturazione cap. 4 Manuale (workflow, format derivato da `registra_fattura.py` e `archivio_documenti_sap.py`) | MANUALE AUTOMAZIONI |
| 🟡 Media | Visibilità repo `portale` e `hub-documentale` (decisione rinviata) | SITI WEB |
| 🟡 Media | Approfondimento ordini d'acquisto come coda di lavoro | SAP SERVICE LAYER |
| 🟡 Media | n8n su Mac (per nodi Apple nativi: Reminders, Calendar) | n8n |
| 🟡 Media | Allineamento Reminders ↔ SAP Activities via n8n | n8n |
| 🟡 Media | Smistamento AudioPen per tag via n8n | n8n |
| 🟡 Media | Risposta Vincenzo Strazzullo (email 11/04 — tool PDF, B1iF, XML, esposizione SL). **Aggiornamento 10/05: caso d'uso preciso ora maturato per tool estrazione documenti** | SAP SERVICE LAYER |
| 🟡 Media | Pulizia attività storiche SAP (< 7000) | SAP |
| 🟡 Media | Estensione `archivio_documenti_sap.py` a watcher real-time (vs lancio manuale) | SAP SERVICE LAYER |
| 🟡 Media | Migrazione storico 1.942 file da `\\sqlprd0303\B1_SHR\VICECONTI\Attachments\` con stesso script in modalità batch | SAP SERVICE LAYER |
| 🟢 Strategica | Decisione infrastruttura SAP (alternative 1/2/3/4) e identificazione partner sistemistico | AUDIT INFRASTRUTTURA |
| 🟢 Strategica | Test di navigazione autonoma di Claude su contesto-ai come dispositivo ipertestuale di Livello 1 | CONTESTO AI |
| 🟢 Strategica | Pipeline OCR/AI estrazione dati da PDF moduli tecnici compilati a mano (Apple Pencil → Claude multimodale → SAP). Cantiere mesi, ma tracciato come orizzonte | SAP SERVICE LAYER |
| 🔵 Bassa | Esplorazione Slack | Trasversale |
| 🔵 Bassa | n8n MCP server | n8n |
| 🔵 Bassa | n8n — problema AudioPen → Telegram → Drive da diagnosticare | n8n |
| 🔵 Bassa | Eventuale quaderno GitHub formativo | Da definire |
| 🔵 Bassa | Trascrizioni Claude Code → archivio GitHub `contesto-ai/trascrizioni-code/`. Pubblicazione automatizzata via Cowork. Pattern stabile | CONTESTO AI |

### Open items chiusi rispetto al 05/05/2026

- ✅ **Directory Analyzer TS Digital** — riattivato settimana 04-08/05/2026. Pipeline registrazione fatture passive ora completa per costruzione (era 🟡 Media)
- ✅ **Scelta architetturale composizione FastAPI/portale** — decisione presa 10/05: convergenza dentro FastAPI come secondo consumer (era 🔴 Alta)
- ✅ **Cantiere PDF semiautomatico con Attachments+Hub** — concluso 10/05 con `archivio_documenti_sap.py` v2 in produzione (era nuovo open item della giornata)

---

## 11. CONSULENTI E PARTNER

| Nome | Ruolo | Stato |
|------|-------|-------|
| Vincenzo Strazzullo (4utime.it) | SAP B1 consulting, Service Layer | Email inviata 11/04/2026, **caso d'uso ora maturato (10/05)** |
| Antonio Forlani (3W Sistemi) | Infrastruttura server | Operativo |
| Z3 Engineering (Var One) | Licenze SAP, S-user | Da contattare |

---

## STORIA REVISIONI

| Versione | Data | Note |
|----------|------|------|
| 1.0 | 03/05/2026 mattina | Prima riscrittura sostanziale (cornice strategica, deploy prima pietra, 6 principi nuovi) |
| 1.1 | 03/05/2026 mattina | Riorganizzazione mappa ecosistema in progressione 100-400s |
| 1.2 | 03/05/2026 mezzogiorno | Riconoscimento Viceconti Hub portale come dispositivo Livello 1 emerso retrospettivamente |
| 1.3 | 05/05/2026 sera | SAP come system of record operativo, evoluzione L1 a canale di interazione, pattern unificato interno/B2B, 5 principi nuovi |
| **1.4** | **10/05/2026 sera** | **Giornata densa di consolidamento operativo + 11 principi nuovi.** Cantiere PDF concluso (`archivio_documenti_sap.py` v2 in produzione, multi-tipo + archivio storico flat, deposito SAP riconfigurato in Dropbox, struttura Hub a 3 cartelle per famiglia consumer, UDF `RifFile` operativo). Directory Analyzer TS Digital riattivato (settimana 04-08/05). Decisione presa sulla composizione FastAPI/portale (convergenza). Pipeline link in descrizione attività SAP → Calendar/Hub/SAP validata end-to-end. Correzione path Dropbox in sezione 9 (`C:\Users\PC\Dropbox`, non `C:\Dropbox`). Nuovi candidati Manuale: trascrizioni Code per cap. 1, sezione "Trap SAP B1" per cap. 3. Open items aggiornati: chiusi 3 (DA, FastAPI/portale, cantiere PDF), riformulati altri, aggiunto cantiere pulizia FATTURE DA FORNITORE come 🔴 Alta. |

---

*Aggiornato al 10 maggio 2026 sera. Sostituisce versione del 5 maggio 2026.*
*Prossimo aggiornamento previsto: dopo cantiere FastAPI `/attivita` o dopo decisione su Fattura da fornitore / Attività.*
