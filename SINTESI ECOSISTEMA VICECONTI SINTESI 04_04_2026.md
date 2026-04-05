# SINTESI ECOSISTEMA VICECONTI
## Documento di visione e stato operativo

*Data: 4 aprile 2026*
*Sostituisce: Sintesi Ecosistema Viceconti 08_03_2026*

---

## 0. PREMESSA STRATEGICA — Aggiornamento aprile 2026

La forma societaria definitiva è la **s.r.l. unipersonale di Prospero**. La Viceconti s.n.c. avvierà la liquidazione volontaria. La scadenza critica non è settembre ma **luglio 2026**: tutte le decisioni formali (statuto, accordo col socio, costituzione) devono essere chiuse entro fine luglio per consentire un avvio operativo a settembre.

Questo cambia la lettura dell'intero ecosistema: ciò che viene costruito da qui ad agosto non è un upgrade dell'esistente, ma l'infrastruttura operativa della nuova società. La continuità è garantita (stessa sede, stesso know-how, stessa architettura IT), ma il soggetto giuridico e strategico è nuovo.

**Tre aree di business della nuova s.r.l.:**

| Area | Ruolo nel 2026 | Stato |
|------|----------------|-------|
| Assistenza Tecnica | Pilastro di cassa, primo contributore di margine | Operativa |
| Vendita locale | Seconda area per contribuzione a breve | Operativa |
| E-commerce HoReCa | Core business in prospettiva, vantaggio difendibile sui segmenti tecnici | Da lanciare a settembre |

**Milestone tecnica raggiunta:** Service Layer SAP attivo dal 29 marzo 2026. 111 fatture registrate automaticamente in produzione, 27 fornitori, zero errori. Il collo di bottiglia critico dell'architettura è sbloccato.

---

## 1. CHI È VICECONTI S.N.C.

**Viceconti s.n.c.** — commercio e assistenza tecnica di attrezzature professionali per la ristorazione e l'hotellerie, con sede operativa a Lauria (Basilicata).

**Contesto strategico:** la s.n.c. è in fase di transizione verso una s.r.l. unipersonale di Prospero. Orizzonte: liquidazione volontaria della s.n.c. entro luglio 2026, avvio operativo della nuova società a settembre 2026.

**Dipendenti:** 2 tecnici (Gianni Limongi, Biagio Rizzo).

**Timeline operativa:** aprile–luglio 2026 = chiusura decisioni formali + sviluppo IT. Settembre 2026 = avvio operativo nuova società.

---

## 2. VISIONE E OBIETTIVO

### La visione

Costruire un ecosistema IT che renda la nuova società completamente autonoma nella gestione dei propri dati e processi, senza dipendenza da consulenti IT esterni. L'AI è lo strumento che rende questo possibile per una microimpresa.

### I due assi operativi

| Asse | Componenti chiave | Obiettivo | Stato |
|------|-------------------|-----------|-------|
| **Assistenza Tecnica** (priorità) | SAP → Query Engine → Service Layer → Viceconti Hub → Hub Documentale → Telegram | Tecnici aggiornano attività SAP da smartphone; dati e documenti consultabili da web | Portali operativi; Service Layer da consolidare |
| **E-Commerce** (standby consapevole) | Pipeline PrestaShop → Database Centralizzato → PrestaShop | Automazione importazione prodotti con contenuti SEO generati da AI | Pilota completato, ripresa a settembre |

### I 3 requisiti per un'architettura funzionante

1. **I dati hanno una casa unica** → Database Centralizzato (no file Excel/JSON "volanti")
2. **Il flusso è tracciabile** → n8n + log strutturati
3. **Posso agire senza toccare codice** → Asana + n8n

---

## 3. MAPPA DELL'ECOSISTEMA

### A. INFRASTRUTTURA IT E ARCHITETTURA

