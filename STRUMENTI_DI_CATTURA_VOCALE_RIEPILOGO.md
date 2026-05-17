# STRUMENTI DI CATTURA VOCALE — Apertura fase post-PLAUD e prima verifica end-to-end

## Nota di esplorazione in corso — 10 maggio 2026

---

## Contesto

L'ultima nota di esplorazione del 24 aprile aveva fissato l'apertura della fase PLAUD — acquisto e attivazione del dispositivo, ampliamento del perimetro di osservazione, registro modificato da "mappatura strumentale" a "esplorazione libera di un fenomeno in corso". Nel ponte 24 aprile → 10 maggio si sono accumulati 16 giorni di sperimentazione intensa che hanno fatto emergere fenomeni non previsti dalla nota precedente, modificato la mappa degli strumenti, e prodotto il primo evento concreto di chiusura di una pipeline vocale end-to-end nel sistema Viceconti.

In questi 16 giorni il progetto è entrato in una fase qualitativamente diversa. Non è più l'esplorazione di "un nuovo strumento" (PLAUD) — è sperimentazione su **un campo allargato** che include almeno otto modalità distinte di cattura/restituzione vocale, configurazioni di workflow inedite, prima verifica empirica di principi che il 24 aprile erano ipotesi.

La frase che apre questa fase potrebbe essere — letta retrospettivamente — quella che Prospero ha pronunciato il 4 maggio sera dopo il primo test della pipeline voce → SAP → AI: *"sembra essere chiuso il primo loop"*. Non è una formulazione conclusiva — è il riconoscimento che qualcosa che era teoria è diventato osservabile.

---

## Sintesi della fase: 16 giorni di sperimentazione intensa

I fatti principali, in ordine cronologico:

