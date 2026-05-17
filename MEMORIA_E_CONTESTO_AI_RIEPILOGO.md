# MEMORIA E CONTESTO AI — Riepilogo della sessione del 15-16 maggio 2026

*Sessione iniziata venerdì 15 maggio sera (ore 22:18), proseguita sabato 16 maggio mattina (dalle 4:50 alle 10:42).*
*Tema centrale: panoramica sui formati architetturali (PDF, HTML, markdown, JSON), introduzione del pattern wiki LLM-maintained, chiusura del Cap. I del weekend operativo.*
*Continua da: MEMORIA E CONTESTO AI RIEPILOGO 09_05_2026.md.*

---

## Stato attuale

La sessione ha attraversato due fasi distinte e una transizione operativa.

**Fase 1 — Confronto preparatorio al weekend (15 maggio sera + 16 maggio alba).** Panoramica strutturata sui quattro formati principali dell'architettura, con il filo rosso "come questi formati si collocano nell'architettura e che relazioni hanno fra loro". Ne è uscita una mappa che vale la pena consolidare come principio architetturale: ogni formato occupa una nicchia funzionale specifica, la maturità sta nel comporre piuttosto che nello scegliere "il formato giusto" come decisione unica.

**Fase 2 — Cap. I del weekend operativo (16 maggio mattina, 09:46-10:20).** Test empirico del Problema 2 (generazione PDF da Service Layer) completato con Code. Esito B confermato con caratterizzazione completa dei 4 canali di trigger PDF. Scoperta collaterale: gotcha `Expect: 100-continue` come trappola infrastrutturale per integrazioni SL da .NET/PowerShell.

**Transizione concettuale alle 10:30**: la rilettura del Cap. I alla luce dell'ipotesi "archivio storico operativo in markdown" emersa alle 4:50 di stanotte **dissolve quasi completamente il cantiere PDF server-side**. Non perché il problema fosse mal posto, ma perché un'evoluzione architetturale più alta lo ha svuotato di significato funzionale.

**Energia residua per il Cap. IV manifest pattern del pomeriggio**: alta. L'utente si concede una pausa caffè prima di entrare nel cantiere centrale del weekend.

---

## Lavoro svolto in questa sessione

### 1. Allineamento iniziale e correzione "memoria datata"

All'apertura della sessione ho fetchato la Sintesi v1.3 fresca da GitHub raw (`https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md`) per evitare l'errore del 9 maggio (basarmi sui file allegati invece dei riepiloghi correnti). L'utente ha poi caricato la v1.5 come file, e questa è diventata il riferimento di lavoro per il resto della sessione.

### 2. Panoramica sui quattro formati architetturali

Discussione progressiva nei turni:

**PDF — Manufatto canonico+consegnabile.** Confermato come imprescindibile per documenti fiscali/contrattuali. È l'unico formato simultaneamente canonico (rappresenta univocamente un oggetto SAP) e consegnabile (funziona ovunque senza dipendenze). La resilienza maggiore del previsto è strutturale, non inerziale di mercato. **Correzione importante**: l'utente non usa Crystal Reports ma il modulo nativo SAP per layout di stampa. Il sotto-cantiere weekend del Cap. I va riformulato in termini di `PrintPreferencesService` o equivalente.

**HTML — Trinità interfaccia+documento+dato strutturato.** Riconosciuta l'evoluzione Excel → HTML → HTML First → API First come scalata cognitiva, non oscillazione. L'HTML conserva una nicchia precisa quando servono simultaneamente compilabilità interattiva + persistenza + dato strutturato (caso modulo tecnico). HTML non è WYSIWYS come JSON: c'è sempre una catena CSS/JS che separa sorgente e DOM renderizzato. Il modulo tecnico in HTML resta in discussione: l'utente considera passaggio a PDF + Apple Pencil su iPad, ma perderebbe il dato strutturato in uscita. **Decisione sospesa**: opzione C (HTML modulo strutturato preliminare + PDF ricevuta sul campo) merita sessione dedicata.

**Markdown — Documento discorsivo strutturato.** Centralità nelle AI è strutturale, non illusione di prospettiva. Tre ragioni: (1) lingua madre testuale delle AI (corpus training), (2) struttura dentro al testo per costruzione (polifonia implicita), (3) WYSIWYS leggibile anche da umani senza rendering.

