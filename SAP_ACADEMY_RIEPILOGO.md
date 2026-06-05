# SAP Academy — Riepilogo del 24 maggio 2026

## Stato attuale

Il progetto SAP Academy ha fatto un salto qualitativo significativo in questa sessione: da formazione episodica a metodologia sistematica. Sono stati scoperti e implementati meccanismi nativi SAP B1 di grande potenza (Formatted Search), è stata identificata una capability strategica non nota (Webhooks Service Layer), e si è consolidata la metodologia di esplorazione della documentazione ufficiale SAP come fonte primaria prima di sviluppare soluzioni custom.

---

## Lavoro svolto in questa sessione

### Mattina — Esplorazione documentazione SAP e scoperta Webhooks

- **Verifica accesso SAP Help Portal**: confermato che né `web_fetch` né `curl` possono estrarre contenuto (SPA Vue.js, contenuto JS-rendered). Il portale risponde 200 OK ma restituisce solo lo shell HTML vuoto.
- **Metodologia PDF stabilita**: il pulsante "Documento completo" del SAP Help Portal scarica il PDF ufficiale (Antenna House, typesetting professionale, inglese, stabile). Il "Crea PDF personalizzato" usa HeadlessChrome e produce solo la pagina corrente in italiano (traduzione automatica). Il vettore corretto per la knowledge base è il Documento completo.
- **Scoperta Webhooks SAP B1 Service Layer**: dal sommario del PDF *Working with SAP Business One Service Layer* (v1.28, 224 pagine, Cap. 7 pagine 176–198) è emerso che SAP B1 espone un sistema di webhook nativo non ancora utilizzato nell'ecosistema Viceconti. Pattern push invece del polling attuale. Prima applicazione identificata: trigger automatico per `archivio_documenti_sap.py` invece dell'avvio manuale.
- **Messaggio di aggiornamento per Sintesi** prodotto e consegnato a Prospero per condivisione con il progetto 401.

### Mattina/pomeriggio — Formatted Search: scoperta e implementazione

**Contesto di partenza**: analisi del pacchetto ARI (Creare documenti elettronici) nella schermata Servizio di integrazione, conclusosi con valutazione di non coerenza con l'architettura API First (B1iF legacy, funzionalità già coperta da Directory Analyzer).

**Implementazioni completate:**

1. **`lk_subject_OCLG`** — Copia `OSCL.Subject` (Tema chiamata di servizio) → `OCLG.Notes` (Osservazioni attività)
   - Join: `OSCL.CallID = OCLG.parentId` dove `OCLG.parentType = '191'`
   - Validato su dati reali

2. **`lk_description_OCLG`** — Copia `OSCL.Descrption` (Osservazioni chiamata) → campo Contenuto attività
   - Stessa struttura di join
   - Visibile in screenshot con funzionamento confermato

3. **`lk_comments_OCLG`** — Query unificata a 4 rami UNION ALL che popola Osservazioni attività dalla sorgente corretta in base all'origine del documento:
   ```sql
   -- Ramo 1: da Chiamata di servizio (parentId > 0)
   SELECT T0.[Subject] FROM OSCL T0
   WHERE T0.[CallID] = $[OCLG.parentId.int] AND $[OCLG.parentId.int] > 0
   UNION ALL
   -- Ramo 2: da Ordine cliente (DocType = 17)
   SELECT T1.[Comments] FROM ORDR T1
   WHERE T1.[DocEntry] = $[OCLG.DocEntry.number] AND $[OCLG.DocType.number] = 17
   UNION ALL
   -- Ramo 3: da Consegna (DocType = 15)
   SELECT T2.[Comments] FROM ODLN T2
   WHERE T2.[DocEntry] = $[OCLG.DocEntry.number] AND $[OCLG.DocType.number] = 15
   UNION ALL
   -- Ramo 4: da Fattura di vendita (DocType = 13)
   SELECT T3.[Comments] FROM OINV T3
   WHERE T3.[DocEntry] = $[OCLG.DocEntry.number] AND $[OCLG.DocType.number] = 13
   ```
   - Testato su attività create da fattura di vendita: funziona
   - Nota operativa: auto-refresh disabilitato per evitare sovrascrittura su riapertura di attività esistenti; trigger manuale via 🔍 per popolamento one-shot

