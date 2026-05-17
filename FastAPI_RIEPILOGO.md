# FastAPI — Riepilogo del 17 maggio 2026

## 0. Inquadramento

Documento di chiusura del **Cap. V FastAPI** del weekend di sviluppo 16-17 maggio 2026. Cattura l'arco completo del cantiere: dalle decisioni di sabato 16/05 (Cap. IV — manifest pattern sulla sola Sintesi), all'estensione di domenica mattina (Cap. V — pattern esteso a tutto contesto-ai), alla scoperta della cache opaca lato consumer AI Claude (domenica notte/mattina), alla risoluzione strutturale via `curl` nel pomeriggio di domenica.

Pensato come riferimento canonico del progetto FastAPI fino al prossimo aggiornamento sostanziale.

**Contesto di partenza**: il progetto FastAPI è la "superficie API unificata dell'ecosistema Viceconti" — piattaforma su Render che espone endpoint specializzati per consumer eterogenei (umani via browser, AI via tool di fetch, automazioni deterministiche, futuro B2B). All'inizio del weekend serviva solo `/sintesi/latest` tramite redirect 302 (implementato sabato 16/05). Obiettivo del weekend: estendere il pattern a tutti i 16 documenti di `contesto-ai` (Sintesi trasversale, 12 riepiloghi progetto, 4 documenti operativi, indice JSON), in modo da disporre di una superficie completa e coerente.

---

## 1. Stato finale del Cap. V (17/05/2026 sera)

### 1.1 Infrastruttura FastAPI completa e operativa

- 17 endpoint funzionanti sotto namespace `/contesto/*`
- Hosting Render Starter ($7/mo) — eliminato il cold start del Free tier
- Pattern proxy uniforme su tutti gli endpoint (post Option B)
- 7/7 test end-to-end superati (vedi Sezione 6)
- Header `Cache-Control: no-store` su tutte le risposte
- Latenze 170–370ms
- Commit corrente di `app.py`: `24d8ccd7` su `viceconti-hub/piattaforma-ai`

### 1.2 Problema cache opaca lato consumer AI Claude — risolto

Il problema strutturale di freschezza dei contenuti per il consumer AI primario (Claude in chat conversazionale via `web_fetch`), scoperto durante i test di stato di regime di domenica notte, è stato **risolto a livello di tool**:

- Soluzione: `curl` via `bash_tool` al posto di `web_fetch` per fetchare i documenti di contesto-ai
- Prerequisito: toggle "Code Execution and File Creation" abilitato nei progetti Claude
- Formula nei prompt sistema in tre varianti (A standard, B sintesi-only, variante speciale per 401.SINTESI)
- Documentato in `ISTRUZIONI RIEPILOGO E NAMING.md` Sezione 8

### 1.3 Documentazione operativa aggiornata

Allineata alla decisione finale di Option B + risoluzione via curl:
- `ISTRUZIONI RIEPILOGO E NAMING.md` aggiornato al 17/05/2026: rimosso pattern manifest+redirect dalla Sintesi, aggiunta Sezione 8 (lettura AI via curl), Sezione 7 rigenerata sulla Mappatura Progetti corrente.

---

## 2. Decisioni architettoniche consolidate

### 2.1 Estensione su servizio esistente, non nuovo servizio

L'estensione del pattern è stata fatta sull'app FastAPI `piattaforma-ai` esistente, non su un servizio separato. Conferma del framing "superficie API unificata dell'ecosistema". Namespace `/contesto/*` per i nuovi endpoint. Niente alias di backward compatibility — il vecchio `/sintesi/latest` migra a `/contesto/sintesi/latest`, decisione consapevole per pulizia del namespace.

### 2.2 Naming convention con date ISO

Le risorse versionate, quando ne esistono, usano `YYYY_MM_DD` invece di semantic versioning `v1.x`. Continuità con la convenzione già in uso nelle cartelle Cowork, auto-documentazione, niente version counter da mantenere. Post Option B, l'uso è limitato (la Sintesi non è più versionata); resta convenzione di riferimento per eventuali documenti futuri con vera semantica di versione.

### 2.3 Upgrade Render Free → Starter ($7/mo)