**JSON — Sorgente di verità strutturata.** Posizione speciale rispetto agli altri tre: gli altri sono superfici di consumo, JSON è la sorgente. Trasparenza WYSIWYS strutturale (non illusione). Asimmetria pieno vs essenziale verificata empiricamente la settimana scorsa con la fattura 2600097 (riduzione 97% senza perdita informativa). Polifonia con disciplina.

### 3. Esplicitazione del pattern wiki LLM-maintained

L'utente ha condiviso il documento `llm-wiki.txt` (pattern descritto da Andrej Karpathy). Il riconoscimento centrale: **l'utente sta già costruendo un wiki LLM-maintained senza chiamarlo così**.

- Sorgenti raw: chat AI, AudioPen, sessioni Code
- Wiki layer: riepiloghi `.md` su contesto-ai
- Schema: `ISTRUZIONI_RIEPILOGO_E_NAMING.md`
- Disciplina ingest: riepiloghi a fine sessione, propagazione via Cowork+GitHub
- Indice: `index.json`

Pezzi mancanti nel sistema attuale rispetto al pattern completo:
- Cross-referencing esplicito tra documenti (wiki-style `[[link]]`)
- Pagine di entità persistenti (Vincenzo Strazzullo, BRUTIUM SRL, AudioPen, ecc.)
- Operazione di lint periodica formale
- Filing delle risposte come pagine wiki (le conversazioni con Claude muoiono nella chat)

Insight strategico: il **Cap. IV manifest pattern + wiki LLM-maintained + markdown = stesso lavoro**. Costruire `/sintesi/latest` non è solo "risolvere problema cache", è dare al wiki LLM-maintained l'identità stabile dei documenti, con il markdown come formato e content negotiation per rendering plurale.

### 4. Insight notturno delle 4:50 — archivio storico operativo in markdown

Alle 4:50 di sabato l'utente ha allegato due esempi (Storico Manutenzioni FAEMA X20 in PDF, Riepilogo Cowork BPER Anticipi in .txt) ponendo la domanda: "il formato finale per documenti come questi potrebbe essere markdown invece di PDF?"

La risposta: **sì, markdown è la scelta giusta per documenti derivati operativi/analitici/storici**. Regola formulata:

> PDF per documenti canonici fiscali/contrattuali. Markdown per documenti derivati operativi/analitici/storici. PDF generato all'occorrenza dai markdown quando serve consegnabile specifico, non come destinazione primaria.

Insight strategico più forte: **ricostruire l'archivio storico in markdown trasforma l'archivio da materiale morto a materia viva del sistema cognitivo aziendale**. Lo Storico FAEMA X20 in markdown diventa simultaneamente: documento operativo per il tecnico, pagina wiki per il LLM, fonte di analisi trasversali, base per arricchimenti futuri. In PDF queste capability sono inaccessibili senza ri-estrazione.

### 5. Riformulazione dell'ipotesi alle 6:50 — versione radicale

L'utente ha posto un'ipotesi più radicale: **non solo documenti derivati, ma anche documenti SAP storici di base (offerte, ordini, DDT, eccetto fatture) ricostruiti in markdown via Service Layer**. Argomento articolato in 6 punti:

1. Per documenti nuovi (post checkbox), il PDF nasce automaticamente
2. Per documenti storici pre-checkbox, ricostruire in PDF richiede passaggi manuali dal client (proibitivo)
3. Ricostruire in markdown via Service Layer è batch automatico, eseguibile in una sessione
4. L'archivio markdown alimenta simultaneamente consultazione operativa e wiki LLM
5. Il valore legale è già coperto da XML SDI + PDF originali (per quelli che esistono)
6. PDF on-demand resta sempre disponibile (rigenerazione da SAP)

**Posizione raffinata insieme**:

| Tipo documento | Formato consigliato | Note |
|---|---|---|
| Offerte, ordini, DDT, ordini di acquisto | Markdown (archivio storico) | DDT di servizio è il caso più forte |
| Fatture (emesse e ricevute) | XML SDI (già canonico per legge) | Markdown ridondante |
| Bolle fornitore | PDF (formato non controllabile dal fornitore) | Archivio passivo |
| Documenti derivati (storici macchina, riepiloghi) | Markdown nativo | Nuova categoria che oggi non esiste |
| Documenti consegnabili occasionali | PDF on-demand | Generato da markdown via renderer |

**Decisione operativa**: progetto a sé, non del weekend. Sedimentazione di 2-3 settimane per verifica, poi avvio progetto dedicato per la pipeline di ricostruzione.

### 6. Test empirico AssoInvoice come prova di concetto

