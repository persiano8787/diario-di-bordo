# DIARIO_BORDO — Stato Progetto

**Data:** 2026-07-15
**Fase:** v6 DEPLOYED — tab Diario/Appunti

## v6 (2026-07-15) — appunti + riorganizzazione
- Tabbar: Diario | Appunti. Appunti = note senza data: tipo Testo o Lista (spuntabile), categorie libere, ricerca dedicata, editor fullscreen autosave. Store `notes` (DB v3)
- Menu: Calendario / Task per turno / Turni / Impostazioni (tema+backup)
- Etichette fasce preset = orari reali turni (07–15/15–23/23–07); calcolo fascia resta elastico

## v5 (2026-07-15)
- Menu Turni (inserisci da calendario ✎ sequenziale / tipi turno CRUD / import .Shifter)
- Note riportate: data origine invece del giorno settimana

## v4 (2026-07-15) — turni di lavoro
- Import backup .Shifter (SQLite, sql.js vendored) — testato su file reale: 3412 turni
- Fascia da ora inizio: 05–11 mattina / 12–18 pomeriggio / 19–04 notte; Off/Vacanza/Close = nessuna
- Badge turno header + rota nel calendario + long-press giorno → cambio turno manuale
- Preset task per fascia (menu → Task per turno), auto-seed oggi 1×/giorno, tag #turno
- Navigazione futuro sbloccata; backup esteso {tasks,shifts,presets}
- Spec: docs/superpowers/specs/2026-07-15-turni-preset-design.md
- v3 inclusa: auto-reload aggiornamenti (da prossima release: 1 sola apertura)

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
