# CONVENZIONI DI NAMING — ECOSISTEMA VICECONTI
## Guida pratica per nominare file e cartelle

*Versione 1.0 — 25 Febbraio 2026*

---

## Perché questo documento

Ogni file e ogni cartella che creiamo in Viceconti ha due lettori: una persona e una macchina.

La persona è il tecnico che cerca un esploso dal telefono in cantiere, l'amministrativo che archivia una fattura, il collega che deve trovare un documento al volo. La macchina è il sistema che indicizza, cerca, collega e — sempre di più — risponde alle nostre domande.

Se un file si chiama `doc1.pdf`, la persona non sa cosa contiene senza aprirlo e la macchina non può aiutare nessuno a trovarlo. Se lo stesso file si chiama `ESPLOSO RICAMBI FORNO RATIONAL SCC 201.pdf`, la persona capisce a colpo d'occhio e la macchina lo trova quando qualcuno chiede "mi serve l'esploso del forno Rational".

**Il nome è l'interfaccia.** È il primo e più importante punto di contatto tra noi e i nostri strumenti digitali. Nominare bene non è un compito in più — è il modo in cui rendiamo il nostro lavoro trovabile, condivisibile e duraturo.

Questo documento contiene regole semplici. Non sono complicate, non richiedono competenze tecniche, e una volta assimilate diventano automatiche. L'obiettivo è che tra qualche mese, nominare correttamente un file sia naturale come etichettare un raccoglitore in archivio.

---

## Il principio guida

> **Nomina come parleresti a un collega che non conosce il contesto.**

Se un collega ti chiedesse "cos'è questo file?", la tua risposta è il nome che il file dovrebbe avere.

Esempio: "È l'esploso ricambi dell'abbattitore Coldline che abbiamo installato da Happy Moments a Lauria."

Ecco, il percorso in Dropbox diventa:
```
CLIENTI / LAURIA / HAPPY MOMENTS / AREA TECNICA / ABBATTITORE COLDLINE / ESPLOSO RICAMBI.pdf
```

Ogni livello della cartella aggiunge un'informazione. Insieme, raccontano una storia completa senza bisogno di aprire il file.

---

## Le regole

### 1. TUTTO MAIUSCOLO

I nomi di cartelle e file usano **lettere maiuscole**.

```
✅  OFFERTA FORNO RATIONAL.pdf
✅  CLIENTI / LAURIA / HAPPY MOMENTS
❌  offerta forno rational.pdf
❌  Clienti / lauria / Happy Moments
❌  Offerta Forno Rational.pdf
```

**Perché:** elimina ogni ambiguità su maiuscole e minuscole. Non ci si deve mai chiedere "era maiuscolo o minuscolo?" — è sempre maiuscolo. I sistemi informatici trattano "Happy" e "happy" come cose diverse; con il tutto maiuscolo il problema non esiste.

---

### 2. SPAZI PER SEPARARE LE PAROLE

Le parole si separano con **spazi normali**. Niente trattini, underscore o altri separatori.

```
✅  HAPPY MOMENTS
✅  ESPLOSO RICAMBI FORNO RATIONAL.pdf
❌  HAPPY_MOMENTS
❌  HAPPY-MOMENTS
❌  HAPPYMOMENTS
```

**Perché:** lo spazio è il separatore più naturale e leggibile. I nostri sistemi (Dropbox, Hub Documentale, GitHub Pages) lo gestiscono correttamente.

**Eccezione — le date:** quando serve una data nel nome, usare il formato `GG_MM_AAAA` con underscore (vedi regola 7).

---

### 3. NIENTE APOSTROFI

Gli apostrofi sono **vietati** nei nomi di cartelle e file. Sostituirli riscrivendo il nome.

```
✅  FA TU
✅  PANIFICIO FA TU
✅  SIREV DI FAVATA
✅  MACELLERIA DELL AGNELLO
❌  FA' TU
❌  SIREV DI FAVATA'
❌  MACELLERIA DELL'AGNELLO
```

**Perché:** l'apostrofo crea conflitti tecnici nei link web, negli URL, e nel codice JavaScript. L'Hub Documentale usa i nomi delle cartelle come indirizzi web — un apostrofo nel nome rompe la navigazione. Rimuoverlo dal nome non crea ambiguità per chi legge.

---

### 4. NIENTE ACCENTI

Le lettere accentate sono **da evitare** nei nomi di cartelle. Sostituire con la lettera senza accento.

```
✅  MATERA
✅  ATTIVITA ASSISTENZA
✅  FESTIVITA 2026
❌  MATERÀ          (improbabile ma per chiarezza)
❌  ATTIVITÀ ASSISTENZA
❌  FESTIVITÀ 2026
```

**Perché:** gli accenti possono creare problemi di encoding tra sistemi diversi (Windows, Mac, web, API). Un file creato su un Mac con "à" potrebbe non essere trovato da uno script su Windows. Senza accenti, il nome è universale.

