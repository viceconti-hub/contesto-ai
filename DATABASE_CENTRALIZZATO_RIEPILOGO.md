# RIEPILOGO SESSIONE DATABASE CENTRALIZZATO — 19 Aprile 2026

## Stato architetturale

Il Database Centralizzato è operativo come hub di normalizzazione dati articoli. Il flusso end-to-end è validato in produzione: da listino fornitore a SAP via Service Layer, senza file Excel intermedi né import manuale.

L'architettura separa rigorosamente la normalizzazione (flessibile, evolve) dall'import (rigido, formato standard unico) e dal sync verso SAP (rigido, Service Layer API).

---

## Percorso e file

```
C:\Viceconti\catalogo_centrale\
├── config.py           v2.0  — 84 fornitori da SAP con CardCode, prefisso, gruppo articoli
├── database.py         v2.0  — Schema v2 con tracking sync SAP, migrazione da v1
├── import_unico.py     v1.0  — Accetta SOLO formato standard (stella polare)
├── sync_sap.py         v1.0  — Service Layer: POST nuovi + PATCH prezzi + gruppo + U_DATAPREZZO
├── converti_morini.py  v1.0  — Listino Morini originale → formato standard
├── converti_coldline.py v1.0 — Listino Coldline originale → formato standard
├── pulisci_morini.py   v1.0  — DELETE/congela articoli da lista
├── fix_fornitore.py          — Fix campo fornitore per dati migrati da v1
├── reset_sync_cold.py        — Reset flag sync per Coldline (utility)
├── prodotti.db               — SQLite, schema v2
├── _inbox\                   — File da importare
├── export\                   — Output futuri
├── backup\                   — Backup automatici
├── logs\                     — Log operazioni
├── listini_fornitori\morini\ — LI0018500.XLSX + MORINI_2026_01.XLSX
└── listini_fornitori\coldline\ — PriceList_IT.xls
```

---

## Formato standard di ingresso (il vincolo rigido)

Colonne obbligatorie: `fornitore`, `codice_articolo`, `descrizione`, `prezzo_listino`

Colonne opzionali: `prezzo_netto`, `sconto`, `ean`, `peso_kg`, `volume_m3`, `dimensioni`, `giacenza`, `unita_misura`, `categoria`, `famiglia`, `imballo_minimo`

Qualsiasi sorgente (listino completo, conferma ordine, input manuale) deve produrre questo formato prima di entrare nel database. La normalizzazione avviene a monte: dai convertitori per fornitore, dall'assistente amministrativa, o manualmente.

Il campo `fornitore` accetta: chiave config (`MOR`, `COLD`, `LF`), nome completo (`MORINI s.r.l.`, `REPA ITALIA SRL`), o alias comune (`MORINI`, `REPA`, `COLDLINE`, `PADERNO`). Risolto automaticamente via `NOMI_FORNITORE_LOOKUP`.

---

## Flusso operativo (4 step manuali)

**Pattern 1 — Listino completo (Morini, Coldline, Paderno...):**
```
1. converti_{fornitore}.py → produce Excel formato standard
2. import_unico.py         → aggiorna prodotti.db
3. sync_sap.py             → crea nuovi + aggiorna prezzi in SAP
```

**Pattern 2 — Conferma ordine (REPA, IDEAM, qualsiasi):**
```
1. Assistente amministrativa → produce Excel formato standard
2. import_unico.py          → aggiorna prodotti.db
3. sync_sap.py              → crea nuovi + aggiorna prezzi in SAP
```

I 4 step sono tutti manuali. L'automazione della catena è il lavoro futuro di n8n.

---

## Operazioni eseguite in produzione — 19 aprile 2026

