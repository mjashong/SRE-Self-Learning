# SRE Self-Learning — Project Handoff

**Status:** two complete, working, self-contained HTML learning apps
**Last updated:** 2026-06-07 (audited directly against both files)

This document gives a fresh Claude (or future-you) everything needed to continue
work without prior chat history. Everything lives as plain files in this iCloud
folder — just open the folder and read the relevant `.html` file plus this doc.

---

## Where everything lives

This folder is `Self Learning/SRE/` in iCloud Drive. Full absolute path:

```
/Users/michaeljeffreyashong/Library/Mobile Documents/com~apple~CloudDocs/Self Learning/SRE/
```

| File | What it is |
|------|------------|
| `Devops Academy Course.html` | **App #1** — "DevOps Mastery Academy" learning platform (~6390 lines, ~1.1 MB) |
| `SRE Lab.html` | **App #2** — "SRE Interview Lab" hands-on lab (~3035 lines, ~260 KB) |
| `HANDOFF.md` | This file |
| `Saved Progress/` | Folder for portable progress backups (see below) |
| `Saved Progress/README.txt` | End-user instructions for backing up / restoring / auto-saving progress |

> **Note on the old handoff:** prior versions referenced `/mnt/user-data/outputs/devops-academy.html`
> and `/tmp/build_*.py` generator scripts. Those were sandbox paths from the original
> build sessions and **no longer exist**. The apps are now edited directly as local
> files in this iCloud folder. There is no build step — the `.html` files ARE the app.

Both apps are single-file: HTML + CSS + JS inline, **no build step, no dependencies**
to run the core content (the SRE Lab loads Pyodide + Google Fonts from CDN on demand;
the Academy loads **nothing external at all** — it makes zero network calls and runs
fully offline).

---

# APP #1 — `Devops Academy Course.html` ("DevOps Mastery Academy")

A single-file interactive learning platform for becoming a Site Reliability Engineer.
Runs in any browser, persists progress to localStorage (plus optional file backups).

## Current feature set

### 7 nav tabs (Lucide-style line-art SVG icons)
Order (HTML lines 896–902):
`Dashboard → My Progress → Projects → Key Terms → Portfolio → Interview Prep → Master Challenge`

- All routed through `showView(name)` **except** Interview Prep, whose nav button calls
  `goInterviewTrack()` directly (line 901, `id="interviewNavBtn"`).
- Interview Prep is intentionally framed as **optional / not required for the Master
  Challenge** (banner on the dashboard, line ~3579).

### 9 learning tracks + 1 interview track (`TRACKS`, ~line 1245)
- Core 9: Docker, Kubernetes (`k8s`), Git & GitHub (`git`), Terraform (`tf`),
  Linux & CLI (`linux`), Networking, Python for SRE (`python`), Observability, SRE Principles (`sre`)
- Plus `interview` (Interview Prep) — separate, teal `--interview` color var
- Each core track: 7–8 lessons mixing `concept` / `quiz` / `terms` / `final` types
- Glossary "spotlight" term teachings woven inline into concept lessons

### Interview Prep track
- 9 topic areas (`iv-docker`, `iv-k8s`, … `iv-sre`), ~108 Q&A cards total
- Each card: `q`, `level` (easy/medium/hard), `a` (HTML answer), `tip`
- Entry function: `goInterviewTrack()`

### 27 hands-on projects (`PROJECTS`, ~line 3653) — 3 per core track (B/I/A)
- IDs: `dp1–3, kp1–3, gp1–3, tp1–3, lp1–3, np1–3, pyp1–3, op1–3, sp1–3` (27 total, verified)
- Each project: `id, level, time, title, desc, tags, steps, outcome`
- 5 projects carry a `costs` field flagging paid services:
  - `tp1`, `tp2` — free-tier (green); `np3` — cheap ~$0.02 (amber); `tp3`, `np2` — paid (red)
- `costs = {level, estimate, notes}` where `level ∈ 'free-tier' | 'cheap' | 'paid'`

### Workshop modal (`WORKSHOP_STEPS`, ~line 4964)
- 27 step-sets (one per project), 5 rich steps each = 135 steps, all verified present
- Step fields: `title, why, instructions, commands[], hint, think, thinkAnswer`
- Step 0 of paid projects renders a `.ws-cost-banner`
- `openWorkshop(pid, trackName, _)` loads steps; `renderWorkshopStep()` renders current step