Eseguito. Eliminato il cold start, che era problema concreto per fetch AI: Render serviva una pagina HTML "WELCOME TO RENDER" durante il riavvio anziché un 503 onesto, e i client AI la consumavano come contenuto valido. Sintomo prima dell'upgrade: contenuto Sintesi sostituito da HTML Render nelle chat AI.

Scoperta laterale: Gemini fallisce comunque indipendentemente dal cold start — Google fa "search indicizzata" piuttosto che vero fetch HTTP, e non sa raggiungere URL dinamici come `piattaforma-ai.onrender.com`. Per Gemini si continuerà a usare `github_url` (pagina blob indicizzabile) come canale primario.

### 2.4 Classificazione documenti per pattern (post Option B)

| Categoria | Pattern | Endpoint |
|---|---|---|
| Sintesi | Proxy uniforme (post Option B) | `/contesto/sintesi/latest` |
| 12 riepiloghi progetti | Parametric proxy | `/contesto/{slug}/latest` |
| 4 documenti operativi | Proxy individuali | `/contesto/<slug>` |
| `index.json` | Proxy JSON | `/contesto/index` |
| `REPORT_SINCRONIZZAZIONE_*` | Non esposto via FastAPI | — |

### 2.5 Unificazione `manifest.json` + `index.json`

`manifest.json` cancellato. `index.json` diventa l'unica fonte di verità: slug → URL sorgente + metadati per FastAPI + indice umano. Evoluzione semantica naturale: prima `index.json` era solo per umani, ora serve due famiglie (umani che leggono, FastAPI che mappa slug a raw URL).

### 2.6 Option B — Abbandono completo del versioning della Sintesi

Decisione chiave del 17/05 pomeriggio. Motivazioni:

- Il workflow Cowork → `push_github.py` è strutturalmente progettato per nomi stabili. Mantenere il versioning per la Sintesi richiedeva modifiche non triviali (sync-delete con whitelist; Cowork che copia con nome data-versionato).
- Nessun documento dell'ecosistema ha vera semantica di versione — i documenti vengono *editati*, non versionati. Le revisioni "v1.x" della Sintesi servivano a tracciare quando il documento veniva aggiornato, ma non a permettere accesso alle versioni storiche.
- `Cache-Control: no-store` sul proxy era ritenuto sufficiente per la freschezza, ipotesi poi smentita dalla cache opaca ma risolta a livello di tool, non di architettura URL.

Conseguenza: tutti gli endpoint diventano proxy uniformi, niente più 302 redirect. Pattern emergente finale: *"proxy è il default universale; redirect+versioning resta nello strumentario architettonico ma non applicato"*, riservato per futuri documenti con vera semantica di versione (es. eventuale Manuale operativo) o per consumer AI privi di `bash_tool` (Gemini, ChatGPT in certe configurazioni, embed AI in altre piattaforme).

### 2.7 `curl` via `bash_tool` come pattern primario di lettura AI

Decisione del 17/05 pomeriggio tardi, conseguenza diretta della scoperta empirica che `bash_tool` accede al network attraverso un proxy diverso da `web_fetch` e non eredita la cache opaca infrastrutturale.

Per i consumer Claude nei progetti Claude.ai:
- Il prompt sistema istruisce esplicitamente Claude a usare `curl` via `bash_tool` per fetchare i documenti di contesto-ai
- Il toggle "Code Execution and File Creation" deve essere abilitato nelle impostazioni del progetto (configurazione una tantum)
- Tre varianti della formula (A standard, B sintesi-only, speciale per 401.SINTESI) coprono tutti i casi

### 2.8 Pattern operativo: Sintesi + riepilogo specifico al primo messaggio

Convenzione consolidata per i prompt sistema dei progetti Claude:
- Al primo messaggio di ogni nuova chat, Claude esegue `curl` di Sintesi + riepilogo specifico del progetto
- Letture aggiuntive (altri riepiloghi, indice, documenti operativi) sono on-demand su richiesta dell'utente
- Pattern hybrid: eager loading del minimo necessario per allineamento, lazy loading di tutto il resto

---

## 3. Cronaca delle 27 ore — dalla scoperta alla risoluzione

### 3.1 Il punto di partenza (sabato 16/05 sera, Cap. IV)

