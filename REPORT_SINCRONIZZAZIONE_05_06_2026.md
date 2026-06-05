# REPORT SINCRONIZZAZIONE — 05/06/2026

Esecuzione automatica del task `sync-riepiloghi-push-github`. Agente in modalità autonoma (utente non presente).

## 1. Riepiloghi sincronizzati in 510

| Progetto | File sorgente | Azione in 510 |
|---|---|---|
| 420.ASSISTENTE AMMINISTRATIVA | `ASSISTENTE AMMINISTRATIVA RIEPILOGO 23_05_2026.md` | Creato (mancava in 510) |
| 310.SAP ACADEMY | `SAP ACADEMY RIEPILOGO 24_05_2026.md` | Aggiornato (510 conteneva ancora la sessione del 13/02) |
| 401.SINTESI ECOSISTEMA VICECONTI | `SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO 05_06_2026.md` | Aggiornato (nuova Sintesi v1.8) |

Tutti gli altri 15 riepiloghi di progetto erano gia' allineati (contenuto identico): AI HUMAN LAB, AUDIT INFRASTRUTTURA, CALENDAR, DATABASE CENTRALIZZATO, ECOSISTEMA TEAM SYSTEM, FastAPI, HUB DOCUMENTALE, MEMORIA E CONTESTO AI, NAMING VICECONTI, NUOVA STRATEGIA SETTEMBRE 2026, PIPELINE PRESTASHOP, SAP SERVICE LAYER, STRUMENTI DI CATTURA VOCALE, VICECONTI HUB, n8n.

Master da 520: `ISTRUZIONI_RIEPILOGO_E_NAMING.md` invariato.

## 2. File obsoleti rimossi da 510

- `REPORT_SINCRONIZZAZIONE_17_05_2026.md` — rimosso (report storico)
- `REPORT_SINCRONIZZAZIONE_18_05_2026.md` — rimosso (sostituito dal report odierno)

Nessun file orfano da rimuovere. `README_push_github.md` e' un file protetto/infrastrutturale e viene mantenuto.

## 3. Esito push GitHub (repo viceconti-hub/contesto-ai, branch main)

| Esito | Conteggio |
|---|---|
| Creati | 1 |
| Aggiornati | 5 (SAP ACADEMY, SINTESI ECOSISTEMA + index.json, index.html, _last_push_tmp.json) |
| Invariati | 17 |
| Eliminati | 1 (REPORT_SINCRONIZZAZIONE_18_05_2026.md su GitHub) |
| Errori | 0 |

Il report odierno viene pubblicato con un secondo push dopo la generazione.

## 4. Cartelle senza riepilogo conforme (intervento manuale consigliato)

- 220.FORMAZIONE IT — nessun riepilogo
- 370.TELEGRAM — nessun riepilogo
- 400.MANUALE DELLE AUTOMAZIONI — nessun riepilogo
- 421.ASSISTENTE DI PROGETTO — nessun riepilogo
- 450.INTERFACCIA SERVICE LAYER — nessun riepilogo
- 470.ASSET PRODOTTI — nessun riepilogo
- 600.CODE — nessun riepilogo
- 700.LLM WIKI — nessun riepilogo
- 490.PIPELINE PRESTASHOP — solo file `.docx`, nessun `.md` conforme (in 510 resta il riepilogo esistente)
- 005.ORGANIZZAZIONE PROGETTI CLAUDE — solo materiali di lavoro, nessun riepilogo di progetto

## Note operative

- PowerShell nativo non disponibile nella sandbox Linux: usata la replica `push_github.py` (v2.1), esito netto sul repo identico.
- Abilitata la cancellazione file nella cartella PROGETTI CLAUDE per rimuovere i report storici da 510.