### Master Challenge (capstone)
- 30 random questions across the 9 core tracks; pass threshold 80%
- `buildMasterChallenge()` pulls from quiz/final lessons (skips `interview`), up to 4/track
- `allTracksComplete()` gates unlock on the hardcoded 9 core track IDs
- Sits as the final card in the tracks-grid; SAME card shape as other tracks, visually
  distinct via gradient bg, animated shine, FINAL badge, gradient icon/title/CTA
- Render fns: `renderMaster()`, `renderMasterQuestion()`, `renderMasterResults()`

### Key Terms / Glossary view (`GLOSSARY`, ~line 4439)
- 136 terms grouped by track (`{t, d, e?}`); `GLOSSARY_TRACK_NAMES` maps IDs → labels
- Live search across name + def + example with highlighting (`gl-highlight`)
- Track filter chips (combine with search) + empty state
- `renderGlossary()`, `glOnSearch()` (updates in place to keep input focused), `glHighlight()`

### Portfolio view (`renderPortfolio()`, ~line 4306)
- Readiness tier (Foundation/Junior/Mid/Senior) via `getReadinessTier()`
- "X / 27 projects complete" headline (line 4357), meta grid (tracks covered, B/I/A done)
- Suggested next project (untouched tracks first) via `suggestNextProject()`
- Per-track cards with B/I/A status dots + skill bullets; "What you can demonstrate" list

### Progress backup & persistence  ⭐ (NOT in older handoffs — added later)
Three layers, all in the "My Progress" tab progress panel (`openProgressPanel()`):

1. **localStorage auto-save (primary, all browsers)**
   - `SAVE_KEY = 'devops_academy_progress_v1'` (line 2707)
   - `autoSave()` (debounced ~600 ms) → `collectSaveData()`; `loadProgress()` merges on boot
   - Saves: per-lesson progress, `projectsDone`, workshop `pwStepProgress`, `lastViewedLesson`, `savedAt`
2. **Export / Import JSON backup (manual, all browsers)**
   - `exportProgress()` downloads `devops-academy-progress-YYYY-MM-DD.json`
   - `importProgress(file)` validates + merges, then `pruneOrphanedProgress()`
3. **Folder auto-save via File System Access API (Chrome/Edge/Brave only)**
   - `supportsFolderSave()`, `connectFolder()` → `showDirectoryPicker({mode:'readwrite'})`
   - Directory handle persisted in **IndexedDB** (`fsIdb()` helper); `writeProgressToFolder()`
     overwrites a single `progress.json` on each save; `disconnectFolder()` clears it
   - Hidden automatically in Safari (which blocks the API)
   - This is what the `Saved Progress/` folder + its README.txt exist to support

### AI Tutor chat — REMOVED (2026-06-07)
- The app previously had a floating AI-tutor chat (`toggleChat`/`sendChat`) plus a
  per-workshop-step "Get AI guidance" button (`pwAskAI`). Both called
  `https://api.anthropic.com/v1/messages` directly, which can't work from a standalone
  browser file (CORS + no API key), so they always fell back to a "needs a backend" notice.
- **All of it was removed** — chat FAB + panel, `.chat-*` CSS, `toggleChat`/`sendChat`/
  `appendMsg`/`sendSuggestion`/`chatKeydown`, `pwAskAI` + `.pw-ask-ai-btn`/`.pw-ai-response`
  CSS, the celebration "Ask the tutor" button, and the `openChat()` helper. The app now
  makes **zero network calls**.
- The **offline** ELI5 (💡) and "Explain this code" (💬) lesson toggles are unaffected and
  still work — those were never part of the AI chat.

### Accessibility / misc
- Font-size zoom levels persisted under `ZOOM_KEY = 'devops_academy_zoom'` (line 1031)
- Responsive (mobile media queries ~420 px); `TRACK_TIMES` holds per-track minute estimates

## Architecture cheat sheet

