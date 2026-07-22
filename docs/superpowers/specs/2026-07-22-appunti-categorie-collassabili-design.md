# Appunti — categorie espandibili/contraibili (v10)

**Data:** 2026-07-22
**Stato:** approvato

## Problema

Il tab Appunti mostra una lista piatta: intestazione categoria (`.cat-head`) seguita da tutte le
card della categoria, per ogni categoria. Con molte note serve troppo scroll per raggiungere una
categoria in fondo.

## Soluzione

Ogni intestazione di categoria diventa tappabile e apre/chiude il proprio gruppo di note.

### Aspetto

Riga tappabile su tutta la larghezza: freccia a sinistra (`▸` chiusa / `▾` aperta), nome categoria,
conteggio note allineato a destra. Il conteggio resta visibile anche a categoria chiusa.

```
▾ CUCINA                      3
  [ Ricetta pane        ]
  [ Spesa settimana     ]

▸ LAVORO                      5

▸ SENZA CATEGORIA             1
```

### Stato iniziale

Al primo avvio dopo l'aggiornamento: tutte le categorie chiuse **tranne** quella dell'appunto
modificato più di recente (`mod` massimo). Lo stato viene salvato subito.

Da quel momento in poi comanda solo ciò che apre/chiude l'utente: l'app non richiude né riapre
niente da sola, salvo il caso "sposta in categoria" descritto sotto.

### Persistenza

- Chiave nuova `localStorage['notesCatOpen']`, valore `{"CUCINA":true,"LAVORO":false,…}`.
- Chiave assente → seed iniziale come sopra.
- Categoria **non presente nella mappa** (creata dopo il seed) → considerata **aperta**.
  Così un appunto appena creato in una categoria nuova non risulta mai nascosto.
- Le chiavi di categorie non più esistenti vengono ripulite quando lo stato viene salvato.
- Fuori dal backup JSON: è preferenza UI, come `theme`.
- Nessuna modifica allo schema IndexedDB.

## Implementazione

`renderNotes()` (index.html:1100) costruisce per ogni categoria:

- `.cat-head` — riga tappabile con `.cat-arrow`, nome, `.cat-count`
- `.cat-group` — wrapper che contiene le card della categoria

Il toggle agisce **solo sul DOM già costruito**: aggiunge/toglie una classe sul `.cat-group` e
ruota la freccia. Nessun re-render, nessuna rilettura di IndexedDB, lo scroll non salta.

Collasso via `display:grid` + `grid-template-rows:1fr` → `0fr` con `overflow:hidden` sul figlio
interno: animazione fluida (~180ms) con altezze variabili, senza calcolare pixel.

## Casi limite

| Caso | Comportamento |
|---|---|
| Sposta in categoria (selezione multipla) | La categoria destinazione viene forzata aperta, altrimenti le note spostate sembrerebbero sparite |
| Modalità selezione (long-press) | Stato collasso invariato; si seleziona solo ciò che è visibile |
| Ricerca appunti | Pannello separato, non toccato |
| Elimina tutti i dati | Rimuove anche `notesCatOpen` |
| Nessun appunto | Empty state invariato |

## Release

- `CACHE='diario-v9'` → `diario-v10` in `sw.js`
- Verifica pre-push: `node --check` sul JS estratto + cross-check `getElementById` vs id nell'HTML