**27-28 aprile** — Prima fase intensa di uso PLAUD nell'attività ordinaria. Il primo trigger reale d'uso è stato accidentale (cellulare scarico, viaggio in macchina di un'ora e mezza). Sensazione immediata di "soglia di attrito attraversata in 30 minuti" — quello che con AudioPen aveva richiesto settimane. Adozione del template "Trascrizione integrale (per uso esterno)" del Dott. Michele Morgese come default per fedeltà al parlato. Coesistenza spontanea di PLAUD (registrazioni lunghe, dialoghi) e AudioPen (monologhi brevi rapidi) senza sovrapposizione.

**28 aprile** — Prima cattura PLAUD di valore strategico: telefonata di chiarimento tecnico con fornitore SAP sulla questione locale-vs-cloud che la sera prima era pensiero solitario stanco di Prospero. Materiale catturato e elaborato in due artefatti distinti (trascrizione integrale + verbale strutturato con action items). Osservazione di campo: rischio reale di **sottovalutare la cattura** — la curva di adozione di uno strumento di cattura è più rapida della curva di valorizzazione del catturato.

**28 aprile** — Scoperta della dettatura system-wide nativa Win+H su Windows 11 (e Fn-Fn su macOS) come modalità non ancora nominata nella mappa. Caso reale immediato: messaggio di 600 parole strategico su SAP a tarda sera — non più capture frammentaria, ma **pensiero ad alta voce esteso direttamente nel campo di destinazione**. Reset del modello: la dettatura nativa non è solo per "email rapide", regge anche pensiero strategico complesso.

**29 aprile circa** — Installazione di Wispr Flow. Test su email modello. Output strutturato (elenchi numerati, paragrafi puliti, custom vocabulary che riconosce nomi propri tecnici). **Sesta modalità**: voce → testo già formattato nel campo.

**1 maggio weekend** — Cartelle AudioPen tematiche come modalità di organizzazione leggera. La cartella "Note 1 Maggio" come contenitore-a-tempo di pensieri di pianificazione del weekend. Pattern emergente: organizzazione episodica vs tassonomica.

**Tra 30 aprile e 4 maggio** — AudioPen rilascia un aggiornamento sostanziale che cade tre limiti che erano stati incorporati nella mappa: (a) il limite dei 15 minuti viene rotto — note concatenabili senza tetto pratico; (b) la trascrizione grezza diventa esposta accanto alla nota elaborata; (c) la SuperSummary diventa personalizzabile con prompt. **La mappa del 24 aprile invecchia in tre giorni.** Verifica empirica: i limiti AI sono datati, non strutturali.

**4 maggio sera** — Primo test end-to-end della pipeline **voce → AudioPen → SAP → Service Layer → JSON → Claude FastAPI**. Sette passaggi, ancora manuali al 100%, ma il messaggio test è arrivato intatto dall'altro capo. **Verifica sperimentale dell'ipotesi 1** della nota del 24 aprile (la voce come materiale che alimenta gli output gestionali). Nello stesso test, scoperta del caso **U_Esito**: dentro un campo SAP esisteva l'istruzione "PER COWORK: CREARE CARTELLA IN HUB DOCUMENTALE..." — istruzione prescrittiva destinata a un agente non umano, scritta mesi prima che esistesse il canale per eseguirla. Concetto emergente: **i campi narrativi accumulano latenza operativa**.

**5 maggio circa** — Arrivo del tasto TTS nativo dentro Claude (icona play accanto a copia/esporta). Tre tap diventano uno per la lettura ad alta voce delle risposte. Annunciato come feature mancante il giorno prima al feedback Anthropic, arriva il giorno dopo.

**Tra 6 e 10 maggio** — Sessione su MacBook Air in postazione stabile rivela un dato contro-corrente: la tastiera scissor del MacBook Air produce fluidità *quasi indistinguibile* dalla cattura vocale. Calibrazione metodologica: la voce non è strutturalmente superiore in assoluto — vince in contesti specifici (mobilità, fatica, mani occupate, vocabolario tecnico).

**10 maggio** — Primo aggiornamento di Contesto AI dopo la fase intensa.

---

## La mappa aggiornata — non più 4 quadranti

La nota del 24 aprile organizzava gli strumenti in una matrice 2×2 (sincrono/asincrono × monologo/dialogo). Quella matrice è ora insufficiente. Lo spazio degli strumenti è cresciuto su almeno tre assi indipendenti che la matrice non rappresentava:

**Asse 1 — Direzione del flusso**
- Voce → testo (AudioPen, PLAUD, microfono Claude, dettatura nativa, Wispr Flow)
- Testo → voce (TTS Claude nativo, Leggi ad alta voce iOS)
- Voce → voce (Voice Mode Claude, Gemini Live)

**Asse 2 — Grado di elaborazione AI**
- Trascrizione fedele (template Dott. Morgese di PLAUD, dettatura nativa)
- Polish leggero (AudioPen rewrite low)
- Formattazione strutturata (Wispr Flow, template Riepilogo PLAUD, SuperSummary AudioPen con prompt)
- Conversazione bidirezionale (Voice Mode, Gemini Live)

**Asse 3 — Personalizzazione lessicale**
- Nessun custom vocabulary (microfono Claude, dettatura nativa, Win+H)
- Custom vocabulary alimentabile (AudioPen, PLAUD, Wispr Flow)

Le otto modalità che si sono finora differenziate, ciascuna con il proprio dominio funzionale stabile:

1. **AudioPen** — pensiero strutturato breve, monologo personale, accumulazione per cartelle tematiche, frontend vocale verso Claude per messaggi con nomi propri
2. **PLAUD Note Pro** — interlocuzioni dialogiche, telefonate strategiche, materiale lungo da elaborare a posteriori
3. **Microfono nativo Claude** — dialogo conversazionale immediato dentro la chat, gesto a basso attrito
4. **Wispr Flow** — voce → testo formattato direttamente nel campo, ottimo per email/messaggi strutturati
5. **Dettatura nativa Win+H** (Windows) e **Fn-Fn** (macOS) — pensiero scritto in tempo reale verso un destinatario qualunque, system-wide
6. **Voice Mode Claude** — dialogo bidirezionale automatico (per ora solo inglese)
7. **Gemini Live** — dialogo bidirezionale italiano in mobilità, riempie il vuoto Voice Mode
8. **TTS Claude nativo + Leggi ad alta voce iOS** — fruizione audio delle risposte AI in italiano

Sono otto e probabilmente non finite. La nota del 24 aprile aveva flaggato come orizzonte gli strumenti collettivi (Plaud NotePin S per il campo); quell'orizzonte resta aperto e non è ancora entrato in scena.

---

## Formulazioni che si sono consolidate

**1. La soglia di attrito percepito ha curva esponenziale, non lineare.** Il primo strumento di una categoria costa settimane di apprendimento. Il secondo della stessa categoria costa minuti. Verificata empiricamente: PLAUD ha attraversato la soglia in 30 minuti d'uso intensivo perché AudioPen aveva preparato il terreno cognitivo. Generalizzabile: un'AI come exocortex per chi è alle prime armi è una cosa diversa dalla stessa AI per chi è già allenato.

**2. I limiti percepiti degli strumenti AI sono datati, non strutturali.** AudioPen ha rotto il limite dei 15 minuti tre giorni dopo che la mappa del 24 aprile l'aveva incorporato come dato. La postura giusta non è ottimizzare attorno ai limiti attuali — è abituarsi a rimettere in discussione la mappa periodicamente. **Ricognizione di obsolescenza** come pratica del Lab: ogni qualche mese, verificare per ciascuno strumento se i limiti che gli avevamo assegnato sono ancora veri.

**3. Il numero di strumenti conta meno della funzione cognitiva di ciascuno.** L'introduzione di uno strumento in più (AudioPen tra microfono Claude e l'invio del messaggio) non aumenta l'attrito se ognuno occupa un regime cognitivo distinto. Tre strumenti coerenti col loro gesto producono meno attrito di uno strumento che deve fare tre cose. Confermato dall'osservazione che la coesistenza AudioPen/PLAUD/Win+H/microfono Claude è naturale e senza conflitti.