```js
// ─── STATE ───
state = { view, currentTrack, currentLessonIdx, progress, quizState, interviewState, lastViewedLesson }
state.progress[tid][lesson.id] = true   // CRITICAL: keyed by lesson.id ('d1','d2'), NOT numeric index
projectsDone = { [pid]: true }           // flat map
pwState = { projectId, currentStep, steps, stepProgress }   // workshop state
glState = { query, track, _focused }     // glossary state

// ─── DATA OBJECTS ───
TRACKS = { docker, k8s, git, tf, linux, networking, python, observability, sre, interview }
PROJECTS = { ... }              // 27 projects (9 tracks × 3 levels)
WORKSHOP_STEPS = { dp1..3, kp1..3, gp1..3, tp1..3, lp1..3, np1..3, pyp1..3, op1..3, sp1..3 }  // 27 × 5 steps
ICONS = { docker, k8s, git, tf, linux, networking, python, observability, sre, interview, master }
GLOSSARY = { docker:[{t,d,e?}], ... }   // 136 entries
TRACK_PORTFOLIO_SUMMARIES = { docker:{beginner,intermediate,advanced}, ... }
TRACK_TIMES = { docker, k8s, ... }      // per-track minute estimates

// ─── KEY FUNCTIONS ───
showView(name)                    // routes dashboard/progress/projects/glossary/portfolio/master
goInterviewTrack()                // Interview Prep entry (NOT via showView)
trackDone(tid)                    // counts state.progress[tid][lesson.id] hits
allTracksComplete()              // true when all 9 cores at 100%
openWorkshop(pid, trackName, _)   // loads WORKSHOP_STEPS[pid]
renderWorkshopStep()              // injects ws-cost-banner on step 0 of paid projects
buildMasterChallenge()            // shuffles question pool into MASTER_CHALLENGE_QUESTIONS
getReadinessTier()                // foundation/junior/mid/senior
suggestNextProject()              // easiest project from untouched-then-incomplete tracks
renderGlossary() / glOnSearch()   // glossary view + in-place search
// backup: autoSave, loadProgress, collectSaveData, exportProgress, importProgress,
//         connectFolder, disconnectFolder, writeProgressToFolder, supportsFolderSave
// (AI tutor chat + per-step pwAskAI guidance were removed 2026-06-07 — no network calls remain)
```

## Critical conventions (still accurate)

1. **`trackDone()` uses lesson ID string keys** (`d1`, `d2`), NOT numeric indices.
   When simulating completion in tests, set `state.progress[tid][lesson.id] = true`.
2. **Buttons containing SVG icons must be updated via `innerHTML`, not `textContent`**
   (e.g. the Master Challenge nav lock/unlock) — `textContent` wipes the SVG child nodes.
3. **The nav uses index-based `active` assignment** in `render()`:
   `btns[0]` Dashboard, `[1]` My Progress, `[2]` Projects, `[3]` Key Terms, `[4]` Portfolio,
   `[5]` Interview Prep, `[6]` Master Challenge. Glossary/Portfolio routes also use
   `getElementById` (`glossaryNavBtn`, `portfolioNavBtn`, `masterNavBtn`, `interviewNavBtn`) — more robust.
4. **Cost data lives on project objects** as `p.costs`. Surfaces in 3 places: card badge,
   card callout inside `.proj-card-steps`, workshop banner on step 0 (`.ws-cost-banner`).
5. **Master Challenge styling** reuses the standard track-card structure with
   `master-card / master-icon / master-name / master-btn` classes layered on. It is a
   standard grid cell (NOT full-row `grid-column: 1/-1` — an earlier iteration tried that
   and was reverted; the user wants it to LOOK like the other cards).

## ⚠️ Known issues / things to verify

- **Project-count inconsistency in the dashboard hero (line ~3597):** the hero text reads
  *"12 hands-on projects (~50 hrs)"* but the app actually defines and surfaces **27**
  projects (portfolio headline at line 4357 correctly says "27 projects"). The "12" is
  stale copy and should be reconciled to 27 (or whatever the intended figure is). Left
  unchanged pending the owner's call on the right number/wording.
- When editing, re-validate the `<script>` block (`node --check` on the extracted script)
  and sanity-check in a browser, since there's no test harness checked into this folder.

---

# APP #2 — `SRE Lab.html` ("SRE Interview Lab")

A separate, standalone hands-on lab focused on getting interview-ready *by doing* —
Kubernetes-first, with Docker, Terraform, Python, and SRE principles. ~3035 lines.
**No cross-links to App #1** — fully independent (own localStorage key, own styling).

## 6 nav tabs (`NAV`, ~line 390; routed via `showView(name)` → `renderView()`)
`Dashboard → Learn → kubectl Lab → Python Lab → Interview → Cheatsheet`

## Feature set

- **Learn** (`LEARN`, ~line 497): 7 modules, ~36 lessons — Kubernetes Core (8),
  K8s Operations (7), K8s Troubleshooting (4), Docker (5), Terraform (5), Python (3),
  SRE Principles (5). Rich content helpers: `eli5()`, `code()`, `diagram()`, `tip()`,
  `warn()`, `gotcha()`, `keypoints()`, `checkq()` (inline multiple-choice), `table()`.
