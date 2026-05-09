# Istruzioni per la creazione e gestione dei file di riepilogo

Questo documento è lo standard operativo per la creazione dei file di riepilogo nei progetti Claude/Viceconti. Deve essere seguito da Claude in ogni sessione di lavoro che produce un riepilogo, e dalla procedura di sincronizzazione che raccoglie i riepiloghi nella cartella centralizzata.

---

## 1. Tipi di documento

Esistono due tipi di documento con naming dedicato:

### RIEPILOGO (riepilogo operativo di progetto)
Documento che fotografa lo stato di avanzamento di un singolo progetto alla data indicata. Contiene: cosa è stato fatto, cosa resta da fare, decisioni prese, blocchi aperti.

Viene prodotto da Claude alla fine di ogni sessione di lavoro significativa su un progetto.

### SINTESI (sintesi trasversale)
Documento che aggrega e sintetizza i riepiloghi di più progetti per dare una visione d'insieme. È riservato alla cartella SINTESI ECOSISTEMA VICECONTI e prodotto periodicamente.

---

## 2. Convenzione di naming

### Schema generale

```
[NOME PROGETTO] RIEPILOGO [GG_MM_AAAA].md
```

### Regole

- **Formato file:** sempre `.md` (markdown). Se serve una copia in altro formato (.docx, .pdf), si genera a parte ma il file master resta .md
- **Nome progetto:** deve corrispondere esattamente al nome della cartella del progetto in PROGETTI CLAUDE
- **Keyword:** `RIEPILOGO` (maiuscolo) per i riepiloghi operativi, `SINTESI` (maiuscolo) per le sintesi trasversali
- **Data:** formato `GG_MM_AAAA` con underscore come separatore (es. `17_03_2026`)
- **La data si riferisce** alla data della sessione di lavoro, non alla data di creazione del file
- **Separatore** tra le parti del nome: spazio semplice (non underscore, non trattino)
- **Nessun altro testo** tra il nome del progetto e la keyword (no "operativo", no "aggiornamento", no "ripresa")

### Esempi conformi

```
SAP SERVICE LAYER RIEPILOGO 17_03_2026.md
AI HUMAN LAB RIEPILOGO 15_03_2026.md
DATABASE CENTRALIZZATO RIEPILOGO 28_02_2026.md
STRUMENTI DI CATTURA VOCALE RIEPILOGO 15_03_2026.md
AUDIT INFRASTRUTTURA RIEPILOGO 16_03_2026.md
SITI WEB RIEPILOGO 04_03_2026.md
n8n RIEPILOGO 17_02_2026.md
SINTESI ECOSISTEMA VICECONTI SINTESI 08_03_2026.md
```

### Esempi NON conformi (da evitare)

```
SAP SERVICE LAYER Riepilogo_Operativo_15-03-2026.md     → keyword non maiuscola, "Operativo" superfluo, trattini nella data
AI HUMAN LAB Riepilogo operativo 15_03_2026.txt          → formato .txt, "operativo" superfluo, keyword non maiuscola
RIEPILOGO OPERATIVO DATABASE CENTRALIZZATO RIPRESA.txt   → keyword in testa, "OPERATIVO" e "RIPRESA" superflui, formato .txt
Contesto_Manutenzione_Infrastruttura.md                  → manca la keyword RIEPILOGO, manca la data
n8n stato progetto al 17_02_2026.txt                     → manca la keyword, formato .txt
```

---

## 3. Struttura interna del file di riepilogo

Ogni file RIEPILOGO deve contenere almeno queste sezioni:

```markdown
# [Nome Progetto] — Riepilogo del [data leggibile]

## Stato attuale
[Descrizione sintetica di dove si trova il progetto]

## Lavoro svolto in questa sessione
[Elenco delle attività completate]

## Decisioni prese
[Scelte fatte durante la sessione, con motivazione]

## Prossimi passi
[Cosa resta da fare, in ordine di priorità]

## Blocchi o dipendenze
[Eventuali ostacoli, attese da terzi, prerequisiti non ancora risolti]
```

Le sezioni "Decisioni prese" e "Blocchi o dipendenze" possono essere omesse se non applicabili alla sessione.

