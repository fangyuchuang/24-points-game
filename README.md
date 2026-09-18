# 24 Points

A self-contained **24 Points** (24 Game) math puzzle with an exhaustive solver. Deal four cards, combine them with `+ − × ÷` and parentheses, and make exactly 24.

The whole game is one HTML file — no build step, no dependencies, no network calls.

**[Play the demo in your browser →](https://fangyuchuang.github.io/24-points-game/)**

## Play

- Open `index.html` directly in any modern browser, or serve the folder with `python3 -m http.server`.
- Deal a hand, tap two cards (first tap = left operand), then tap an operator.
- Reduce four cards to one. Land on 24 and you win.
- Keyboard: tap two cards, then press `+`, `-`, `*`, or `/`.

Every dealt hand is verified solvable by the solver before it reaches you, so there is no dead-end deal.

## The solver

The interesting part is not the UI — it's enumerating the search space correctly.

Given *n* numbers, pick any ordered pair `(a, b)` and replace it with one of `a+b`, `a−b`, `a×b`, `a÷b` (division skipped when `b ≈ 0`). That leaves *n−1* numbers; recurse. When one number remains, compare it against 24 with an epsilon.

Because both `(a, b)` and `(b, a)` are enumerated, `a−b` and `a÷b` get both operand orders for free. Addition and multiplication are symmetric, so they generate duplicate expressions — those are collapsed at the end by keying results on the expression string.

```
branching factor   = n × (n − 1) × 4        (3 ops when b ≈ 0)
search tree size   ≈ 4ⁿ × n! × C(n, 2)      for n = 4: a few thousand nodes
```

For four cards this runs in well under a millisecond, which is why the game can afford to (a) reject unsolvable deals at shuffle time and (b) offer "show all solutions" instantly. The same recursion works for any target and any hand size — pass a different `target` to `solve()` and it generalises.

Floating point is used throughout with an epsilon of `1e-6`. Chained division makes exact fractions awkward in JavaScript, and epsilon comparison is precise enough for the values a 24 hand can produce.

## Project structure

```
index.html    game UI + solver, no dependencies
```

## Why 24 Points

It's a compact, well-defined constraint problem: a tiny search space, yet one that punishes sloppy operator precedence and division-by-zero handling. It's also a genuinely useful brain teaser — if you want the strategy side, this [walkthrough of 24 game strategies](https://iqiqgame.com/blog/how-to-play-the-24-game-strategies-to-solve-any-hand--iqiqgame) covers the patterns that make hands solvable in your head.

## More puzzles

- [Play 24 Points online](https://iqiqgame.com/play/calculate-24-points-game-online-free) — the browser version of this puzzle, with a timer and scored rounds.
- [More free brain and puzzle games](https://iqiqgame.com/) — sudoku, killer sudoku, 2048, lights out, and other logic puzzles.
- [Working memory exercises](https://iqiqgame.com/blog/how-to-improve-working-memory-7-science-backed-exercises) — the mental-arithmetic habit this game trains, and what the research says about it.

## License

MIT.