| # | Progetto | Stato | Aggiornamento |
|---|----------|-------|---------------|
| A1 | Sintesi Ecosistema | Attivo | Questo documento |
| A2 | Diagramma Architettura | ~~Progetto separato~~ → **Mantenuto in questo progetto** | Confluito in Sintesi Ecosistema (marzo 2026) |
| A3 | Audit Infrastruttura | Attivo — presidio continuativo | Da audit statico a monitoraggio proattivo |
| A4 | SAP Service Layer | **Operativo** ✅ | 111 fatture registrate in produzione (29/03); da consolidare |
| A5 | SAP Academy | Attivo | Documentazione tecnica in crescita |
| A6 | n8n | **In avvio operativo** | Installazione prevista sabato 5 aprile sul PC Lauria |

### B. PORTALI WEB E FRONTEND

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| B1 | Viceconti Hub + Hub Documentale | Operativo | Link bidirezionale Hub ↔ Hub Doc: priorità Q2 |
| B2 | Telegram Bot | Operativo | Notifiche chiamate di servizio |

### C. E-COMMERCE E PRODOTTI

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| C1 | Pipeline PrestaShop | Standby consapevole | Ripresa settembre 2026 |
| C2 | Database Centralizzato | Operativo/non integrato | SQLite, 9.785 prodotti Morini |
| C3 | Asset Prodotti | In analisi | — |
| C4 | E-commerce Vini | Esplorazione | Integrazione futura Horeca Lab |

### D. STRATEGIA E GOVERNANCE

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| D1 | Nuova Strategia Settembre 2026 | **Fase operativa** | Forma societaria definitiva (s.r.l. unipersonale); scadenza critica luglio 2026 |
| D2 | Naming Viceconti | Completato | 10 regole, standard operativo adottato |

### E. AI, AUTOMAZIONE E FORMAZIONE

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| E1 | AI Istruzioni per l'Uso | Attivo | Manuale operativo Claude/Gemini/ChatGPT |
| E2 | AI Automation Nuove Idee | Esplorativo | — |
| E3 | AI Human Lab | Attivo — consolidamento teorico | ~90 giorni; framework concettuale stabilizzato |
| E4 | Assistenti Intelligenti | Progettazione | Prerequisiti: Service Layer ✅, n8n (in avvio) |
| E5 | Formazione IT | Attivo | — |
| E6 | Strumenti Cattura Vocale | **In espansione attiva** | AudioPen + microfono nativo Claude + Jamie Live; PLAUD in valutazione |
| E7 | Memoria e Contesto AI | **Nuovo progetto** | GitHub Pages come soluzione transitoria; API Memory Tool come obiettivo strutturale |

### F. DOCUMENTAZIONE OPERATIVA

| # | Progetto | Stato | Note |
|---|----------|-------|------|
| F1 | Diario di Bordo | Attivo | — |
| F2 | Fa Tu Cantiere (Pilota) | Da avviare | — |

---

## 4. ARCHITETTURA IT

### 4.0 Infrastructure Layer — fondamenta fisiche ⚠️

Prerequisito strategico esplicito: **nessun obiettivo di settembre è raggiungibile su fondamenta instabili.**

| Server | Ruolo | RAM | Uptime | Stato |
|--------|-------|-----|--------|-------|
| SQLPRD0303 | DB SAP + Service Layer (temporaneo) | 7,6/12 GB (63%) | ~24h al 04/04 | Riavviato 03/04 da 3W Sistemi |
| BPRD0303 | Terminal server SAP | Da monitorare | Da rilevare | Riavviato 14/03 |

**Evento di riferimento:** 13 marzo 2026 — entrambi i server inutilizzabili (uptime 86 e 1.073 giorni, patch mai applicate). Causa radice: Service Layer sovradimensionato (10 nodi) installato su SQLPRD0303 in competizione con SQL Server sulla RAM.

**Interlocutori:** 3W Sistemi / Antonio Forlani = contatto operativo diretto. Var Group = provider fisico, risponde tramite 3W per questioni SAP.

**Azioni aperte entro agosto 2026:**
- Finestra manutenzione Windows su SQLPRD0303 (KB5073723, KB5066738, driver Broadcom) con 3W Sistemi
- Piano manutenzione periodica: riavvii bimestrali, alert RAM >85%
- Spostamento Service Layer da SQLPRD0303 a BPRD0303 (intervento strutturale, medium term)
- Valutazione terzo server DMZ per esposizione sicura verso internet

