# Audit Infrastruttura — Riepilogo del 4 aprile 2026

## Stato attuale

L'infrastruttura è tornata a regime dopo la crisi del 13 marzo 2026. Il riavvio controllato di SQLPRD0303, eseguito il 3 aprile da 3W Sistemi (Antonio Forlani), ha azzerato l'uptime dopo 1090 giorni e portato la RAM dal 92% al 63%. Il Service Layer è operativo con 3 nodi (ridotti da 10). Gli aggiornamenti Windows pendenti su SQLPRD0303 sono ancora da installare in finestra pianificata.

Il progetto ha ampliato il proprio scopo: da audit statico dell'infrastruttura a presidio continuativo della sua salute, con l'obiettivo di trasformare la gestione da reattiva a proattiva.

---

## Lavoro svolto nel periodo (marzo–aprile 2026)

### Gestione crisi SQLPRD0303 (13–16 marzo 2026)

- Rilevati dati critici: RAM 10,7/12 GB (89%), uptime 1074 giorni, Apache Commons Daemon 2,1 GB, 10 nodi Service Layer × 64 worker thread
- Identificata causa radice: Service Layer sovradimensionato (10 nodi) installato su SQLPRD0303 invece che su BPRD0303, in competizione diretta con SQL Server sulla RAM disponibile
- Inviata email di intervento urgente a Var Group (15/03) con diagnosi precisa, sequenza di intervento corretta (shutdown ordinato SQL Server → riavvio VM) e richiesta di pianificazione manutenzione periodica
- Var Group ha ridotto silenziosamente i nodi Service Layer (probabilmente il 16/03), portando Apache da 2,1 GB a 155 MB, senza comunicazione esplicita
- Apertura ticket #1382783 (16/03): chiuso il 31/03 da Var Group senza intervento reale sul riavvio

### Riavvio controllato SQLPRD0303 (3 aprile 2026)

- Escalation a 3W Sistemi (Antonio Forlani) via WhatsApp dopo la chiusura senza esito del ticket Var Group
- Riavvio eseguito in giornata da 3W Sistemi con shutdown ordinato SQL Server
- Risultati post-riavvio:
  - **Uptime:** azzerato (5 ore al momento della rilevazione)
  - **RAM:** 7,6/12,0 GB (63%) — miglioramento di 29 punti percentuali
  - **Apache Commons Daemon (Service Layer):** 721 MB (da 2,1 GB)
  - **SQL Server:** 2,0 GB stabile
  - **CPU:** 13% a riposo

### Approfondimenti architetturali (marzo 2026)

Nel corso delle sessioni sono stati esplorati i seguenti temi, ora documentati come conoscenza acquisita del progetto:

- **Logica dei due server:** separazione DB (SQLPRD0303) e applicativo (BPRD0303) per isolamento dei carichi. Il fallimento a cascata del 13/03 — BPRD0303 imballato per connessioni SQL in attesa — dimostra che l'isolamento riduce ma non elimina le propagazioni.
- **Rete privata virtuale nel datacenter:** i due server comunicano direttamente sulla rete 192.168.122.x senza VPN, perché sono già nella stessa rete privata gestita da Var Group. La VPN è necessaria solo per attraversare internet dall'esterno.
- **Proxy e reverse proxy:** il proxy è un intermediario che protegge il server interno esponendo solo un punto di accesso controllato. Applicato all'architettura SAP: BPRD0303 come proxy verso il Service Layer su SQLPRD0303, con SQLPRD0303 mai esposto direttamente.
- **Architettura DMZ con terzo server:** identificata come soluzione architetturale ottimale per il medio termine. Un terzo server Var Group (Linux, autogestito) come unico punto di esposizione verso internet — proxy verso Service Layer, sede di n8n, webhook, automazioni web-facing. Firewall interno che isola SQLPRD0303 dal traffico non proveniente da BPRD0303.
- **Mislocazione Service Layer:** confermato che il Service Layer appartiene architetturalmente a BPRD0303 (layer applicativo), non a SQLPRD0303 (layer dati). Lo spostamento è un miglioramento strutturale da pianificare nel medio termine con Vincenzo Strazzullo.

