# Sistema "Segretaria Artificiale" Viceconti S.N.C.

## Sintesi integrata - Gennaio 2026

---

## Il problema

Input caotico multi-canale (telefonate, WhatsApp, email, incontri casuali, note a mano, audio, foto) che deve diventare azione strutturata in SAP Business One. Oggi questo passaggio dipende interamente dalla memoria e interpretazione umana.

---

## Principi teorici fondamentali

### 1. Pattern entropia-struttura

I dati entrano con alta entropia (formati diversi, qualità variabile, semantica implicita) e devono uscire con bassa entropia (strutturati, validati, azionabili). Ogni sistema informativo è una macchina di riduzione dell'entropia.

### 2. Trade-off ricchezza/catturabilità

| Canale | Contesto | Tracciabilità |
|--------|----------|---------------|
| Incontro di persona | Massimo | Zero |
| Telefonata | Alto | Bassa |
| WhatsApp con foto | Medio | Alta |
| Email | Basso | Massima |

Questo trade-off è strutturale, non eliminabile.

### 3. Separazione cattura vs strutturazione

Nessuno strumento fa bene entrambe le cose. Due momenti distinti: prima catturare tutto (friction zero), poi strutturare (AI-assistita).

### 4. Il contesto compensa l'input

```
[gesto minimo] + [regole minime] + [contesto iniettato] = output strutturato
```

Se il contesto iniettato è ricco, anche un input sciatto diventa azionabile.

---

## Chiarimenti concettuali su AI e LLM

### Cosa l'LLM NON fa

- Non "impara" dai tuoi dati (quello è fine-tuning, raramente necessario)
- Non memorizza tra sessioni (via API ogni chiamata parte da zero)
- Non interroga SAP direttamente
- Non accumula esperienza come un dipendente

### Cosa l'LLM FA

- Traduce linguaggio naturale → struttura dati (JSON)
- Interpreta immagini (classificazione, non riconoscimento SKU preciso)
- Applica regole definite nel prompt di sistema
- Gestisce ambiguità e segnala dati mancanti

### Il pattern corretto: RAG, non addestramento

La "memoria" della segretaria artificiale è esterna al modello. Vive in database e file che vengono iniettati come contesto ad ogni richiesta. Il modello consulta, non possiede.

### Terminologia chiarita

| Termine | Significato corretto |
|---------|---------------------|
| **Fine-tuning** | Modificare i pesi del modello con dati propri (costoso, raramente necessario) |
| **RAG** | Retrieval-Augmented Generation: iniettare contesto rilevante nel prompt |
| **Context window** | La "memoria di lavoro" del modello, limitata alla sessione corrente |
| **Pesi (weights)** | I parametri numerici che *sono* il modello - pubblici (open-source) o proprietari |
| **Prompt di sistema** | Le istruzioni che definiscono il comportamento del modello |

---

## Architettura del sistema

```
[Input caotico qualsiasi]
        ↓
[Telegram: punto di ingresso unico]
  - Gruppi separati per tipo (Assistenza, Ordini, Offerte)
  - Il canale stesso è già metadata/disambiguatore
        ↓
   ┌─────────┐
   │   LLM   │ ← Prompt di sistema con regole
   │  (API)  │ ← Contesto iniettato (clienti, alias, prodotti)
   └─────────┘
        ↓
   Output strutturato (JSON)
   {
     "tipo": "assistenza",
     "cliente": "ULV S.r.l.",
     "oggetto": "Vetrina refrigerata",
     "urgenza": "normale",
     "dati_mancanti": []
   }
        ↓
   ┌─────────────┐
   │    n8n +    │
   │   Python    │ ← Lookup SAP, validazioni, scritture
   └─────────────┘
        ↓
[SAP Business One] ← System of Record
        ↓
[Calendar/Notifiche] ← derivati
```

### Separazione delle responsabilità

| Componente | Cosa sa | Cosa fa |
|------------|---------|---------|
| LLM | Regole di interpretazione (dal prompt) | Capisce il messaggio, estrae dati, classifica |
| LLM | Nulla di SAP, nulla dei tuoi dati | Non interroga database |
| Script Python | Connessione SAP, mapping codici | Traduce output LLM in azioni concrete |
| Database | Clienti, articoli, stock | Fornisce dati a Python |

