# Hello World Page Specification

## 1. Purpose
Create a single-file "Hello, World." landing page with soft glassmorphism styling and subtle ambient motion.

## 2. Output File
- Target: `index.html` (project root)
- HTML + CSS only
- No JavaScript

## 3. Design Tokens (`:root`)
Define variables for:
- Background color layers (`--bg-1`, `--bg-2`, `--bg-3`)
- Typography (`--text`, `--muted`)
- Glass panel (`--glass`, `--border`)
- Elevation (`--shadow`)
- Accent (`--accent`)

## 4. Global Layout
- Full-page gradient background with two radial glow layers
- Body centered with CSS Grid
- Hide overflow by default, allow mobile override

## 5. Ambient Orbs
Three absolutely positioned decorative circles:
- `.orb.a`, `.orb.b`, `.orb.c`
- Different sizes/colors/positions
- Shared floating animation (`float`)
- Slight stagger with animation delays

## 6. Main Card
- Element: `.card`
- Width: `min(700px, 100%)`
- Rounded glass panel with blur
- Border + shadow for depth
- Entrance animation (`rise`) on load

## 7. Card Content
Order inside `<main class="card">`:
1. Upper label row with live dot
2. H1 headline containing highlighted span text
3. Supporting Japanese paragraph (with line break)
4. Footer caption

## 8. Typography
- Base stack includes `Space Grotesk`, `Noto Sans JP`, `Segoe UI`
- Headline uses large responsive clamp size
- `h1 span` uses gradient text clipping

## 9. Micro-interactions
- `.dot` pulse animation (`pulse`)
- Orbs floating (`float`)
- Card entrance (`rise`)

## 10. Accessibility
- Main card uses `role="main"` and `aria-label="Hello World"`
- Decorative elements marked with `aria-hidden` where applicable

## 11. Responsive Rule
`@media (max-width: 480px)`:
- Allow body vertical scrolling
- Reduce card corner radius

## 12. Non-Functional Requirements
- Keep code minimal and readable
- Preserve single-file portability
- Do not introduce external JS/CSS dependencies
