# Build the Aqueduct — handoff

Live presentation game for a 6-minute group talk at the IE EMBA face-to-face session in Segovia.
Topic: acting on team feedback, and closing the loop, builds trust and psychological safety.
Owner: Ariel (engineering director, EMBA student). Audience: EMBA cohort in a room, one projector.

## What exists
- `index.html` — the whole game. One file, vanilla HTML/CSS/JS, no build step, no dependencies.
- Only external call: Google Fonts (Bricolage Grotesque, Atkinson Hyperlegible). Falls back to system sans if offline.
- Open by double-clicking `index.html` in a browser. That is also how it runs on stage.
- A published copy exists as a claude.ai artifact (version 1). This folder is now the source of truth.

## How it runs on stage
- Presenter A reads the feedback aloud. The room votes with 1–4 fingers.
- Presenter B drives the laptop:
  - `1`–`4` pick the majority vote (only during a round)
  - `Space` / `Enter` / `→` advance
  - `R` restart, `T` hide/show timer, `F` fullscreen
- Timer starts on the first advance, turns red at 5:30.

## Kahoot mode (`K`, or open `index.html#kahoot`; remembered in localStorage)
- For running a Kahoot poll alongside: Kahoot in one half of the screen, the game in the other.
- Kahoot cannot be embedded or read automatically. Presenter B clicks the Kahoot winner in the game.
- Layout stacks: arch on top, short panel below. Round choices become Kahoot-coloured buttons
  (1 red triangle, 2 blue diamond, 3 yellow circle, 4 green square). Answer order in Kahoot must match.
- Normal mode is unchanged, so Kahoot can be dropped on the day if Wi-Fi fails.
- Kahoot questions and set-up: `KAHOOT.md`.

## Scene flow
`intro → round 1 → result → later → round 2 → result → later → round 3 → result → final → cta`

## Game rules (in `ROUNDS` and `choose()`)
- Arch has 9 voussoirs. Start at 3 stones (`START`), goal 9 (`TARGET`). Keystone fills last (`FILL_ORDER`).
- Each round: one team member's feedback, four leader reactions:
  1. Defend → −1 stone, character shrinks
  2. Thank, do nothing → 0, character dims
  3. Act quietly → +1
  4. Act and close the loop → +2, character brightens
- Round 3 bonus: closing the loop in round 3 after closing it earlier gives +1 extra and lights up the whole team.
- Arch only completes if the room closes the loop at least twice. This is intentional: "act quietly" every round must fail.

## Delayed consequences (the core teaching mechanic, in `next()` and the `later` scene)
- If round 1 is ignored (delta ≤ 0), Marta goes silent and grey, and the "later" scene shows her empty speech bubble.
- If round 1 closed the loop, Marta disagrees openly in the meeting.
- Before round 3: Priya joins; if any loop was closed, Jonas and Lea also join. If round 2 was ignored, Rui goes silent.

## Content
- Characters: Marta (senior engineer), Rui (design engineer), Priya (graduate engineer). Generic engineering-team names.
- Copy lives in the `ROUNDS` array and in the `render()` scene templates. Plain English, short sentences.
- Segovia hook: the aqueduct stands with no mortar, stones carry each other.

## Design
- Single dark "dusk" look, projector-friendly. Tokens in `:root`: sky blues, granite stones, amber accent, red/green for outcomes.
- Two-column layout (arch + team left, feedback panel right), stacks below 820px.
- Respects `prefers-reduced-motion`.

## Hard constraints (don't break these)
- Total talk is 6 minutes. Keep 3 rounds. Each round should take about 75 seconds.
- No live network dependency beyond fonts: no live AI calls, no backend. Venue Wi-Fi is not trusted.
- Must work from a local file on a laptop, fullscreen, driven by keyboard.
- Every scene must keep the message legible: leaders must act AND report back.

## Possible next steps (not requested yet, ask Ariel)
- Replace generic scenarios with real stories from the presenters' teams.
- Presenter notes / speaker script per scene (e.g. a hidden notes overlay on `N`).
- Embed the fonts as base64 so it is fully offline.
- Optional sound effects on stone drop/fall (needs a click first to unlock audio).
- Clicker support: map PageDown/PageUp (presentation remotes) to advance/back.
- A "back" key to undo a mis-pressed vote.

## Testing
- Open `index.html`, play all three rounds with different vote combos:
  - all 4s → arch completes, team grows
  - all 3s → arch stops at 6, cannot close
  - 1, then 2, then 4 → Marta silent, Rui silent, no bonus
- Check at 1920×1080 fullscreen and at a narrow width.
- `node --check` on the extracted script catches syntax errors.
