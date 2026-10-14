# stardraft-editor.github.io

The StarDraft 2 website: https://stardraft-editor.github.io

The website is managed here, apart from the program
([stardraft-editor/stardraft](https://github.com/stardraft-editor/stardraft)).

- `build.py` makes `index.html`. The roadmap part comes from `docs/project/ROADMAP.md` in the stardraft repository;
  everything else (texts, Korean, screenshots, layout) is here. Do not edit `index.html` by hand.
- `build.py` also makes `privacy.html`, the privacy policy (`PRIVACY` in `build.py`, English only). Its facts
  come from the program: what feedback, crash reports, the update check and the euddraft download send. When the
  program changes that, change the page and `PRIVACY_DATE`.
- `python build.py` reads ROADMAP.md from GitHub; `python build.py --roadmap <path>` reads a local one.
- `assets/shots/` are screenshots of the program; `build.py` says how they are made. `assets/icons/` are copies of the
  program's UI icons. `assets/social.jpg` is the link preview (Discord and other sites).

## How the roadmap gets to the page

ROADMAP.md in stardraft is the source. The page shows its `## Now`, `## Next` and `## Later` sections: for each item
(`- [ ] **R<n> Title.** *Tag:* area.`), the title, the tag and the first indented paragraph. The rest of an item is
only in ROADMAP.md. Done items (`- [x]`) are not shown. The text above the first item of a section is that section's intro on the page.

The page is rebuilt in three ways:

1. **A push of ROADMAP.md to stardraft main.** stardraft's Roadmap workflow (`.github/workflows/roadmap.yml` there)
   clones this repository, runs `build.py --roadmap docs/project/ROADMAP.md` and pushes `index.html` here. It uses the
   secret `SITE_DEPLOY_KEY` in stardraft: the private half of a write deploy key of this repository (only this one).
2. **The Build workflow here** (`.github/workflows/build.yml`): after a push of `build.py`, `assets/icons/` or the
   workflow, each day at 04:17 UTC, and by hand (`gh workflow run Build`). It reads ROADMAP.md from stardraft main.
3. **By hand:** `python build.py`, then commit `index.html` with `build.py`.

GitHub Pages serves `main` as it is, about a minute after a push.

## Changing the page

- **A roadmap item (add, change, move, finish):** change ROADMAP.md in stardraft, not this repository. Its push
  rebuilds the page.
- **Korean:** each roadmap item needs a Korean title and paragraph in `KO_ROADMAP` in `build.py` (by its id, `R<n>`).
  Without one, the item shows in English in Korean, and the build names it ("No Korean text for R52"). Section intros
  are in `KO_INTROS`, tags in `KO_TAGS`. Add them here when an item is added or its text changes, and push.
- **Any other text** (hero, personas, buttons, footer): `EN` and `KO` in `build.py`, both languages together.
- **Layout and style:** `TEMPLATE` in `build.py`.
- After a change to `build.py`, run `python build.py` and commit `index.html` with it (the Build workflow would also
  do it). `python build.py --check` exits 1 if `index.html` is out of date.

Links to other sites open in a new tab (`new_tab` in `build.py` adds it); the download button does not. The download
button gets the newest release's `win64.zip` from GitHub's API in the browser, and the release page without it.
