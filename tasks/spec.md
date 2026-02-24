# Task Log UI Specification (Single HTML)

## 1. Goal
Build a **single-file HTML task log UI** that records work traces with automatic timestamps.

This is **not** a project management tool. It is a lightweight writing surface where each line has a fixed start time and optional end time.

Priority:
1. Lightweight
2. Simplicity
3. Robustness

## 2. Output Constraints
- Create only one runtime file: `tasks/index.html`
- Put all CSS and JavaScript inside that HTML file
- No external libraries
- No external CSS/JS
- Desktop browser first (no mobile-specific implementation required)

## 3. Current Persistence Rule
- UI settings are persisted in `localStorage`
- Storage key: `tasklog_ui_settings_v1`
- Persisted values:
  - `colorMode`
  - `fontPreset`
  - `fontCustom`
- Task rows themselves are **not** persisted

## 4. UI Layout
Single column layout:
1. Top-right gear button (open settings modal)
2. `全出力` button
3. Task list area
4. On page load, auto-create the first empty row

## 4.1 Visual Design (Glassmorphism)
- Apply glassmorphism styling to current UI while keeping existing layout metrics intact.
- Keep sizes/radius/spacing of existing controls as-is (no structural resize).
- Background must have layered feel:
  - Base gradient per color mode
  - At least two softly blurred circular blobs floating in background
- Glass surfaces must be used on:
  - Task list container
  - Export button
  - Gear button
  - Settings modal panel
  - Modal close button
  - Modal form controls (`select`, `input`)
- Glass surface characteristics:
  - Semi-transparent gradient fill
  - Soft border line
  - Backdrop blur + mild saturation
  - Subtle outer shadow + light inset highlight
- Color mode adaptation examples:
  - Light: white/gray atmospheric blobs on bright background
  - Iceberg Dark: deep navy background with ice-gray blobs

## 5. Line Format
Each row consists of:
- `indent` (visual only, full-width spaces)
- `status/prefix` (non-editable)
- `title` (editable task name)

### 5.1 Incomplete format
`🟨HHMM-　タスク名`

### 5.2 Complete format
`✅HHMM-HHMM　タスク名`

Rules:
- HHMM has no seconds
- Use system current time
- Separator after prefix is full-width space `　`
- Start time is fixed when the row is created
- End time is fixed when marked complete

## 6. Row Creation Behavior
- Press `Enter` inside a title to create next row
- New row start time = current HHMM
- New row initial state = incomplete (`🟨HHMM-　`)
- **Indent inheritance required**: new row copies indent level from source row

Example:
- Before Enter
  - `🟨1727-　テスト`
  - `　🟨1727-　テスト２`
- After Enter on second row
  - `🟨1727-　テスト`
  - `　🟨1727-　テスト２`
  - `　🟨1727-　`

## 7. Status Toggle Behavior
Clicking prefix area toggles state.
`Shift+Enter` (pressed in a row title) must perform the same toggle.

### 7.1 Incomplete -> Complete
- Click `🟨...`
- Set end time = current HHMM
- Render `✅開始-終了　タスク名`

### 7.2 Complete -> Incomplete
- Click `✅...`
- Clear end time
- Render back to `🟨開始-　タスク名`
- Keep original start time

## 8. Edit Control
- Timestamp/prefix area must be non-editable
- Only task title is editable
- Prevent structure breakage by separating prefix and title into different elements

Recommended DOM per row:
- `.row`
  - `.indent` (text node of repeated full-width spaces)
  - `.status` (clickable prefix text)
  - `.title` (`contenteditable=true`)

## 9. Indent Behavior
- `Tab`: add one full-width space indent at row head
- `Shift+Tab`: remove one full-width space indent (min 0)
- Indent is visual only; no parent/child logic

## 10. Delete Behavior
- If title is empty and user presses `Backspace`:
  - Remove that row
  - Move caret to end of previous row title (if exists)

## 11. Caret Hit Area Behavior
- Clicking a row should be treated like editor trailing-space click:
  - From task text through the right edge of the list box, place caret at end of that row title
- If user clicks inside list box below existing rows:
  - Place caret at end of the nearest upper row (practically the last row)

## 12. Export Button (`全出力`) Behavior
On click:
1. Serialize all rows in display format
2. Join with `\n`
3. Copy to clipboard
4. Clear all rows immediately (no confirm dialog)
5. Recreate first row (same as initial page load) so user can continue typing

### 12.1 Output example
`✅1706-1820　開発`
`　✅1706-1710　設計確認`
`　🟨1710-　開発`

## 13. Settings Modal
Open by clicking gear icon.

### 13.1 Font controls
- System monospace preset
- Additional monospace presets
- Custom `font-family` text input

### 13.2 Color mode options
- Light
- Dark
- Iceberg Light
- Iceberg Dark

Each mode must provide its own glass tokens (background gradient, blob colors, panel alpha, border alpha, shadow profile), not only text colors.

### 13.3 Default mode
- Default color mode must be `Iceberg Dark` when no saved setting exists

### 13.4 Save/restore
- Any settings change should be saved to `localStorage`
- On page load:
  - Read storage
  - Apply stored values if valid
  - Fallback to defaults when missing/invalid

## 14. Data Model (per row)
Store row state in `dataset`:
- `data-start`: string HHMM
- `data-end`: string HHMM or empty
- `data-done`: `'1'` or `'0'`
- `data-indent`: numeric string (0+)

## 15. Essential Functions (recommended)
- `hhmmNow()` -> current HHMM
- `rowPrefix(row)` -> formatted prefix text
- `renderRow(row)` -> refresh indent + status display from dataset
- `createRow(afterRow, focusTitle, indentLevel=0)` -> append/insert row
- `placeCaretEnd(el)` -> move caret to end
- `toggleRowState(row)` -> toggle done/undone and render
- `lineText(row)` -> serialize one row for export
- `copyText(text)` -> clipboard write with fallback
- `applyTheme(mode)`
- `applyFont()`
- `saveSettings()` / `loadSettings()`

## 16. Event Requirements
- Click on `.status`: toggle done/undone
- Click on `.row` (excluding `.status`): place caret at row-end
- Click on list blank area under rows: place caret at last row-end
- Keydown on `.title`:
  - `Enter`: create next row with indent inheritance
  - `Tab` / `Shift+Tab`: indent adjust
  - `Shift+Enter`: toggle done/undone
  - `Backspace` on empty title: delete row and move caret
- Export button click: export+copy+clear+create first row
- Settings controls change/input: apply immediately and save

## 17. Implementation Notes
- Keep DOM minimal and explicit
- Avoid unnecessary abstractions
- Keep code readable and compact
- Prefer event delegation on list container
- Preserve full-width space handling exactly
