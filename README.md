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

- **Intro:** on the setup screen, pick the companies, then press **▶ Play intro**. It's a movie-trailer cold open of about 110 seconds, paced so everyone can follow. It has a voiceover, a company roll call with logos slamming onto every wall, the rules, and a "shout the answer" warm-up for the whole room. It then opens the market automatically. **❚❚ Pause** (or the space bar) freezes it mid-sentence so you can explain something, and **▶ Play** carries on from exactly where it stopped. **Skip intro ›** jumps straight in, and **Skip to game** skips it entirely. During a game, **Replay intro** on the back wall plays it again. The game clock pauses, and play carries on where it left off afterwards.
- **Companies:** teams play as six parody companies (Amazoom, Costalot Coffee, Greggles, TokTik, Primarket, Nandoughs). Tap the ones playing (2–6); the order you tap is the turn order. To use your own logos, see `assets/logos/README.md`.
- **Host:** a dry, witty British host with several versions of each line. He never repeats a recent line and stays quiet on some routine moments so it doesn't get old.
- **Music:** a lo-fi city soundtrack is generated live in the browser. It drops right down while the host speaks or a question is up, and has its own **Music: on/off** button. The host's voice is loudness-boosted, and sound effects play quieter while he's talking.
- **Dice:** **Roll dice** uses the on-screen dice; **Physical dice** lets you tap the total from real dice (2–12).

## Stand on your answer

Multiple-choice questions also appear on the **floor** as big zones, one per option. Each zone has its own letter, shape and colour (● A, ▲ B, ■ C, ★ D), so colour-blind players can tell them apart. The team walks onto its answer, and the tutor taps that option, either on the front wall or on the floor zone itself. The right zone then lights up with **✓ CORRECT**, and a wrong pick is marked **✗ WRONG**. In the Bonus Round, every team races to stand on the answer.

Open questions with no options stay on the wall only. Turn the floor zones off with **Answers on the floor** on the setup screen (the setting is remembered on that PC).

## Keeping the whole room in it (v9)

- **Steal it:** if a team gets a *buy* question wrong, the answer isn't revealed. Instead the other teams race to stand on the right answer on the floor (20 seconds). The tutor taps **"<Team> steals it"** for the first team on the right zone, and that team buys the square at its price. Only teams that can afford it are offered.
- **Confidence bet:** for buy, rent challenge, house, Pop Quiz and Real World questions, the question appears first and the options follow 10 seconds later. A team that already knows the answer can press **🔥 Confident ×2** before the options show, to win big or lose big:
  - Buy: 25% off, or a full-price penalty if wrong.
  - House: half price, or the full cost as a penalty if wrong.
  - Rent challenge: the owner pays *you*, or you pay four times the rent if wrong.
  - Pop Quiz: £200, or −£100 if wrong.
  - Real World: £300, or −£150 if wrong.
  **Show options** skips the wait.
- **⚡ Speed bonus / 🔥 Confidence bonus (+£50):** any correct answer in the first 10 seconds earns an extra £50. A correct answer after going 🔥 Confident always earns it too, on top of the double reward, because the team committed before seeing the options. Usually that means answering from memory before the options appear. Just tap **✓ Correct** as soon as they say it.
- **Explain why (+£50):** after a correct answer, the explanation stays hidden while the team explains *why* it's right. The tutor taps **✓ Explained why** (+£50) or **Skip**, and then the explanation appears.
- **Comeback questions:** questions anyone gets wrong go into a pile. They come back in Pop Quiz and the Revision Room (marked *Comeback question*), so the class gets a second go at what it didn't know.
- **Breaking news:** every 10 minutes, a market event hits everyone. Some last one round:
  - Rent strike
  - Economic boom (double GO money)
  - Property crash (25% off)
  - Housing shortage (double rent)

  Others happen instantly:
  - Interest rates rise
  - Windfall tax (10% of cash)
  - Government grant (+£200)
  The active event shows on the turn panel.
- **Class debrief:** when the game ends, press **Most-missed questions** to see the 5 questions the class got wrong most, with the right answers. It's handy for a 2-minute recap.

## Everything in plain pounds

