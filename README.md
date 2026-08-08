# Dead Air — muted fast money

A two-player head-to-head guessing duel for one phone. Both players wear headphones
loud enough to kill the room. A third person reads the prompt out loud. The players
lip-read, blurt an answer at the same time, and whoever lands the higher survey
answer takes the round.

No build step, no dependencies. One HTML file.

## The rules

1. Players 1 and 2 put headphones on. Volume up.
2. The reader holds the phone and reads the prompt out loud, once, normally.
3. Both players shout an answer **at the same time** — that's what keeps it fair.
   If they take turns, the second player gets a free second look at the reader's mouth.
4. The reader taps **Reveal board**, then marks what each player said (or taps *missed*).
5. Higher-ranked answer wins the round. Same answer, or both off the board, is a push.

Example: *"Name the most likely place to see someone with no shirt on."*
Player 1 says **beach** (rank 1). Player 2 says **house party** (rank 5). Player 1 takes it.

## Reader notes

- Your face is the difficulty slider. Deadpan makes it brutal, reacting gives it away.
- Turn off **Show point values** in settings if you keep flinching at the board.
- Turn on **Keep the prompt on the board** if you're filming — viewers need to be in on
  the question while the players aren't.

## Deploy to GitHub Pages

```
git init
git add .
git commit -m "dead air"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

Then: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
Live at `https://<you>.github.io/<repo>/` in about a minute.

Open it on your phone and use *Add to Home Screen* — the manifest makes it launch
fullscreen with no browser chrome, which matters when you're passing the phone around.

## Adding your own prompts

Everything lives in the `DATA` array near the top of the `<script>` block in `index.html`.
Each prompt is `[question, [[answer, points], ...]]`, ranked top to bottom:

```js
["Name a chore nobody wants to do", [
  ["Dishes", 32],
  ["Cleaning the bathroom", 26],
  ["Laundry", 18],
  ["Taking out the trash", 14],
  ["Vacuuming", 8]
]]
```

Order is what decides the round — points are only used if you switch the win condition
to **Board points**. Six answers per prompt is the practical maximum on a phone screen.

Ship at least 5 answers per prompt so the *Answers shown on the board* setting has
something to work with at every value.

## Settings

| Setting | What it does |
| --- | --- |
| Player names | Colours follow them: player 1 orange, player 2 yellow |
| Rounds per game | 3 / 5 / 7 / 10 |
| Winner decided by | Rounds won, or total board points |
| Answers on the board | 3–6 visible ranks |
| Categories | Toggle any mix of the five |
| Timer | Off, or 10 / 15 / 20 / 30s — running out auto-reveals the board |
| Keep the prompt on the board | Leaves the question visible during the reveal |
| Score automatically | Jumps to the result once both answers are marked |
| Show point values | Hide to keep your reactions honest |
| Standings between rounds | Adds a scoreboard stop each round |
| Sound / vibration / colour slam | Feedback, all independently switchable |

## Notes on the mobile build

- Page never scrolls or rubber-bands. Only the settings pane and the answer list
  scroll, and both use `overscroll-behavior: contain`.
- Text selection, the blue tap-highlight flash, long-press callouts, double-tap zoom
  and pinch zoom are all off.
- Layout uses `100dvh` and `env(safe-area-inset-*)`, so the address bar and the notch
  don't eat the buttons.
- `prefers-reduced-motion` kills every animation.
- Settings persist to `localStorage`, wrapped in try/catch — if storage is blocked the
  game falls back to memory and still runs, it just forgets between reloads.