**Nota:** questa regola riguarda i nomi di file e cartelle, non il contenuto dei documenti. Dentro un PDF o un Word, le lettere accentate si usano normalmente.

---

### 5. NIENTE CARATTERI SPECIALI

Sono **vietati** nei nomi: `' " # & @ ! ? * : ; < > | \ / ( ) [ ] { }`

```
✅  ROSSI E FIGLI
✅  ORDINE N 12345
✅  FATTURA NUM 001-2026
❌  ROSSI & FIGLI
❌  ORDINE #12345
❌  FATTURA N° 001/2026
```

**Caratteri permessi:** lettere (A-Z), numeri (0-9), spazi, trattino (`-`), underscore (`_`), punto (`.` solo per le estensioni dei file).

**Perché:** i caratteri speciali hanno significati tecnici nei sistemi informatici. `&` è un operatore, `#` è un marcatore di indirizzo web, `/` è un separatore di cartelle. Usarli nei nomi crea ambiguità per le macchine.

---

### 6. NOMI DESCRITTIVI E COMPLETI

Il nome deve dire **cosa** contiene il file, non essere un codice criptico.

```
✅  ESPLOSO RICAMBI FORNO RATIONAL SCC 201.pdf
✅  MANUALE USO E MANUTENZIONE LAVASTOVIGLIE WINTERHALTER.pdf
✅  SCHEDA ASSISTENZA ABBATTITORE COLDLINE 10 TEGLIE.pdf
❌  doc1.pdf
❌  scan_20260225.pdf
❌  IMG_4521.jpg
❌  Copia di forno (2).pdf
```

**Per i file ricevuti dall'esterno** (scan, foto, allegati email): rinominare prima di archiviare. Bastano 10 secondi per trasformare `IMG_4521.jpg` in `FOTO DATI MACCHINA FORNO RATIONAL.jpg`.

**Perché:** un file trovato è un file usato. Un file con nome criptico è un file perso anche se tecnicamente esiste.

---

### 7. DATE NEL FORMATO GG_MM_AAAA

Quando un nome include una data, usare il formato giorno-mese-anno con underscore.

```
✅  FATTURA 001 DEL 15_01_2026.pdf
✅  RAPPORTO ASSISTENZA 22_02_2026.pdf
✅  RIEPILOGO ASSISTENZE AL 24_02_2026.xlsx
❌  FATTURA 001 DEL 15/01/2026.pdf      (la barra è vietata)
❌  FATTURA 001 DEL 15-01-2026.pdf      (meno leggibile)
❌  FATTURA 001 DEL 2026-01-15.pdf      (formato non familiare)
```

**Perché:** il formato GG_MM_AAAA è quello che usiamo quotidianamente in Italia. L'underscore evita la barra `/` (vietata nei nomi file) e il trattino `-` che potrebbe confondersi con un separatore di parole.

---

### 8. STRUTTURA DELLE CARTELLE: OGNI LIVELLO AGGIUNGE INFORMAZIONE

La gerarchia delle cartelle segue una logica dal generale al particolare. Ogni livello aggiunge un'informazione che non è nel livello precedente.

**Esempio per CLIENTI:**
```
CLIENTI / {COMUNE} / {NOME CLIENTE} / {AREA} / {ATTREZZATURA} / file
```

Dove:
- **COMUNE** → localizzazione geografica (LAURIA, MARATEA, TORTORA...)
- **NOME CLIENTE** → ragione sociale o nome riconoscibile
- **AREA** → tipo di documentazione (AREA TECNICA, AREA COMMERCIALE...)
- **ATTREZZATURA** → macchina specifica, con marca e modello

**Esempio concreto:**
```
CLIENTI / LAURIA / HAPPY MOMENTS / AREA TECNICA / ABBATTITORE COLDLINE 10 TEGLIE /
├── ESPLOSO RICAMBI.pdf
├── MANUALE USO.pdf
├── SCHEMA ELETTRICO.pdf
└── FOTO DATI MACCHINA.jpg
```

**Il nome della cartella attrezzatura è particolarmente importante.** Deve contenere: tipo di macchina + marca + modello (se noto). Questo è il "vocabolario" che permette di cercare trasversalmente — quando qualcuno chiede "tutti gli esplosi Coldline", il sistema cerca la parola "Coldline" nei nomi delle cartelle attrezzatura di tutti i clienti.

---

### 9. TIPI DI DOCUMENTO: USARE NOMI STANDARD

Per i file tecnici, usare sempre gli stessi nomi per lo stesso tipo di documento:

