# TrussLab | Interactive Truss Force Analyzer

Interactive 2D structural analysis: tension, compression and deformation. Draw a truss, place supports and loads, and see the forces. An educational engineering sandbox.

Everything lives in one file, `index.html`. No build step, no dependencies, no folders.

## Put it on GitHub Pages

1. Open your `Truss-Force-Analyzer` repository on GitHub.
2. Choose **Add file > Upload files** and drag in `index.html`. It replaces your current `index.html` because the name is the same.
3. Commit. After about a minute, hard-refresh your page (Ctrl+Shift+R).

Your old files (a separate stylesheet or script) are no longer used. You can leave them or delete them.

## How to use it

- **Examples**: load a Pratt-style, Warren or King-post truss.
- **Tools** (also keys V, J, M, L, P, R, X): Move / edit, Joint, Member, Load, Pin, Roller, Delete. Plus Clear all, Undo / Redo, Fit and zoom. Ctrl + scroll also zooms.
  - Move: drag joints (snaps to a 0.25 m grid), click a joint or member to inspect it, drag empty space to pan.
  - Joint: click empty space. Member: click two joints. Load: click a joint to apply or remove the load. Pin / Roller: click a joint to toggle the support.
- **Parameters**: the joint load (kN) sets every loaded joint at once; EA is shared by all members; the magnification is a multiplier on an auto-scaled deformation, so the shape is always visible.
- **Analysis is live.** It re-solves every time you drag or edit. "Analyze forces" replays the deformation animation.
- **Results** (second section): largest force, maximum displacement, the member force table (click a row to highlight the member), reactions, and the analysis status.
- **Export results (.csv)**: member forces, reactions and joint displacements.

## The solver

It uses the direct stiffness method, so it handles any truss you draw, determinate or indeterminate:

- member stiffness `k = EA / L`, assembled into `K u = F`
- member force `N = (EA / L) [-c -s c s] u` (tension positive)
- reactions `R = K u - F`
- degree of indeterminacy `m + r - 2j`

Unstable structures (mechanisms, missing supports) are detected and explained instead of giving wrong numbers.

I checked it against hand solutions: a Pratt truss with a single load, a king-post truss (tie 8.333 kN, rafters 9.718 kN, king post zero) and a Warren truss (peak chord force 30 kN), plus an indeterminate truss where equilibrium still holds.

Model: linear elastic, 2D pin-jointed truss, small displacement. Loads act at joints. Supports are a pin (x/y constrained) or a vertical roller (y constrained). Not intended for real-world structural design or safety decisions.

## Tweak it

- Default values (joint load 10 kN, EA 100,000 kN): the `P` variable in the `<script>`.
- Example geometry: the `EXAMPLES` object.
- Source link in the header: the `pill` link in `index.html`.
- In the browser console, `TrussLab.solve()` returns the full result object for the current truss.