| Operazione | Risultato |
|---|---|
| Migrazione DB v1 → v2 | 10.198 prodotti migrati, schema aggiornato |
| Coldline sync produzione | 1.737 nuovi creati, 166 prezzi aggiornati, 0 errori |
| Pulizia Morini | 11.710 eliminati, 10 congelati, 0 errori |
| REPA/IDEAM sync TEST | 4 REPA (1 nuovo + 3 esistenti), 5 IDEAM nuovi |
| Morini sync TEST | 20 nuovi creati (test) |
| Coldline sync TEST | 6 nuovi + 4 aggiornati + 5 nuovi (due run) |
| Articolo TEST.000001 | Creato e aggiornato prezzo via Postman (validazione API) |

---

## Schema database — tabella prodotti

| Campo | Ruolo |
|---|---|
| `sku` | Chiave primaria (es. MOR.0000171, COLD.A12060100101, LF.3090016) |
| `codice_fornitore` | Codice senza prefisso |
| `fornitore` | Chiave config (MOR, COLD, LF) |
| `descrizione` | Descrizione articolo |
| `prezzo_listino` | Prezzo lordo / listino fornitore |
| `sconto` | Sconto (testo: "50", "50+15") |
| `prezzo_netto` | Prezzo netto dopo sconti |
| `ean`, `peso_kg`, `volume_m3`, `dimensioni` | Dati fisici |
| `giacenza`, `unita_misura`, `categoria`, `famiglia` | Dati catalogo |
| `hash_riga` | MD5 per rilevare modifiche |
| `ultimo_sync_sap` | Data ultimo sync verso SAP |
| `prezzo_ultimo_sync` | Prezzo al momento dell'ultimo sync |
| Campi SEO (nome_seo, meta_title, ecc.) | Per uso futuro PrestaShop |

Tabelle accessorie: `storico_prezzi` (audit variazioni), `log_operazioni` (tracciabilità), `fornitori` (legacy).

---

## Config.py — registro fornitori

84 fornitori con: `prefisso_sku`, `card_code` (SAP), `nome`, `gruppo` (ItemsGroupCode SAP). Lookup inverso `NOMI_FORNITORE_LOOKUP` per risolvere alias (REPA → LF, COLDLINE → COLD, PADERNO → PAD).

Gruppi articoli SAP: ATTREZZATURE=119, HOTELLERIE=118, RICAMBI=103, ACCESSORI=121.

---

## Sync SAP — comportamento dettagliato

Fase 1 (nuovi): per ogni articolo con `ultimo_sync_sap IS NULL`, verifica se esiste in SAP. Se non esiste → POST con ItemCode, ItemName, Mainsupplier, SupplierCatalogNo, SalesVATGroup (V2), PurchaseVATGroup (A2), ItemsGroupCode, ItemPrices PriceList 1, U_DATAPREZZO. Se esiste già → PATCH solo prezzo + U_DATAPREZZO.

Fase 2 (prezzi): per ogni articolo già sincronizzato con `prezzo_netto != prezzo_ultimo_sync` → PATCH prezzo + U_DATAPREZZO.

Flag CLI: `--test` (TEST_Viceconti), `--dry-run`, `--fornitore`, `--limite`, `--solo-nuovi`, `--solo-prezzi`.

---

## Definizione prezzi (da implementare nella prossima sessione)

| Campo | Significato | Esempio Coldline | Esempio Morini |
|---|---|---|---|
| Listino pubblico fornitore | Prezzo che il fornitore pubblica | 2.652 | Non esiste |
| Nostro listino (= SAP PriceList 1) | Il nostro prezzo di riferimento | 2.652 (= listino fornitore) | Netto × 2 (es. 6,12) |
| Sconto acquisto (dal nostro listino) | Quanto scontiamo per calcolare il nostro costo | 50% | 50% |
| Prezzo acquisto (netto a noi) | Cosa paghiamo al fornitore | 1.326 | 3,06 |
| Sconto vendita (dal nostro listino) | Quanto scontiamo al cliente | 30% | 20% |
| Prezzo vendita | Cosa paga il cliente | 1.856 | 4,90 |

