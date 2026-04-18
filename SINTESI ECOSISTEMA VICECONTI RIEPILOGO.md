# SINTESI ECOSISTEMA VICECONTI
## Documento di visione e stato operativo

*Aggiornato al 18 aprile 2026 — sostituisce versione dell'8 marzo 2026*

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

---

## 2. STATO OPERATIVO — Aprile 2026

### Componenti in produzione

| Componente | Stato | Note |
|-----------|-------|------|
| **SAP Business One 10.0 + Service Layer** | ✅ Operativo | Attivo dal 29/03/2026. 111 fatture registrate automaticamente. Server SQLPRD0303 stabilizzato |
| **SAP Query Engine v2.5** | ✅ Operativo | Estrazioni orarie, JSON su GitHub Pages via API |
| **n8n v2.14.2** | ✅ Operativo | PC Lauria, ngrok dominio statico, Task Scheduler auto-start. Workflow: AudioPen → Telegram NOTE PERSONALI + Google Drive |
| **Viceconti Hub + Hub Documentale** | ✅ Operativi | GitHub Pages. 3.068 file, 257 clienti |
| **Contesto AI su GitHub Pages** | ✅ Operativo | repo contesto-ai, file .md a nome fisso, fetch validato su Claude/ChatGPT/Gemini |
| **`RIAPRI E MODIFICA MODULO TECNICO.html`** | ✅ Beta produzione | v4: auto-idratazione, modulo vuoto, campi header editabili, sovrascrittura stesso nome file |
| **`crea_moduli_vuoti.py`** | ✅ Operativo | Legge JSON da GitHub, genera HTML pre-compilati, non sovrascrive file esistenti. 94 moduli in Dropbox |
| **`crea_offerta.py`** | ✅ Operativo | Versione batch: file multipli, --cartella, --dry-run, sposta in PROCESSATI. 29 offerte create (260080-260108) |
| **`registra_fattura.py`** | ✅ Operativo (incompleto) | 111 fatture registrate, da completare |
| **ASSISTENTE AMMINISTRATIVA** | ✅ Avviata | Progetto Claude operativo da 13 aprile 2026. Prima settimana completata |
| **Apple Reminders** | ✅ Componente adottata | Layer di task management tra cattura grezza e sistemi di destinazione |

### Workflow modulo tecnico — flusso completo in produzione beta

```
SAP Query Engine (server, ogni ora)
→ Attivita_Assistenza.json → push GitHub

crea_moduli_vuoti.py (PC Lauria, da schedulare)
→ legge JSON da GitHub
→ genera MODULO_*.html per ogni attività aperta
→ salva in Dropbox\MODULI ATTIVITA' APERTE\
→ non sovrascrive mai file esistenti

Tecnico apre RIAPRI E MODIFICA MODULO TECNICO.html
→ trascina il MODULO_*.html corrispondente
→ compila Descrizione Lavori + ricambi
→ salva → stesso nome file (sovrascrittura diretta)

Prospero lancia crea_offerta.py --cartella .
→ crea Offerta di Vendita SAP
→ sposta file in MODULI_PROCESSATI

Prospero chiude attività in SAP
→ al prossimo ciclo il modulo non viene rigenerato
```

---

## 3. PRINCIPI ARCHITETTURALI CONSOLIDATI

| Principio | Descrizione |
|-----------|-------------|
| **HTML come trinità** | Interfaccia + documento + dato strutturato in un file solo |
| **Attività SAP come trigger documentale universale** | Qualsiasi tipo di attività SAP può generare un HTML pre-compilato che uno script trasforma in qualsiasi documento SAP. Assistenza → offerta/DDT è il primo caso. Offerte, ordini, consegne replicano lo stesso pattern |
| **Puzzle (non architettura sequenziale)** | I pezzi dell'automazione possono essere completati in qualsiasi ordine senza dipendenze sequenziali obbligatorie |
| **Digitizzazione ≠ automazione** | Digitizzazione rende le cose digitali. Automazione le rende auto-eseguibili. Confonderle porta a aspettative irrealistiche |
| **Ogni canale di input ha una destinazione finale** | Telegram non è mai la destinazione. Il flusso deve uscire verso il sistema giusto prima che l'informazione diventi difficile da recuperare |
| **Separazione input/struttura** | Cattura e strutturazione sono atti cognitivi distinti. Il modello a 2 step li separa deliberatamente: prima cattura libera, poi strutturazione per destinazione |
| **Le automazioni abilitano il disegno esistente** | Il disegno operativo era già buono — sedimentato in anni di pratica. Il problema era la chiusura di SAP. Le automazioni non inventano nuovi processi, sblocano quelli esistenti |
| **Paradosso di Jevons organizzativo** | Migliorare l'efficienza di un sistema tende ad aumentare la domanda su quel sistema. L'esecuzione dei task è il capitolo che viene dopo l'organizzazione ed è quello più grande |