---

## Decisioni prese

- **Interlocutore operativo per SQLPRD0303:** 3W Sistemi (Antonio Forlani) è il contatto diretto per interventi sul server database. Var Group è il provider fisico ma risponde tramite 3W per le questioni SAP.
- **Aggiornamenti Windows:** da non installare autonomamente. Richiedono finestra pianificata con shutdown ordinato di SQL Server. Il tasto "Install now" su SQLPRD0303 non va premuto senza coordinamento.
- **DMZ come obiettivo architetturale:** la configurazione Internet → Server DMZ → firewall → rete interna è confermata come direzione evolutiva corretta, da attivare dopo stabilizzazione dell'infrastruttura corrente.

---

## Prossimi passi

1. **Finestra manutenzione Windows** — richiedere a 3W Sistemi la pianificazione dell'installazione degli aggiornamenti pendenti su SQLPRD0303 (KB5073723, KB5066738, driver Broadcom) con sequenza: shutdown SQL Server → installazione → riavvio → verifica SAP e Service Layer
2. **Piano manutenzione periodica** — definire con 3W Sistemi: riavvii programmati (mensili o bimestrali), soglia alert RAM (85%), finestra oraria di intervento
3. **Conferma nodi Service Layer** — verificare formalmente che la configurazione a 3 nodi sia quella attiva e documentarla come configurazione di regime
4. **Spostamento Service Layer su BPRD0303** — da pianificare con Vincenzo Strazzullo come intervento strutturale nel medio termine
5. **Valutazione terzo server DMZ** — aprire conversazione con Var Group (specifiche, costi, isolamento firewall interno) nella stessa finestra della manutenzione Windows

---

## Blocchi e dipendenze

- **Aggiornamenti Windows SQLPRD0303:** pronti, in attesa di finestra pianificata con 3W Sistemi
- **Spostamento Service Layer:** richiede disponibilità di Vincenzo Strazzullo e stabilizzazione previa dell'infrastruttura
- **Terzo server DMZ:** da valutare costi con Var Group — non prioritario finché l'accesso esterno al Service Layer non è necessario operativamente
- **Comunicazione Var Group:** il ticket #1382783 chiuso senza intervento reale evidenzia un problema di gestione: Var Group non comunica proattivamente le modifiche effettuate (riduzione nodi) e chiude ticket senza esecuzione. Da affrontare nella prossima interazione.

---

## Quadro tecnico di riferimento (stato al 4 aprile 2026)

| Server | Ruolo | RAM | Uptime | Note |
|--------|-------|-----|--------|------|
| SQLPRD0303 | DB SAP + Service Layer | 7,6/12 GB (63%) | ~24 ore | Riavviato 03/04, aggiornamenti Windows pendenti |
| BPRD0303 | Terminal server SAP | Da monitorare | Da rilevare | Riavviato 14/03 |

| Componente | Stato | Configurazione attuale |
|------------|-------|------------------------|
| SQL Server 2019 | Operativo | 2,0 GB RAM, stabile |
| Service Layer (Apache) | Operativo | 3 nodi (ridotti da 10), 721 MB |
| SLD / License Server | Operativo | Porta 40000/30000 |
| Aggiornamenti Windows | Pendenti | KB5073723, KB5066738, driver Broadcom |

---

*Riepilogo del 4 aprile 2026 — progetto AUDIT INFRASTRUTTURA*
*Consolida: Contesto_Manutenzione_Infrastruttura.md (15/03), sessioni chat marzo–aprile 2026, elementi infrastrutturali da SAP SERVICE LAYER RIEPILOGO 03_04_2026.md*
