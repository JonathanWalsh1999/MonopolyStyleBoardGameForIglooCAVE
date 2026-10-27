# Campus Tycoon – Igloo CAVE edition

A Monopoly-style learning game for the Igloo CAVE webview layer.
Canvas size: **12,566 × 4,516 px** (8 wall projectors + 2 floor projectors).

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The whole game, including the sample questions. |
| `assets/audio/` | Host voice lines and sound effects (ElevenLabs). |
| `assets/characters/` | Optional character images that replace the built-in monsters. |
| `src/` | Source: edit `index.template.html` or `build.py`, then run `python3 src/build.py`. |
| `questions.xlsx` | The question bank template (also holds team names, topic names and settings). |
| `README.md` | This guide. |

## Put it on GitHub Pages

1. Go to **Settings → Pages**. Under *Build and deployment*, choose **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
2. After a minute or two the game is live at https://jonathanwalsh1999.github.io/MonopolyStyleBoardGameForIglooCAVE/

Notes:
- On a free GitHub account the repository needs to be **public** for Pages. The site is public either way, so never put learner names or results in the repo.
- Ask college IT to check `github.io` isn't blocked on the Igloo PC.

## Set it up in the Igloo

Sound: the first tap on the setup screen unlocks audio in the browser.

1. Add a **WebView layer** in Igloo Control and point it at the GitHub Pages URL (or the local file).
2. Set the webview resolution to **12566 × 4516**, the same canvas as the projector map.
3. The page scales to the webview's **width** (the Igloo webview reports a shorter height than it projects, so height is ignored). On a laptop, add `?preview` to fit the whole canvas in the window.
4. **Alignment check:** add `?grid` to the URL, or press **G**, to show the projector test grid (same layout as Igloo's test pattern) over the game. Press G again to hide it. The grid also shows an **Alignment** panel on the floor: it displays the webview's real size and has buttons to make the walls taller or shorter and to move everything up or down. Settings are remembered on that PC, or can be fixed in the link with `?sy=1.020&oy=40`.
5. Control everything with the cast mouse from the Igloo tablet. All buttons are on the **front wall**, with Pause / End game / Export on the **back wall**.

## Changing the questions

Tutors edit `questions.xlsx` (it can live in OneDrive or SharePoint). The workbook's first sheet explains every column.

Two ways to load it:
- **Setup screen → "Load Excel file"** and pick the file. This works offline too.
- **On GitHub Pages**, replace `questions.xlsx` in the repository. It loads automatically when the game opens.
  You can also point at a different file with `?q=other-file.xlsx`.

Topic codes: `1A`–`8B` are the 22 properties. `U1`–`U8` are unit-level questions shared across a unit, and are good for the harder house questions. The other codes are `HUB1`–`HUB4`, `MATHS`, `ENGLISH`, `POPQUIZ`, `REALWORLD`, `REVISION`, `BONUS` and `ANY`.

## Companies, sound and dice

- **Companies:** teams play as six parody companies (Amazoom, Costalot Coffee, Greggles, TokTik, Primarket, Nandoughs). On the setup screen, tap the ones playing (2–6); the order you tap is the turn order. To use your own logos, see `assets/logos/README.md`.
- **Host and sound:** a dry, witty British host plus sound effects, all made with ElevenLabs and stored in `assets/audio`. **Sound: on/off** mutes everything.
- **Music:** a lo-fi city soundtrack is generated live in the browser (no file needed). It starts on the first tap, gets quieter while the host speaks or a question is up, and has its own **Music: on/off** button.
- **Dice:** **Roll dice** uses the on-screen dice; **Physical dice** lets you tap the total from real dice (2–12).
- **Revision Room:** on its turn, a team in the Revision Room presses **Answer to escape**. A correct answer gets them out and they roll straight away. Wrong answers cost nothing. They get one attempt per turn and are released after 3 attempts.

### Replacing a sound
Upload an MP3 to `assets/audio` with the same file name as the one you're replacing (for example `host_buy_1.mp3`).

## The rules (as built)

- **Unowned property:** the team can buy it or pass.
  - To buy, they answer a question. **Correct:** they pay the price and own it.
  - **Wrong:** they pay half the price. **Chicken out:** they pay a quarter.
  - Penalties go into the **Bonus Round pot**.
- **Someone else's property:** pay the rent, or **challenge**. A correct answer means no rent; a wrong one means double rent. Chickening out of a challenge means paying the normal rent.
- **Own a whole unit:** you can build houses. Houses 1–2 bring level 2 questions, houses 3–4 bring level 3, and a hotel is a "teach it back" challenge judged by the tutor.
- **Pop quiz:** +£100 for a correct answer.
- **Real world:** +£150 if right, −£75 if wrong.
- **Late fee:** goes into the pot.
- **Bonus Round:** an open question for every team. The tutor clicks the winner, who takes the pot + £100.
- **Revision Room:** answer a question to get out; teams are freed automatically after 3 turns. A team that runs out of money goes here instead of being knocked out, and gets a £100 study grant when it escapes.
- **End:** when the timer hits zero, the current turn finishes. The winner has the highest **net worth + accuracy bonus** (up to £500).
- **Export results:** downloads a CSV of every answer by team and topic, which is useful as impact evidence. If downloads are blocked inside the Igloo webview, run the game in a normal browser on the Igloo PC instead.

## Known limits

- The rounded font loads from Google Fonts. Offline, it falls back to Arial, which works fine.
- There's no save/resume: refreshing the page restarts the game.
- Running from a double-clicked file works, but the Excel file won't auto-load; use the **Load Excel file** button.
