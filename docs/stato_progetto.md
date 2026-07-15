# DIARIO_BORDO — Stato Progetto

**Data:** 2026-07-15
**Fase:** v1 creata in chat browser claude.ai — da recuperare file + risolvere hosting

## Cos'è
PWA single-file (HTML) — diario task giornaliero. Cattura veloce da telefono Android con dettatura Wispr Flow.

## Design v1 (deciso in chat browser)
- PWA installabile home Android, apertura → pagina di oggi, input già a fuoco
- Task spuntabili: barrato verde, resta visibile (non elimina)
- Task in sospeso giorni precedenti → banner ambra "↻ Porta a oggi" (scelta manuale, opzione A)
- #tag inline → pillole blu filtrabili
- Ricerca testuale su storico, navigazione giorni ‹ ›
- Tema chiaro/scuro, palette: carta chiaro, blu-inchiostro accento, verde salvia fatto, ambra riportato
- Dati: IndexedDB locale, export JSON + Markdown, import
- Wispr Flow = tastiera di sistema, nessuna API: basta campo a fuoco

## Problema aperto: hosting
- NAS TrueNAS: errore risolto (serviva /index.html) MA:
  1. NAS non raggiungibile fuori casa
  2. HTTP semplice = niente service worker → PWA offline non funziona comunque
- Soluzione proposta: GitHub Pages (HTTPS, gratis, raggiungibile ovunque, solo codice — le note restano su IndexedDB telefono)

## Prossimi step
1. Recuperare index.html dall'artifact chat browser (download utente) OPPURE ricostruire qui
2. Repo git + GitHub Pages (conferma utente prima di push)
3. Installare PWA da URL https, test offline (modalità aereo)
4. v2 possibili: scorciatoia dinamica long-press icona, flag task ricorrente (opzione B), sync cross-device
