# shogi

Single-file Japanese chess (shogi) kifu viewer. Open `index.html` in a browser — that's it.

[Japanese version](README.ja.md)

## Why this exists

Drop-in viewer for a single recorded shogi game. No build, no dependencies, no server. The 84th Meijin-sen Game 2 (Fujii Sōta vs. Itodani Tetsurō, 89 moves) is preloaded.

## Quick example

```sh
python3 -m http.server 8000
# open http://localhost:8000/
```

Use `←` / `→` to step, `Space` to auto-play, click a move in the list to jump.

## What's where

- `index.html` — the whole viewer (HTML / CSS / JS / kifu data, ~680 lines)
- `CLAUDE.md` — repo guide, structure, data format
- `DESIGN.md` — design decisions (single-file, kifu-string resolver)

## Notes

- The kifu string is embedded in `index.html` as the `KIFU` constant; the parser handles `▲`/`△` (sente/gote), `同` (same square as previous move), `成`/`馬`/`龍`/`と` (promotion), `打` (drop), and `(封)` (sealed move). `from` is back-derived via `reachableSquares()`.
- Parser and resolver were verified by replaying all 89 moves with Node — please don't touch them; extend by adding alongside.

## License

MIT
