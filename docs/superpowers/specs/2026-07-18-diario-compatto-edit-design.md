# Diario: card più compatte + note modificabili inline

**Data:** 2026-07-18
**Scope:** modifica a `index.html` (single-file PWA). Nessun cambio schema DB, nessun bump versione dati.

## Obiettivo
Nella pagina **Diario** (task giornalieri):
1. Ridurre l'altezza delle card **> 40%** per vedere più note a schermo — **senza rimpicciolire il testo**.
2. Permettere la **modifica del testo** di una nota (finora si poteva solo spuntare ✓ o eliminare ✕).

## 1. Compattezza (CSS `.task*`)
Il risparmio arriva da padding/gap/check e dall'eliminare la riga meta sempre presente (che oggi ospita `time`). Il testo resta identico.

| Elemento | Ora | Nuovo |
|---|---|---|
| `.task-text` font | .98rem / line-height 1.45 | **invariato** |
| `.tasks` gap | 8px | 4px |
| `.task` padding | 12px 14px | 6px 12px |
| `.check` size | 24×24 | 20×20 |
| `.time` posizione | riga `.task-meta` sotto (+6px margin-top) | inline a fine testo |
| `.task-meta` | sempre renderizzata (per time) | renderizzata **solo se** ci sono tag o flag carried |

Stima pitch card monoriga: ~73px → ~37px ≈ **-49%**.

### Regole render
- `time` mostrato inline (piccolo, colore soft) accanto/dopo il testo, non su riga separata.
- `.task-meta` (tag `#…` + flag `↻ da …`) renderizzata solo quando `tags.length > 0 || t.carried`. Quando presente, resta a capo sotto il testo.

## 2. Modifica inline
- Tap su `.task-text` → si trasforma in `<textarea>` precompilata col testo corrente, focus attivo, auto-grow in altezza.
- **Salvataggio:** `Enter` (senza shift) **oppure** `blur` (tocco fuori) → `putTask` con nuovo `text` → re-render (tag ricalcolati da eventuali `#tag` nel testo).
- **Shift+Enter** = nuova riga (nessun salvataggio).
- **Testo svuotato** al salvataggio → **annulla** (ripristina testo originale, NON elimina). Per eliminare resta la ✕.
- Campi invariati dalla modifica: `done`, `carried`, `origin`, `time`, `day`, `id`.
- ✓ (toggle done) e ✕ (elimina) restano invariati; tap sul check non entra in edit.

## Edge / test
- Nota `carried` modificabile identicamente.
- Un solo task in edit per volta (aprendo un edit, gli altri restano in view; opzionale: blur salva quello attivo prima).
- Testo multiriga: textarea cresce, card si adatta.
- Nessun cambio a export JSON/MD né allo schema IndexedDB.

## Release
- Un commit.
- Bump `CACHE='diario-vN'` in `sw.js` (N+1 rispetto all'attuale).
- Push autorizzato senza chiedere (repo solo-codice, da regola progetto).
