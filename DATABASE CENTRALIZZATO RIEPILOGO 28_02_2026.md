RIEPILOGO OPERATIVO — DATABASE CENTRALIZZATO

Ripresa attività 28 Febbraio 2026

DOVE ERAVAMO RIMASTI (16 Gennaio 2026)

Cosa Stato Revisione import_listino.py ✅ Completata (v1.5) File aggiornati generati ✅ config.py, database.py, import_listino.py, README.md Sostituzione file sul PC ⚠️ NON COMPLETATA — errore database lock al test Test import Morini ❌ Fallito per file non sostituito correttamente

Ultimo errore: sqlite3.OperationalError: database is locked — causato da file import_listino.py non aggiornato (avevi modificato solo l'header, non il contenuto).

OBIETTIVO ATTUALE

Aggiornare i prezzi in SAP per alcuni fornitori

Flusso target:

Excel fornitore ──► import_listino.py ──► prodotti.db ──► export_sap.py ──► CSV per SAP
        │                                      │
        └── _inbox/                            └── export/


STATO COMPONENTI

Componente Stato Note prodotti.db ✅ Operativo 9.785 prodotti Morini già importati config.py ⚠️ Da aggiornare v1.3 con _inbox/, path_immagini database.py ⚠️ Da aggiornare v1.1 con context manager import_listino.py ⚠️ Da aggiornare v1.5 con fix database lock README.md ⚠️ Da creare Documentazione completa export_sap.py ❌ Da creare Genera CSV per import SAP

AZIONI PER RIPRENDERE

STEP 1: Verifica stato attuale (5 min)

cd C:\Progetti\catalogo_centrale

# Verifica versione file
type import_listino.py | Select-String "Versione"
# Deve dire: Versione: 1.5

# Verifica struttura cartelle
dir
# Deve avere: _inbox/ (se presente = già aggiornato)


STEP 2: Se file NON aggiornati → Sostituisci

Devo rigenerare i 4 file e tu li sostituisci in:

C:\Progetti\catalogo_centrale\
├── config.py          ← SOSTITUISCI
├── database.py        ← SOSTITUISCI
├── import_listino.py  ← SOSTITUISCI
└── README.md          ← NUOVO


STEP 3: Test import (5 min)

cd C:\Progetti\catalogo_centrale
venv\Scripts\activate

# Test con 10 righe
python import_listino.py --fornitore MOR --file MORINI_2026_01.xlsx --limite 10


STEP 4: Creare export_sap.py (da fare insieme)

Questo è il pezzo mancante per il tuo obiettivo.

DOMANDE PER TE

Prima di procedere:





Puoi verificare la versione attuale di import_listino.py? (comando sopra)



Hai i listini aggiornati dei fornitori che vuoi importare? Quali fornitori?



Conosci il formato CSV che SAP richiede per l'import prezzi? (se hai un esempio, caricalo)

Dimmi cosa trovi e procediamo!