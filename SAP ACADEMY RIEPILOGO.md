# Sessione Vincenzo Strazzullo - 13 Febbraio 2026
## Sintesi Tecnica e Operativa

---

## 1. FILOSOFIA E CONTESTO

### L'evoluzione del programmatore
> "Il programmatore che si dispera è legato a un percorso che probabilmente non vuole o non può emancipare."

Vincenzo usa la metafora del **casellante autostradale**: le competenze si evolvono, non si perdono. Lui stesso si è riposizionato come "consulente AI" perché da programmatore può fare cose che un utente normale non può fare con l'AI (parte agentica, automazioni).

### Service Layer è LA direzione di SAP
> "La nuova interfaccia UI che SAP sta realizzando in HTML5 ha come sottostrato il Service Layer. SAP stessa utilizza il Service Layer. Di conseguenza è uno strumento che SAP non solo consiglia, ma lo usa e quindi difficilmente lo abbandonerà."

**Risposta implicita a 3W Sistemi**: SDK Web NON è l'alternativa. Service Layer è lo strumento strategico.

---

## 2. CONCETTI FONDAMENTALI

### Cos'è il Service Layer
- È una **REST API** basata su protocollo HTTP
- Segue lo standard **OData** (internazionale)
- Risponde e si interroga in formato **JSON**
- Può essere utilizzato **remotamente** (non come le vecchie DI API che richiedevano rete locale)

### Differenza DI API vs Service Layer
| DI API (vecchie) | Service Layer |
|------------------|---------------|
| Solo rete locale | Funziona via internet |
| Solo Windows | Qualsiasi sistema |
| Richiede SDK | Solo HTTP/JSON |

### Formato JSON - Base
```json
{
    "nome_campo": "valore",
    "altro_campo": "altro_valore"
}
```
- Parentesi graffa apre/chiude il pacchetto
- Nome campo tra virgolette, due punti, valore tra virgolette
- Parentesi quadra `[]` indica struttura complessa (array)

---

## 3. STRUMENTO DI TEST: POSTMAN

Vincenzo consiglia **Postman** per testare le API:
- Strumento per capire come funziona un'API
- Usato da tutti gli sviluppatori
- Serve non solo per test iniziali, ma per costruire e verificare tutte le chiamate

---

## 4. PROCEDURA DI LOGIN

### Endpoint
```
POST https://<nome_server>:50000/b1s/v1/Login
```

### Body (JSON)
```json
{
    "CompanyDB": "SBO_Viceconti",
    "UserName": "manager",
    "Password": "viceconti"
}
```

### Risposta attesa
Se funziona, SAP risponde con un **SessionId** (token) da usare in tutte le chiamate successive.

### Verifica funzionamento
> "99.9% ce l'hai, se no non ti funzionerebbe la fatturazione elettronica"

---

## 5. OPERAZIONI PRINCIPALI

### Tipi di chiamata HTTP
| Verbo | Operazione | Esempio |
|-------|------------|---------|
| **GET** | Lettura | Leggi articolo, lista ordini |
| **POST** | Scrittura/Creazione | Crea ordine, movimentazione |
| **PATCH** | Aggiornamento | Modifica quantità |

### Lettura articolo
```
GET https://<server>:50000/b1s/v1/Items('codice_articolo')
```
Risposta: tutti i campi dell'articolo + strutture collegate (listini, ecc.)

### Lettura con filtri (linguaggio OData)
```
GET https://<server>:50000/b1s/v1/ProductionOrders?$select=campo1,campo2&$filter=Status eq 'R'&$orderby=Priority
```
- `$select` = solo questi campi
- `$filter` = condizione (equivale a WHERE in SQL)
- `$orderby` = ordinamento

### Scrittura (esempio movimentazione magazzino)
```
POST https://<server>:50000/b1s/v1/InventoryGenExits
{
    "BaseEntry": 225,
    "DocumentLines": [{
        "LineNum": 0,
        "Quantity": 2,
        "BatchNumber": "xxx"
    }]
}
```

---

## 6. MAPPING NOMI TABELLE → SERVICE LAYER