Sabato pomeriggio: implementato il manifest pattern per la sola Sintesi, con stable URL `/sintesi/latest` su `piattaforma-ai.onrender.com` che redirige (302) al file versionato corrente (es. `SINTESI_2026_05_14.md`) sul raw GitHub. Prima conferma empirica positiva: una chat Claude in altro progetto ha fetchato per la prima volta `/sintesi/latest` e ha visto correttamente il contenuto v1.5. Test ha confermato che il 302 follow-redirect funziona meccanicamente.

Non è stato fatto, sabato sera, il test di stato di regime — aggiorni il contenuto sul server, richiedi di nuovo lo stesso URL, verifichi se vedi il nuovo o il vecchio. Quello è arrivato il giorno dopo.

### 3.2 La diagnosi iniziale (sabato notte tardi)

Al termine della sessione di implementazione del Cap. IV, durante i test triangolanti, è emersa una scoperta architettonica significativa.

Sintomo: dopo aver pushato la Sintesi v1.5 con header "Aggiornato al 14 maggio", una chat Claude continuava a vedere "10 maggio" (la versione precedente). Il browser fresco invece vedeva correttamente il 14 maggio.

Conclusione: esiste una cache opaca a livello del tool `web_fetch` di Claude (lato infrastruttura Anthropic), upstream rispetto allo stato conversazionale. Questa cache non rispetta `Cache-Control: no-store` dell'origine. Probabile motivazione: ragioni di costo (fetch è caro, cachare risparmia).

A quel punto si conosceva: l'esistenza della cache, il fatto che fosse upstream del conversazionale, l'inefficacia del `Cache-Control: no-store`. Non si conosceva: TTL, generalità (altri fetcher AI?), eventuali workaround.

### 3.3 La triangolazione (domenica mattina, nuova chat)

Mattina del 17/05, nuova chat Claude FastAPI, per chiudere il Cap. V che era partito col piede di estendere il pattern a tutti i documenti.

Test di triangolazione cruciale: stesso documento (Sintesi), tre canali, tre stati cachati distinti:

| Canale | Versione vista | Età contenuto |
|---|---|---|
| Server vero (Cowork, browser, email index) | v1.5 — 14 maggio | corrente |
| FastAPI proxy via `web_fetch` | v1.4 — 10 maggio | ~7 giorni vecchia |
| GitHub raw via `web_fetch` | v1.3 — 5 maggio | ~12 giorni vecchia |

Tre cache entry diverse per lo stesso documento sottostante. La firma è inequivocabile:

1. La cache è **URL-keyed**: due URL diverse → due cache entry diverse → due contenuti diversi
2. Il TTL è nell'ordine delle **settimane**, non delle ore (v1.3 è di 12 giorni fa)
3. La cache è **infrastruttura condivisa tra istanze**: quello che una Claude vede oggi può essere ciò che un'altra Claude ha cachato settimane fa
4. La cache opaca affligge **anche fonti completamente non-FastAPI** (raw GitHub) — non è specifica del proxy

La diagnosi è passata da "ipotesi con incertezza" a forense.

### 3.4 Le cinque opzioni di mitigazione

Articolate la mattina del 17/05:

**Opzione 1 — Zero intervento (accettare la latenza)**: eliminata empiricamente. Il TTL settimane rende inaccettabile la latenza per qualsiasi caso d'uso operativo.

**Opzione 2 — Cache-busting query param `?v=<date>`**: rischio non quantificato. Alcune cache CDN normalizzano la query string scartando i param non significativi; non sapevamo se la cache di Claude lo facesse. Senza test empirico, non garantita.

**Opzione 3 — Versioning del path della destinazione**: l'unica con garanzia strutturale. Cambia il path stesso dell'URL, e quel path è univoco per quella versione del contenuto. Costo: richiede gestione del versioning lato push e aggiornamento dei link nei prompt sistema ad ogni nuovo aggiornamento.

**Opzione 4 — Email come side channel**: scoperta empirica del 17/05 mattina. Il tool Gmail di Claude legge contenuto fresco perché passa per infrastruttura diversa da `web_fetch`. Pattern: invio del contenuto inline via email; Claude legge dall'email anziché fetchare l'URL. Utile per snapshot critici occasionali.