| Tipo di documento | Nome standard |
|-------------------|---------------|
| Esploso ricambi | `ESPLOSO RICAMBI.pdf` |
| Manuale d'uso | `MANUALE USO.pdf` oppure `MANUALE USO E MANUTENZIONE.pdf` |
| Schema elettrico | `SCHEMA ELETTRICO.pdf` |
| Scheda tecnica | `SCHEDA TECNICA.pdf` |
| Certificazione | `CERTIFICAZIONE.pdf` oppure `CERTIFICAZIONE CE.pdf` |
| Foto dati macchina | `FOTO DATI MACCHINA.jpg` |
| Scheda assistenza | `SCHEDA ASSISTENZA.pdf` |
| Rapporto intervento | `RAPPORTO ASSISTENZA GG_MM_AAAA.pdf` |
| Collaudo | `COLLAUDO.pdf` oppure `COLLAUDO GG_MM_AAAA.pdf` |
| Offerta | `OFFERTA.pdf` oppure `OFFERTA GG_MM_AAAA.pdf` |
| Fattura | `FATTURA NNN DEL GG_MM_AAAA.pdf` |
| Ordine | `ORDINE NNN DEL GG_MM_AAAA.pdf` |

**Se ci sono più documenti dello stesso tipo** per la stessa attrezzatura, aggiungere un dettaglio:
```
ESPLOSO RICAMBI GRUPPO MOTORE.pdf
ESPLOSO RICAMBI VASCA.pdf
MANUALE USO ITALIANO.pdf
MANUALE USO INGLESE.pdf
```

**Perché:** usare sempre le stesse parole per lo stesso concetto rende la ricerca prevedibile. Se un tecnico cerca "esploso", trova tutti gli esplosi. Se qualcuno li chiama "vista esplosa", "disegno ricambi", "spare parts" in modo casuale, la ricerca diventa una lotteria.

---

### 10. QUANDO SI RINOMINA UNA CARTELLA ESISTENTE

Se una cartella esistente viola queste regole (ha apostrofi, caratteri speciali, nomi criptici), va rinominata. Ma con attenzione:

1. **Rinominare in Dropbox** — il cambio si propaga automaticamente a tutti i dispositivi sincronizzati
2. **Avvisare** — se la cartella è condivisa o usata da altri, segnalare il cambio
3. **Dopo il cambio**, la prossima scansione notturna aggiorna automaticamente l'Hub Documentale
4. **I vecchi link** ai file dentro la cartella rinominata smetteranno di funzionare — la scansione ne genera di nuovi

**Non rinominare tutto in una volta.** Procedere per sezione o per cliente, verificando che l'Hub si aggiorni correttamente.

---

## Riepilogo visuale

```
┌─────────────────────────────────────────────────────┐
│                 REGOLE RAPIDE                        │
│                                                     │
│   ✅ TUTTO MAIUSCOLO                                │
│   ✅ SPAZI tra le parole                            │
│   ✅ NOMI DESCRITTIVI (cosa contiene il file)       │
│   ✅ DATE come GG_MM_AAAA                           │
│   ✅ NOMI STANDARD per i tipi di documento          │
│                                                     │
│   ❌ Apostrofi         FA' TU → FA TU               │
│   ❌ Accenti            non nei nomi file/cartelle   │
│   ❌ Caratteri speciali  & # @ ! ? * : / \ ecc.     │
│   ❌ Nomi criptici       doc1.pdf, IMG_4521.jpg     │
│   ❌ Underscore tra parole  HAPPY_MOMENTS            │
│                                                     │
│   Caratteri permessi: A-Z  0-9  spazio  -  _  .    │
└─────────────────────────────────────────────────────┘
```

---

## Domande frequenti

**"Devo rinominare tutti i file vecchi?"**
No, non subito. Le regole si applicano da oggi in poi per i nuovi file e cartelle. I file esistenti verranno allineati progressivamente, sezione per sezione, quando se ne presenta l'occasione.

**"Ho ricevuto un file con un nome illeggibile, cosa faccio?"**
Rinominalo prima di metterlo in Dropbox. 10 secondi di rinomina oggi risparmiano minuti di ricerca domani, per te e per tutti.

**"Come chiamo una cartella se non sono sicuro del nome?"**
Segui il principio guida: nomina come parleresti a un collega. Se il nome ha senso detto a voce, ha senso anche scritto.

**"Posso usare abbreviazioni?"**
Sì, ma solo se sono universalmente comprese in azienda. `SCC` per un modello Rational va bene (tutti sanno cosa sia). `RIC` per ricambi è ambiguo — meglio scrivere `RICAMBI` per intero.

**"I nomi troppo lunghi non sono un problema?"**
Un nome lungo ma chiaro è sempre meglio di un nome corto ma criptico. Windows supporta percorsi fino a 260 caratteri — nella pratica non è un limite reale.

---

## Nota finale

Queste regole non sono un vincolo burocratico. Sono il fondamento su cui costruiamo strumenti che ci fanno lavorare meglio: l'Hub Documentale, la ricerca intelligente, le notifiche automatiche ai tecnici.

Ogni file ben nominato è un file che si trova. Ogni cartella ben strutturata è una risposta che il sistema può dare al tecnico in cantiere, senza che nessuno debba cercargliela manualmente.

Il rigore di oggi è la velocità di domani.

---

*Documento di riferimento — Progetto AI-Driven Automation, Viceconti s.n.c.*
*Per domande o dubbi sulle convenzioni, rivolgersi a Prospero.*
