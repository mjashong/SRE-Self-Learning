# SRE Self-Learning

Two self-contained HTML apps for going from zero to **Site Reliability Engineer** — no
build step, no dependencies, no server. Open the file in a browser and start learning.
The course app runs **fully offline**; the lab loads a Python runtime from a CDN on demand.

## The apps

### 📚 DevOps Mastery Academy — `Devops Academy Course.html`
A complete, structured learning platform.

- **9 learning tracks** — Docker, Kubernetes, Git & GitHub, Terraform, Linux & CLI,
  Networking, Python for SRE, Observability, and SRE Principles (concept lessons, quizzes,
  key-term spotlights, and a final test per track).
- **Interview Prep** — ~108 real interview Q&A cards across all 9 topics (optional track).
- **27 hands-on projects** — 3 per track (Beginner / Intermediate / Advanced), each with a
  guided 5-step workshop. Paid-service projects are clearly flagged (free-tier / cheap / paid).
- **Master Challenge** — a 30-question capstone quiz that unlocks once all 9 core tracks
  are complete.
- **Key Terms glossary** — 136 searchable, filterable terms.
- **Portfolio view** — readiness tier (Foundation → Senior), per-track skills, and a
  suggested next project.

### 🧪 SRE Interview Lab — `SRE Lab.html`
A hands-on lab to get interview-ready *by doing*, Kubernetes-first.

- **kubectl Lab** — a simulated, broken-on-purpose cluster with 14 missions; run real
  `kubectl` commands (get / describe / logs / scale / rollout / …) and watch it validate.
- **Python Lab** — 8 executable exercises (run real Python in the browser via Pyodide,
  with a test suite for each).
- **Learn** — ~36 lessons across Kubernetes, Docker, Terraform, Python, and SRE principles,
  with ELI5 explanations, diagrams, and inline checks.
- **Interview** — ~30 flashcards + 13 "walk me through…" scenarios with model answers.
- **Cheatsheets** + a **15-day study plan**.

## Getting started

Just open either `.html` file in a browser — **double-click it**, or:

```
open "Devops Academy Course.html"    # macOS
```

No install, no toolchain. The Academy app needs nothing external. The SRE Lab fetches
Pyodide + Google Fonts from a CDN, so the Python Lab needs internet the first time you use it.

## Saving & backing up progress

Progress auto-saves to your browser's **localStorage** on every action. To keep a portable
copy or move between devices/browsers, the in-app progress panel ("My Progress") offers:

- **Export / Import** a dated `progress.json` backup (works in every browser).
- **Folder auto-save** via the File System Access API (Chrome / Edge / Brave) — writes a
  single `progress.json` into a folder you choose on every change.

The `Saved Progress/` folder is a convenient place to keep those backups; since this repo
lives in iCloud Drive, anything saved there syncs across your devices. See
[`Saved Progress/README.txt`](Saved%20Progress/README.txt) for step-by-step instructions.

## Repo layout

| Path | What it is |
|------|------------|
| `Devops Academy Course.html` | The DevOps Mastery Academy course platform |
| `SRE Lab.html` | The SRE Interview Lab |
| `Saved Progress/` | Portable progress backups + an end-user guide |
| `HANDOFF.md` | Developer handoff: architecture, conventions, and how to continue work |

## Working on the code

Both apps are **single-file** (HTML + CSS + JS inline) and hand-edited — there is no build
step. After changing JS, sanity-check the script and open the file in a browser to verify.
See [`HANDOFF.md`](HANDOFF.md) for the full architecture map, data objects, key functions,
and gotchas.
