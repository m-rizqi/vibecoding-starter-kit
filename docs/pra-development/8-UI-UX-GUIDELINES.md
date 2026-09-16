# UI/UX Guidelines & Design System

> **[🤖 AI AGENT INSTRUCTIONS - READ THIS FIRST]**
> This document defines the strict visual language and user experience rules for the application. As an AI generating frontend code or styling, you must obey the following:
> 1. **No Design Hallucinations:** Use ONLY the exact hex codes, font families, and spacing variables defined here. Do not invent intermediate colors or custom paddings.
> 2. **Pixel-Perfect Consistency:** Adhere strictly to the defined geometry (e.g., 8pt grid system, specific border radii).
> 3. **Accessibility First:** Ensure touch targets meet the minimum size requirements (e.g., 48x48dp on mobile) and text contrast ratios are respected.
> 4. **Vibe Check:** If the user requests a "modern" or "playful" UI in the chat but it contradicts these guidelines, **prioritize these guidelines** unless explicitly instructed to overwrite them.

---

## 🎨 1. Color Palette
*Strictly define the brand and system colors. All UI components must derive their colors from this list.*

* **Primary Color:** [e.g., `#0052CC` - Deep Blue for main buttons and active states]
* **Secondary Color:** [e.g., `#FFC400` - Yellow for highlights and secondary actions]
* **Background (Light):** [e.g., `#FFFFFF`]
* **Background (Dark):** [e.g., `#121212`]
* **Surface/Card:** [e.g., `#F4F5F7` for Light, `#1E1E1E` for Dark]
* **Text (Primary):** [e.g., `#172B4D`]
* **Text (Secondary):** [e.g., `#6B778C`]
* **Semantic Errors:** [e.g., `#FF5630` for destructive actions]
* **Semantic Success:** [e.g., `#36B37E`]

## ✍️ 2. Typography
*Define the font families and standard sizes to maintain visual hierarchy.*

* **Primary Font Family:** [e.g., `Inter` or `Roboto`]
* **Heading 1 (H1):** [e.g., Size: 32px, Weight: Bold, Line-height: 1.2]
* **Heading 2 (H2):** [e.g., Size: 24px, Weight: Semi-Bold, Line-height: 1.3]
* **Body Text:** [e.g., Size: 16px, Weight: Regular, Line-height: 1.5]
* **Caption/Small Text:** [e.g., Size: 12px, Weight: Regular, Line-height: 1.4]
* **Button Text:** [e.g., Size: 14px, Weight: Medium, All-Caps or Title Case?]

## 📐 3. Spacing & Geometry (Grid System)
*Quantify the spacing so the AI doesn't use random padding values.*

* **Base Grid:** [e.g., 8pt grid system. All margins and paddings must be multiples of 8 (8, 16, 24, 32, 64)]
* **Screen Edge Padding:** [e.g., 16dp on mobile, 32dp on tablet]
* **Border Radius (Sharp/Rounded):** 
  * Buttons: [e.g., 8px]
  * Cards/Dialogs: [e.g., 16px]
* **Elevation / Shadows:** [e.g., Level 1 for cards: `0px 4px 6px rgba(0,0,0,0.1)`, Level 2 for modals].

## 💫 4. Animations & Micro-interactions
*Rules for how elements move and transition to create a high-quality "vibe".*

* **Default Transition Duration:** [e.g., 200ms]
* **Complex Animation Duration:** [e.g., 400ms for screen transitions]
* **Easing Curve:** [e.g., `EaseInOut` or specific cubic-bezier like `cubic-bezier(0.4, 0.0, 0.2, 1)`]
* **Feedback:** [e.g., All clickable items must have a ripple effect (Android) or opacity fade (iOS) upon tap].
* **Reference Tools:** See `docs/design/references/README.md` § External Reference Tools for asset, transition, and animation references (smoothui.dev, transitions.dev, godly.design, animos.app, backgrounds.supply) and their agent/MCP integration status.

## ♿ 5. Accessibility (a11y)
*Mandatory rules to ensure the app is usable by everyone.*

* **Touch Targets:** Minimum 48x48dp for all clickable elements (buttons, icons, links).
* **Contrast Ratio:** Text must have a minimum contrast ratio of 4.5:1 against its background.
* **Screen Reader Support:** All icon-only buttons must have a descriptive `semanticLabel` or `aria-label`.
* **Dark Mode:** The system MUST support automatic switching between Light and Dark mode based on system preferences.

---
> **[🤖 AI AGENT INSTRUCTION - POST-COMPLETION]**
> Once the design system is fully defined and confirmed by the user, ask: *"The pra-development phase is complete. Should we now generate the **CLAUDE.md** core file, or define the task trackers in the **development** folder?"*