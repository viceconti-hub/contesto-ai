# Strumenti di Cattura Vocale — Riepilogo del 26 marzo 2026

## Stato attuale
Il progetto è in fase di espansione attiva. AudioPen Prime è consolidato come componente stabile dell'ecosistema (stato: Adottato). Nella sessione del 26 marzo sono emersi nuovi use case operativi e sono stati validati strumenti aggiuntivi che ridefiniscono la mappa complessiva degli strumenti di cattura vocale.

---

## Lavoro svolto in questa sessione

### Panoramica tecnologica
Prodotta una mappatura a quattro livelli delle estensioni possibili degli strumenti di cattura vocale:
- **Livello 1**: funzionalità AudioPen Prime già disponibili e non pienamente sfruttate (SuperSummaries, Custom Styles, tag sistematici, "view original transcript")
- **Livello 2**: catena di automazione AudioPen → Zapier/webhook → n8n → destinazione finale
- **Livello 3**: strumenti adiacenti per use case che AudioPen non copre (meeting AI con diarization, hardware dedicato per ambienti rumorosi, voice-to-action nativa del SO)
- **Livello 4**: AudioPen come strato di input per processi strutturati (direzione SAP)

### Identificazione del problema: speaker diarization
Emerso il bisogno di separare le voci in una conversazione multi-partecipante (caso concreto: tecnico che illustra esito sopralluogo → distinta materiali). AudioPen non copre questo caso. Identificata la funzione tecnica necessaria: **speaker diarization**.

### Approfondimento PLAUD
Analizzata la gamma PLAUD come soluzione hardware con diarization nativa:
- Tutta la gamma supporta speaker diarization in 112 lingue, vocabolario personalizzato, 10.000+ template per output strutturati
- **PLAUD Note**: aggancio magnetico al telefono, ottimale per telefonate (sensore VCS), 30h registrazione
- **PLAUD Note Pro**: array 4 microfoni MEMS, SNR 67-72dB, ottimale per ambienti tecnici rumorosi
- **PLAUD NotePin S**: wearable (collana/bracciale/clip), 20h registrazione, form factor ideale per sopralluogo
- TCO 3 anni: ~$459 (hardware ~$159 + $99/anno subscription Pro)
- Lettura strategica: PLAUD non sostituisce AudioPen — copre use case distinti e complementari (interazioni multi-voce vs cattura cognitiva personale)

### Scoperta e validazione: microfono nativo Claude
Identificato e testato il microfono nativo di Claude.ai come canale di input vocale diretto in chat. Validato con successo nella sessione odierna. Elimina il workflow AudioPen → copia → incolla per le interazioni conversazionali dirette.

Comportamento sulle pause: il microfono nativo si interrompe sulle pause lunghe. Identificato **Jamie Live** come soluzione intermedia per il parlato continuo con pause naturali.

### Mappa aggiornata a tre strumenti
| Strumento | Funzione | Quando usarlo |
|---|---|---|
| AudioPen | Cattura asincrona + rielaborazione + archiviazione | In macchina, cantiere, quando serve output strutturato o archiviazione |
| Microfono nativo Claude | Input vocale diretto in chat | Interazione conversazionale fluida, più messaggi consecutivi |
| Jamie Live | Trascrizione continua con pause naturali | Input vocale diretto in chat per parlato lungo o con ritmo naturale |

### Workflow validati nella sessione
- **AudioPen → Claude → documento → invio**: prodotto e inviato "BRIEFING Progetto Sushi Sapri 28 03 2026.docx" al progettista. Workflow: voce → trascrizione AudioPen → strutturazione Claude → documento Word → WhatsApp
- **Voce → Claude → messaggio → invio WhatsApp**: testato e completato. Messaggio inviato alle 16:14 senza tastiera
- **Accumulo silenzioso multi-nota**: testato con microfono nativo. Dettate 4 attività SAP consecutive, Claude risponde "okay" tra una e l'altra, produce riepilogo strutturato a richiesta

### Scambio con Gemini — sviluppi in programma
Confermata l'architettura di automazione con n8n come orchestratore principale (bypass Zapier):
- **Flusso 1**: AudioPen → webhook → n8n → Telegram aziendale (istruzioni ai tecnici)
- **Flusso 2**: AudioPen → webhook → n8n → Google Drive/cloud → cartella locale PC → trigger automazioni locali
- **Flusso parallelo**: unico webhook AudioPen smista contemporaneamente verso Telegram e verso la cartella locale, eliminando dipendenze sequenziali e rischio "collo di bottiglia fisico"

---

## Decisioni prese

- Il microfono nativo Claude è adottato come canale di input per interazione conversazionale diretta — non sostituisce AudioPen ma ne integra il ruolo
- Jamie Live confermato come soluzione per il problema delle pause nel parlato continuo
- PLAUD in valutazione — decisione di acquisto non ancora presa, ma il NotePin S è il form factor più adatto al contesto operativo (sopralluogo, mani libere)
- n8n confermato come orchestratore esclusivo per le automazioni (Zapier non necessario)
- La catena AudioPen → Claude → documento → invio è validata come workflow operativo ripetibile

---

## Prossimi passi

1. Esplorare sistematicamente le funzionalità AudioPen Prime non ancora usate: SuperSummaries, tag per sintesi tematiche, stili custom per template operativi
2. Costruire il primo flusso n8n: AudioPen → webhook → Telegram aziendale
3. Aggiungere ramo parallelo: stesso webhook → cartella cloud → PC locale
4. Valutare acquisto PLAUD NotePin S per use case con tecnici e sopralluoghi
5. Definire template AudioPen dedicati per output operativi ricorrenti: riepilogo sopralluogo, briefing tecnico, istruzioni tecnici
6. Testare la modalità vocale bidirezionale di Claude (pulsante nero forma d'onda) in sessione dedicata

---

## Blocchi o dipendenze

- Il flusso AudioPen → SAP rimane bloccato dal prerequisito SAP Service Layer (progetto A4)
- La valutazione PLAUD richiede un test in condizioni reali di cantiere prima dell'acquisto

---

## Osservazioni per AI Human Lab

**20 marzo 2026 — AudioPen come innesco di vocalizzazione**
Uso inatteso: avvio della registrazione per strutturare ad alta voce una proposta prima di un colloquio. La registrazione è stata interrotta prima del termine, ma la vocalizzazione è continuata autonomamente. Distinzione concettuale: lo strumento ha operato come *innesco*, non come *protesi*. Formulazione del soggetto: *"come se fossero state neutralizzate tutte le password di accesso"* — collasso di un sistema di vincoli operante in modo invisibile, non apprendimento additivo.

**26 marzo 2026 — Collasso tra contenuto e istruzione**
Con il microfono nativo Claude il confine tra *depositare* informazioni e *attivare* un processo si dissolve: voce, contenuto e istruzione diventano un gesto unico. Con AudioPen i due momenti erano strutturalmente separati. Osservazione: l'ecosistema si ridefinisce non per esplorazione sistematica degli strumenti disponibili, ma per emersione progressiva dei bisogni — ogni strumento adottato crea le condizioni per vedere il problema successivo.
