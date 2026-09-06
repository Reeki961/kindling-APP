# Kindling

A pocket companion for conversations — open-ended questions, situation-based
openers, escape hatches for when things stall, and a workshop for learning to
tell stories out loud.

Offline-first, installable, and entirely private: there is no server, no account,
and nothing ever leaves your device.

---

## Why it exists

Plenty of people find writing easy and speaking hard. You can compose a good
paragraph without effort, then go blank the moment you have to say something
interesting out loud — especially when your social battery is already flat.

Kindling is built around that asymmetry rather than against it. The storytelling
workshop deliberately starts where you're strong: **write the story out in full,
then distill it down to something you can speak from.**

Everything in the app is designed for the moment you actually need it — one or
two taps, large text, instant load, no thinking required.

---

## What's in it

**Questions** — 121 open-ended questions across 7 categories. A single
*Give me one* button opens a full-screen card: one question at a time, tap
anywhere for the next, shuffled with no repeats until the deck runs out.

**Openers** — 60 first lines grouped by situation (party, work, one-on-one,
strangers, group hangout, online/date), phrased the way you'd actually say them
rather than as interview questions.

**Rescue** — 60 lines to use verbatim when a conversation dies, a story flops,
you've forgotten someone's name, you zoned out, or you need to leave. A big
red button hands you one instantly.

**People** — what you learned about someone last time, so you always have
somewhere to start.

**Stories** — nine written guides on storytelling craft, plus a three-step
workshop:

1. **Write it out** — every detail, no filter
2. **Distill** — guided prompts reduce it to 3–5 beat cue-cards
3. **Practise** — full-screen beats only, so you rehearse aloud from memory,
   with optional voice recording so you can hear whether it landed

Tag stories with **triggers** — the topics that open a door to them — so
searching "airports" surfaces the right story even if the word never appears in
its text. Practice is tracked as a streak.

---

## Privacy

- **No server, no account, no telemetry.** The app is static files.
- All data lives in your browser's `localStorage`; voice recordings live in
  `IndexedDB`. Neither is ever uploaded.
- A **discreet mode** shrinks and dims the text and kills the ambient glow, so
  it's readable at arm's length but not from the next seat.
- A **hide button** blanks the screen instantly, and the app can blank itself
  when backgrounded so the app-switcher preview shows nothing.
- Export and import your data as a JSON file at any time.

Because the data is device-local, the export file is the only backup that
survives clearing site data or switching phones.

---

## Running it

```bash
npm install
npm run dev
```

Then open the printed `localhost` URL.

To build:

```bash
npm run build
```

The app is a PWA — installing it to a home screen and using it offline requires
serving it over HTTPS. It will not install over plain HTTP on a local network.

---

## Adding content

Questions, openers and rescue lines are plain lists in `src/data/`. Add a line,
then **bump `SEED_VERSION` in `src/storage/store.js`** — without that, new
entries won't appear for anyone who already has the app open, because the merge
step deliberately skips seeds it has already applied.

There's a Python helper that handles id numbering and JS string escaping:

```bash
python3 tools/seed.py check                              # validate all data files
python3 tools/seed.py add questions deeper my-list.txt   # bulk add from a file
```

---

## Tech

React and Vite, deliberately minimal — four dependencies, no router, no state
library, no CSS framework. One stylesheet. Service worker via `vite-plugin-pwa`.

```
src/
├── data/          content: questions, openers, rescues, tips, themes
├── views/         one file per screen
├── components/    shared pieces
├── storage/       store.js (text, localStorage) + audio.js (blobs, IndexedDB)
├── context/       all state transitions live in AppContext.jsx
├── utils/         ids, streak maths
└── styles.css     everything visual
```

Stored data is versioned and migrated on load, so adding a field never breaks an
existing install.

---

## Licence

MIT — do what you like with it.
