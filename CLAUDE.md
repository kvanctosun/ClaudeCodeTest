# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

GitHub: `github.com/kvanctosun/ClaudeCodeTest`

## Git Workflow

After every meaningful change, commit and push to GitHub so no work is ever lost:

```bash
git add <files>
git commit -m "<type>: <short description>"
git push
```

Commit message format:
- `feat:` — new feature or content
- `fix:` — bug fix
- `refactor:` — restructuring without behavior change
- `style:` — visual/CSS changes only
- `chore:` — config, docs, or tooling changes

Keep messages specific and descriptive (e.g. `feat: add level 6 with diagonal corridor` not `update parking`). Never batch unrelated changes into one commit.

## Project Structure

Two standalone browser games — each is a single self-contained HTML file with all CSS and JavaScript inline. No build step, no dependencies, no package manager.

- `tictactoe.html` — Tic Tac Toe (2-player or vs minimax CPU)
- `parking.html` — Retro Park (top-down canvas parking puzzle, 5 levels)

To run: open either HTML file directly in a browser.

## Architecture

### tictactoe.html
- State: `board` (9-element array), `current` (active player), `gameOver`, `vsCPU`, `score`
- CPU uses full minimax (`minimax` / `bestMove` functions) — always plays optimally
- `WINS` constant defines all 8 winning combos; `checkWin` / `checkWinFor` are separate functions (one uses the live `board`, one takes an array argument for minimax recursion)

### parking.html
- Canvas-based (960×640), pixel-art rendering (`imageSmoothingEnabled = false`)
- `LEVELS` array defines each level: `spots` (parking targets), `starts` (car initial positions), `obstacles` (walls/cones/pillars), `parkOrder` (required parking sequence, `null` = free order)
- Car objects created via `makeCar()`, spot objects via `makeSpot()`
- Car models: `sedan`, `suv`, `sports`, `truck`, `compact` — each with fixed pixel dimensions in `CAR_DIMS`
- Gameplay loop: click a car to select → steer with arrow keys → park within `PARK_DIST` px and `PARK_ANGLE` degrees of the target spot
- Collision detection runs against lot boundaries and all obstacles each frame
