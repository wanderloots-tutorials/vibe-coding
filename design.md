# Design System Document: Mission Intelligence & Telemetry

## 1. Overview & Creative North Star

### Creative North Star: "The Obsidian Sentinel"
This design system is engineered to move beyond the "sci-fi trope" and into the realm of high-stakes aerospace engineering. The visual language balances extreme data density with absolute clarity. We are not building a movie prop; we are building an authoritative, mission-critical instrument.

The aesthetic is **"Tactical Minimalism."** It breaks the standard "web portal" look by utilizing a ruthless adherence to a 0px radius (Hard-Edge Brutalism) and intentional asymmetry. By favoring tonal layering over structural lines, we create an interface that feels like a singular, integrated machine rather than a collection of floating boxes.

---

## 2. Colors & Surface Architecture

The palette is rooted in the void of deep space, using ultra-low-frequency neutrals to allow critical telemetry data to "pop" with phosphoric intensity.

### Surface Hierarchy & Nesting
Depth is achieved through **Tonal Recess**, not elevation.
- **Base Layer:** `surface` (#10131a) – The infinite void. Used for the primary application background.
- **Secondary Tier:** `surface_container_low` (#191b23) – Large structural regions (e.g., sidebar or footer).
- **Tertiary Tier:** `surface_container` (#1d1f27) – Standard data card backgrounds.
- **Accent Tier:** `surface_container_highest` (#32353d) – Hover states or active panel focus.

### The "No-Line" Rule
To maintain a high-end editorial feel, prohibit the use of 1px solid borders for sectioning. Boundaries must be defined by the shift from `surface` to `surface_container_low`.

### The "Ghost Border" Exception
While we avoid structural lines, a "Ghost Border" may be used for telemetry modules to simulate a HUD (Heads-Up Display) effect. Use the `outline_variant` token at **20% opacity**. This creates a suggestion of a container without breaking the visual flow.

---

## 3. Typography: The Dual-Engine Logic

The typography system relies on a high-contrast pairing between technical precision and human-centric legibility.

### Display & Headlines (The Mission Command)
- **Font:** `Space Grotesk` (System Token: `display`, `headline`)
- **Usage:** Used for mission titles, countdowns, and high-level status headers.
- **Character:** Aggressive, wide, and authoritative. Use `headline-lg` (2rem) for major module headers to create an editorial anchor point in a sea of data.

### Telemetry & Labels (The Data Stream)
- **Font:** `Inter` (System Token: `title`, `body`, `label`)
- **Usage:** Used for all UI labels, descriptions, and functional buttons.
- **Telemetry Modification:** For raw numeric data (Velocity, O2 levels, etc.), use a Monospace variant of `Inter` or `Space Mono` to ensure tabular lining—this prevents "jumping" numbers during live updates.

---

## 4. Elevation & Depth

In a mission-critical dashboard, traditional drop shadows are distracting. We use **Tonal Stacking** and **Phosphoric Glow**.

- **The Layering Principle:** Instead of lifting a card with a shadow, "cut" it into the surface. A `surface_container_lowest` card placed inside a `surface_container_high` section creates a recessed, high-tech instrument look.
- **Glassmorphism:** For floating modals or "Critical Alert" overlays, use `surface_variant` with a **12px Backdrop Blur**. This maintains the "Obsidian" feel while ensuring the underlying data remains visible as a textured blur.
- **Ambient Glow:** Instead of shadows, "Danger" or "Success" states should emit a subtle, 4% opacity outer glow of their respective functional color (`error` or `tertiary`) to simulate a glowing hardware LED.

---

## 5. Components

### Buttons: The "Module" Style
- **Primary:** `primary` (#adc6ff) fill with `on_primary` (#002e69) text. **0px Border Radius.**
- **Secondary:** `outline` border (20% opacity) with `primary` text.
- **State Change:** On hover, use `primary_container` to create a subtle "power up" brightness shift.

### Telemetry Chips
- **Status Chips:** No background fill. Use a `2px` left-side border of `tertiary` (Success) or `error` (Danger) paired with `label-sm` monospace text. This mimics industrial labeling tape.

### Input Fields
- **Architecture:** `surface_container_highest` background. No bottom line.
- **Focus State:** A high-contrast `primary` 1px Ghost Border.
- **Typography:** `label-md` for floating labels, always set in `primary` color to denote interactivity.

### Cards & Data Grids
- **Constraint:** Forbid the use of divider lines.
- **Separation:** Use the Spacing Scale `4` (0.9rem) or `5` (1.1rem) to create clear air between data points. Use `surface_container_low` to group related telemetry sets.

---

## 6. Do’s and Don’ts

### Do:
- **DO** use asymmetry. Large telemetry on the left, condensed logs on the right. It feels "engineered" rather than "templated."
- **DO** use `secondary` (#ffb955) for "Caution" states exclusively. It is the most high-contrast color in the system and must be preserved for visual alerts.
- **DO** use the `0.5` to `2` spacing tokens for tight, "data-dense" clusters to simulate a professional cockpit.

### Don’t:
- **DON'T** use rounded corners. Everything must be 0px. Softness implies consumer-grade; hardness implies aerospace-grade.
- **DON'T** use gradients on surfaces. Keep all backgrounds solid to maintain the "Obsidian" depth.
- **DON'T** use standard grey shadows. If an element must float, its "shadow" is actually a `surface_container_lowest` glow.
- **DON'T** use center-alignment for data. All telemetry should be left or right-aligned to a rigid vertical grid to assist in rapid eye-scanning.
