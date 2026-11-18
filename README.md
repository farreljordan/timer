# timer — latte workflow stopwatch

A single-file, client-only stopwatch for one espresso + milk drink. Tap one
button per step; every step's split and the runs are kept in the browser.

No backend, no build step, no dependencies. `index.html` is the whole app.

## Profiles

A profile is a named set of steps and timers. **Latte** is the default and has
the workflow below; another drink gets its own profile (New profile), created as
a copy of the active one so you can trim it down. Switching profile swaps the
steps and timers everywhere — a run in progress ends, since it was measuring the
other recipe. Renaming is the Profile name field; Delete this profile asks once
more (the last profile cannot be deleted). Runs are a single log across profiles.

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

## Timers

A step can carry one or more small timers. Each appears only while its step is
the current one — press `Run`, it waits its own delay (default 5 s), then counts
up; at its target (default 55 s) it beeps, vibrates and says the target was
reached, and keeps counting so you can see the overshoot.

The default is a single timer named **Steam** on *Milk — prep & steam* with a
5 s delay and a 55 s target. Add more on the Runs page under **Timers**: name,
the step it sits on, delay, target, sound, vibration. Several timers on one step
stack in the panel and run independently.

Editing steps or timers ends a run in progress (the run measured the old steps);
timers pinned to a removed step move to the first step and say so. Each past run has its own × to delete it
(tap the row to see its splits); `Reset everything` clears the run and the whole
history after a second tap. Steps and timers survive both.

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
