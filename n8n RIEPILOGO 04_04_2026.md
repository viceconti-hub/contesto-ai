# n8n — Riepilogo del 4 aprile 2026

## Stato attuale

Progetto in fase di avvio operativo dopo circa tre mesi di standby consapevole. La configurazione di n8n non è ancora iniziata (software non installato), ma il lavoro concettuale e architetturale è stato completato in due sessioni (febbraio e aprile 2026). Il ruolo di n8n nell'ecosistema è ora definito con precisione: orchestratore di flussi event-driven sul PC Lauria, complementare a Task Scheduler che resta sul server SAP.

## Lavoro svolto in questa sessione

- Definizione del ruolo di n8n rispetto agli altri strumenti dell'ecosistema:
  - **n8n** → orchestrazione event-driven e flussi tra componenti (PC Lauria)
  - **Task Scheduler** → cron semplici sul server SAP (resta dov'è, non viene sostituito)
  - **Cowork schedulato** → compiti che richiedono giudizio e comprensione (analisi log, riepiloghi)
  - **Claude Code** → costruzione e sviluppo (pair programming)
  - **Claude.ai** → ragionamento strategico e architetturale
- Chiarimento sui limiti di n8n: stato persistente complesso e elaborazioni di lunga durata restano nell'engine layer (script Python/PowerShell), n8n li invoca e coordina
- Identificazione del primo caso d'uso concreto: flusso AudioPen → n8n → Telegram + salvataggio locale (flusso parallelo "a Y")
- Validazione dell'analisi architetturale proposta da Gemini sul flusso parallelo, con precisazione che n8n self-hosted sul PC Lauria non è "in cloud"
- Consolidamento del principio: n8n non sostituisce Task Scheduler sul server SAP — coesistenza pulita con confini chiari

## Decisioni prese

- **n8n si installa sul PC Lauria**, non sul server SAP. Comunica con il server SAP solo attraverso interfacce definite (JSON su GitHub, file su Dropbox, in futuro Service Layer)
- **Task Scheduler resta sul server SAP** per il SAP Query Engine e gli altri script di produzione. Migrazione verso n8n solo se c'è un vantaggio concreto, non per centralizzazione fine a sé stessa
- **Modalità installazione: npm globale** (non Docker), per accesso diretto a filesystem locale, script PowerShell/Python, SQLite
- **Un solo giorno dedicato** alla configurazione iniziale (sabato 5 aprile — ponte pasquale), con obiettivo: installazione + primo workflow AudioPen funzionante
- **Consolidamento prima dell'orchestrazione** confermato come principio: i componenti dell'engine layer sono stati revisionati prima di aggiungere n8n

## Prossimi passi

1. **Verifiche pre-installazione:** `node --version` sul PC Lauria + modalità di esportazione dati di AudioPen (webhook, API, integrazione Zapier)
2. **Sabato 5 aprile — installazione n8n** via npm globale sul PC Lauria
3. **Primo workflow:** AudioPen → n8n (webhook) → ramo A: Telegram gruppo tecnici + ramo B: salvataggio locale (file o SQLite)
4. **Valutazione post-installazione:** una volta operativo, valutare quali altri flussi event-driven migrare in n8n (es. Telegram → SQLite, monitoraggio componenti)

## Blocchi o dipendenze

- **Node.js sul PC Lauria** — da verificare se installato e quale versione
- **Modalità export AudioPen** — da verificare se espone webhook, API, o solo integrazione Zapier nativa
- **PC Lauria acceso** — n8n self-hosted dipende dalla disponibilità della macchina (attualmente già operativa 24/7 per gli indicizzatori Hub Documentale)
