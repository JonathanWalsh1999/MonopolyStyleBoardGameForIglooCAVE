# Campus Tycoon: project context

Read this first when picking the project back up. It records what the game is, how it's built, the rules as they stand, and the decisions behind them.
Last updated: 29 Oct 2026, at Version 9.6 (working prototype).

---

## 1. What it is

A Monopoly-style educational board game for **further-education college students**, played in an **Igloo CAVE**. The CAVE has 4 projected walls plus a projected floor.

- Teams answer subject questions to buy properties, avoid rent and earn money.
- It runs as a **single web page** in the Igloo **WebView layer**, which bypasses Igloo Control Engine.
- It's hosted on **GitHub Pages**:
  - Repo: `JonathanWalsh1999/MonopolyStyleBoardGameForIglooCAVE`
  - Live: https://jonathanwalsh1999.github.io/MonopolyStyleBoardGameForIglooCAVE/
- **Players:** 2–6 teams (companies) in one room.
- **Who runs it:** the tutor, from the Igloo tablet. The tablet casts a mouse onto the canvas, so every control is a big on-screen button.
- **Who it's for:**
  - College learners, *not* young children, so the tone is a dry, witty game show.
  - Some learners struggle with maths or logic, so everything must be spelled out in plain pounds.

## 2. CAVE canvas and layout

- **Full canvas:** 12566 × 4516 px.
- **Walls**, each 1080 px high (the board strip runs along the top 380 px of all four):

  | Projectors | x range | Position |
  |---|---|---|
  | P1–P2 | 0–3436 | Left |
  | P3–P4 | 3436–6283 | Front |
  | P5–P6 | 6283–9719 | Right |
  | P7–P8 | 9719–12566 | Back |

- **Floor (P9–P10):** x 3436, y 1080, 2847 × 3436.
  - Shows ambient city visuals and the team logo.
  - Shows the **stand-on-your-answer zones** for multiple-choice questions: A ●, B ▲, C ■, D ★. Each zone has a letter, a shape and a colour, so colour-blind players can tell them apart.
