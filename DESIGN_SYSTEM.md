# Design System: "The Celebration Garden"

The visual language blends high-end **tech** (Google / Material You inspired) with the **whimsical aesthetic** of a boutique party planner.

---

## Core Aesthetic Principles

- **Floating layers** — elements feel like they exist on layered planes, not in rigid boxes
- **Rounded everything** — Super-Ellipse (squircle) radius on all containers and buttons
- **Frosted pastels** — soft, airy backgrounds with high-contrast readable type
- **Motion with purpose** — animations reward interaction, never distract

---

## Color Palette: "Frosted Pastels"

| Role | Color | Usage |
|------|-------|-------|
| Background Primary | Soft White `#FAFAFA` | Page canvas |
| Background Accent | Soft Lavender `#EDE9FE` | Section highlights |
| Background Accent | Mint `#D1FAE5` | Success / positive states |
| Background Accent | Peach `#FDE8D8` | Warm call-to-action areas |
| Typography Primary | Charcoal `#1C1C1E` | Body text, headings |
| Typography Secondary | Navy `#1E3A5F` | Subheadings, emphasis |
| Interactive / Brand | Deep Lavender `#7C3AED` | Buttons, links, highlights |
| Confetti Colors | Lavender, Mint, Peach, Coral | Micro-interaction bursts |

**Rule:** Never use purple gradients on white — that is the generic AI aesthetic we are avoiding.

---

## Typography

### Pairing Principles
- Use a **distinctive display font** for headings (characterful, memorable)
- Use a **refined, readable body font** that complements without competing
- Never use: Inter, Roboto, Arial, or system-ui as primary choices

### Suggested Direction
- Display: Something with personality — playful-editorial or refined-whimsical (e.g., a serif with quirk, or a rounded display sans)
- Body: A clean, slightly warm sans-serif that reads well at small sizes on mobile
- Accent / Tags: Monospace or a handwritten-style font for "Celebration Note" ad units

*Final font selection to be confirmed during design phase with brand direction.*

---

## Spacing & Shape

| Property | Value |
|----------|-------|
| Border Radius (cards, containers) | `24px – 32px` (Super-Ellipse / squircle) |
| Border Radius (buttons) | `999px` (pill) or `20px` |
| Card Shadow | `0 4px 24px rgba(0,0,0,0.06)` |
| Grid | Soft 12-col grid, elements allowed to break grid intentionally |
| White Space | Generous — elements breathe on the "Stage" |

---

## Component Patterns

### Product "Pack" Block
- Appears as a physical card with subtle drop shadow
- Pre-defined aspect ratio (CSS) to prevent CLS
- On hover: Rive mascot reacts (points / tucks)
- CTA button triggers confetti burst on click

### Blog Post Layout
1. Large, high-resolution lifestyle photography (hero)
2. "Stitch-style" chips / tags — `#FirstBirthday` `#Stickers` `#PlanningTips`
3. Body content with embedded product proxy links
4. Contextual "Celebration Note" ad injection (styled as handwritten card/sticker)
5. Related posts grid at bottom

### "Celebration Note" Ad Units
- Used for: Google AdSense, StoryStork.AI cross-sells, GetJoyBox cross-sells
- Style: Looks like a handwritten notecard or pastel sticker
- Must have fixed dimensions before load (no CLS)
- Positioned inline with content, never as sidebar interruptions

### Tags / Chips
- Pill-shaped, pastel background, small body font
- Clickable — filter blog content by tag
- Example: `#FirstBirthday` `#BedtimeStories` `#Crafting`

---

## Motion & Animation

### Philosophy
- One well-orchestrated page load (staggered reveals) > scattered micro-interactions
- Scroll-triggered reveals on blog sections and product blocks
- Hover states that feel surprising and delightful

### Implementation Rules
- CSS-only animations preferred for HTML/static elements
- Rive for mascot and interactive state machines
- `animation-delay` for staggered card/section reveals on load
- All animated containers have pre-defined dimensions (no CLS)
- No autoplay video — use Rive interactive animations instead

### Confetti Micro-interaction
- Trigger: Clicking "View on Etsy", "Save for Later", registration links
- Implementation: Lightweight, vector-based
- Colors: Brand pastels (lavender, mint, peach, coral)
- Duration: ~800ms, no repeat
- Must not block interaction or cause layout shift

---

## Mascot (Rive)

The brand mascot (Stork or brand character) is a first-class UI element.

- Lives in a fixed position or within product block zones
- Reacts via Rive State Machines to scroll, hover, and click events
- Asset format: `.riv`, sub-100kb
- Loaded async — container has pre-defined size
- States: Idle, Pointing, Tucking, Cheering

See [FEATURES.md](./FEATURES.md#45-interactive-mascot-rive) for state trigger table.

---

## Accessibility

- All pastel backgrounds must meet WCAG AA contrast with charcoal/navy text
- Animations must respect `prefers-reduced-motion`
- All interactive elements have visible focus states
- Images have descriptive alt text (also benefits SEO)
