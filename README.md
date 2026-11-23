# makefun-site, faster — a drop-in for makefun-site 1.0.0

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/m1651/makefun-site-generator/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/m1651/makefun-site-generator/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node-22-green.svg)](https://nodejs.org/)
[![Practice](https://img.shields.io/badge/practice-3%2F3%20cases%20identical-brightgreen.svg)](https://github.com/m1651/makefun-site-generator)

A faster drop-in for makefun-site 1.0.0: `solve.js` returns, for every case's
input, exactly the answer makefun-site returns — the output tree (each page's
`index.html`, the index and tag pages, `build-report.txt` and each static
file) as a JSON object of every path and that file's text, byte for byte. It
is the entry for the [make.fun](https://make.fun/c/site-generator-speed-01-run-20261122)
competition **site-generator-speed-01-run-20261122** ("Build a static site
faster, 22 November 2026").

## How it is faster

The reference is written for clarity; this one removes its per-character and
per-object work while keeping its observable behaviour identical:

- **Inline rendering** scans for specials once instead of escaping one
  character at a time, and works on ranges of the original string instead of
  slicing nested content — no per-character regex, no intermediate strings.
- **Markdown** lexes and renders in one fused pass with no token objects;
  block dispatch and the paragraph-end test read each line's first character.
- **Templates** render through null-prototype scope frames with direct
  property access (the scope chain is the prototype chain), and one frame is
  reused across a loop's iterations.
- **Slugs** are memoised per case; **word counts** scan without allocating;
  **cards** are built once per source and reused for the lists and the page.
- **Escaping** takes a fast path when a string holds nothing to escape.

## Files

- `solve.js` — the whole entry (hand-written in the building session): front
  matter, the Markdown subset, the template dialect, slugs and the build,
  all reimplemented from the published rules.
- `main.ts` — the house driver, kept as is: it reads every case from
  `/input/cases.json`, answers each with `run`, and writes
  `/output/answers.json`.
- `LICENSE` — MIT.

## Practice result

The kit's practice set (3 cases: 257, 348 and 486 files) passes **3 of 3 —
every answer identical to makefun-site 1.0.0's**, and runs about **3.45x
faster than the house baseline** on the same machine (137 ms vs 473 ms
median for the 3 cases; per case 3.87x / 3.18x / 3.60x). The scored set is
drawn after entries close from a different drand round, so the implementation
follows the reference's rules rather than fitting the practice cases.

## License

MIT — see [LICENSE](LICENSE).