---

## 4. Istruzioni per Claude

Quando lavori su un progetto in PROGETTI CLAUDE e la sessione produce risultati significativi:

1. **Alla fine della sessione**, proponi la creazione di un file di riepilogo
2. **Usa esattamente** il naming descritto sopra: `[NOME CARTELLA PROGETTO] RIEPILOGO [GG_MM_AAAA].md`
3. **Salva il file** nella cartella del progetto corrispondente
4. **Non rinominare** i riepiloghi precedenti: ogni sessione produce un file nuovo con la sua data
5. **Formato .md** sempre, senza eccezioni per il file master

---

## 5. Regole per la sincronizzazione nella cartella centralizzata

La cartella `SINTESI_RIEPILOGHI_COWORK` raccoglie una copia del riepilogo più recente di ogni progetto attivo.

### Logica di selezione
Per ogni cartella in PROGETTI CLAUDE (esclusa SINTESI_RIEPILOGHI_COWORK stessa):
1. Cerca file che contengono la keyword `RIEPILOGO` o `SINTESI` nel nome
2. Tra quelli trovati, prendi quello con la data più recente nel nome (formato GG_MM_AAAA)
3. Se non esiste nessun file con la keyword, segnala la cartella nel report come "senza riepilogo"
4. Se la cartella è vuota, segnala nel report come "cartella vuota"

### Comportamento della copia
- Il file viene copiato con il nome originale (invariato)
- Se nella destinazione esiste già un file dello stesso progetto con data precedente, il vecchio viene sostituito dal nuovo
- Ad ogni esecuzione viene generato un file `REPORT_SINCRONIZZAZIONE_[GG_MM_AAAA].md` che documenta l'esito

---

## 6. Elenco cartelle progetto attive

Aggiornato al 17/03/2026. Usare come riferimento per la sincronizzazione.

| Cartella | Ha riepilogo conforme | Note |
|---|---|---|
| AI AUTOMATION NUOVE IDEE | No | Da creare |
| AI HUMAN LAB | Sì (da rinominare) | File attuale: AI HUMAN LAB Riepilogo operativo 15_03_2026.txt |
| AI ISTRUZIONI PER L'USO | No | Cartella di riferimento/metodo, valutare se serve |
| ASSET PRODOTTI | — | Cartella vuota |
| ASSISTENTI INTELLIGENTI | No | File attuale: sintesi-segretaria-artificiale-viceconti GENNAIO 2026.md |
| AUDIT INFRASTRUTTURA | Sì (da rinominare) | File attuale: Contesto_Manutenzione_Infrastruttura.md |
| DATABASE CENTRALIZZATO | Sì (da rinominare) | File attuale: RIEPILOGO OPERATIVO DATABASE CENTRALIZZATO RIPRESA 28_02_2026.txt |
| DIARIO DI BORDO | No | Da creare |
| ECOMMERCE VINI | No | Da creare |
| FA TU CANTIERE PROGETTO PILOTA | — | Cartella vuota |
| FORMAZIONE IT | No | Da creare |
| NAMING VICECONTI | No | Da creare |
| NUOVA STRATEGIA SETTEMBRE 2026 | Sì (da rinominare) | File attuale: MAPPA_DECISIONALE_SETTEMBRE_2026_12_03_2026.docx |
| n8n | Sì (da rinominare) | File attuale: n8n stato progetto al 17_02_2026.txt |
| PIPELINE PRESTASHOP | No | Da creare |
| SAP ACADEMY | No | Da creare |
| SAP SERVICE LAYER | Sì (da rinominare) | File attuale: SAP SERVICE LAYER Riepilogo_Operativo_15-03-2026.md |
| SINTESI ECOSISTEMA VICECONTI | Sì | Usa keyword SINTESI |
| SITI WEB | Sì (da rinominare) | File attuale: HUB_DOCUMENTALE_Riepilogo_Operativo_04_03_2026.md |
| STRUMENTI DI CATTURA VOCALE | Sì (da rinominare) | File attuale: STRUMENTI DI CATTURA VOCALE RIEPILOGO OPERATIVO 15_03_2026.txt |
| TELEGRAM | — | Cartella vuota |
