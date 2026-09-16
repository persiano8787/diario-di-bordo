# DIARIO_BORDO — storico sessioni

> Diario del progetto: decisioni, motivazioni, cronaca. Non caricato all'avvio; grep quando serve.

---

# Diario dal vault claude-notes (spostato qui il 2026-09-16)

# DIARIO_BORDO

PWA diario task giornaliero — cattura veloce Android + Wispr Flow. Nato in chat browser claude.ai, migrato in workspace 2026-07-15.

## Decisioni
- PWA single-file > app nativa (sproporzionata) > pagina web (troppi step apertura)
- IndexedDB locale + export JSON/MD come rete sicurezza; sync futura via export
- Task non spuntati: riporto manuale a oggi (banner ambra, scelta per task) — no automatismo
- Palette funzionale: blu accento, verde=fatto, ambra=riportato, tema scuro incluso
- Wispr Flow Android = tastiera sistema → serve solo input a fuoco all'apertura

## Problemi risolti
- "FORBIDDEN: No directory listing" su NAS → mancava /index.html nell'URL
- Hosting NAS inadatto: irraggiungibile fuori casa + HTTP no service worker → si va di GitHub Pages

## Deploy (2026-07-15)
- App live: https://persiano8787.github.io/diario-di-bordo/ (repo pubblico persiano8787/diario-di-bordo)
- Fix: SW blob URL (Chrome lo rifiuta) → sw.js reale stale-while-revalidate; manifest data: URL → manifest.json
- Release: push su main basta; bump CACHE version in sw.js

## v5–v8 (2026-07-15) — iterazioni con feedback utente
- v5: menu Turni, inserimento turni sequenziale da calendario (✎, avanza da solo), tipi turno CRUD con propagazione ai giorni, note riportate con data
- v6: tabbar Diario|Appunti; appunti tipo Testo/Lista, categorie, ricerca, autosave; menu Impostazioni (tema+backup); store notes DB v3
- v7: history-stack pannelli (back Android chiude pannello/naviga menu), selezione multipla appunti (long-press: elimina/categoria/salva .md), titolo+categoria in header editor, Elimina tutti i dati, turni export/import JSON proprio, RIMOSSO Shifter+sql.js (resta in git history)
- v8: lista tipi turno vuota resta vuota; predefiniti solo a prima install (scelta utente: crea i turni da sé)

## Decisioni tecniche chiave
- Pattern release: bump CACHE diario-vN in sw.js → commit → push → app si auto-aggiorna (controllerchange reload, dalla v4)
- Back Android: panelStack + history.pushState per pannello, suppressPops counter per chiusure da UI, pReplace per transizioni senza history
- Verifica pre-push: node --check su JS estratto + cross-check getElementById vs id HTML

## v9 (2026-07-18) — Diario compatto + modifica inline
- Richiesta: card Diario più compatte (più note a schermo) + note modificabili. Vincolo utente: NON rimpicciolire testo, ridurre altezza >40%.
- Compattezza (~49% pitch): gap/padding/check ridotti + `time` inline invece di riga meta separata (meta renderizzata solo se tag o carried). Testo .98rem/1.45 invariato.
- Modifica inline: tap su `.task-text` → textarea in-place. Salva su Invio/blur, Shift+Invio = a capo, vuoto = annulla (non elimina; per eliminare resta ✕). Edita `t.text` grezzo (con #tag), non il renderText. Guardia `saved` per evitare doppio save Invio+blur.
- Nessun cambio schema DB. Spec: docs/superpowers/specs/2026-07-18-diario-compatto-edit-design.md

## v4 (2026-07-15) — turni
- Import .Shifter (backup app Shifter = SQLite; sql.js vendored, lazy). Testato: 3412 turni dal file reale.
- Fascia da ora inizio (05–11/12–18/19–04), preset task per fascia auto-seed su oggi, badge header, rota in calendario, long-press = cambio turno.
- Auto-update SW (controllerchange reload) da v3.

## Prossimi step
- Test v4 su telefono: import file, preset, seed
- Idee: task ricorrenti, sync cross-device, scorciatoia long-press icona

## v10 (2026-07-22) — Appunti: categorie collassabili
- Richiesta: categorie espandibili/contraibili nel tab Appunti (troppo scroll con molte note).
- Scelte utente in brainstorming: default primo avvio = tutte chiuse tranne categoria dell'appunto più recente (solo seed, non ricalcolato ad ogni apertura); header con freccia + conteggio note.
- Stato: `localStorage['notesCatOpen']` = {categoria:bool}. Categoria assente dalla mappa = APERTA (regola: un appunto appena creato non deve mai risultare nascosto). Chiavi orfane ripulite ad ogni render.
- Toggle non re-renderizza: solo classe su `.cat-group` + rotazione freccia → niente rilettura IndexedDB, scroll stabile.
- Animazione collasso con `grid-template-rows:1fr→0fr` + `overflow:hidden` su `.cat-inner`: funziona con altezze variabili senza misurare px (evita il classico max-height hardcoded).
- Caso limite gestito: "sposta in categoria" forza aperta la destinazione, altrimenti le note spostate sembrano sparite. Wipe dati rimuove la chiave.
- Fuori dal backup JSON: è preferenza UI come `theme`.
- Verificato: node --check su JS estratto + cross-check id (nessun id mancante). Runtime NON testato in browser da qui (no jsdom/playwright in ambiente) — test su telefono/desktop a carico utente.

- v10 testata su telefono 2026-07-22: auto-update SW in una sola apertura confermato, categorie chiuse + conteggio OK.
