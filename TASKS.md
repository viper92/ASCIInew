# Proposed Maintenance Tasks

## 1) Typo fix task
**Task:** Correct the misspelling `simplier` to `simpler` in the controls-update comment.

- **Why:** Small but visible quality issue in inline documentation.
- **Location:** `asciinew.html` near the controls event handler comment.
- **Acceptance criteria:** Comment reads naturally and keeps the original intent.

## 2) Bug fix task
**Task:** Guard half-block sampling against odd frame heights to avoid reading past the RGBA buffer.

- **Why:** In half-block mode, the renderer reads `y + 1` for the lower pixel row. If `h` is odd, the final iteration can compute an out-of-range index and produce invalid color data.
- **Location:** `renderFrame()` in the half-block branch where `i2` is computed from `(y+1)`.
- **Acceptance criteria:**
  - No out-of-bounds read when `h` is odd.
  - Visual output remains stable in half-block mode.
  - Logic is explicit (e.g., clamp `y+1` to `h-1` or skip the final unmatched row).

## 3) Code comment / documentation discrepancy task
**Task:** Align palette UI labels/options with actual implemented palette behavior (or implement missing palette modes).

- **Why:** The UI offers `ansi16`, `xterm16`, and `vga-auto`, but palette selection logic currently falls back to DOS for unhandled values. This makes the current labels/documentation misleading.
- **Location:** Palette `<select id="paletteMode">` options and `updateDerived()` palette mapping logic.
- **Acceptance criteria (pick one path):**
  - **A:** Remove/rename unsupported options to match implemented behavior, **or**
  - **B:** Implement real handling for all listed options.
  - Add a short inline comment documenting the final behavior.

## 4) Test improvement task
**Task:** Add automated tests for rendering edge cases and control-to-state mapping.

- **Why:** Current project has no automated regression coverage for key logic paths.
- **Suggested scope:**
  - Unit test for palette mapping behavior in `updateDerived()`.
  - Unit/integration test for half-block rendering with odd heights to prevent buffer overread regressions.
- **Acceptance criteria:**
  - Tests fail before fix and pass after fix for at least one known issue (odd-height half-block case).
  - Test command is documented in project docs.