| Tabella SAP | Nome Service Layer |
|-------------|-------------------|
| OITM | Items |
| OCLG | Activities |
| ORDR | Orders |
| ... | (traduzione inglese della parola) |

> "99 su 100 il nome corrisponde al nome della funzione in inglese"

### Documentazione
L'indirizzo `https://<server>:50000/b1s/v1/` aperto nel browser mostra l'help con tutte le entità disponibili.

---

## 7. ARCHITETTURA E SICUREZZA

### Opzione 1: Accesso via VPN (attuale)
- Ti colleghi in VPN
- Postman sulla tua macchina interroga il server SAP
- Sicuro ma richiede VPN attiva

### Opzione 2: Esposizione diretta (rischiosa)
- Apri porta 50000 pubblicamente
- Rischio attacchi brute force su login
- Sconsigliato da Vincenzo

### Opzione 3: Layer intermedio (consigliata da Vincenzo)
- Server separato che vede SAP internamente
- Espone solo le API che servono
- Maggiore sicurezza (non esponi il database)

### Opzione 4: Scrittura da SAP verso servizio esterno pubblico
> "Rendi pubblico chi riceve, non chi manda"

Esempio PrestaShop:
- Script gira sul server SAP
- Legge da Service Layer
- Scrive su PrestaShop (che è pubblico)
- SAP non è mai esposto

---

## 8. PROSSIMI PASSI (indicati da Vincenzo)

### Step 1: Verifica che funzioni
1. Installa Postman sul server SAP (o in VPN)
2. Prova login a `https://SQLPRD0303:50000/b1s/v1/Login`
3. Se risponde con SessionId → funziona

### Step 2: Se non funziona
> "Puoi tranquillamente chiedere a chi ha fatto le installazioni... qualcuno ti deve garantire che questo service deve funzionare perché altrimenti non funzionano un sacco di cose base"

### Step 3: Definire cosa esporre
- Decidere quali oggetti (login, attività, business partner, ecc.)
- Si possono limitare le API esposte per sicurezza
- "Nessuno potrà scrivere le fatture se non lo permettiamo"

### Step 4: Apertura porta (se necessario)
- Porta consigliata: **443** (HTTPS)
- Da aprire sul server client (BPRD0303), non sul database
- Richiede intervento di VarHub/VarGroup

---

## 9. SCRITTURA DIRETTA DA APP

Vincenzo conferma che è possibile scrivere direttamente da app esterna:
> "Tu puoi scrivere. Immagina che io ho un'app su Android, scrivo la nota direttamente sull'app Android, lancio service layer, scrivi in SAP direttamente. Questa cosa che ti ho fatto vedere qui, Save Product, questo scrive direttamente, non c'è nessuna attesa."

---

## 10. RIFERIMENTI SCREENSHOT

| Timestamp | Contenuto |
|-----------|-----------|
| 17:31 | Introduzione Postman |
| 21:01 | Esempio chiamata remota |
| 22:11-22:55 | Esempio API esterna (Metel) |
| 24:03 | Struttura JSON |
| 26:17-29:21 | Spiegazione login |
| 35:53-40:56 | Lettura articolo con risposta completa |
| 41:28-41:49 | Query con filtri OData |
| 43:15-43:59 | Lettura batch number, intro scrittura |
| 44:16 | Esempio scrittura (InventoryGenExit) |
| 45:48-47:49 | Ricapitolazione procedura login |
| 55:32-55:36 | Conferma scrittura diretta da app |

---

## 11. CITAZIONI CHIAVE

### Su sicurezza e architettura
> "Se tu esponi direttamente l'IP pubblico porta 50000 del service layer, in teoria potresti essere attaccato"

### Su disponibilità Vincenzo
> "Posso tranquillamente darti support... ti aspetto per le attività successive"

### Su strategia operativa
> "Chiarisciti come vuoi sfruttare questa cosa, ma proprio scrivendoti che cosa vuoi da SAP"

---

*Documento elaborato: 15 Febbraio 2026*
*Fonte: Registrazione sessione Vincenzo Strazzullo - 13/02/2026*