**Opzione 5 — Umano-courier**: l'utente apre il link nel browser, copia il contenuto, lo incolla in chat. Tecnologicamente primitivo ma robusto al 100%. Riconosciuto in seguito come pattern già in uso implicito: ogni handoff di chat con upload di file di contesto è umano-courier sistematico.

### 3.5 L'inversione di flusso (domenica primo pomeriggio)

Esplorato un pattern alternativo per chat conversazionale Claude: invece di umano-courier eager (carico massiccio di file all'inizio di ogni chat), umano-courier lazy on-demand:
- Indice inline nel prompt sistema (testo statico, non URL)
- Claude legge l'indice dal prompt e chiede selettivamente all'utente di aprire/incollare il documento specifico necessario per la query corrente

Pattern elegante, ma con costo cognitivo non trascurabile (round-trip iniziali per ogni chat). Sviluppato come piano operativo, poi superato dalla scoperta successiva.

### 3.6 La scoperta del bypass via curl (domenica pomeriggio)

Punto di svolta: analizzando l'Hub Documentale (link Dropbox ai PDF dei DDT), è emersa la differenza tra `web_fetch` (bloccato da `robots.txt` di Dropbox) e `bash_tool` + `curl` (che ignora `robots.txt` e fa richieste HTTP dirette). Il primo test su un DDT è passato correttamente.

Da lì la domanda naturale: se `curl` bypassa `robots.txt`, bypassa anche la cache opaca? Test empirico immediato sul link raw GitHub della Sintesi via curl:

```bash
curl -sL "https://www.dropbox.com/scl/fi/.../SINTESI...md?...&dl=1"
```

Risultato: contenuto fresco corrente (con header data di test impostato a "31 dicembre 2999"), letto in tempo reale, nessuna cache. La cache opaca è specifica del tool `web_fetch` di Anthropic, non del network di Anthropic in generale: `bash_tool` accede attraverso un proxy di rete diverso, senza quel layer cache.

### 3.7 Conferma empirica e collocazione del principio

Conferma indipendente fatta nella chat FastAPI con due fetch curl, entrambi restituiscono la v1.5 corretta della Sintesi (raw GitHub e FastAPI proxy). La cache opaca è circoscritta al canale `web_fetch`.

Implicazione: la soluzione strutturale al problema cache opaca era già nel toolbox di Claude. Non serviva nessuna modifica al server, nessun versioning del path, nessuna pipeline parallela. Serviva un toggle ("Code Execution and File Creation") per progetto e un'istruzione di prompt che dicesse "usa curl invece di web_fetch".

Le cinque opzioni di mitigazione restano nel record architettonico come strumenti utili per scenari residuali:
- Opzione 3 (versioning del path) per consumer AI senza accesso a bash_tool
- Opzione 5 (umano-courier) come tampone tattico extra-eccezionale
- Le altre tre (latenza, query param, side channel email) effettivamente decadono per Claude

---

## 4. Architettura: due interfacce di contesto-ai

Articolazione consolidata nel pomeriggio del 17/05.

Prima del Cap. IV-V c'era un'unica interfaccia (pagina HTML pubblica su GitHub Pages + raw URL di GitHub). Ora c'è una separazione consapevole tra due interfacce dello stesso ecosistema documentale:

- **Interfaccia umana**: HTML statica su `viceconti-hub.github.io/contesto-ai/`. Snapshot view, sfogliabile, invariata.
- **Interfaccia AI**: FastAPI live view su `piattaforma-ai.onrender.com/contesto/*`. Costruita coscientemente per consumer AI, con stable URLs, nomenclatura semantica, accesso via `curl` + `bash_tool`.

Stessa fonte di verità (repo `contesto-ai`), due proiezioni specializzate. Conforme al principio architettonico "Duplicazione consapevole per destinatari divergenti".

Da promuovere come paragrafo nella Sintesi v1.6.

---

## 5. Principi consolidati promossi alla Sintesi v1.6

### 5.1 Specializzazione del principio "L'apparato del consumer guida la scelta del formato"

> *L'apparato del consumer include la scelta del tool dentro lo stesso consumer.* `web_fetch` e `curl` (via `bash_tool`) sono due apparati diversi dentro lo stesso modello Claude, con comportamenti di cache opposti. La scelta tra tool dentro lo stesso modello è leva architettonica, non dettaglio operativo.

Prima del Cap. V parlavamo del consumer Claude come entità monolitica con un certo comportamento di fetch. In realtà Claude è un'orchestrazione di strumenti diversi (`web_fetch`, `bash_tool`, image_fetch, gmail tool, ecc.), ciascuno con proprietà tecniche distinte. La progettazione architettonica AI-readable deve scegliere quale strumento il consumer userà, non solo quale URL/formato esporre.

### 5.2 Le restrizioni di uno strumento non sono restrizioni del sistema

> *Prima di accettare un vincolo come strutturale, verificare se è un vincolo dello strumento specifico o del sistema sottostante. Una restrizione di uno strumento non implica una restrizione del sistema.*

Lezione metodologica dalla giornata del 17/05: il riflesso istintivo, di fronte al risultato negativo di `web_fetch`, è stato cercare la soluzione nella stessa famiglia di strumenti (versioning del path, query param, ecc.). La soluzione effettiva era in un'altra famiglia, raggiungibile con un toggle di configurazione. Il principio mette in guardia da future deduzioni analoghe.

### 5.3 Cache opaca lato consumer AI — riformulato come tool-specific

> *Il fetch tool di Claude (`web_fetch`) mantiene cache infrastrutturali che ignorano `Cache-Control: no-store` dell'origine, con TTL settimane. `bash_tool` con `curl` passa per proxy diverso, senza quella cache. La cache opaca non è "del consumer" ma "del tool specifico nel consumer".*

Riformulazione del principio inizialmente articolato come "Cache opaca lato consumer AI" durante il Cap. IV. La granularità più fine illumina dove cercare la soluzione la prossima volta che un problema simile emergerà.

### 5.4 Pattern proxy uniforme come default architettonico

> *Per documenti senza vera semantica di versione (la maggioranza), il pattern proxy uniforme con stable URL è il default. Redirect + versioning resta nello strumentario per consumer AI privi di `bash_tool` o per documenti con vera semantica di versione.*

Decisione di Option B promossa a principio generale.

### 5.5 Pattern di lettura AI conversazionale

> *Il context window è il canale primario di consumo per AI conversazionale, popolato preferenzialmente via `curl` da `bash_tool`. L'URL HTTP è canale primario per consumer non-AI (umani via browser, automazioni deterministiche). I due canali coesistono accanto, non sovrapposti.*

Riconoscimento articolato sviluppato durante la giornata, ridotto in intensità dalla scoperta curl ma rimanente valido come framing.

---

## 6. Test end-to-end superati

7/7 test eseguiti da Cowork sull'endpoint base `https://piattaforma-ai.onrender.com`:

| # | Endpoint | Status | Time | Note |
|---|---|---|---|---|
| 1 | `/contesto/index` | 200 | 277ms | JSON valido, 4 sezioni, 12 riepiloghi + 4 doc op., `meta.fastapi_base` presente |
| 2 | `/contesto/sintesi/latest` | 200 | 170ms | markdown 39.8 KB |
| 3 | `/contesto/sap_academy/latest` | 200 | 183ms | "Sessione Vincenzo Strazzullo - 13 Febbraio 2026" |
| 4 | `/contesto/n8n/latest` | 200 | 366ms | "n8n — Riepilogo del 22 aprile 2026" |
| 5 | `/contesto/istruzioni` | 200 | 226ms | "Istruzioni per la creazione..." |
| 6 | `/contesto/naming_viceconti` | 200 | 360ms | "CONVENZIONI DI NAMING — ECOSISTEMA VICECONTI" |
| 7 | `/contesto/inesistente/latest` | 404 | 173ms | `{"detail":"Progetto 'inesistente' non trovato"}` — FastAPI HTTPException pulito |

Verifiche aggiuntive:
- Tutti i markdown hanno `Content-Type: text/markdown; charset=utf-8`
- L'index ha `Content-Type: application/json; charset=utf-8`
- `Cache-Control: no-store` presente su tutte le risposte
- Latenze 170–370ms (Render Starter, no cold start)
- Il 404 è pulito (non 500/502)

Test indipendente di lettura via curl: ✅ contenuto fresco confermato su tutti gli endpoint.

**L'architettura è validata end-to-end** sia lato server sia lato consumer AI con la formula curl.

---

## 7. Schema `index.json` corrente

```json
{
  "meta": {
    "repo": "viceconti-hub/contesto-ai",
    "base_raw": "https://raw.githubusercontent.com/viceconti-hub/contesto-ai/main/",
    "fastapi_base": "https://piattaforma-ai.onrender.com",
    "ultimo_aggiornamento": "<ISO timestamp>"
  },
  "sintesi_trasversale": [
    {
      "nome": "Sintesi Ecosistema Viceconti",
      "slug": "sintesi",
      "tipo": "SINTESI",
      "file": "SINTESI_ECOSISTEMA_VICECONTI_RIEPILOGO.md",
      "raw_url": "...",
      "github_url": "...",
      "fastapi_url": "https://piattaforma-ai.onrender.com/contesto/sintesi/latest",
      "ultimo_aggiornamento": "2026-05-14"
    }
  ],
  "riepiloghi_progetti": [ ... 12+ entries ... ],
  "documenti_operativi": [ ... 4 entries con esposto_fastapi (bool) ... ]
}
```

Slug consolidati: `sintesi`, `contesto_ai`, `strategia_settembre_2026`, `strumenti_cattura_vocale`, `sap_academy`, `n8n`, `naming_viceconti`, `istruzioni`, `readme_push_github`, `fastapi`.

---

## 8. Stato dei pending residui

### 🟢 Completati con il Cap. V

- ✅ Estensione manifest/proxy pattern a tutti i documenti contesto-ai
- ✅ Upgrade Render Starter
- ✅ Unificazione manifest.json + index.json
- ✅ Decisione Option B (abbandono versioning Sintesi)
- ✅ Risoluzione cache opaca via curl
- ✅ Aggiornamento `ISTRUZIONI RIEPILOGO E NAMING.md` (17/05/2026)
- ✅ Riepilogo FastAPI (questo documento)

### 🟡 In corso o imminenti

- **Step 4 — Aggiornamento `push_github.py` v2.1** (semplificato post Option B). Modifiche necessarie:
  - Aggiungere costante `FASTAPI_BASE = "https://piattaforma-ai.onrender.com"`
  - Aggiungere dict `SLUG_OVERRIDES` (mappa nome esteso → slug breve)
  - Aggiungere dict `ESPOSTO_FASTAPI_RULES` per i 4 doc operativi
  - Helper `make_slug()` e `get_esposto_fastapi()`
  - Modificare `build_index_json()` con tre builder separati per categoria
  - **Non più necessari** (post Option B): `extract_sintesi_version`, caso speciale Sintesi in `display_name`, gestione `current_version`

- **Step 5 — Aggiornamento prompt sistema dei progetti Claude** con la formula curl. Ondate proposte:
  - **Wave 0** (preliminari): uniformare naming 390.FastAPI ↔ FastApi; pubblicare questo riepilogo su contesto-ai (slug `fastapi`); verificare endpoint
  - **Wave 1** (pilota, oggi sera): 410.MEMORIA E CONTESTO AI, 390.FastAPI, 320.STRUMENTI DI CATTURA VOCALE
  - **Wave 2** (domani): 100, 310, 350 con Variante A; 401 con variante speciale; 210 con Variante A su slug `naming_viceconti`
  - **Wave 3** (asincrono, nei giorni successivi): i 16 progetti senza riepilogo (Variante B), in batch
  - Template della formula in tre varianti (A, B, speciale) definito durante la sessione del 17/05 pomeriggio, mantenuto centralmente nel progetto Cowork di allineamento prompt sistema

### 🟡 Aggiornamento Sintesi v1.6 a fine weekend

Con i principi consolidati nel Cap. V (vedi Sezione 5 di questo documento):
- Specializzazione del principio "L'apparato del consumer guida la scelta del formato"
- Le restrizioni di uno strumento non sono restrizioni del sistema
- Cache opaca lato consumer AI riformulata come tool-specific
- Pattern proxy uniforme come default
- Due interfacce di contesto-ai (umana HTML + AI FastAPI via curl)
- Pattern di lettura AI conversazionale

### 🔴 Cap. II — `/attivita` endpoint con Service Layer

**Riabilitato come progettato.** La scoperta cache opaca aveva sospeso il design originale (stato live SAP via API per AI). Con curl come pattern di lettura, la promessa "stato live SAP via API per AI" è onorabile. Costruire l'endpoint sapendo che il consumer AI deve essere istruito a usare curl, non `web_fetch`.

---

## 9. Reference paths e endpoint

### Repository GitHub

- `viceconti-hub/contesto-ai` (pubblico): markdown documenti, `index.json`. Source of truth per i contenuti.
- `viceconti-hub/piattaforma-ai` (privato): codice `app.py` FastAPI. Commit corrente: `24d8ccd7`.

### URL operativi FastAPI

Base: `https://piattaforma-ai.onrender.com`

| Endpoint | Funzione |
|---|---|
| `/contesto/index` | Indice JSON completo |
| `/contesto/sintesi/latest` | Sintesi trasversale |
| `/contesto/{slug}/latest` | Riepilogo di progetto specifico |
| `/contesto/istruzioni` | Istruzioni riepilogo e naming |
| `/contesto/naming_viceconti` | Convenzioni di naming dell'ecosistema |

### Pattern di accesso da Claude in chat

```bash
# Al primo messaggio di una nuova chat (Variante A standard):
curl -s https://piattaforma-ai.onrender.com/contesto/sintesi/latest
curl -s https://piattaforma-ai.onrender.com/contesto/<slug_progetto>/latest

# Su richiesta dell'utente:
curl -s https://piattaforma-ai.onrender.com/contesto/index
curl -s https://piattaforma-ai.onrender.com/contesto/<altro_slug>/latest
curl -s https://piattaforma-ai.onrender.com/contesto/naming_viceconti
curl -s https://piattaforma-ai.onrender.com/contesto/istruzioni
```

### Cartelle locali (PC Lauria)

- `C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\510.SINTESI_RIEPILOGHI_COWORK` — output pubblicato Cowork
- `C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\520.SINTESI RIEPILOGHI COWORK CARTELLA DI LAVORO` — master sync
- `C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\520.SINTESI RIEPILOGHI COWORK CARTELLA DI LAVORO\push_github.py` — script v2.0 attuale (da aggiornare a v2.1)
- `C:\Users\PC\Il mio Drive\PROGETTI CLAUDE\390.FastAPI` — cartella del progetto FastAPI (verificare uniformità naming)

---

## 10. Lessons learned per progetti futuri

Tre lezioni metodologiche dal weekend che meritano di restare nel record:

**Verifica il vincolo prima di progettare la soluzione.** La cache opaca è stata trattata per ore come vincolo strutturale del sistema. Era vincolo di un singolo tool. La verifica del livello a cui il vincolo opera deve precedere il design di workaround complessi.

**Le architetture si progettano sapendo i tool dei consumer.** Specificare l'URL e il formato non basta. Bisogna specificare *quale tool del consumer userà quell'URL* — e quali proprietà ha quel tool (cache, restrizioni, formati gestiti, autenticazione). Questa specificazione è prima architettura, non implementazione.

**I costi sembrano alti finché non si riconosce cosa si è imparato.** Il Cap. V FastAPI è stato cantiere di un'architettura per consumer che il cantiere non serviva davvero (web_fetch con cache opaca). Ma in cambio ha consegnato:
- Diagnosi solida della cache opaca (utile per qualsiasi progetto futuro con consumer AI)
- Mappa delle cinque opzioni di mitigazione (utili per scenari residuali)
- Pattern di accesso via curl (riusabile per Cap. II /attivita e tutti i futuri endpoint)
- Riconoscimento che il toolset del consumer è architettura (lente nuova per tutti i progetti)

L'architettura FastAPI stessa non è invalidata: serve consumer non-AI (browser umani, automazioni deterministiche, futuro B2B) come progettata. Per Claude conversazionale, serve la stessa architettura attraversata da `curl` invece che da `web_fetch`. La superficie API resta sovraordinata; cambia il modo in cui Claude la consuma.

---

*Documento di chiusura redatto il 17 maggio 2026 pomeriggio tardi. Riferimento canonico del progetto FastAPI fino al prossimo aggiornamento sostanziale. Per il dettaglio operativo della pipeline `push_github.py` e dell'`index.json`, vedere `ISTRUZIONI RIEPILOGO E NAMING.md`. Per il contesto trasversale dell'ecosistema Viceconti, vedere la Sintesi.*
