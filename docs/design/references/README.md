# 🎨 Design References & Moodboards

Welcome to the **Design References** folder. This directory serves as the creative foundation and visual inspiration for the project's user interface. 

It is designed to hold moodboards, UI screenshots, competitor analyses, and aesthetic references to ensure the generated frontend code perfectly matches the desired "vibe".

---

## 🤖 FOR AI AGENTS: Creative Directives
> **[AI AGENT INSTRUCTIONS - SYSTEM OVERRIDE]**
> If you are an AI assistant (especially a frontend or UI/UX agent) reading this repository, use this folder to understand the visual context of the project.
> 1. **Visual Analysis:** If you have vision capabilities, analyze the images provided in this folder to grasp the aesthetic (e.g., minimalist, brutalist, corporate, playful, dark mode).
> 2. **Vibe Alignment:** Use these references to guide your CSS, Tailwind classes, or Flutter styling. For example, if the moodboard shows soft, rounded elements, ensure you use higher `border-radius` and soft shadows in your code.
> 3. **Guideline Synergy:** Always combine the inspiration found here with the strict mathematical rules (colors, spacing, typography) defined in `docs/pra-development/8-UI-UX-GUIDELINES.md`. 
> 4. **Do Not Copy Blindly:** Use these files as *inspiration*, not as exact blueprints to clone, unless explicitly instructed by the user.

---

## 👨‍💻 FOR HUMANS: How to Use This Folder
As the "Vibecoder", you need to feed the AI's creativity. Words alone are often not enough to describe a UI aesthetic.

### What to put in this folder:
* **🖼️ Images (`.png`, `.jpg`, `.webp`):** Screenshots of apps you like, Dribbble shots, Behance layouts, or Pinterest moodboards.
* **📝 Markdown Links (`.md`):** Create simple markdown files containing links to your Figma files, live websites you want to emulate, or design system references.
* **🎨 Color Palettes:** Images of color swatches or gradients that represent the brand's identity.

### Best Practices:
* **Name files descriptively.** Instead of `image1.png`, use `reference-login-minimalist-dark.png`. This helps text-based AI agents understand the intent even without seeing the image.
* **Keep it focused.** Don't clutter this folder with 50 different images. Pick 3-5 strong references that perfectly capture the vibe you want.

---

## 🔗 External Reference Tools (assets, transitions, animation)

> **[🤖 AI AGENT INSTRUCTIONS]**
> When building or styling frontend UI in phase 2 (`source-codes/{frontend,mobile}`), consult these sites for visual assets, transitions, and animation patterns — alongside the moodboards in this folder and the hard rules in `docs/pra-development/8-UI-UX-GUIDELINES.md`. Referenced in `.claude/rules/04-frontend-standards.md`.

| Site | What it's for | Agent integration |
|---|---|---|
| [smoothui.dev](https://smoothui.dev) | React + Tailwind + Shadcn/ui + Motion component library with polished animations | **MCP connected** — generic `shadcn` MCP server registered in `.mcp.json` (`npx shadcn@latest mcp`, no API key). Once a shadcn-based frontend is scaffolded, add `"registries": {"@smoothui": "https://smoothui.dev/r/{name}.json"}` to that project's `components.json` to pull components through the MCP server. |
| [transitions.dev](https://transitions.dev) | Copy-paste UI transitions for web apps | No MCP server. Ships a coding-agent **skill** (`transitions.dev/skill.html`) — install that skill in-project when transitions work starts, or copy-paste snippets directly. No API key for the free tier; Pro tier is a paid subscription. |
| [godly.design](https://godly.design) | Curated gallery of bold/experimental web design | No official MCP or API. Browse manually for inspiration. (An unofficial third-party MCP — `notsointresting/design-inspiration-mcp` — wraps this site via a `browse_godly` tool; deliberately **not installed**, since it's an unverified third party running arbitrary code against this project. Revisit only if the user explicitly asks to trust it.) |
| [animos.app](https://animos.app) | Turns static designs into motion showcases | No MCP/API yet (on their roadmap). Browse manually; revisit once they ship one. |
| [backgrounds.supply](https://backgrounds.supply) | Procedural/curated background asset packs (gradients, textures) | No MCP/API. Premium downloads require sign-in at `app.backgrounds.supply` — **not connected** (browse-only reference for now, per user decision). Ask the user again if a specific project needs premium asset downloads. |

**Note:** a sixth site, `dec.gallery`, does not resolve (DNS failure) — skipped per user decision. If the intended site turns out to be `deck.gallery` (deck/presentation design inspiration) or something else, tell the AI agent the correct URL and this table gets updated.