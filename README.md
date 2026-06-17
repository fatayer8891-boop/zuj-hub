# ZUJ Course Development Hub

Central repository for the ZUJ course program. It does two jobs:

1. **Coordination center** — the protocol, context index, roadmap index, and templates that keep multi-AI course development (Manus + Claude) organized.
2. **Website source** — a [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) site that assembles the three course repositories into one professional, searchable site and deploys it to GitHub Pages.

## The repo model

Four repositories:

- `zuj-iot`, `zuj-robotics`, `zuj-computer-vision` — one per course. Each holds its own material, `COURSE_CONTEXT.md`, and `ROADMAP.md`, and uses a lean `main → develop → feature/[task]` branch flow.
- `zuj-hub` — **this repo**. Pulls the three course repos in as submodules and builds the site.

The course repos stay fully independent. This repo consumes them; they don't depend on it.

## Layout

```
zuj-hub/
├── mkdocs.yml                 # site config (theme, nav)
├── requirements.txt           # build dependencies
├── docs/
│   └── index.md               # site landing page
│   └── iot/                   # ← course repos mount here as submodules
│   └── robotics/
│   └── computer-vision/
├── hub/
│   ├── HANDOFF_PROTOCOL.md    # how the AIs coordinate (read this)
│   ├── CONTEXT_INDEX.md       # links to each COURSE_CONTEXT.md
│   └── ROADMAP_INDEX.md       # cross-course roadmap view
├── templates/
│   ├── COURSE_CONTEXT.template.md
│   └── ROADMAP.template.md
└── .github/workflows/deploy.yml
```

## Course content contract

For a course repo to slot cleanly into the site, it should have an `index.md` at its root as the course landing page, plus its material as markdown. The hub's `mkdocs.yml` nav then points at `iot/index.md`, `iot/<unit>.md`, etc.

## Adding a course to the site

1. Mount the course repo as a submodule under `docs/`:
   ```bash
   git submodule add https://github.com/YOUR-USERNAME/zuj-iot.git docs/iot
   ```
2. Uncomment that course's block in `mkdocs.yml` under `nav:` and point the entries at its files.
3. Preview locally (below), then commit. Repeat for the other two courses.

## Build & preview locally

```bash
pip install -r requirements.txt
mkdocs serve      # live preview at http://127.0.0.1:8000
mkdocs build      # writes the static site to ./site
```

If you cloned this repo fresh, pull the submodule content first:
```bash
git submodule update --init --recursive
```

## Pull the latest course content into the site

Course repos evolve independently. To bring their latest commits into the hub:
```bash
git submodule update --remote
git add docs/
git commit -m "Update course content"
```
Pushing that commit (or running the workflow manually) rebuilds the site.

## Deployment

- In **Settings → Pages**, set the source to **GitHub Actions**.
- The workflow in `.github/workflows/deploy.yml` builds and deploys on every push to `main`, and exposes a **Run workflow** button (`workflow_dispatch`) for manual rebuilds — this is your "rebuild after a course changed" trigger.
- If the course repos are **private**, the default token can't read them. Create a Personal Access Token with repo access, add it as a repository secret named `SUBMODULES_PAT`, and uncomment the `token:` line in the workflow.

## Working principle

No AI commits autonomously. Assistants propose changes; a human reviews and commits. See `hub/HANDOFF_PROTOCOL.md`.