### 4.1 I layer dell'architettura applicativa

```
INFRASTRUCTURE LAYER → SQLPRD0303, BPRD0303, Var Group / 3W Sistemi
        ↓
COMMAND LAYER        → Asana, Task Scheduler (server SAP), browser/smartphone
        ↓
ORCHESTRATOR LAYER   → n8n (PC Lauria — installazione 05/04/2026)
        ↓
ENGINE LAYER         → SAP Query Engine v2.5, Service Layer SAP ✅,
                       indexer.py, Script Telegram, Pipeline PrestaShop (standby)
        ↓
PERSISTENCE LAYER    → SAP B1 (SQL Server), DB Centralizzato (SQLite),
                       GitHub Pages, Dropbox
        ↓
PORTALI WEB          → Viceconti Hub, Hub Documentale, PrestaShop, Telegram
```

### 4.2 Componenti operativi — Stato al 4 aprile 2026

| # | Componente | Tecnologia | Stato | Note |
|---|-----------|------------|-------|------|
| 1 | SAP Business One 10.0 | SAP B1 10.0, SQL Server | Operativo | Lettura + scrittura via Service Layer |
| 2 | Service Layer SAP | Apache Commons Daemon, 3 nodi | **Operativo — da consolidare** | Sbloccato 13/03; 111 fatture in produzione 29/03; /Drafts non risolto |
| 3 | SAP Query Engine v2.5 | PowerShell | Operativo, da consolidare | 4 query attive, ogni ora 8-20 lun-sab |
| 4 | Viceconti Hub | HTML statico, GitHub Pages | Operativo | 4 report, motore data-driven |
| 5 | Hub Documentale | HTML statico, GitHub Pages | Operativo (v2.0/v2.1) | 3.068 file, 257 clienti |
| 6 | Script Telegram | Batch Windows | Operativo, da consolidare | Bot notifiche gruppo tecnici |
| 7 | indexer.py | Python, Dropbox API | Operativo, da consolidare | Indicizzazione Hub Documentale |
| 8 | Pipeline PrestaShop | Python, Claude API | Standby | Ripresa settembre 2026 |
| 9 | DB Centralizzato | SQLite | Operativo/non integrato | 9.785 prodotti Morini |
| 10 | n8n | Node.js (PC Lauria) | **In installazione** | Primo workflow: AudioPen → Telegram + salvataggio locale |
| 11 | registra_fattura.py | Python | **Nuovo — operativo** | Script automazione fatture acquisto servizi; indice 80 fornitori |

### 4.3 Prerequisito strategico: stabilità infrastrutturale

