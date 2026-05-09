# Test py — file di prova v2.0

File temporaneo per verificare `push_github.py` v2.0:

1. Il nome locale ha **spazi**: `TEST py DELETE ME.md`.
2. Sul repo deve apparire come `TEST_py_DELETE_ME.md`.
3. Deve comparire in `index.html` (sezione documenti operativi) e in
   `index.json` (`documenti_operativi`).
4. Quando questo file viene rimosso dal locale, lo script successivo deve
   eliminarlo dal repo e dagli indici.

Questo file viene cancellato dopo il test e non deve restare nel progetto.