Il prezzo caricato in SAP è sempre **il nostro listino**. Lo sconto acquisto e lo sconto vendita sono applicati a partire da questo valore.

Nota: per Morini lo sconto acquisto è 50% calcolato dal nostro listino (che è netto fornitore × 2), quindi il risultato è esattamente il prezzo netto del fornitore. L'ordine di acquisto in SAP riporta il nostro listino con sconto 50%.

---

## Decisioni aperte — prossima sessione

1. **Correzione logica prezzi**: il database deve distinguere chiaramente tra prezzo acquisto e nostro listino. Lo script sync deve mandare a SAP il nostro listino, non il prezzo di acquisto. Per Morini serve il moltiplicatore ×2. Aggiornare anche `import_unico.py` per rendere `prezzo_netto` opzionale (oggi è obbligatorio).

2. **Migrazione codici Morini**: passare da MOR.xxxxx (senza zeri) a MOR.0xxxxxx (7 cifre). Dopo la pulizia restano ~19.466 articoli Morini in SAP (31.206 - 11.710 eliminati - 10 congelati - 20 del primo test). I 777 con giacenza richiedono trasferimento saldi.

3. **Import nuovo listino Morini** + sync produzione (dopo migrazione codici).

4. **Aggiornamento riepilogo su GitHub** (DATABASE CENTRALIZZATO RIEPILOGO.md è fermo a febbraio).

5. **Convertitore conferma ordine Morini** (formato Excel allegato ai documenti transazionali).

6. **Automazione n8n** (futuro): monitoraggio email/cartella → convertitore → import → sync → report Telegram.

---

## Conoscenza tecnica Service Layer acquisita

| Endpoint | Metodo | Uso |
|---|---|---|
| `/Login` | POST | Autenticazione, ritorna B1SESSION cookie |
| `/Items` | POST | Crea articolo — payload minimale sufficiente |
| `/Items('SKU')` | GET | Legge articolo — verifica esistenza |
| `/Items('SKU')` | PATCH | Aggiorna campi specifici senza toccare il resto |
| `/Items('SKU')` | DELETE | Elimina articolo (solo se senza transazioni) |
| `/ItemGroups` | GET | Lista gruppi articoli con codici |
| `/Logout` | POST | Chiude sessione |

Timeout sessione: 31 minuti. SSL verification: disabilitata (certificato self-signed).

---

## Insight architetturali emersi nella sessione

**Il formato standard come stella polare**: la rigidità del formato di ingresso del database semplifica tutto quello che viene prima e dopo. La normalizzazione a monte è flessibile e può evolvere; l'import e il sync sono rigidi e testati una volta sola.

**Parallelo con Apple Reminders**: il Database Centralizzato e Reminders sono le prime "barre fluviali" — punti fermi dove i flussi informativi caotici rallentano e si depositano in forma strutturata.

**Service Layer vs import Excel**: con il Service Layer le due fasi (nuovi articoli + aggiornamento prezzi) restano concettualmente separate ma diventano un'unica esecuzione dello script. Il PATCH è chirurgico: aggiorna solo i campi specificati.

**Ogni export decide la completezza**: non serve un campo "stato" nel database. Ogni destinazione (SAP, PrestaShop, B2B futuro) sa cosa le serve e verifica i campi necessari. Un articolo REPA con solo codice+descrizione+prezzo è "completo per SAP" e "incompleto per PrestaShop".

**Il database come hub multi-destinazione**: SAP è solo un export. Lo stesso database alimenterà PrestaShop B2C, la futura piattaforma B2B, report interni. L'opzione di un sito B2B indipendente diventa sostenibile perché il costo di alimentarlo è un export in più, non un lavoro duplicato.

---

*Sessione del 19 aprile 2026 — Viceconti s.n.c.*
*Prossimo aggiornamento: da caricare su GitHub come DATABASE CENTRALIZZATO RIEPILOGO.md*
