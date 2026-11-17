# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

GitHub Pages site for the paper *"Dynamic Relative-Motion and Frustum-Safe Tracking for Distributed 3D UAV Formations"* (earlier working title: "Distributed 3D Leader–Follower Formation Control with Field-of-View Safety via Control Barrier Functions") by Santjoko, Suganda, Pan, and Hu (University of Houston). Plain static HTML; no JS framework or build step.

## Deployment

- `.github/workflows/jekyll-gh-pages.yml` runs on every push to `main`: builds with `actions/jekyll-build-pages` (source `./`, destination `./_site`) and deploys to GitHub Pages. There's no `_config.yml` and no Liquid in the HTML, so Jekyll effectively just copies files through.
- `CNAME` pins the custom domain `research.noelsadventure.my.id`; don't delete it or the Pages domain resets. The same URL is hardcoded in `index.html` meta tags (e.g. `citation_pdf_url`), so update those if the domain changes.
- The site is served from a single root `index.html` that pulls everything from `static/`. The Bulma CSS/JS and FontAwesome bundle are vendored under `static/css/` and `static/js/` — no CDN dependency for the core layout.

## Site structure — non-obvious bits

- `index.html` includes the **Google Analytics tag `G-PYVRSFMDRL`** carried over from the prior site, plus a **MathJax** script for the inline LaTeX in the methodology figure caption. Both are easy to miss when editing the head.
- The teaser is a local `<video>` (`static/videos/VIDEO_3DLFF.mp4`), not a YouTube embed. Experiments has five parts: Python sim (`cbf_two_level.mp4`), Gazebo/CrazySim Crazyflie (`Simulation-compressed-low.mp4`, `scenario.png`, `crazysim_conflicting.png`), a separate 21-UAV Gazebo section (`formation_chase_topdown_tier.mp4`), and two ModalAI Seeker hardware sections (`TrackingExp.mp4`, `SafetyExp_margin.mp4`). The Python and CrazySim sections are kept from the previous paper version on purpose.
- Geometry and architecture figures (`Geometry.png`, `Control_Architecture.png`) are the lab's own figures. The intro figure was removed. `intro2.png`, `diagrammmm.png`, `architecture4.png`, `topview.png`, `exp_shorten.png` and `Experiment-compressed-low.mp4` are now unreferenced.
- The "Paper" / "arXiv" / "Code" hero buttons: only **Paper** is wired up (points to `static/pdfs/paper.pdf`). arXiv and Code are placeholder `href="#"` until the paper is public — update both when the preprint and code are released.
- Sections alternate white ↔ light-gray backgrounds (`.section` and `.section.is-light-bg`) to act as visual dividers. The pattern is white → white → light → white → light → white → light → white → white (Title, Teaser, Abstract, Intro, Key, Method, Exp, Conclusion, BibTeX). If you add a section, pick the bg that maintains contrast with its neighbors.
- The page uses the modernized Academic Project Page Template's CSS (`static/css/index.css`) — that file owns the scroll-to-top button, the BibTeX copy button, and the `.figure-caption` / `.section.is-light-bg` rules. Be aware before swapping CSS.
- `static/images/icon.png` is the lab logo copied from the StewartPlatformSafeControl project page, and it serves as the favicon. `static/images/favicon.svg` is the original template author's favicon, kept around but unused — safe to delete.

## Assets policy

- `.gitignore` excludes `references/`, `*.pdf`, `*.zip`, and a long list of LaTeX/secret/editor artifacts. The paper PDF that's actually served (`static/pdfs/paper.pdf`) is a deliberate exception — it lives outside `references/` so the gitignore rules don't catch it, but the `*.pdf` rule does still match it. If `git add static/pdfs/paper.pdf` fails with "ignored by .gitignore", use `git add -f static/pdfs/paper.pdf` (or add a `!static/pdfs/*.pdf` negation rule).
- **Currently `static/pdfs/paper.pdf` does not exist in the repo** (no `static/pdfs/` directory, and it is not tracked), so the Paper button and `citation_pdf_url` are dead links until the PDF is added and force-added with `git add -f`.
- Source materials (paper LaTeX zip, raw simulation video, original `Paper.pdf`) historically lived in `OLD/references/` and were never published. If you need them again, restore from git history at commit `d3ae773` or earlier.

`README.md` is a short public-facing description (authors, live site link); it isn't a dev guide.

## Local preview

No build step. From the repo root:

```powershell
python -m http.server 8000
# then open http://localhost:8000
```