**4. AudioPen → Claude come pattern stabile per output multipli.** Lo stesso gesto vocale di input (AudioPen) può produrre output diversi a seconda del contenuto: promemoria → Reminders, email → bozza Gmail, messaggio operativo → WhatsApp, briefing → Antonio, nota → archivio Lab. Claude come **nodo di smistamento**, non come destinazione finale.

**5. Il microfono Claude ha un dominio specifico: dialogo conversazionale.** Non è in competizione con AudioPen e PLAUD, occupa il regime live-conversazionale (gesto immediato, contesto già aperto). Il limite tecnico (no custom vocabulary) lo rende meno adatto quando i nomi propri tecnici pesano nel messaggio. La scelta tra microfono Claude e AudioPen quando ci si rivolge all'AI dipende dalla **densità di nomi propri tecnici** nel messaggio.

**6. La voce non sostituisce la tastiera — la sostituisce in contesti specifici.** Mobilità, fatica, mani occupate, pensiero esplorativo libero, oggetti tecnici con nomi propri. In altri contesti (postazione stabile, tastiera buona, pensiero strutturato consapevole), la tastiera può essere uguale o migliore. È stata una calibrazione metodologica importante: l'assunto implicito "voce > tastiera" che permeava la mappa è stato rivisto.

---

## Il riconoscimento del 4 maggio: la pipeline voce → SAP è verificata

Vale la pena nominarlo come evento separato, perché non è solo un test riuscito — è la **verifica sperimentale** dell'ipotesi 1 del 24 aprile (la voce come materiale di contesto che alimenta gli output gestionali) e la **convergenza con il progetto FastAPI** sul punto strutturale comune.

Il loop verificato:

```
voce (AudioPen, computer Windows)
  → trascrizione automatica nella cartella FastAPI di AudioPen
  → copia-incolla manuale nel campo Notes di un'attività SAP
  → sincronizzazione SAP → Service Layer
  → interfaccia Claude Service Layer (PC Lauria)
  → pulsante "Esporta JSON" 
  → upload del file in chat Claude FastAPI
  → lettura del messaggio
```

Sette passaggi, niente di automatico nel suo insieme. Il principio funziona, ma il workflow è grezzo. Le ottimizzazioni vengono dopo.

L'evento ha aperto due osservazioni che valgono come materiale concettuale per il progetto:

**Caso U_Esito.** Dentro un campo SAP è stata trovata l'istruzione *"PER COWORK: CREARE CARTELLA IN HUB DOCUMENTALE..."* — depositata mesi prima che esistesse il canale per eseguirla. Concetto: i campi narrativi possono contenere **istruzioni operative latenti** destinate ad agenti non umani, scritte in anticipazione. Quando il canale di esecuzione arriva, lo stock viene riconosciuto in massa. Implicazione per la cattura vocale: ciò che si registra oggi a voce può diventare attivabile retroattivamente domani — questo richiede una **postura di scrittura consapevole**.