Students never have to work out "half", "double" or "dice × 8" themselves:
- **Every question card shows the outcomes in pounds** before anyone answers, for example *✓ Pay £450 and own it · ✗ Pay a £230 fine · 🐔 Chicken out: £110 fine*. Confident mode shows its own amounts.
- **Rent always says why**, for example *"Rent is £100 (they own 2 hubs)"* or *"Rent is £56 (you rolled 7, and 7 × £8 = £56)"*, plus exactly what a challenge would cost.
- **Fines are rounded to the nearest £10.**
- The **This square** panel uses three plain lines instead of a table of numbers.


- **Cash is king:** the winner is the team with the most **cash minus debt** when the market closes. Property is worth nothing at the end: it only earns rent while the game runs. So buy squares that will pay back in time, and don't borrow more than you can repay.
- **Prices and rent:** properties cost £400–£800. Base rent is a wildcard: some squares have high rent for their price, others are pricey with low rent. Each square shows its price and base rent, but nothing flags which are the bargains, so teams have to work it out. Houses multiply the rent (×3, ×6, ×10, ×13; hotel ×16).
- **Life Happens** (3 squares): a random real-life bill or windfall. The bills are an HMRC tax bill (20% of your cash), a broken boiler (£600), a speeding fine (£400), a cracked phone (£250), council tax (£300) and a failed MOT (£500). The windfalls are a tax refund (+£300), a viral post (+£400) and selling old trainers (+£150). Bills go into the Jackpot.
- **Can't pay?** An **automatic payday loan** covers the shortfall at 40% per lap. That debt comes off your final score.
- **Jackpot:** penalties, bills and chicken-outs go into the Jackpot (it starts at £100). Landing on **Bonus Round** opens a question to every team; the first correct answer wins it all.
- **Loans** (the **Loans** button at the start or end of a turn, or **Get a loan** when a property is unaffordable):
  - **Bank loan** (£250 or £500): 10% interest per lap, but the team must answer a money question correctly to be approved. A wrong answer costs nothing.
  - **Payday loan** (£250 or £500): instant, 40% interest per lap, capped once interest reaches 100% of the loan. That mirrors the real UK rule that you never repay more than double what you borrowed, and real payday APRs run into the thousands.
  - Interest is added every time a team passes GO. Repay any loan from the Loans menu once there's enough cash. Debt counts against net worth.

- **Repaying:** press **Loans**, then **Repay** next to a loan. Teams can pay £100, £250 or £500 off at a time, or pay the whole loan off if they have the cash. Paying part of a loan shrinks the amount interest is charged on, so the next lap's interest is smaller.

## Fair turns

- **60 seconds per question.** A countdown shows on the question card and ticks for the last 10 seconds. Running out of time counts as a wrong answer, except for the hotel "teach it back", where the tutor decides.
- **Equal turns:** when the game clock runs out, the current round finishes, then there's one **Final round** so every company gets one last turn. Every company ends with exactly the same number of turns. This can run a few minutes past the clock, so set the game length a little shorter than the lesson. **End now** on the back wall stops immediately if needed.
- **Revision Room:** *landing* on it means just visiting, so nothing happens. Only a team **sent** there by the *Go to the Revision Room* square is locked in; its token gets a red outline. On its next turn it can't roll. It presses **Answer to escape**, and a correct answer gets it out, then it rolls straight away. Wrong answers cost nothing. There's one attempt per turn, and the team is released after 3 attempts.

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
- **Life Happens:** a random bill or windfall (see above).
- **Bonus Round:** an open question for every team. The tutor clicks the winner, who takes the pot + £100.
- **Revision Room:** see *Fair turns* above.
- **End:** when the clock hits zero, the round finishes and then there's one final round. The winner has the most **cash minus debt**. Game lengths are 15, 30, 45, 60 or 75 minutes.
- **Questions:** a challenger never gets the same question the owner answered to buy that square.
- **Export results:** downloads a CSV of every answer by team and topic, which is useful as impact evidence. If downloads are blocked inside the Igloo webview, run the game in a normal browser on the Igloo PC instead.

## Known limits

- The rounded font loads from Google Fonts. Offline, it falls back to Arial, which works fine.
- There's no save/resume: refreshing the page restarts the game.
- Running from a double-clicked file works, but the Excel file won't auto-load; use the **Load Excel file** button.
