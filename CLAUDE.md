# Build the Aqueduct — handoff

Live presentation game for a 6-minute group talk at the IE EMBA face-to-face session in Segovia.
Topic: acting on team feedback, and closing the loop, builds trust and psychological safety.
Framework: **Ask · Listen · Act · Tell**. Story: everyone says listen; but people don't speak unless you ask;
and they never speak again if you don't act and tell. "Tell" is the keystone.
Presented by Group E: Lina, Omar, Paul, Ariel, Rodrigo, Sultan (first names only on screen; the site is public, so no photos or full names).
Owner: Ariel (engineering director, EMBA student). Audience: EMBA cohort in a room, one projector.

## What exists
- `index.html` — the whole game. One file, vanilla HTML/CSS/JS, no build step, no dependencies.
- Only external call: Google Fonts (Bricolage Grotesque, Atkinson Hyperlegible). Falls back to system sans if offline.
- Open by double-clicking `index.html` in a browser. That is also how it runs on stage.
- A published copy exists as a claude.ai artifact (version 1). This folder is now the source of truth.

## How it runs on stage
- Set-up before the talk: browser window with the game on one half of the screen, Kahoot on the other. Press `P` once.
  The opening screen has a small "Open Kahoot ↗" link (footer, bottom left; `KAHOOT_URL`) that opens the group's Kahoot in its own window.
- Presentation mode (`P`, remembered): full screen for the opening and the closing, back to the side-by-side window
  for the setup screen (people join Kahoot) and the three rounds. Follows back/forward too. Turns on Kahoot buttons.
  Full screen needs a key press or click, so it applies on the first key after loading.
- Presenter A reads the feedback aloud. The room votes in Kahoot (or with 1–4 fingers if Kahoot is off).
- Presenter B drives the laptop:
  - `1`–`4` pick the majority vote (only during a round)
  - `Space` / `Enter` / `→` / `PageDown` advance
  - `←` / `Backspace` / `PageUp` go back one step, including undoing a vote (snapshot stack `past`; the clock keeps running)
  - Clickers send PageUp/PageDown, so a presentation remote works for next/back
  - `R` restart, `T` hide/show timer, `F` fullscreen, `?` show/hide key hints
- Timer starts on the first advance, turns red at 5:30.

## Layout follows the window shape
- Wide windows (full screen) use two columns. Narrow ones (aspect ratio ≤ 5:4, e.g. half the screen next to Kahoot,
  or width ≤ 820px) stack the arch above a short panel, hide the wide-only lines (`.wide-only`) and,
  on result screens, show only the chosen card. Switches by itself when full screen is entered or left.

## Kahoot mode (`K`, or open `index.html#kahoot`; remembered in localStorage)
- Content only, not layout: round choices become Kahoot-coloured buttons
  (1 red triangle, 2 blue diamond, 3 yellow circle, 4 green square) and copy says "Vote in Kahoot".
  Answer order in Kahoot must match.
- Kahoot cannot be embedded or read automatically. Presenter B clicks the Kahoot winner in the game.
- Kahoot can be dropped on the day if Wi-Fi fails: press K to go back to finger votes.
- Kahoot questions and set-up: `KAHOOT.md`. `Kahoot-polls.pdf` (from `kahoot-polls.html`) is the upload for Kahoot AI; keep all three in sync with `ROUNDS` order.

## Scene flow
`open (3 steps) → setup → round 1 → result → round 2 → result → round 3 → result → close`
- `open`: the framework reveals step by step (`OPEN_STEPS`): Listen → Ask → Act + Tell. Space advances each step.
  Each step shows a supporting line from the week's sessions (`src`, `by`).
  The opening panel is built once with room for all lines, so the framework boxes never move; each step only
  toggles classes: the next box lights up (with a short glow), the new line fades in, earlier lines grey out, the quote cross-fades.
- `setup`: Segovia hook and game rules.
- `close`: arch result plus framework score (how many of 3 rounds each step was done), and the "this week" ask.
- Time plan: open ~0:50, rounds ~3:30, close ~0:40.

## Game rules (in `ROUNDS` and `choose()`)
- Arch has 9 voussoirs. Start at 3 stones (`START`), goal 9 (`TARGET`). Keystone fills last (`FILL_ORDER`).
- Each round: one team member's feedback, four leader reactions:
  1. Defend → −1 stone, character shrinks (failed to listen)
  2. Listen only → 0, character dims
  3. Act, don't tell → +2 on the spot (looks as good as 4), then two weeks later one stone cracks (−1, net +1)
  4. Act and tell → +2, character brightens