**Distinzione descrittivo vs prescrittivo.** I campi narrativi non sono un blocco unico. Possono contenere descrizione (per consumer semantici che leggono) o prescrizione (per agenti che eseguono). Distinguere i due tipi al momento della cattura, anche solo con prefissi convenzionali ("PER COWORK:", "PER AI:"), è una pratica di scrittura che si annuncia.

---

## Apertura concettuale: il corpo come canale

Una traccia che era stata aperta il 24 aprile come "qualcosa che non riesce ancora a essere nominato" e che si è approfondita in questa fase. Non si chiude — si articola.

La voce e la tastiera sono entrambi canali dal mentale all'esterno, ma di natura diversa. La tastiera richiede traduzione (pensiero → segnale motorio → caratteri). La voce è continuazione (il pensiero è già verbale internamente, la vocalizzazione lo continua). Il salto cognitivo è minore per la voce, perché il pensiero è già linguistico. Ma c'è qualcosa di più — la voce mobilita il **respiro**, il **timing fonatorio**, ed è il primo canale di comunicazione umana, biologicamente costitutivo. Quando si parla si mobilita un canale che il corpo conosce come costitutivo, non come strumento.

L'osservazione di Prospero — chitarrista — sulla simmetria tra mano-su-chitarra e mano-su-tastiera-computer apre un'altra finestra. La memoria procedurale digitale è la stessa categoria nei due casi, ma la chitarra ha continuità temporale (la nota dura nel tempo e si modifica) che la tastiera computer non ha (caratteri discreti). Lo strumento musicale e la tastiera computer non sono identici, anche se sono cugini.

Apertura per il Lab: **prestare attenzione alla memoria procedurale come fenomeno** — osservarla, allenarla deliberatamente in un dominio (typing), riflettere su cosa succede — può cambiare il modo in cui si abita la memoria procedurale anche nel dominio musicale che si pratica già. Non transfer di abilità, ma transfer di consapevolezza.

Questo è materiale teorico aperto, non chiuso. Va trattato come traccia in evoluzione.

---

## Apertura metodologica: l'esperimento dentro cui ci troviamo

Una formulazione prodotta da Prospero il 5 maggio circa, che vale la pena fissare come postulato di partenza del Lab più che come osservazione tra le altre:

*"Siamo tutti dentro un grande esperimento di psicologia cognitiva. Non solo noi attori utilizzatori sul campo, ma anche chi realizza queste nuove applicazioni."*

Non è retorica — è descrittiva. I ruoli classici della ricerca sono dissolti: i realizzatori non sanno con precisione cosa producono nelle teste degli utilizzatori, gli utilizzatori non sono soggetti passivi ma producono teoria operativa, gli AI sono insieme oggetto sotto osservazione e strumento dell'osservazione. Non c'è un fuori — non esiste un gruppo di controllo che fa le stesse cose senza AI per fare il confronto.

Implicazioni metodologiche per il progetto:
- L'osservazione di campo invecchia velocemente (vedi caso AudioPen).
- Le note vanno datate con cura — il documento del 24 aprile vale al 24 aprile, non in assoluto.
- La responsabilità epistemica richiede di registrare anche i fallimenti, le illusioni, i passaggi in cui un'aspettativa si rivela infondata.

---

## Postura di lavoro per la fase prossima

La fase di esplorazione "uso libero, raccolta materiale dal campo, poi analisi" che era stata replicata dal precedente metodologico AudioPen di gennaio-marzo ha funzionato. Il rischio della prossima fase è opposto: avere troppo materiale e voler organizzare prematuramente. Una postura calibrata:

**Continuare la cattura, senza sopra-organizzare.** Lasciare che gli strumenti continuino a depositare materiale nei loro contenitori naturali (cartelle AudioPen, cloud PLAUD, file system, chat Claude). Risolvere l'archiviazione formale solo quando un caso d'uso reale lo richiederà.

**Tre direzioni concrete su cui c'è già materiale operativo:**

1. **Riduzione dell'attrito sulla pipeline voce → SAP.** Tre opzioni testabili nei prossimi giorni: Wispr Flow direttamente nei campi SAP, PLAUD AutoFlow → n8n → SAP, AudioPen → n8n via webhook (con il workflow del 19 aprile da risistemare).

