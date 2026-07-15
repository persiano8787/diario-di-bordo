# DIARIO_BORDO — Stato Progetto

**Data:** 2026-07-15
**Fase:** v1 DEPLOYED su GitHub Pages — da installare su telefono

## Deploy
- Repo pubblico: https://github.com/persiano8787/diario-di-bordo
- URL app: **https://persiano8787.github.io/diario-di-bordo/**
- Fix applicati vs versione chat browser: sw.js reale (stale-while-revalidate) al posto di blob URL rifiutato da Chrome; manifest.json file al posto di data: URL
- Aggiornamenti: modifica → commit → push → telefono si aggiorna al secondo avvio con rete
- Cache SW: bump `CACHE='diario-vN'` in sw.js a ogni release

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
1. Installare PWA su telefono da URL https, test offline (modalità aereo)
2. v2 possibili: scorciatoia dinamica long-press icona, flag task ricorrente (opzione B), sync cross-device
