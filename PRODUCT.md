# Latte timer (web)

## What it is

A single-page, client-only stopwatch for one espresso + milk workflow. The user
stands at the counter with a phone; each step of the drink is timed by tapping
one button. No account, no server, no network call after load.

## Who and where

One person, in their own kitchen, phone propped or in one hand, often damp
hands, bright or early-morning ambient light. Interaction must survive
inattention and a mistimed tap.

## Mode

**Operate** — the visitor completes a task. Precision and restraint over
expression: tap targets, glanceability, and a legible record of what happened
matter more than any visual idea.

## Requirements (from the brief, verbatim where possible)

- Every section is listed; the current one is highlighted — not just the current
  section shown alone.
- Buttons: **Next** and **Undo**, only while a run is in progress. Idle offers
  **Start**.
- Handled entirely on the client. Previous runs persist in the browser.
- Clean and simple.

## Sections

1. Dose — weigh & spray
2. Grind
3. Load & tamp
4. Cleanup — shot pulling (the machine is extracting; the user tidies)
5. Milk — prep & steam
6. Pour & build
7. Final cleanup

## Assumptions (labelled, since no interview was run)

- Sections and their order are fixed in code; changing them is an edit, not a
  setting.
- History is per-browser (localStorage), last runs kept, newest first. No sync
  across devices by design.
- One run at a time; starting a new run abandons nothing silently — a finished
  run is already in history.
- Clearing history is a deliberate, small action, not on the main path.

## Out of scope

Targets per section, sound, sharing, multi-user, recipes other than the above.
