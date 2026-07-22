# DIARIO_BORDO — Stato Progetto

**Data:** 2026-07-22
**Fase:** v10 DEPLOYED — da testare su telefono

## v10 (2026-07-22) — Appunti: categorie collassabili
- Header categoria tappabile: freccia `▾/▸` che ruota + nome + conteggio note a destra (visibile anche da chiusa).
- Stato in `localStorage['notesCatOpen']`. Primo avvio: tutte chiuse tranne la categoria dell'appunto con `mod` più recente. Poi comanda solo l'utente.
- Categoria nuova (non nella mappa) = aperta, così un appunto appena creato non sparisce. Chiavi orfane ripulite al render.
- Toggle senza re-render né rilettura IndexedDB: classe su `.cat-group`, collasso `grid-template-rows:1fr→0fr` (~180ms, altezze variabili senza calcolo px). Scroll non salta.
- Sposta in categoria (selezione multipla) forza aperta la destinazione. Elimina dati rimuove anche la chiave.
- Nessun cambio schema DB. SW cache v9→v10. Spec: docs/superpowers/specs/2026-07-22-appunti-categorie-collassabili-design.md

## v9 (2026-07-18) — Diario compatto + modifica inline
- Card Diario ~49% più basse: gap 8→4, padding 12×14→6×12, check 24→20, ora inline a fine testo (riga meta solo se tag/carried). **Testo invariato** (.98rem/1.45) per leggibilità.
- Modifica nota: tap sul testo → textarea inline; Invio o blur salva, Shift+Invio a capo, testo vuoto annulla (non elimina). `time/done/carried/origin` invariati.
- SW cache v8→v9. Spec: docs/superpowers/specs/2026-07-18-diario-compatto-edit-design.md

## v8 (2026-07-15)
- Fix: lista tipi turno vuota resta vuota (Elimina dati o cancellazione manuale); predefiniti solo a prima installazione. Utente crea i propri tipi.

## v7 (2026-07-15)
- History-stack pannelli: back hardware chiude pannello in cima, sottomenu impilati, back in Appunti → Diario
- Appunti: long-press → selezione multipla (elimina / sposta categoria / salva .md); titolo+categoria nella barra superiore editor
- Impostazioni: Elimina tutti i dati (doppia conferma)
- Turni: contiene Task per turno; esporta/importa turni JSON proprio ({tipo:'turni',shiftTypes,shifts}); RIMOSSO import Shifter + sql.js

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