- Not obvious on purpose: answers are shown as plain actions with no labels, in a different order each
  round (`order` in `ROUNDS`). Labels are revealed on the result screen. Keys 1–4 pick the shown position.
- Each round starts with what the leader asked (`ask` in `ROUNDS`), so Ask and Listen are shown, and the vote is about Act and Tell.
- Round 3 bonus: closing the loop in round 3 after closing it earlier gives +1 extra and lights up the whole team.
- Arch only completes if the room closes the loop at least twice. This is intentional: "act, don't tell" every round must fail.

## Delayed consequences (the core teaching mechanic, in `next()` and the `later` scene)
- No separate scene: the consequence is one "Two weeks later" line (`st.later`) at the top of the next round, plus the team animation.
- If the round was "act, don't tell", that line is the crack line (`crack` in `ROUNDS`) and a stone falls. After round 3 it shows on the closing screen.
- If round 1 is ignored (delta ≤ 0), Marta goes silent and grey.
- If round 1 closed the loop, Marta disagrees openly in the meeting.
- Before round 3: Priya joins; if any loop was closed, Jonas and Lea also join. If round 2 was ignored, Rui goes silent.

## Links to the Segovia week (Self-Leadership Week, Oct 2026)
- Assignment brief: Appreciative Inquiry in a company, 6 minutes, free format (told it is guidance).
  Upload to Blackboard before 9:30 Fri Oct 9. Final presentations Fri 12:10, chaired by Juan Luis Ramón.
- Opening step 1 has no quote on purpose: "listen" is the familiar claim the next two steps complicate.
- Opening step 2: "The questions we ask influence what people notice, discuss, and believe is possible" (Appreciative Inquiry, Tania Romero).
- Opening step 3: "Asking without acting erodes trust faster than not asking at all" (Workplace Wellbeing Practices, Juan Luis Ramón).
- Rounds are Clarke's "sliding door moments" from Psychological Safety (Eva Herber): challenging the status quo (Marta),
  sharing an idea (Rui), admitting a mistake (Priya); joining a team appears when Jonas and Lea join.
- Psychological safety is team-level, trust is between people (Eva Herber), hence "Trust builds psychological safety".
- Closing: "Trust is built in very small moments" (Brené Brown, quoted in Psychological Safety);
  arch label "Growth is individual. Success is collective." (Burn On Awareness, Marcos Cajina).
- `Build-the-Aqueduct.pdf`: backup of every scene (all 4s path, plus the all 3s ending). Regenerate after copy changes.

## Content
- Characters: Marta (senior engineer), Rui (design engineer), Priya (graduate engineer). Generic engineering-team names.
- Copy lives in the `ROUNDS` array and in the `render()` scene templates. Plain English, short sentences.
- Segovia hook: the aqueduct stands with no mortar, stones carry each other.

## Design
- Single dark "dusk" look, projector-friendly. Tokens in `:root`: sky blues, granite stones, amber accent, red/green for outcomes.
- Two-column layout (arch + team left, feedback panel right), stacks below 820px.
- Type scales with min(width, height), so 1280×720 projectors and half-screen Kahoot windows fit without scrolling.
  Extra tightening for short screens in the `max-height:760px` block.
- Arch has a faint upper tier of small arches (`.tier`), Segovia's two-tier signature.
- Face moods change real size (font-size), never `transform: scale`, so faces never overlap each other or the caption.
- Presenter chrome stays quiet: outlined "Next" button; key hints hidden until `?` (or `H`).
- Checked with an overlap/overflow sweep: every scene, three vote paths, at 1920×1080, 1366×768, 1280×720, 1024×768,
  and Kahoot mode at 960×950, 960×1000, 640×620. Zero overlaps, zero overflow.
- Favicon: inline base64 SVG (stone arch with amber keystone on the dusk background), so it works offline.
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

## Testing
- Open `index.html`, play all three rounds with different vote combos:
  - Keys are display positions. Act and tell is 2, 3, 1. Act, don't tell is 1, 4, 2.
  - 2, 3, 1 (always act and tell) → arch completes, team grows
  - 1, 4, 2 (always act, don't tell) → +2 each round, a stone cracks each time, ends at 6, Tell 0/3
  - 3, 1, 1 (defend, listen only, act and tell) → Marta silent, Rui silent, no bonus, 4 stones
- Check at 1920×1080 fullscreen and at a narrow width.
- `node --check` on the extracted script catches syntax errors.
