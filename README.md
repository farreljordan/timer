# timer — latte workflow stopwatch

A single-file, client-only stopwatch for one espresso + milk drink. Tap one
button per step; every step's split and the runs are kept in the browser.

No backend, no build step, no dependencies. `index.html` is the whole app.

## Steps

1. Dose — weigh & spray
2. Grind
3. Load & tamp
4. Cleanup — shot pulling
5. Milk — prep & steam
6. Pour & build
7. Final cleanup

## Behaviour

The steps themselves are editable on the Runs page: rename a step, drag its
handle to reorder, remove it, or add one. The list lives in `localStorage`, so it
survives reloads; editing it ends a run that is in progress (a half-finished run
measured different steps).

- `Next` records the current step's split and starts the next one. After the
  last step the run is written to history and the button becomes `Start new run`.
- `Undo` is a correction, not a reset: the time already spent in the current
  step is folded into the previous one and that step's clock keeps running from
  the merged total. At the first step (nothing recorded yet) it cancels the run.
- Run state, history, settings and the steam timer persist in `localStorage`, so
  closing the tab mid-drink resumes where you were. A step left open for more
  than 6 hours is discarded instead of becoming a split.
- The screen wake lock is held while a run is active.
- `n` / `u` on a keyboard do the same as the two buttons.

## Steam timer

Appears only on the section chosen in settings (default: *Milk — prep & steam*).
`Run` waits a configurable delay (default 5 s) before counting, then counts up;
at the target (default 55 s) it beeps, vibrates and says the target was reached,
and keeps counting so you can see the overshoot.

Everything is configurable on the Runs page (the `Runs` chip in the header):
which section, delay, target, sound, vibration. `Reset everything` there clears
the run and the history after a second tap; settings survive.

## Keys in localStorage

| Key | Contents |
|---|---|
| `latte.run.v1` | run in flight: `{ startedAt, index, sectionStartedAt, marks[], done, sectionsKey }` |
| `latte.history.v1` | finished runs, newest first, `KEEP = 12` |
| `latte.config.v1` | steam timer settings |
| `latte.steam.v1` | steam timer state: `{ phase, delayEndsAt, startedAt, hit, lastElapsed }` |

A run records the section list it started against (`sectionsKey`), so editing
the steps never resumes an old run into the wrong ones.

## Serving it

One static file. Put `index.html` anywhere that serves files and open it — no
build step, no dependencies, no server-side anything.
