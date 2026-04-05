# SAP Service Layer — Riepilogo del 3 aprile 2026

## Stato attuale

Il Service Layer è operativo in produzione e validato per la registrazione automatica di fatture di acquisto di servizi. In questa sessione è stata completata la prima automazione contabile reale su SAP B1 via API, con 111 fatture registrate in produzione senza errori. Il server SQLPRD0303 è stato riavviato dopo 1090 giorni di uptime e la RAM è scesa dal 92% al 63%.

---

## Lavoro svolto in questa sessione

### Registrazione fatture acquisti via Service Layer

**Validazione payload su TEST_Viceconti:**
- Identificati e risolti i due errori bloccanti:
  - `-5002` "Item number is missing" → causa: mancanza di `DocType: "dDocument_Service"` a livello header
  - `-10` "VAT Allocation Date is mandatory" → causa: campo `VatDate` obbligatorio, mappato sulla stessa data di `DocDate`
- Template payload definitivo validato per fatture di servizi (senza movimentazione magazzino)

**Test operazioni di correzione:**
- `DELETE` su fattura registrata → bloccato da SAP anche via Service Layer (errore -5006) — comportamento corretto e atteso
- `POST /PurchaseInvoices(DocEntry)/Cancel` → funziona, crea storno automatico pulito — questa è la procedura di correzione a regime

**Comportamento di `/Drafts` — non risolto:**
- Tutti i tentativi di `POST /Drafts` con `DocObjectCode: "oPurchaseInvoices"` hanno prodotto una registrazione diretta invece di un documento parcheggiato
- Il documento parcheggiato Cimbali visibile nell'elenco bozze del client era stato creato manualmente, non via Service Layer
- Il perché `/Drafts` registri invece di parcheggiare rimane inspiegato — da chiarire con Vincenzo Strazzullo
- Per il flusso fatture Wind a regime si usa il POST diretto a `PurchaseInvoices`, indipendentemente dalla risoluzione di questo punto

**Script Python `registra_fattura.py` (sviluppato in Claude Code):**
- Fase 1: script minimale su singolo XML Wind Tre → TEST_Viceconti → OK
- Fase 2: batch 48 fatture Wind Tre → TEST_Viceconti → 42 OK, 6 bloccate per periodo contabile chiuso (dicembre 2025 — non un errore dello script)
- Fase 3: integrazione indice Excel `Indice_Fornitori_SAP.xlsx` come lookup table PIVA → CardCode/AccountCode/VatGroup
- Lancio completo su `SBO_Viceconti` (produzione): **111 fatture registrate, 27 fornitori, zero errori**
- Post-registrazione: eliminazione automatica del file XML dalla cartella APERTE

**Indice fornitori `Indice_Fornitori_SAP.xlsx`:**
- 80 fornitori unici catalogati
- 27 fornitori con mapping completo (compilati con query SQL su OPCH/PCH1/OCRD + integrazione manuale per SEO CUBE SRL e VAR GROUP SPA)
- 10 fornitori esclusi per ora (fatture con movimentazione magazzino o dati mancanti)
- Il file è la lookup table dello script — da aggiornare quando si aggiungono nuovi fornitori al perimetro

**Struttura cartelle operativa:**
```
FATTURE DA FORNITORE/
├── APERTE/           → XML da registrare (input script)
├── ARCHIVIO/[mese]/  → XML rinominati per mese (storico)
└── REGISTRATE/       → ZIP originali e PDF storici
```

### Infrastruttura — Riavvio SQLPRD0303

- Ticket #1382783 aperto il 16/03 con Var Group: chiuso il 31/03 senza intervento reale (uptime invariato a 1090 giorni, RAM invariata al 92%)
- Escalation a 3W Sistemi (Antonio Forlani) via WhatsApp il 03/04 → riavvio controllato eseguito in giornata
- Risultati post-riavvio:
  - **Uptime:** azzerato (5 ore)
  - **RAM:** 7,6/12,0 GB (63%) — dal 92% al 63%, 4,4 GB liberi
  - **Apache Commons Daemon (Service Layer):** 721 MB — da 2,1 GB
  - **SQL Server:** 2,0 GB — stabile

---

## Decisioni prese

- **Flusso fatture a regime:** POST diretto a `PurchaseInvoices` + `Cancel` in caso di errore. Le Drafts non sono necessarie per il perimetro attuale (fatture Wind e fornitori servizi ricorrenti con mapping validato).
- **Perimetro automazione:** solo fatture di servizi (senza entrata merci). Le fatture acquisto con movimentazione magazzino restano manuali.
- **Spostamento in ARCHIVIO:** lo script elimina il file XML da APERTE dopo registrazione riuscita. Lo spostamento fisico in ARCHIVIO avviene separatamente (da valutare se automatizzare).
- **Linguaggio script:** Python (coerente con Pipeline PrestaShop, più leggibile per parsing XML e gestione Excel rispetto a PowerShell).
- **Interlocutore infrastruttura:** 3W Sistemi (Antonio Forlani) è il contatto operativo diretto per interventi su SQLPRD0303. Var Group è il provider fisico ma risponde tramite 3W.

---

## Prossimi passi

1. **Completare il riempimento dell'indice fornitori** — aggiungere CardCode SAP per i fornitori attualmente esclusi, man mano che si vuole estendere il perimetro
2. **Spostamento file in ARCHIVIO** — aggiungere allo script il movimento XML → ARCHIVIO/[anno]/[mese]/ dopo registrazione riuscita (attualmente solo eliminazione da APERTE)
3. **Manutenzione periodica SQLPRD0303** — richiedere a 3W Sistemi la pianificazione di riavvii programmati (mensili o bimestrali) e monitoraggio RAM con alert sopra 85%
4. **Aggiornamento SAP B1** — prerequisito per stabilità a lungo termine; da pianificare con Vincenzo Strazzullo

---

## Blocchi e dipendenze

**Domande aperte per Vincenzo Strazzullo:**
1. Documento parcheggiato via Service Layer — conferma del payload corretto per `POST /Drafts`
2. B1iF — è installato? Ha esperienza di configurazione?
3. Tool PDF SAP — funzionamento, costi, cartella di destinazione configurabile
4. Esposizione Service Layer senza VPN — per accesso futuro da tablet dei tecnici

**Periodo contabile chiuso (dicembre 2025):** 6 fatture Wind bloccate su TEST_Viceconti per periodo chiuso. Le stesse fatture sono state registrate correttamente su SBO_Viceconti (produzione) dove i periodi sono aperti. Non è un problema dello script né un blocco operativo.

---

## Conoscenza tecnica acquisita (da conservare)

| Campo | Valore | Note |
|---|---|---|
| `DocType` | `"dDocument_Service"` | Obbligatorio a livello header per fatture di servizi |
| `VatDate` | uguale a `DocDate` | Obbligatorio, distinto da `TaxDate` |
| `DocumentStatus` | non specificare | SAP registra direttamente; `bost_Open` = aperta non pagata, non bozza |
| DELETE su fattura registrata | bloccato (-5006) | Anche via Service Layer |
| Storno fattura | `POST /PurchaseInvoices(DocEntry)/Cancel` | Unica operazione di annullamento disponibile |
| `/Drafts` | documento parcheggiato | `DocObjectCode: "oPurchaseInvoices"` necessario |

---

*Sessione del 3 aprile 2026 — Viceconti s.n.c.*