- **kubectl Lab** (`MISSIONS`, ~line 1893): 14 missions against a **simulated cluster**
  (`makeCluster()`, 3 nodes, broken-on-purpose pods: CrashLoopBackOff / ImagePullBackOff /
  Pending). `runKubectl(raw, cl)` (~line 1995) parses real kubectl (get/describe/logs/scale/
  set image/rollout/delete/expose/exec/port-forward + flags). Each mission has a `check()`
  validator; `checkMissions()` detects completion and toasts. `labReset()` re-breaks the cluster.
- **Python Lab** (`PY_EX`, ~line 2229): 8 executable exercises (warm-up → advanced) run via
  **Pyodide v0.26.2** (WASM, lazy-loaded from CDN, `loadPyodideOnce()`). `pyRun()` executes,
  `pyCheck()` runs a pytest-style test suite. Each: prompt, ELI5, starter, solution, tests.
- **Interview** (~line 2866): two modes — **Flashcards** (`FLASH`, ~30 cards, filter by
  topic, shuffle, ←/→/space keys) and **Scenarios** (`SCENARIOS`, ~13 "walk me through…"
  questions with structured model answers).
- **Cheatsheet** (`CHEATS`, ~line 2389): 7 reference categories (kubectl inspect/operate,
  Pod YAML, Docker, Terraform, Python for SRE, SRE numbers), 70+ rows.
- **Dashboard** (`renderDashboard()`, ~line 2477): progress stats + a 15-day study plan
  (`PLAN`, ~line 2372) with checkbox tracking (`togglePlan()`).

## State & persistence
```js
state = { view, lessonsDone:{}, missionsDone:{}, pyDone:{}, planDone:{}, curLesson, curMission, curPy }
```
- `save()` (debounced 500 ms) / `load()` → localStorage key **`sre_lab_progress_v1`** (line 400)
- Export/import: `exportProgress()` → `sre-lab-progress-YYYY-MM-DD.json`; `importProgressFile()`
- Folder auto-save (Chrome/Edge/Brave) via File System Access API: `connectFolder()` /
  `disconnectFolder()` → writes `sre-lab-progress.json`
- Light/dark theme toggle (`toggleTheme()`); toasts via `showToast()`

## Styling identity
Dark-default (navy `--bg:#0a0e1a`, indigo `--accent:#6366f1`), tech-colored accents
(`--k8s`, `--docker`, `--tf`, `--python`, `--sre`, `--term`). Fonts: **Sora** (body),
**JetBrains Mono** (code). macOS-style `.terminal`, 3D-flip `.flash` cards.

---

# The `Saved Progress/` folder

Holds portable `progress.json` / `*-progress-*.json` backup snapshots. Because it lives in
iCloud Drive, anything saved here syncs across devices. `README.txt` is the **end-user**
guide explaining:
- Progress already auto-saves to the browser's localStorage on every click.
- Safari can't silently write files (sandbox) → use Export/Import, or point Safari's
  download location at this folder.
- Chrome/Edge/Brave can auto-save here hands-free via the "Choose folder…" File System
  Access feature in the app's progress panel.

Keep `README.txt` in sync if the backup UI/wording in either app changes.

---

# How both apps are built/maintained now

- **No build step, no generator.** Each app is a single hand-edited `.html` file. Earlier
  sessions used Python generators + jsdom tests in a sandbox (`/tmp`, `/mnt`); that tooling
  is gone and is not needed — edit the HTML directly.
- When you change JS, extract the `<script>` and run `node --check` to catch syntax errors,
  then open the file in a browser to verify behavior (there's no checked-in test harness).
- String-anchor edits must match exactly (indentation, quotes, escapes).
- Don't use `textContent` on any element that contains an SVG icon (wipes the icon).

# Ideas for future work (not yet implemented)

- Reconcile the App #1 hero "12 projects" vs. actual 27 (see Known Issues).
- Full lesson-text search in App #1 (glossary search currently only covers the 136 terms).
- Export portfolio readiness as a shareable image / PDF.
- Cross-link the two apps (e.g. App #1 Interview Prep ↔ App #2 SRE Interview Lab).
- Mobile gesture polish for workshop / lab step navigation.

# How to continue work in a new chat

Open this folder and say: **"I'm continuing work on the SRE Self-Learning apps. Read
HANDOFF.md, then the relevant .html file (`Devops Academy Course.html` or `SRE Lab.html`)."**
That's all the context needed — no prior tool-call history required.