---

## 4. MAPPA DELL'ECOSISTEMA

### A. INFRASTRUTTURA IT E ARCHITETTURA

| Progetto | Stato | Note |
|----------|-------|------|
| Sintesi Ecosistema | Attivo | Questo documento |
| SAP Service Layer | ✅ Operativo | Attivo da 29/03/2026 |
| SAP Academy | Attivo | Documentazione tecnica SAP |
| n8n | ✅ Operativo | Workflow AudioPen → Telegram + Drive |
| Audit Infrastruttura | Completato | — |

### B. PORTALI WEB E FRONTEND

| Progetto | Stato | Note |
|----------|-------|------|
| Viceconti Hub + Hub Documentale | ✅ Operativi | GitHub Pages |
| Contesto AI GitHub Pages | ✅ Operativo | repo contesto-ai |
| Telegram | ✅ Operativo | Notifiche + canale note personali |

### C. WORKFLOW ASSISTENZA TECNICA

| Componente | Stato | Note |
|-----------|-------|------|
| RIAPRI E MODIFICA MODULO TECNICO.html | ✅ Beta | v4 in produzione da 13/04 |
| crea_moduli_vuoti.py | ✅ Operativo | Da schedulare con Task Scheduler |
| crea_offerta.py | ✅ Operativo | Versione batch |
| aggiorna_attivita.py | ✅ Disponibile | Scrive U_Esito su Attività SAP |
| registra_fattura.py | 🟡 Incompleto | 111 fatture OK, da completare |

### D. ASSISTENTI INTELLIGENTI

| Assistente | Stato | Note |
|-----------|-------|------|
| Architettura IT (questo progetto) | ✅ Operativo | Modello di riferimento |
| ASSISTENTE AMMINISTRATIVA | ✅ Avviata | Prima settimana completata 13-18/04 |
| Altri assistenti per aree | 🔴 Da avviare | Lunedì 13/04 avvio pianificato |

### E. GESTIONE INPUT E TASK

| Componente | Stato | Note |
|-----------|-------|------|
| AudioPen | ✅ Operativo | Voice-to-text, formato tag + struttura |
| Apple Reminders | ✅ Adottato | Layer task management. Struttura liste consolidata |
| Gmail MCP | ✅ Connesso | Solo bozze (no invio diretto) |
| Calendar MCP | ✅ Connesso | 18 calendari mappati |
| Apple Notes MCP | ✅ Disponibile | Solo note tastiera (Pencil → immagine PNG) |

### F. STRATEGIA E GOVERNANCE

| Progetto | Stato | Note |
|----------|-------|------|
| Nuova Strategia Settembre 2026 | Attivo | S.r.l. unipersonale, luglio liquidazione SNC |
| Manuale Automazioni | 🔴 Da avviare | Struttura identificata, materiale raccolto (viaggio 18/04) |
| Naming Viceconti | Completato | — |

### G. E-COMMERCE

| Progetto | Stato | Note |
|----------|-------|------|
| Pipeline PrestaShop | Standby consapevole | Da riprendere H2 2026 |
| Database Centralizzato | Operativo/non integrato | 9.785 prodotti |

---

## 5. APPLE REMINDERS — Struttura liste (18 aprile 2026)

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

## 6. MANUALE AUTOMAZIONI — Struttura identificata

**Cap. 1 — Aggiornamento Sistema Informativo**
- 1.1 Aggiornamento Documentale
  - 1.1.1 Ordini d'Acquisto (🔴 non iniziato)
  - 1.1.2 Entrata Merci — DDT fornitori (🔴 non iniziato)
  - 1.1.3 Fatture Acquisti (🟡 impostato — `registra_fattura.py`)

Materiale base raccolto nel viaggio del 18 aprile (questionario completo). Struttura da sviluppare sabato 19 aprile.

**Fornitori con documenti transazionali strutturati (Gmail):**

