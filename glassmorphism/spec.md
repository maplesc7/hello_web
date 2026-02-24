# Glassmorphism Showcase Specification

## 1. Purpose
Create a single-page visual showcase that demonstrates premium glassmorphism style in pure HTML/CSS.

## 2. Output File
- Target: `glassmorphism/index.html`
- Single-file implementation
- No JavaScript required

## 3. External Dependencies
- Allow Google Fonts only:
  - `Space Grotesk`
  - `Manrope`

## 4. Theme Tokens (`:root`)
Define core design tokens:
- Background: `--bg-1`, `--bg-2`
- Typography: `--ink`, `--muted`
- Glass layers: `--glass`, `--glass-strong`, `--line`
- Highlights: `--accent`, `--accent-2`
- Elevation: `--shadow`
- Radius scale: `--radius-xl`, `--radius-lg`, `--radius-md`

## 5. Global Styling
- Reset with `box-sizing`, `margin`, `padding`
- Body uses multi-layer background:
  - 2 radial gradients for colored glow
  - 1 dark linear gradient base
- `overflow-x: hidden`

## 6. Atmospheric Layers
- `body::before` and `body::after`:
  - Large blurred circular gradients
  - Fixed position
  - Loop animation (`floatBlob`)
- `.grain` overlay:
  - Fixed full-screen micro-noise pattern
  - `mix-blend-mode: soft-light`

## 7. Core Glass Surface Class
Define reusable `.glass` class:
- Semi-transparent gradient fill
- 1px translucent border
- Rounded corners
- Backdrop blur + saturation
- Deep soft shadow

## 8. Layout Structure
Main container `.wrap`:
- Width: `min(1120px, 92vw)`
- Center aligned

Sections:
1. Top nav (`.nav.glass`)
2. Hero area (`.hero`) with two columns
3. Feature card grid (`.grid`)
4. Footer note text

## 9. Navigation
- Left brand (`.brand`) with glowing dot
- Right pill links (`.chip`)
- Hover behavior: color lift + slight Y translation

## 10. Hero Section
### 10.1 Main panel (`.hero-main.glass`)
- Kicker badge
- Large heading
- Lead paragraph
- Two action buttons (`.btn-primary`, `.btn-secondary`)
- Decorative radial glow via `::after`

### 10.2 Side panel (`.hero-side.glass`)
- Three stat blocks
- Each stat has label + value

## 11. Feature Grid
- 3 cards on desktop (`.card.glass`)
- Each card:
  - Title
  - Description paragraph
  - Small rounded badge
- Staggered `fadeUp` animation by nth-child delays

## 12. Motion
Required keyframes:
- `fadeUp`: fade in + upward movement
- `floatBlob`: subtle floating translation

## 13. Responsive Rules
- `@media (max-width: 940px)`:
  - Hero becomes single column
  - Grid becomes 2 columns
- `@media (max-width: 620px)`:
  - Nav stacks vertically
  - Grid becomes 1 column
  - Heading max-width relaxed

## 14. Content Copy
Use the existing Japanese copy and section labels as in current implementation.

## 15. Non-Functional Requirements
- Keep everything in one HTML file
- Maintain visual hierarchy and premium contrast
- Avoid additional scripts or frameworks