*(Sezione aggiunta a seguito dell'evento 14 marzo 2026)*

Qualsiasi obiettivo di sviluppo è condizionato dalla stabilità del layer server. L'infrastruttura fragile azzera il valore di tutto ciò che ci sta sopra. La gestione infrastrutturale si è trasformata da reattiva a proattiva: Audit Infrastruttura è ora un progetto di presidio continuativo, non un audit una-tantum.

---

## 5. SVILUPPI RECENTI SIGNIFICATIVI

### Service Layer: da collo di bottiglia a componente operativo

Il Service Layer SAP — bloccato dalla fondazione dell'ecosistema — è ora operativo in produzione. Il primo use case concreto è l'automazione della registrazione di fatture acquisto di servizi: `registra_fattura.py` legge XML da TS Digital, li processa tramite Service Layer e registra direttamente su SAP B1. 111 fatture, 27 fornitori, zero errori al primo lancio in produzione.

**Aperto:** comportamento anomalo di `/Drafts` (registra invece di parcheggiare) da chiarire con Vincenzo Strazzullo. Non blocca il flusso operativo corrente.

### Strumenti di cattura vocale: mappa a tre strumenti

| Strumento | Funzione | Quando |
|-----------|----------|--------|
| AudioPen Prime | Cattura asincrona, rielaborazione, archiviazione | Macchina, cantiere, output strutturato |
| Microfono nativo Claude | Input vocale diretto in chat | Interazione conversazionale fluida |
| Jamie Live | Trascrizione continua con pause naturali | Parlato lungo o con ritmo naturale |

PLAUD NotePin S in valutazione per use case con speaker diarization (sopralluoghi, conversazioni multi-voce). Flusso `AudioPen → n8n → Telegram + cartella locale` validato architetturalmente, da implementare con n8n.

### Memoria e Contesto AI: nuovo progetto

Problema identificato: la discontinuità di contesto tra sessioni Claude è il principale punto di attrito operativo. Soluzione a breve termine in implementazione: file di contesto aggregato su GitHub Pages, fetchato automaticamente all'apertura via prompt di sistema. Soluzione strutturale (API Memory Tool) condizionata ai prerequisiti: Service Layer ✅, n8n (in avvio). Sequenza corretta: GitHub Pages adesso → API Memory Tool quando l'infrastruttura regge.

### n8n: da pianificato a in installazione

Ruolo chiarito con precisione: orchestratore event-driven sul PC Lauria (non sul server SAP). Task Scheduler resta sul server SAP per i cron di produzione — coesistenza pulita. Installazione via npm globale prevista sabato 5 aprile. Primo workflow: AudioPen → webhook → n8n → ramo Telegram + ramo salvataggio locale.

---

## 6. PRIORITÀ OPERATIVE

### Blocchi critici aperti

| # | Blocco | Impatto | Azione |
|---|--------|---------|--------|
| 1 | Aggiornamenti Windows SQLPRD0303 | Rischio instabilità server | Finestra pianificata con 3W Sistemi |
| 2 | Accordo col socio (s.n.c.) | Blocca costituzione nuova s.r.l. | Percorso legale in corso con Italo |
| 3 | DSCR da calcolare | Condiziona strumenti di gestione del debito | Con Antonella |

### Priorità tecniche Q2 2026

| # | Azione | Impatto |
|---|--------|---------|
| 1 | Installazione e primo workflow n8n (5 aprile) | Avvio orchestrazione event-driven |
| 2 | Implementazione GitHub Pages per contesto AI | Riduce attrito discontinuità sessioni |
| 3 | Finestra manutenzione Windows SQLPRD0303 | Stabilità infrastrutturale |
| 4 | Consolidamento Service Layer (exit code, log, /Drafts) | Prerequisito per uso a regime |
| 5 | Link bidirezionale Viceconti Hub ↔ Hub Documentale | Valore immediato per tecnici |
| 6 | Token GitHub: rigenerare (scade 9 maggio 2026) | Sicurezza |

### Priorità strategiche (verso luglio 2026)

| # | Decisione / Azione | Scadenza |
|---|-------------------|----------|
| 1 | Definire statuto s.r.l. con avvocato (Italo): clausole governance, buyback, diritti socio minoritario | Aprile–maggio |
| 2 | Formalizzare accordo con il socio o avviare procedura legale | Giugno |
| 3 | Costituzione nuova s.r.l. + cessazione operativa s.n.c. | Luglio |
| 4 | Risorsa umana: risolvere gap part-time/full-time o ricerca ufficio di collocamento | Aprile–maggio |

---

## 7. AI HUMAN LAB — nota separata

Il Lab è un progetto autonomo di ricerca sull'impatto AI, non strumento dell'ecosistema operativo. Stato: ~90 giorni, framework concettuale stabilizzato. Punti fermi: ipotesi del reagente, rimozione del filtro della vergogna cognitiva, tesi sull'identità fenomenologica dell'interlocuzione. Percorso pianificato: Bonolis → Cacciari (coscienza nostalgica come concetto da abitare prima di renderlo operativo).

Il Lab registra le trasformazioni dell'ecosistema come materiale empirico — non le guida.

---

*Questo documento sintetizza lo stato dell'ecosistema Viceconti al 4 aprile 2026.*
*Sostituisce e aggiorna la Sintesi dell'8 marzo 2026.*
*Fonti: riepiloghi SAP SERVICE LAYER, AUDIT INFRASTRUTTURA, n8n, STRUMENTI DI CATTURA VOCALE, MEMORIA E CONTESTO AI, NUOVA STRATEGIA SETTEMBRE 2026, AI HUMAN LAB — tutti aggiornati a marzo–aprile 2026.*