| Fornitore | Mittente | Tipo |
|-----------|---------|------|
| Fimar | noreply@fimargroup.it | Conferme ordine + DDT |
| Sirman | noreply@sirman.com | Solo DDT |
| Morini | noreply@morinionline.it | Fatture + DDT |
| Klimaitalia | no-reply@klimaitalia.it | Ordini + Conferme |
| Sambonet Paderno | noreply@sambonet.it | Fatture + DDT + Distinte |

---

## 7. ARCHITETTURA IT

### Layer

```
COMMAND LAYER        → Claude (chat + progetti), Reminders, Asana, trigger manuali
        ↓
ORCHESTRATOR LAYER   → n8n (PC Lauria + Mac da configurare)
        ↓
ENGINE LAYER         → SAP Query Engine v2.5, crea_moduli_vuoti.py,
                       crea_offerta.py, registra_fattura.py, aggiorna_attivita.py
        ↓
PERSISTENCE LAYER    → SAP B1, GitHub Pages, Dropbox, Apple Reminders/Calendar
        ↓
INTERFACCE           → Viceconti Hub, Hub Documentale, MODULO TECNICO HTML,
                       Telegram, PrestaShop
```

### Ambiente tecnico

| Risorsa | Dettaglio |
|---------|-----------|
| ERP | SAP Business One 10.0, SQL Server, Service Layer operativo |
| Server DB | SQLPRD0303 (192.168.122.99) |
| PC sviluppo | Win11 sede Lauria — script, n8n, Task Scheduler |
| MacBook Air | Mobile — Claude Desktop, Apple Reminders/Notes MCP |
| Script path | C:\Viceconti\viceconti-hub |
| Dropbox moduli | C:\Users\PC\Dropbox\HUB DOCUMENTALE\FILE TEMPORANEI\MODULI ATTIVITA' APERTE |
| GitHub org | viceconti-hub (repos: portale, hub-documentale, contesto-ai) |

### Credenziali e scadenze

| Credenziale | Scadenza | Priorità |
|-------------|----------|---------|
| **Token GitHub** | **9 maggio 2026** | **🔴 Urgente — rigenerare** |
| Token Dropbox OAuth2 | Nessuna | — |

---

## 8. OPEN ITEMS — Priorità aggiornate al 18 aprile 2026

| Priorità | Attività | Progetto |
|----------|---------|---------|
| 🔴 Alta | Token GitHub — rigenerare (scade 9 maggio) | SITI WEB |
| 🔴 Alta | Task Scheduler per `crea_moduli_vuoti.py` sul PC Lauria | SAP SERVICE LAYER |
| 🔴 Alta | Manuale Automazioni — Cap. 1 struttura completa | Nuovo |
| 🔴 Alta | Completamento `registra_fattura.py` | SAP SERVICE LAYER |
| 🟡 Media | n8n su Mac (per nodi Apple nativi: Reminders, Calendar) | n8n |
| 🟡 Media | Allineamento Reminders ↔ SAP Activities via n8n | n8n |
| 🟡 Media | Smistamento AudioPen per tag via n8n | n8n |
| 🟡 Media | Caricamento ASSISTENTE AMMINISTRATIVA RIEPILOGO.md su GitHub | ASSISTENTI |
| 🟡 Media | Fix URL fetch Sintesi su GitHub (filename: SINTESI non RIEPILOGO) | SITI WEB |
| 🟡 Media | Pulizia attività storiche SAP (< 7000) | SAP |
| 🟡 Media | Google Calendar → n8n → Hub Documentale + esito SAP | GOOGLE CALENDAR |
| 🔵 Bassa | n8n — problema AudioPen → Telegram → Drive da diagnosticare | n8n |
| 🔵 Bassa | Email Vincenzo Strazzullo — risposta in attesa (tool PDF, B1iF, XML, esposizione Service Layer) | SAP SERVICE LAYER |

---

## 9. CONSULENTI E PARTNER

| Nome | Ruolo | Stato |
|------|-------|-------|
| Vincenzo Strazzullo (4utime.it) | SAP B1 consulting, Service Layer | Email inviata 11/04/2026, risposta in attesa |
| Antonio Forlani (3W Sistemi) | Infrastruttura server | Operativo |
| Z3 Engineering (Var One) | Licenze SAP, S-user | Da contattare |

---

*Aggiornato al 18 aprile 2026. Sostituisce versione dell'8 marzo 2026.*
*Prossimo aggiornamento previsto: fine weekend 19-20 aprile 2026 dopo analisi Manuale Automazioni.*
