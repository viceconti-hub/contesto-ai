# DIRECTORY ANALYZER — Riepilogo per progetto dedicato
## Riattivazione ciclo passivo per automazione registrazione fatture acquisti

*Generato: lunedì 4 maggio 2026*
*Origine: estrazione da SINTESI ECOSISTEMA per progetto dedicato*

---

## CONTESTO E OBIETTIVO

Viceconti S.N.C. utilizza la piattaforma TeamSystem TS Digital per la gestione della fatturazione elettronica. Lo strumento client **Directory Analyzer** versione 4.4.3.0 è installato sul server SAP e attualmente configurato per il **ciclo attivo** — invia automaticamente le fatture emesse da SAP verso lo SDI passando per AGYO. Funziona stabilmente: 5.154 fatture in uscita gestite con successo.

L'obiettivo del progetto è **attivare il ciclo passivo** dello stesso strumento — far sì che Directory Analyzer scarichi automaticamente in una cartella locale del server le fatture di acquisto che arrivano dai fornitori via SDI. Questo è il primo stadio di una pipeline più ampia per l'automazione completa della registrazione fatture acquisti in SAP.

---

## STATO DI CIÒ CHE SAPPIAMO CON CERTEZZA

**Strumento installato e funzionante**:
- Directory Analyzer v4.4.3.0 sul server SAP Viceconti
- Configurazione Ufficio_001 attiva per ciclo attivo AGYO
- 5.154 fatture inviate con successo
- FEPA disattivato, Conservazione Cloud non attiva

**Utenze applicative TS Digital esistenti** (verificate nello screenshot del 22 aprile):
- "UTENZA 10/01/2019" — id `1e06aab4-b50d-4fc6-8f4b-05536abddad0` — vecchia, da non toccare
- "Utenza Tecnica 08/02/23" — id `fce0ecb6-fce9-4509-a143-97515334eaea` — più recente, probabilmente in uso per ciclo attivo

**Pipeline più ampia di destinazione** (parzialmente esistente):
```
fornitori → SDI → AGYO → [Directory Analyzer ciclo passivo, da attivare]
    → cartella grezza locale → [script smistamento Python, da scrivere]
    → Dropbox\FATTURE DA FORNITORE\APERTE
    → [registra_fattura.py, già operativo] → SAP via Service Layer
```

**registra_fattura.py** già in produzione: 111 fatture registrate, 27 fornitori, zero errori. Il pezzo finale della pipeline funziona.

**Manuale Directory Analyzer 4.4.3.0** disponibile nei materiali del progetto (65 pagine).

---

## STATO DI CIÒ CHE VA VERIFICATO

**Sull'utenza applicativa**:
- Quale delle due utenze applicative è effettivamente in uso per il ciclo attivo?
- Quali permessi/scope ha attivati?
- È adeguata anche per il ciclo passivo, o serve estenderne i permessi?
- È preferibile estendere quella esistente o creare un'utenza dedicata al solo download passivo?

**Sulla configurazione AGYO ciclo passivo**:
- Quali sono esattamente i passaggi di configurazione lato portale TS Digital?
- È necessaria un'attivazione specifica del servizio "Ricezione" che oggi potrebbe non essere abilitata?
- Esistono costi aggiuntivi per attivare il ciclo passivo che non sono già coperti dal contratto attuale?

**Sulla configurazione Directory Analyzer**:
- Come si crea una seconda configurazione (Ufficio_002) dedicata al solo download passivo, mantenendo intatta quella esistente per il ciclo attivo?
- Quali sono i parametri obbligatori e quelli opzionali per il download passivo?
- Esiste opzione "scarica solo da data X in poi" per evitare il download massivo dello storico?
- Quale frequenza di polling è raccomandata?

**Sull'archivio storico**:
- Lo storico fatture passive degli ultimi 24+ mesi è disponibile per il download retroattivo, o solo le fatture nuove dalla data di attivazione?
- Se è disponibile lo storico, esistono filtri temporali per gestirlo gradualmente?

---

## PRECONDIZIONI PER L'ATTIVAZIONE