- **Front wall:**
  - Turn panel: dice, message, action buttons (dockA).
  - Question card (#qcard), with answer buttons (dockB) below it.
  - Info panel.
- **Back wall:** timer, Pause, End now, Export CSV, Sound and Music toggles, **Replay intro**.
- **Scaling:**
  - The page scales by WIDTH only, because the webview reports 12566×3978 but actually projects 4516.
  - `?preview` fits the whole canvas on a laptop. `?native` shows it at 1:1.
  - `?grid` (or the G key) opens the calibration grid. Calibration is saved in localStorage key `cal2`.
- **Known Igloo setting:** the WebView resolution must be **12566 × 4516** with Dynamic Browser Resolution OFF. Otherwise a black strip appears at the back of the floor. This is a setting the user changes in Igloo, not code.

## 3. Repo layout and build

| Path | Purpose |
|---|---|
| `src/index.template.html` | **The game source** (≈950 lines: HTML, CSS and JS in one file). Edit this file, never `index.html` directly |
| `src/build.py` | Injects the sample questions into the template (marker `/*__SAMPLE_DATA__*/null`) and writes `index.html` and `questions.xlsx` (via openpyxl) to the repo root. Run `python3 src/build.py` |
| `index.html` | Built output, served by Pages |
| `questions.xlsx` | Question bank, auto-loaded over http. Sheets: Questions, Names, Settings, plus a help sheet |
| `assets/audio/*.mp3` | About 134 ElevenLabs clips: `host_*`, `intro_*` and `sfx_*` |
| `assets/logos/` | Optional PNG logo overrides (see the README there) |
| `tools/audio-manifest.json` | URL map for the audio-fetch Action. Empty `{}` when idle |
| `.github/workflows/fetch-audio.yml` | Downloads the URLs in the manifest into `assets/audio`, commits them, and resets the manifest |
| `README.md` | Tutor-facing guide to the rules and setup |
| `PROJECT_CONTEXT.md` | This file |

- **Excel loading:**
  - The game has a built-in xlsx reader using DecompressionStream, so no CDN is needed.
  - Teachers can also use **Load Excel file** on the setup screen, or the `?q=file.xlsx` URL parameter.
- **Question columns:** Topic, Level (1–3), Question, Option A–D, Answer (a letter, or free text for open questions), Explanation.
- **Topic codes:**
  - `1A`–`8B`: the 22 property topics.
  - `U1`–`U8`: unit-level questions, used for harder house questions and as a fallback.
  - `HUB1`–`4`, `MATHS`, `ENGLISH`, `POPQUIZ`, `REALWORLD`, `REVISION`, `BONUS`, `ANY`.
- **Sample bank:** 120 questions (3 at level 1 per property, plus unit-level 2 and 3 questions, and so on).

## 4. Working method (for Claude)

- **Git:**
  - Clone to `/home/claude/monopolystyleboardgameforigloocave`.
  - Always `git pull` first, because the Action commits audio.
  - The repo's git config must be `user.name Claude` / `user.email noreply@anthropic.com`.
  - Commits end with the Co-Authored-By and Claude-Session trailers.
  - The remote prints a "repository moved" notice. It's harmless.
- **Audio:**
  - Clips are made with the ElevenLabs MCP: `creative_generate_speech`, voice **Michael** `nIzSjWglUk4gpecYBkIQ`, model `eleven_v4`, flow `9dMcQ0MSjeozvw8YnRPf`, `generations_count: 1`.
  - SFX use `creative_generate_in_flow` with `eleven_text_to_sound_v2`.
  - The container **cannot download** from storage.googleapis.com. So: put the URLs in `tools/audio-manifest.json`, push, and the Action fetches the files. Then `git pull`.
- **Loudness** (ffmpeg is installed):
  - Voice: `acompressor=threshold=-24dB:ratio=3:attack=5:release=120,loudnorm=I=-13:TP=-1:LRA=7`.
  - SFX: about I=-21. Thinking loop: -27. Tick: -23.
- **Testing:**
  - Use Playwright (Python) headless, with `python3 -m http.server` on a spare port and `?preview`.
  - Call game functions via `page.evaluate`, then crop screenshots to check them.
  - Google Fonts are blocked in the sandbox; that error is expected.
  - `evaluate("playIntro()")` blocks until the whole intro finishes. Use `()=>{playIntro();return 0}` instead.
- **The user prefers multiple-choice questions (AskUserQuestion)** to long typed answers when a design decision is needed.

## 5. Current rules (v9.6)

### Setup

- Pick the companies taking part. These are parody brands with logos drawn in code:
  - Amazoom
  - Costalot Coffee
  - Greggles
  - TokTik
  - Primarket
  - Nandoughs
- Choose a game length: 15, 30, 45, 60 or 75 minutes.
- Choose whether to show **Answers on the floor** (on or off).
- Optionally press **▶ Play intro**: a trailer-style intro of about 110 seconds. It has **❚❚ Pause** (or the space bar) and **Skip** buttons, and **Replay intro** works mid-game.

### Money

- Teams start with £1,500 and get £200 for passing GO.
- **Properties** cost £400–£800. Base rent is a deliberate *wildcard*: some squares are cheap with high rent, others expensive with low rent. Nothing on the board flags which is which (the user's choice).
- **Houses** multiply rent ×3, ×6, ×10, ×13; a hotel is ×16. A house costs £150–£300.
- **Skills hubs** cost £500. Rent is £50, £100, £150 or £250 depending on how many hubs the owner has. The square says "rent from £50".
- **Utilities** (Maths and English) cost £450. Rent is the roll × £8, or the roll × £20 if one team owns both.
- **Winner:** the team with the most **cash minus debt**. Property is worth £0 at the end (the user's choice, like real Monopoly).

### Turns

- Roll the on-screen dice, or press **Physical dice** and tap the total (2–12).
- **Fair turns:** when the clock runs out, the current round finishes, then there's one **Final round**.

### Questions

- Every question has a 60-second clock. Running out of time counts as wrong.
- **Buy:** answer to buy. Right = pay the price. Wrong = a fine of half the price. Chicken out = a quarter. Fines are rounded to £10 and go into the **Jackpot**.
- **Rent:** pay it, or **challenge**. Right = pay £0. Wrong = exactly double the rent shown that turn. Chicken out = the normal rent.
- **Houses:** level 2 questions for houses 1–2 and level 3 for houses 3–4. A **hotel** needs a teach-back that the tutor judges.
- **Options hidden for 10 seconds** (buy, challenge, house, Pop Quiz, Real World). Saying the right answer before they appear wins **+£50**. **Show options now** skips the wait.
- **Explain why:** after a right answer, the tutor can award **+£50** for a good explanation. The explanation text stays hidden until the tutor decides.
- **Every question card shows the outcomes in pounds**, e.g. "✓ Pay £450 and own it · ✗ Pay a £230 fine · 🐔 Chicken out: £110 fine".
- **Rent messages say why**, e.g. "(they own 2 hubs)" or "(you rolled 7, and 7 × £8 = £56)".

### Special squares and events

- **Steal it:** if a team gets a buy question wrong, the answer stays hidden. The other teams race to stand on the right floor zone (20 seconds), and the first one there can buy the square.
- **Comeback questions:** questions anyone got wrong come back later in Pop Quiz or the Revision Room.
- **Squares:**
  - **Pop Quiz:** +£100.
  - **Real World:** +£150, or a £75 fine if wrong.
  - **Life Happens** (3 squares): random real bills (HMRC 20% of cash, boiler £600, fine £400, and so on) or windfalls.
  - **Bonus Round:** every team answers, and the winner takes the Jackpot plus £100.
  - **Revision Room:** landing on it means just visiting. A team *sent* there by "Go to the Revision Room" must answer to escape: 1 attempt per turn, released after 3. Its token gets a red outline.
- **Breaking news** every 10 minutes:
  - Lasting one round: rent strike, boom (double GO money), property crash (25% off), housing shortage (double rent).
  - Instant: rate rise, windfall tax (10% of cash), grant (+£200).

### Loans

- **Bank loan:** 10% per lap, but needs a correct money question.
- **Payday loan:** 40% per lap, capped at double what was borrowed (the UK rule).
- **Can't pay a bill or rent?** An **automatic payday loan** covers the shortfall.
- **Repaying:** press **Loans**, then **Repay**. Pay £100, £250, £500 or the whole loan. Partial repayment shrinks the amount interest is charged on.

### End of the game

- **Most-missed questions:** a class debrief showing the top 5 with answers.
- **Export:** downloads a CSV of every answer, by team and topic. It's useful as Ofsted impact evidence.

## 6. Host and audio

- **The host** is the voice "Michael": dry, witty and British.
- **No-repeat list:** he won't reuse any of his last 14 lines, and he stays quiet on some routine moments.
- **Priority queue:** he **never talks over himself**. Big moments jump the queue (steal, jackpot, news, time's up, winner). Stale small comments get dropped. There is **one reaction per answer**: the "explain why" line only plays if he hasn't just commented.
- **Music:** a lo-fi soundtrack generated live in the browser (WebAudio). It ducks to 12% while he speaks, and sound effects play at 45% volume under his voice.

## 7. Decisions and the user's feedback (why things are the way they are)

- The board is on the walls, not the floor. The user felt a floor board was a bad idea, so the floor is ambient plus answer zones.
- The first version looked "for 5-year-olds", so it was restyled as City Tycoon, with parody companies meant to make students laugh.
- Students were confused by the Revision Room, so the "just visiting" and "sent there" cases are now explicit.
- There was no debt pressure, so v8 added Life Happens bills, auto payday loans and cash-only winning.
- Payday loans were raised from 25% because real rates are far higher. They're now 40% per lap, capped at double.
- **Keep it simple for learners who aren't confident with maths:**
  - The 🔥 Confident ×2 bet was **removed** in v9.6 as too complicated, and "×2" gave the wrong impression.
  - Always show actual £ amounts; never "half" or "double" alone.
- The intro was too fast, so it was slowed by about 1.35×, with pauses added.
- The voice was quiet and mumbly, so all clips were loudness-normalised and the music ducks harder.
- The user is colour blind, so the floor zones use letter plus shape plus colour, with a colour-blind-safe palette.
- The challenger never gets the same question the owner answered to buy that square.

## 8. Open items and next steps

- **Word import (proposed, not built):** tutors will send ordinary Word documents of questions and answers, and asking them to fill in a template is too complex. The plan is a **Load Word file** button that reads a .docx as-is:
  - It detects numbered or "?" questions, a)/A. options, and "Answer:" lines.
  - It maps headings like "Unit 1 – Topic A" to topics.
  - Open questions use **Show answer**.
  - A check screen reports, for example, "Found 42: 30 multiple choice, 12 open, 3 unreadable".
  - The user should send a real (anonymised) tutor document to build against.
- The fallback, until then, is for the user to send the Word document to Claude to convert into `questions.xlsx`.
- **Question counts to ask tutors for:**
  - Minimum about 30. Recommended 60–100, roughly 3 per topic plus 10 general ones.
  - A 4-team game asks about 15–20 questions in 30 minutes and about 35–45 in 60 minutes. This is an estimate, not measured.
- **Possible later ideas:**
  - A floor "what you can do now" helper card.
  - Questions written by students.
  - Phones as controllers.
  - A save/resume feature (refreshing the page currently restarts the game).
- **Unused code:** `goConfident()`, `S.conf` and the `host_confident*` clips. They're harmless leftovers from the removed Confident feature and can be deleted.
- **Ofsted:** the user plans impact case studies after half term. The CSV export and the most-missed list are the evidence sources.