---

## Regole di classificazione visiva

| Se l'immagine mostra... | E il contesto è... | Allora... |
|-------------------------|-------------------|-----------|
| Attrezzatura (vetrine, forni, banchi) | Messaggio cliente | → Assistenza |
| Prodotto consumabile (detergenti, capsule) | Gruppo ordini | → Ordine |
| Qualsiasi cosa | Mittente interno | → Task interno |

**Principio:** Se un dato non è chiaro → segnalare "da specificare", non inventare.

---

## Knowledge base necessaria

| Elemento | Esempio | Dove vive |
|----------|---------|-----------|
| Alias clienti | "Ulv" → ULV S.r.l. | Tabella mapping |
| Contatti → Clienti | Maddalena Papaleo → Gran Sole | Anagrafica SAP + estensione |
| Prodotti da foto | Care-Tab → SKU-2341 | Catalogo (futuro) |
| Regole implicite | "al passaggio" = non urgente | Prompt di sistema |

Questa knowledge base si costruisce incrementalmente, come l'onboarding di un dipendente. La differenza: il dipendente accumula memoria, il sistema la consulta ogni volta.

---

## LLM locale vs API cloud

### Fattori di decisione

| Aspetto | API Cloud | Locale |
|---------|-----------|--------|
| Costo per chiamata | ~$0.01-0.02 | Zero (ma hardware iniziale) |
| Qualità visione | Superiore | Inferiore |
| Manutenzione | Zero | Su di te |
| Setup | Immediato | Ore/giorni |
| Privacy | Dati escono dalla rete | Dati restano in casa |
| Disponibilità | Dipende da internet/provider | Autonoma |

### Per il volume Viceconti (~50 messaggi/giorno)

- Costo API stimato: ~€15-20/mese
- Costo hardware locale: €800+ iniziali
- Break-even: 3-4 anni

**Verdetto:** API è la scelta corretta. Il locale ha senso per volumi altissimi, vincoli privacy estremi, o scopo formativo.

---

## Replicabilità del pattern

| Funzione | Tipo record SAP | Stessa architettura |
|----------|-----------------|---------------------|
| Assistenza tecnica | Service Call | ✓ |
| Richieste offerte | Quotation | ✓ |
| Conferme ordini | Order | ✓ |

Cambia solo: tipo record, regole di parsing, contesto rilevante.

---

## Prossimo passo

Test manuali della trasformazione AI prima di costruire l'infrastruttura:

1. Input realistico (foto WhatsApp reali)
2. Contesto reale (anagrafica clienti)
3. Prompt di sistema con regole
4. → Validare che l'output JSON sia corretto

Solo dopo: portare in n8n e collegare a SAP.

---

## Costi stimati

€20-40/mese per ~600 richieste/mese. Trascurabile rispetto al tempo risparmiato.

---

## Appendice: esempi concreti analizzati

### Caso 1: Foto vetrina + "Ulv: al passaggio"

```
Input: foto vetrina refrigerata + testo "Ulv: al passaggio"
        ↓
Classificazione: ASSISTENZA (immagine attrezzatura)
Cliente: "Ulv" → lookup → ULV S.r.l.
Urgenza: "al passaggio" → normale
        ↓
Output JSON → Python → Service Call SAP
```

### Caso 2: WhatsApp Maddalena Papaleo - capsule Rational

```
Input: screenshot con foto prodotto + "Ti avviso che ne sono rimasti due"
        ↓
Classificazione: ORDINE (immagine consumabile)
Cliente: Maddalena Papaleo → lookup → Gran Sole
Articolo: immagine → Rational Active Green
Quantità: non specificata → "da confermare"
        ↓
Output JSON → Python → richiesta conferma → Order SAP
```

### Caso 3: WhatsApp Carmen La Regina - Care-Tab per Hotel Sirio

```
Input: 2 foto Care-Tab + "Buongiorno ordine x hotel sirio"
        ↓
Classificazione: ORDINE (esplicito nel testo + immagine consumabile)
Cliente: "hotel sirio" → lookup → Hotel Sirio
Articolo: immagine → Care-Tab
Quantità: non specificata → "da confermare"
        ↓
Output JSON → Python → richiesta conferma → Order SAP
```

---

*Documento generato da sintesi di conversazioni - Gennaio 2026*