L'utente ha condiviso trascrizione di video presentazione di AssoInvoice, il tool AssoSoftware per la lettura di fatture elettroniche XML SDI. **AssoInvoice è esempio in produzione del pattern "sorgente strutturato + rendering on-demand"** che si applicherà ai documenti markdown:

- Sorgente: file XML (illeggibile raw, è la fattura ai fini fiscali)
- Renderer: AssoInvoice scarica XSLT da AssoSoftware e lo applica al volo
- Tre layout selezionabili (semplificata, completa, ministeriale) sulla stessa fattura
- Foglio di stile riusabile da altri software

Mappa del parallelo:

| Caso fattura XML SDI | Caso archivio markdown futuro |
|---|---|
| Sorgente XML sul disco | Sorgente .md su Dropbox/GitHub |
| Renderer AssoInvoice desktop | Renderer browser/app mobile/FastAPI |
| XSLT scaricato da AssoSoftware | CSS + template Pandoc centralizzato |
| Tre viste della stessa fattura | Scheda mobile, tabella desktop, PDF on-demand |

**Conseguenza importante**: il pattern del "rendering centralizzato versionato" significa che documenti vecchi possono ricevere nuovo styling al successivo refresh, senza riprocessamento. Il PDF ha lo stile cotto dentro per costruzione, il markdown no. È una proprietà che importa quando hai 50.000 documenti storici.

### 7. Risposta alla domanda strutturale "JSON come sorgente invece di markdown?"

L'utente ha posto la domanda. Risposta: tecnicamente possibile ma sub-ottimale, perché perderebbe due cose: leggibilità raw del sorgente, e adatabilità al wiki LLM-maintained (markdown è scrivibile dall'AI come prosa fluente, JSON come oggetto strutturato).

**Pattern raccomandato**: markdown con frontmatter YAML. Una sola sorgente, due strati:
- Frontmatter YAML = JSON in formato leggibile (metadati strutturati, ricercabili)
- Body markdown = prosa narrativa + tabelle per dati strutturati

Pattern de facto standard di Obsidian, Hugo, Jekyll, MkDocs. Si allinea, non sperimenta.

### 8. Delta v1.3 → v1.5 della Sintesi

Estratto e ricostruito il delta tra le due versioni (9 giorni, 11-14 maggio). Salti principali:

- **Cantieri completati**: `archivio_documenti_sap.py` v2 in produzione, DA riattivato, `registra_fattura.py` operativo, struttura Hub a 3 cartelle, decisione FastAPI/portale
- **Capability nuove**: Claude Code mobile (Opus 4.7 1M), propagazione gerarchica nativa SAP, campo `Stato documento` enumerato
- **Principi**: 35 nuovi consolidati in 9 giorni (notevole)
- **Programma weekend strutturato in 4 cantieri**: PDF (Cap. I), interfaccia Service Layer (Cap. II), voci operative (Cap. III), manifest pattern (Cap. IV)

**Discrepanza notata**: il manifest pattern in v1.5 è ancora 🟡 media priorità ("priorità ridotta dopo Claude Code mobile"), mentre il PROGRAMMA WEEKEND del 14 sera (aggiornato 15 mattina) lo ha rivalutato come Cap. IV centrale. Il programma weekend è già evoluzione della v1.5.

### 9. Cap. I del weekend — Test Problema 2 con Code

Brief preparato e consegnato a Code. Test eseguito con due fasi:

**Fase A (07:54-10:05)**: Code ha incontrato un muro apparente — tutti i POST al Service Layer restituivano 500 Internal Server Error con body vuoto. Ha sviluppato l'ipotesi sbagliata "SL POST broken globally". 

**Fase B (10:05-10:07)**: L'utente ha portato evidenza esterna (PATCH funzionante su attività 7159 via proxy.py). Diagnosi corretta: gotcha `Expect: 100-continue`. `.NET HttpWebRequest` e `Invoke-RestMethod` PowerShell mandano di default questo header sui POST con body; SAP B1 SL Apache non lo gestisce e risponde 500/empty. Workaround: `[System.Net.ServicePointManager]::Expect100Continue = $false`. Python `requests` e `urllib` non hanno il problema.

