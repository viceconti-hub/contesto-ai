# Test v1.1 - file di prova

File temporaneo per verificare il comportamento di `push_github.ps1` v1.1:

1. Il nome locale ha **spazi**: `TEST v11 DELETE ME.md`.
2. Sul repo GitHub deve apparire come `TEST_v11_DELETE_ME.md`.
3. Quando questo file viene rimosso dal locale, lo script successivo deve
   eliminarlo anche dal repo (sync delete).

Questo file viene cancellato dopo il test e non deve restare nel progetto.
