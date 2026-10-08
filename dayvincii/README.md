# Dayvincii

One puzzle a day. Solve it against the clock, then see how you rank.

A single-file, mobile-first browser game. No build step, no dependencies, no accounts.

- Four puzzle types: cryptic clues, logic, ciphers, number sequences
- Same puzzle for everyone, switching at midnight UTC
- Wrong answer +15s, hint +30s
- Result screen: time, points, rank, histogram of everyone's times, top five, streak
- Practice mode (does not count)

## Run it

Open `index.html` in a browser, or serve the folder:

    python3 -m http.server 8000

## Host it on GitHub Pages

1. Create a repo and push these files to `main`.
2. Settings, Pages, deploy from branch `main`, folder `/ (root)`.
3. It goes live at `https://<you>.github.io/<repo>/`.

## Add puzzles

Everything is in `index.html`.

- `CRYPTIC` and `LOGIC`: add `{q, a:[answers], h:'hint'}` entries. Answers are matched ignoring case, spaces and punctuation. Put the clue's enumeration in the clue text, e.g. `(6)`.
- `PHRASES`: add uppercase phrases for the cipher type.
- Sequences are generated in `buildSeqRaw`.
- `CYCLE` sets which type appears on which day.

Order is shuffled per cycle, and nothing seen in the last 8 puzzles of a cycle appears in the first 8 of the next.

## Rankings

Your progress (streak, solved, today's result) is saved in the browser's `localStorage`.

There is no server, so on a plain static host the rank is measured against a simulated field of 180 players, and the result screen says so. To get a real global board, replace `sset`, `slist` and `sget` (near the top of the script) with calls to a small backend. The scheme already used:

- one record per player per day, key `t:YYYY-MM-DD:<time in ms, 8 digits>:<player id>`, value `{n: handle, s: points, w: wrong}`
- rank is the position of your key in the sorted list of that day's keys, so only keys need listing, not values

Handles and times are the only data shared. Players are identified by a random id stored in their browser.