4. **Fix query CardCode automatico fornitori** (`lk_coge_OPCH` area CardCode OCRD):
   - Bug identificato: `MAX(SUBSTRING(CardCode, 2, 6))` faceva confronto lessicografico su stringhe; vecchi codici corti non zero-padded (es. `F890`) battevano codici lunghi corretti (es. `F051583`) perché `'8' > '0'`
   - Secondo bug: padding a 6 cifre errato per fornitori che usano 7 cifre
   - Fix implementato e validato:
   ```sql
   SELECT 'F' + RIGHT('0000000' + LTRIM(STR(
       MAX(CAST(SUBSTRING(CardCode, 2, LEN(CardCode)-1) AS INT)) + 1
   )), 7)
   FROM OCRD WHERE CardType = 'S'
   AND ISNUMERIC(SUBSTRING(CardCode, 2, LEN(CardCode)-1)) = 1
   ```

5. **`lk_taxdate_OPCH` / `lk_taxdate_OINV`** — Query per allineare Data allocazione IVA a Data documento:
   ```sql
   SELECT $[OPCH.DocDate.date]   -- per fatture acquisto
   SELECT $[OINV.DocDate.date]   -- per fatture vendita
   ```
   - Verificato in Parametrizzazione documento che non esiste opzione nativa per questa sincronizzazione
   - Configurazione in Parametrizzazione modulo: in corso (da completare e testare)

---

## Decisioni prese

- **Formatted Search: trigger manuale preferito all'auto-refresh** per i campi Osservazioni attività, per evitare sovrascrittura del contenuto su riapertura di attività già compilate manualmente.

- **Webhooks: cantiere non aperto questo weekend** — la scoperta è reale e rilevante ma non urgente. `archivio_documenti_sap.py` funziona già manualmente. Il cantiere webhook è pianificato come estensione naturale del progetto n8n (350), da avviare con calma.

- **Principio "funzionalità native prima" confermato e metodologizzato**: la Formatted Search è un secondo esempio concreto (dopo la checkbox PDF del 14/05) di capability nativa SAP che era sempre esistita ma non era nota. La metodologia L1/L2/L3 (Mappa capability → Schede operative → Wiki) è il sistema per ridurre strutturalmente il rischio di sviluppare custom ciò che esiste già.

- **Formatted Search nella cassetta degli attrezzi**: non si va a cercare applicazioni ora. Si usa quando il lavoro operativo fa emergere un bisogno compatibile.

---

## Prossimi passi

1. **Completare configurazione `lk_taxdate_OPCH` e `lk_taxdate_OINV`** in Parametrizzazione modulo su Fattura da fornitore e Fattura di vendita — testare che `$[OPCH.DocDate.date]` funzioni come query standalone nel motore FS
2. **Aggiornare ramo `'L'`** nella query CardCode automatico con la stessa logica CAST/ISNUMERIC per coerenza
3. **Mappa L1 Service Layer** (cap. 7 Webhooks in evidenza) — da produrre nella prossima sessione SAP Academy come primo nucleo della knowledge base
4. **Weekend 2 giugno**: avvio LLM wiki in formato carpasi — i PDF SAP scaricati sono il materiale di ingresso

---

## Blocchi o dipendenze

- `lk_taxdate_*`: da verificare se la sintassi `$[OPCH.DocDate.date]` senza FROM clause è supportata dal motore Formatted Search della versione SAP B1 10.0 installata. Se non supportata, alternativa pronta: `SELECT MAX(DocDate) FROM OPCH WHERE DocNum = $[OPCH.DocNum.number]`
- Mappa L1: richiede sessione dedicata, non avviata oggi per scelta deliberata di non sovraccaricare il weekend