**Test completato (10:07-10:12)**:
- Offerta 260172 (DocEntry 10818) creata via SL con successo
- 0 PDF generati in `DOCUMENTI_SAP\` dopo creazione
- Verifica estesa ricorsiva su `HUB DOCUMENTALE\**`: 0 PDF generati ovunque
- Variante punto 6: apertura 260172 dal client + Update/Save → 0 PDF
- Variante aggiuntiva: Anteprima PDF esplicita → PDF generato immediatamente

**Esito B con caratterizzazione completa**:

| Canale | PDF auto in `DOCUMENTI_SAP\`? |
|---|---|
| Creazione ex-novo dal client desktop | SÌ (al primo save) |
| Apertura + Aggiorna/Save dal client (doc esistente) | NO |
| Anteprima PDF esplicita dal client | SÌ (immediato, file timestamped) |
| POST via SL `/Quotations` | NO |

**Modello mentale stabilito**: la checkbox è agganciata al **rendering del layout di stampa**, non al ciclo di vita del documento. Due eventi UI lo triggherano (primo save da client + anteprima esplicita).

### 10. Rilettura strategica delle 10:30 — collasso del cantiere PDF server-side

L'utente ha proposto rilettura: alla luce dell'ipotesi "archivio storico operativo in markdown via SL" emersa alle 4:50, il Problema 2 perde significato funzionale. Confermata l'analisi:

- Prima del test: Problema 2 = continuità funzionale (avere PDF se canale cambia)
- Dopo la rilettura: Problema 2 = capability on-demand (PDF generato quando serve consegnabile specifico)

**Conseguenze**:

1. Il "buco PDF" non è più un buco — è documento canonico in altro formato (markdown)
2. Il "report monitoraggio completezza archivio" dovrebbe sorvegliare coerenza markdown ↔ SAP, non presenza PDF
3. Lo storico nasce coerente per costruzione, senza dipendere dal cantiere PDF server-side
4. **Cantiere PDF server-side declassificato**: da 🟡 con scadenza implicita a 🟡 custode strategico lungo periodo

**Ricaduta sul Cap. IV**: il manifest pattern non è più solo "fix cache + memoria dinamica", ma "**prototipo dell'infrastruttura sorgente+renderer che servirà tutto l'archivio storico futuro**". Il valore architetturale è molto più alto di quanto il programma weekend nomini esplicitamente. Implicazione: nella scelta tra "sistema singolo `/sintesi/latest`" e "sistema generico fin da subito", **il sistema generico è la scelta naturale perché il caso d'uso futuro più grande è l'archivio storico**.

---

## Decisioni prese

1. **Mappa dei 4 formati come principio architetturale** (candidato per Sintesi v1.6): ogni formato occupa una nicchia funzionale specifica. PDF canonico+consegnabile, HTML trinità interfaccia+documento+dato in casi specifici, markdown documento discorsivo, JSON sorgente strutturata. La maturità sta nel comporre, non nello scegliere.

2. **Markdown come formato dell'archivio storico operativo**: non sostituisce PDF SAP attivi, crea categoria nuova ("archivio consultabile sintetico") che oggi non esiste. PDF resta sempre disponibile come capability on-demand.

3. **Pattern markdown + frontmatter YAML come standard de facto** per documenti polifonici dell'ecosistema. Una sorgente, tre strati (metadati strutturati, prosa narrativa, tabelle dati).

4. **Pattern wiki LLM-maintained come nome esplicito di prassi già in corso**: i riepiloghi `.md` su contesto-ai *sono* già un wiki LLM-maintained in forma embrionale. Pezzi mancanti identificati per consolidamento futuro.

5. **Cap. I del weekend chiuso**: Esito B confermato con caratterizzazione completa. Gotcha `Expect: 100-continue` documentato. Decisione Strazzullo definitiva (tool declinato, calcolo si chiude ulteriormente: il tool non avrebbe risolto Problema 2 nemmeno se acquistato).

6. **Cantiere PDF server-side declassificato da 🟡 con scadenza implicita a 🟡 custode strategico di lungo periodo**, attivabile solo se emergono casi d'uso specifici che non esistono nel workflow attuale.

7. **Ricostruzione archivio storico in markdown**: progetto a sé, fuori dal weekend. Sedimentazione di 2-3 settimane per verifica, poi avvio progetto dedicato.

8. **Cap. IV ridefinito**: non più solo "fix cache + memoria dinamica", ma "prototipo infrastrutturale sorgente+renderer che servirà tutto l'archivio storico futuro". Implica scelta naturale per sistema generico fin da subito.

9. **Decisione modulo tecnico HTML vs PDF+Apple Pencil**: sospesa per sessione dedicata. Valutare opzione C (HTML strutturato preliminare + PDF ricevuta sul campo).

10. **Comunicazione conclusiva a Vincenzo Strazzullo**: bozza preparata. Tono: chiusura del dossier tool, conferma orientamento sul punto 4 (framework intermedio FastAPI) come terreno comune.

---

## Prossimi passi

### Immediati (oggi pomeriggio, dopo pausa caffè)

1. **Cap. IV manifest pattern** — entrare nel cantiere con cornice rinnovata. Prima domanda di design: redirect 302 vs proxy interno. Le altre tre (dove vive il manifest, granularità, transizione prompt sistema) dipendono parzialmente dalla prima.

2. **Ricordare nuova cornice strategica**: il Cap. IV serve 1 documento (la Sintesi) oggi, ma il pattern serve potenzialmente migliaia di documenti dell'archivio storico futuro. Sistema generico fin da subito.

### Weekend (domani)

3. **Cap. II endpoint `/attivita`** — costruisce sull'infrastruttura FastAPI del Cap. IV. Domenica mattina.

4. **Cap. III voci operative** — estensioni Service Layer, Directory Analyzer, credenziali Google n8n. Domenica pomeriggio.

5. **Aggiornamento Sintesi a v1.6 a fine weekend**: risultati cantieri, nuovi principi consolidati, riformulazione open items.

### Comunicazione esterna

6. **Email conclusiva a Vincenzo Strazzullo**: chiusura tool, conferma collaborazione sul punto 4 della sua email di aprile.

### Sedimentazione e progetti futuri

7. **Progetto "ricostruzione archivio storico in markdown"**: avvio dopo 2-3 settimane di sedimentazione. Richiede definizione template markdown canonico, script estrazione SL, validazione su campione, esecuzione batch.

8. **Decisione modulo tecnico HTML vs PDF**: sessione dedicata, non urgente.

9. **Consolidamento pattern wiki LLM-maintained**: dopo Cap. IV completato. Aggiungere cross-referencing esplicito tra riepiloghi, pagine entità persistenti, operazione lint periodica, filing risposte come pagine wiki.

### Aggiornamenti Sintesi v1.6 (consolidamento principi nuovi emersi)

10. **Principio "limite del formato vs limite del canale"** (scoperta 16/05 mattina su `.md` Dropbox vs Drive)
11. **Principio "wiki LLM-maintained come architettura della memoria semantica aziendale"** (16/05 sera, da llm-wiki.txt)
12. **Principio "archivio storico operativo come categoria di documento che oggi non esiste"** (16/05 alba)
13. **Caratterizzazione completa checkbox "Esportare in PDF"** (16/05 mattina, dopo test Code)
14. **Gotcha `Expect: 100-continue`** come pattern di trappola infrastrutturale per integrazioni SL da .NET/PowerShell
15. **Riconoscimento retrospettivo: tool Strazzullo non avrebbe risolto Problema 2** (importante per chiudere dossier con lucidità)
16. **Mappa dei 4 formati architetturali** come principio composito (PDF canonico, HTML trinità, markdown derivato, JSON sorgente)
17. **Pattern "rendering centralizzato versionato"** (lezione AssoInvoice: il foglio di stile evolve, i documenti restano, lo stile non è cotto dentro)
18. **Collasso del cantiere PDF server-side per evoluzione del contesto** — caso di studio di come un'evoluzione architetturale più alta può svuotare un cantiere precedente del suo significato funzionale

---

## Blocchi o dipendenze

- **Gotcha `Expect: 100-continue`**: gestito, ma resta vincolo trasversale per qualunque integrazione SL da PowerShell/.NET/C#. Necessario applicare workaround sempre.

- **Riferimento al "Crystal Reports server-side" nel programma weekend**: va riformulato. L'utente non usa Crystal Reports, usa il modulo nativo SAP per layout di stampa. Il sotto-cantiere 5 del Cap. I (ora obsoleto per via del collasso del cantiere PDF server-side) andrebbe riformulato in termini di `PrintPreferencesService` se mai si attiverà.

- **Manifest pattern in v1.5 ancora 🟡 media**: discrepanza con programma weekend del 15 mattina che lo ha rivalutato a Cap. IV centrale. Da allineare nella v1.6.

- **Decisione modulo tecnico HTML vs PDF+Apple Pencil**: sospesa, non blocca il weekend ma resta nodo aperto.

---

## Considerazioni strategiche emerse

### Il pattern del "collasso di cantiere per evoluzione del contesto"

Stamattina è successa una cosa rara e preziosa: un cantiere strategico (PDF server-side) che era stato preparato con cura per il weekend, una volta eseguito il test empirico, è stato declassificato non perché fallito, ma perché un'evoluzione architetturale più alta (archivio storico in markdown) ne aveva svuotato il significato funzionale. Vale la pena nominare questo pattern perché probabilmente si ripeterà: man mano che l'architettura matura, alcuni cantieri previsti perderanno significato perché problemi più alti li avranno assorbiti. Riconoscerlo precocemente evita di spendere energia su cantieri vestigiali.

### La forza del test empirico anche quando "conferma il sospetto"

L'Esito B era largamente atteso. Eppure il test non è stato inutile, anzi: ha prodotto (1) la caratterizzazione completa dei 4 canali, (2) la scoperta del gotcha Expect: 100-continue, (3) il modello mentale "checkbox agganciata al rendering del layout di stampa, non al ciclo di vita del documento". Tre informazioni che senza il test sarebbero rimaste tacite. Il valore del test empirico non è "scoprire l'inatteso", è "rendere esplicita la struttura del fenomeno".

### Il pattern AI + umano con conoscenza tacita

Code è andato sul muro `Expect: 100-continue` per due ore prima che l'utente portasse evidenza esterna (PATCH su attività 7159) che ha sbloccato la diagnosi. È il pattern "AI può elaborare solo i campi che le vengono indicati. La conoscenza di quali campi esistono è dell'operatore umano" già presente nei principi consolidati. L'AI ha costruito un'ipotesi plausibile e completamente sbagliata ("SL POST broken globally") che si autorafforzava con i dati. L'unico modo per uscirne era l'iniezione di evidenza esterna dall'umano. Pattern da ricordare.

### Il valore di nominare prassi già in corso

Il pattern wiki LLM-maintained che si è chiarito con il documento di Karpathy non è "nuovo strumento da introdurre". È "nome esplicito di prassi già in corso da settimane senza che la chiamassi così". Nominare le prassi tacite le rende ottimizzabili. È un pattern di consolidamento metacognitivo che vale la pena praticare consapevolmente — chiedersi periodicamente "questa cosa che sto facendo bene ha già un nome?".

### Il senso di chiusura del giro sui formati

L'utente è arrivato alla fine della discussione sui 4 formati con un quadro molto più nitido di quello iniziale. Non perché abbia imparato cose nuove, ma perché ha *visto le connessioni* tra cose che già sapeva. È un risultato cognitivo importante: il sapere si consolida quando le relazioni tra i pezzi si rendono esplicite. La sessione ha funzionato come operazione di consolidamento, non di apprendimento.

---

## FILE PATHS PRESERVATI

Per la nuova chat che aprirai dopo la pausa, questi sono i file rilevanti:

- `/mnt/user-data/uploads/SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO_14_05_2026.md` (v1.5, 457 righe)
- `/mnt/user-data/uploads/PROGRAMMA_WEEKEND_16_17_MAGGIO_2026.md` (237 righe)
- `/mnt/user-data/uploads/MEMORIA_E_CONTESTO_AI_RIEPILOGO_09_05_2026.md` (243 righe, sessione precedente)
- `/mnt/user-data/uploads/llm-wiki.txt` (pattern Karpathy)
- `/mnt/user-data/uploads/Storico_Manutenzioni_FAEMA_X20_Hotel_Pisacane.pdf` (esempio documento derivato)
- `/mnt/user-data/uploads/RIEPILOGO_COWORK_DEL_28_04_2026.txt` (esempio analisi BPER anticipi)
- `/mnt/user-data/uploads/TEST_PDF_SL_2026-05-16.md` (riepilogo Code, Esito B test Problema 2)

URL canonici dell'ecosistema da fetchare in nuova chat:

- `https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md` (Sintesi corrente su GitHub raw — al momento v1.3, v1.5 non ancora pushata)
- `https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/index.json` (indice navigabile)

---

*Sessione iniziata venerdì 15 maggio 2026 ore 22:18, proseguita sabato 16 maggio dalle 4:50 alle 10:42.*
*Tema centrale: panoramica sui 4 formati architetturali, introduzione pattern wiki LLM-maintained, chiusura Cap. I weekend.*
*Continua da: MEMORIA E CONTESTO AI RIEPILOGO 09_05_2026.md*
*Prossima sessione (oggi pomeriggio): Cap. IV manifest pattern via FastAPI con cornice strategica rinnovata.*
