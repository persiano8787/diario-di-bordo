# Turni di lavoro + task preimpostati — Design

**Data:** 2026-07-15 · **Stato:** approvato ("vediamo come viene e decidiamo dopo")

## Obiettivo
Importare/impostare turni di lavoro nell'app e far apparire automaticamente task preimpostati in base alla fascia del turno (mattina/pomeriggio/notte).

## Contesto turni utente
Principali: 7 (07–15, mattina), 15 (15–23, pomeriggio), 23 (23–07, notte). Occasionali: 11, 21, Tr (10–18), ecc. Sorgente: backup app Shifter = SQLite (`dias`: fecha YYYYMMDD, turno1→FK; `tablaTurnos`: texto, horaInicio1, horaFinal1).

## Decisioni
- **Import diretto .Shifter** in-app via sql.js (vendored nel repo, ~1.5MB, precached dal SW). Caricato lazy solo all'import.
- **Fascia da ora inizio**: 05–11 mattina, 12–18 pomeriggio, 19–04 notte. Esclusi (nessuna fascia): Off, Vancanza/Vacanza, Close, A/L, turni con inizio=fine.
- **Modifica manuale**: long-press (contextmenu) su giorno del calendario → picker turno.
- **Vista**: badge turno nell'header sotto la data + etichetta turno colorata per fascia in ogni cella calendario (rota).
- **Preset**: menu → "Task per turno", 3 liste (una riga = un task). Auto-inserimento SOLO pagina di oggi, una volta per giorno (flag `seeded` sul record turno); task inseriti con tag `#turno`, poi normali (spunta/edit/cancella, non tornano).
- **Storage**: IndexedDB v2, nuovo store `shifts` {day,code,label,start,end,fascia,seeded}. Preset e tipi turno in localStorage. Re-import preserva flag seeded, note utente intatte.
- **Backup**: export/import JSON esteso a {tasks, shifts, presets}; retrocompatibile con vecchio formato array.

## Fuori scope v1
Note di Shifter (dias.notas), doppio turno (turno2), pattern ricorrenti, mapping fascia personalizzabile.