Prima di configurare lato Directory Analyzer:

1. Conferma da TeamSystem che l'utenza applicativa scelta abbia i permessi corretti per il ciclo passivo
2. Conferma che il servizio "AGYO Ricezione" sia attivo sul contratto Viceconti
3. Verifica struttura cartelle destinazione sul server SAP — proposta: `C:\TS_SYNCRO\01601090762\AGYO-DOWNLOAD\Ciclo Passivo\` (analogo alla struttura ciclo attivo)

Importante: la cartella destinazione del download grezzo NON deve essere dentro Dropbox per evitare race condition durante la sincronizzazione. Lo smistamento verso Dropbox\FATTURE DA FORNITORE\APERTE avverrà tramite script Python successivo.

---

## SEQUENZA OPERATIVA POST-RISPOSTA TEAMSYSTEM

Una volta ricevute le indicazioni dall'assistenza:

**Fase 1 — Configurazione utenze e servizi** (lato portale TS Digital)
- Verifica/abilitazione permessi utenza applicativa
- Attivazione servizio AGYO Ricezione se necessario
- Generazione eventuali nuove credenziali

**Fase 2 — Configurazione Directory Analyzer**
- Creazione configurazione dedicata al ciclo passivo (Ufficio_002 o estensione Ufficio_001)
- Definizione cartella destinazione grezza
- Impostazione filtri temporali per evitare download massivo
- Test con fatture passive recenti

**Fase 3 — Script smistamento Python**
- Script `smista_fatture_passive.py` da scrivere
- Funzioni: monitoraggio cartella grezza, parsing XML per estrazione data, copia in Dropbox APERTE + ARCHIVIO/[anno]/[mese]
- Idempotenza: log dei file processati per non duplicare
- Schedulazione via Task Scheduler

**Fase 4 — Test end-to-end**
- Fattura test nella cartella grezza
- Verifica smistamento corretto
- Lancio registra_fattura.py
- Verifica registrazione in SAP

**Fase 5 — Manutenzione e monitoraggio**
- Log centralizzati
- Alert in caso di anomalie
- Documentazione operativa

---

## RIFERIMENTI

**Documenti**:
- Manuale Directory Analyzer 4.4.3.0 (PDF, nei materiali del progetto)
- `Indice_Fornitori_SAP.xlsx` — 80 fornitori, 27 con mapping completo CardCode/AccountCode/VatGroup
- Script esistenti: `registra_fattura.py`, `crea_offerta.py` in `C:\Viceconti\viceconti-hub`

**Contatti**:
- Antonella (commercialista) — possibile interlocutore per aspetti contrattuali TeamSystem
- 3W Sistemi (Antonio Forlani) — supporto infrastrutturale server SAP, non specifico TeamSystem

**Identificativi tecnici**:
- Partita IVA Viceconti: 01601090762
- Versione Directory Analyzer: 4.4.3.0
- Server: SQLPRD0303 (Var Group)

---

## NOTE METODOLOGICHE PER IL PROGETTO

**Questo non è un progetto urgente** — è un'automazione di una coda di lavoro che oggi viene gestita manualmente. Si può procedere con calma, aspettando risposte ufficiali e testando con cura prima di andare in produzione.

**Approccio proposto**: meglio creare una configurazione separata (Ufficio_002) dedicata al ciclo passivo, piuttosto che modificare quella attiva del ciclo attivo. Isolamento completo: se qualcosa va storto sul nuovo, l'invio fatture attive in produzione (5.154 documenti gestiti) non viene toccato.

**Una nota sul timing**: Directory Analyzer + script smistamento + registra_fattura.py compongono insieme un'automazione di tipo **sostitutivo** (un gesto manuale ripetitivo viene fatto da una macchina). Si valuta sul risparmio di tempo, non sull'apertura di capacità nuove. Buon investimento, ma non strategico — quindi nessuna pressione a chiudere rapidamente.

---

*Riepilogo generato per il progetto dedicato Directory Analyzer*
*Da utilizzare come contesto iniziale nel nuovo progetto Claude*