2. **Convergenza col progetto FastAPI sui campi narrativi.** Gli ordini fornitore esposti via Service Layer hanno già campi semi-narrativi nel JSON. Una **quarta fonte di popolamento** dei campi narrativi (oltre a manuale selettivo, generazione AI in batch, estrazione da fonti esistenti) è la cattura vocale durante il lavoro reale. Materiale per quando il progetto FastAPI consoliderà la formulazione strategica.

3. **Distinzione descrittivo/prescrittivo nei campi narrativi.** Pratica di scrittura che vale la pena introdurre già adesso — anche solo come convenzione personale — perché ogni nota vocale che entra nei sistemi gestionali oggi può essere letta domani da agenti diversi con scopi diversi.

**Tre direzioni di apertura concettuale che restano in attesa:**

- Il corpo come canale (voce, respiro, memoria procedurale, simmetria chitarra-tastiera).
- L'AI agentica come fonte di paradossi (Cowork che risolve subito impedisce l'investimento strutturale che a regime sarebbe più efficiente).
- L'integrazione Claude-SAP come possibile vantaggio competitivo strategico per Viceconti.

Queste tre tracce sono materiale del Lab in senso stretto. Non chiedono soluzione operativa, chiedono di essere coltivate e riprese quando il pensiero matura.

---

## Cose che restano aperte

- **Voice Mode di Claude in italiano.** Promesso da Anthropic, non ancora rilasciato. Quando arriverà, modificherà il caso d'uso "macchina + voce + interlocuzione AI" che oggi è coperto da Gemini Live.
- **Custom vocabulary del microfono Claude.** Limite noto, fonte di errori sui nomi propri tecnici. Non c'è interfaccia per alimentarlo.
- **Workflow n8n di smistamento note AudioPen.** Interrotto dal 19 aprile. Da decidere se ripristinare o lasciare cadere — dipende da come si stabilizza il pattern AudioPen → Claude come nodo di smistamento.
- **Destinazione strutturata del materiale vocale accumulato.** Domanda aperta nel riepilogo del 24 aprile, ancora aperta. La risposta naturale (cartelle Dropbox dei singoli clienti come dossier) è stata nominata ma non implementata.
- **Sperimentazione con i corpi non-Prospero.** L'ipotesi 3 del 24 aprile resta in attesa del primo collaboratore o cliente che entri nel sistema vocale durante un'interazione reale.
- **Soluzione di lettura automatica delle risposte Claude in italiano.** Parzialmente risolta dall'arrivo del tasto TTS nativo, da verificare la qualità della voce in italiano nei prossimi giorni di uso.
- **NotePin S di PLAUD per uso campo con tecnici.** Flaggato come orizzonte il 24 aprile, non entrato in scena.

---

## Sequenza naturale (non piano)

I lavori che hanno bisogno di uso ripetuto per stabilizzarsi: la pratica della distinzione descrittivo/prescrittivo nei campi narrativi, l'uso di Wispr Flow su email reali con vocabolario aziendale, il TTS nativo Claude su risposte lunghe in mobilità. Sono cose che si fanno facendo.

I lavori che hanno bisogno di un'occasione concreta per essere testati: l'integrazione PLAUD AutoFlow → workflow di automazione, la prima sperimentazione con un corpo non-Prospero (collaboratore, cliente in sopralluogo), il primo caso reale di campo narrativo vocale che alimenta un'offerta retroattivamente.

I lavori concettuali che non hanno bisogno di niente — solo del tempo che occorre per maturare: il corpo come canale, il paradosso dell'AI agentica, il vantaggio competitivo dell'integrazione Claude-SAP, la simmetria tra memoria procedurale chitarra e tastiera.

---

*Nota di esplorazione in corso — 10 maggio 2026*
*Apertura fase post-PLAUD: 16 giorni di sperimentazione intensa, prima verifica end-to-end della pipeline voce → SAP, mappa estesa a 8 modalità, riconoscimento dei limiti AI come datati e non strutturali, traccia del corpo come canale lasciata in evoluzione.*
*Continua da: STRUMENTI DI CATTURA VOCALE RIEPILOGO 24_04_2026.md*
*Prossimo aggiornamento: dopo riduzione dell'attrito sulla pipeline voce → SAP o emergenza di un nuovo fenomeno di campo significativo.*
