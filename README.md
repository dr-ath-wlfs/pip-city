# Pip City — Learn Python the Fun Way

A gamified Python-learning web app for coding clubs and classrooms. Students
join with just a name and bring a dark city back to life by completing 8
quests — each one teaches a core Python idea and lights up a building, awards
XP, badges, and levels from Egg to Mayor of Pip City.

## Features

- 8 progressive quests: `print()`, variables, turtle, loops, lists, functions,
  conditionals, and a final free-build project (design your own city district)
- Autograded challenges written in Python — instant feedback per check
- Turtle canvas with animated drawing, plus a free Playground with 4 example
  programs (rainbow spiral, shooting star, little houses, snowflake)
- Gamification: XP, levels, badges, confetti, and a city map that fills up
- Player progress saved to SQLite by name (no passwords); falls back to an
  in-memory session when no backend is reachable
- 25-second watchdog: infinite loops (`while True:`) are stopped safely
- Works on mobile (responsive layout)

## Quick start

```bash
npm install
npm run dev
```

Open http://localhost:5000 — the first run of Python code takes a few seconds
while Pyodide downloads from the CDN.

## The quests

| # | Lesson | Python skill | Building | XP |
|---|--------|--------------|---------|-----|
| 1 | Hello, Pip! | `print()` | Power Station | 100 |
| 2 | The Bakery Order | variables | Bakery | 100 |
| 3 | Square One | turtle basics | Art Museum | 100 |
| 4 | Loop Coaster | `for` loops | Ferris Wheel | 100 |
| 5 | Rainbow Tower | lists | Rainbow Tower | 100 |
| 6 | Robot Workshop | functions | Robot Workshop | 100 |
| 7 | Weather Station | `if` / `elif` / `else` | Weather Station | 100 |
| 8 | Design Your District | free project | City Hall | 200 |

## Tech stack

- **Frontend:** React + Vite + TypeScript, Tailwind CSS, CodeMirror 6
- **Backend:** Express + Drizzle ORM + SQLite
- **Python:** Pyodide 0.26.4 in a Web Worker, with a custom `turtle` module
  streaming commands to a canvas renderer

## Project structure

```
client/            React app
  public/          static assets
  src/lib/
    lessons.ts      quest definitions + autograder checks
    pyEngine.ts     Pyodide worker manager (run, restart, checks)
    pyodide-worker.js.txt  the Python runtime worker (embedded as a blob)
    turtleRenderer.ts  canvas renderer for turtle commands
  src/pages/       Home, City, Lesson, Playground, Profile
server/            Express API + storage (SQLite)
shared/            shared types
```

## Adding a lesson

Open `client/src/lib/lessons.ts` and copy an existing lesson object. Each
lesson has story cards, starter code, and a list of checks — small Python
snippets that receive `source` (the student's code), `output` (printed text)
and `turtle_stats()` (drawing stats) and pass by not raising an error, e.g.:

```js
{
  name: "Prints a total",
  code: "expect('12' in output or int(output.split()[-1]) >= 1, 'Print the total')",
}
```

## Deployment

Any Node host works (Render, Railway, Fly.io, a school server):

```bash
npm install
npm run build
NODE_ENV=production node dist/index.cjs   # serves on $PORT or 5000
```

The SQLite database file is created automatically on first run.

You can also host the static build (`dist/public`) on GitHub Pages or any
static host — the app detects the missing backend and keeps players in the
browser session (progress lasts for the session only, with no persistence).

## Notes and limitations

- `input()` is not supported in the browser editor — students store answers
  in variables instead (lessons are written accordingly)
- The turtle module covers the common commands (`forward`, turns, `penup`/
  `pendown`, colors, pensize, `circle`, `begin_fill`/`end_fill`, `dot`,
  `write`, `speed`, `goto`, `setheading`, stamps) but not tkinter-specific
  features like click events or multiple screens
- The first Python run is slow while Pyodide downloads (~10 MB from jsDelivr);
  later runs are instant

## License

MIT — see [LICENSE](LICENSE).
