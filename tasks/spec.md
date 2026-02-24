# Task Log UI Specification (Single HTML)

## 1. Goal
Build a single-file HTML task log UI that records work traces with automatic timestamps, plus a plain memo area.

This is not a project management tool. It is a lightweight writing surface:
- Top: timestamped task lines
- Bottom: free-form memo textarea

Priority:
1. Lightweight
2. Simplicity
3. Robustness

## 2. Output Constraints
- Create only one runtime file: `tasks/index.html`
- Put all CSS and JavaScript inside that HTML file
- No external libraries
- No external CSS/JS
- Desktop browser first

## 3. Persistence Rule
- UI settings are persisted in `localStorage`
- Storage key: `tasklog_ui_settings_v1`
- Persisted values:
  - `colorMode`
  - `fontPreset`
  - `fontCustom`
- Task rows are not persisted
- Memo textarea content is not persisted

## 4. UI Layout
Single column app with two main zones:
1. Top toolbar: `全出力` button and gear button side-by-side (`gear` immediately right of export)
2. Editor work area (fills remaining app height), split into:
  - Top task list pane
  - Bottom memo textarea pane

Rules:
- Default split ratio is 50% : 50%
- User can drag the horizontal splitter to resize task/memo pane heights
- Both panes must show vertical scrollbars when content exceeds visible height

## 5. Visual Design (Glassmorphism)
Apply glassmorphism styling to:
- Task list container
- Memo textarea
- Export button
- Gear button
- Settings modal panel
- Modal close button
- Modal form controls (`select`, `input`)

Background must keep layered atmosphere (gradient + soft blobs).

## 6. Task Line Format
Each task row consists of:
- `indent` (visual only, full-width spaces)
- `status/prefix` (non-editable)
- `title` (editable task name)

Display format:
- Incomplete: `🟨HHMM-　タスク名`
- Complete: `✅HHMM-HHMM　タスク名`

Rules:
- Time format is `HHMM` (no seconds)
- Start time fixed at row creation
- End time fixed when marked complete
- Separator after prefix is full-width space `　`

## 7. Row Creation Behavior
- `Enter` in a title creates next row
- New row start time = current `HHMM`
- New row state = incomplete
- Indent inheritance required (copy indent from source row)

## 8. Status Toggle Behavior
Toggle by:
- Clicking `.status`
- Pressing `Alt+C` in `.title`

State rules:
- Incomplete -> Complete: set end time to current `HHMM`
- Complete -> Incomplete: clear end time, keep original start time

## 9. Edit Control
- Prefix area is non-editable
- Only `.title` is editable

Recommended row DOM:
- `.row`
  - `.indent`
  - `.status`
  - `.title` (`contenteditable=true`)

## 10. Indent Behavior
- `Tab`: add one full-width-space indent
- `Shift+Tab`: remove one indent (min 0)

## 11. Delete Behavior
- If title is empty and user presses `Backspace`:
  - Remove row
  - Move caret to end of previous row title (if exists)

## 12. Caret Hit Area Behavior
- Clicking row area (except `.status`) places caret at end of that row title
- Clicking blank space under rows places caret at end of last row title

## 13. Export Button (`全出力`) Behavior
On click:
1. Serialize all task rows in display format
2. Join with `\n`
3. Copy to clipboard
4. Clear task rows immediately (no confirm)
5. Recreate first empty task row

Notes:
- Export/clear applies to task rows only
- Memo textarea is unaffected

## 14. Memo Textarea Behavior
- Dedicated free-form memo area
- No timestamp/status/indent logic
- Must include a placeholder that explains it is memo-only
- Not persisted

## 15. Settings Modal
Open by gear button.

Controls:
- Font preset
- Custom `font-family`
- Color mode:
  - Aurora Glass
  - Light
  - Dark
  - Iceberg Light
  - Iceberg Dark

Defaults:
- Default color mode is `Aurora Glass`

Save/restore:
- Any settings change saves to `localStorage`
- On load, apply valid stored values; fallback to defaults

## 16. Data Model (per task row)
Store row state in dataset:
- `data-start`: string `HHMM`
- `data-end`: string `HHMM` or empty
- `data-done`: `'1'` or `'0'`
- `data-indent`: numeric string (0+)

## 17. Essential Functions (recommended)
- `hhmmNow()`
- `rowPrefix(row)`
- `renderRow(row)`
- `createRow(afterRow, focusTitle, indentLevel=0)`
- `placeCaretEnd(el)`
- `toggleRowState(row)`
- `lineText(row)`
- `copyText(text)`
- `setTaskRatio(percent)`
- `initSplitResize()`
- `applyTheme(mode)`
- `applyFont()`
- `saveSettings()` / `loadSettings()`

## 18. Event Requirements
- Click on `.status`: toggle done/undone
- Click on `.row` (excluding `.status`): place caret at row-end
- Click blank area in list: place caret at last row-end
- Keydown on `.title`:
  - `Enter`: create next row (indent inheritance)
  - `Alt+C`: toggle done/undone
  - `Tab` / `Shift+Tab`: indent adjust
  - `Backspace` on empty title: delete row and move caret
- `全出力` click: export + copy + clear task rows + create first row
- Splitter drag: resize top/bottom pane ratio
- Settings control change/input: apply immediately and save

## 19. Implementation Notes
- Keep DOM minimal and explicit
- Avoid unnecessary abstractions
- Prefer event delegation for task list
- Preserve full-width space handling exactly
