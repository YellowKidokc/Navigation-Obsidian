# Obsidian Page Template System — Installation Guide

## Prerequisites

- [Obsidian](https://obsidian.md) v1.0+
- [Templater](https://github.com/SilentVoid13/Templater) plugin
- [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin (optional, for dynamic banner content)

## Files

```
.obsidian/snippets/page-template-system.css   — CSS for banner + sticky footer
_templates/page_template.md                    — Default template (all note types)
_templates/page_template_dataview.md           — Template with live Dataview stats
_templates/page_template_dt_unit.md            — Template for DT Unit notes
_templates/page_template_axiom.md              — Template for Axiom notes
_templates/page_template_paper.md              — Template for Paper notes
```

## Setup Steps

1. **Copy files into your vault**
   - Copy `.obsidian/snippets/page-template-system.css` into your vault's `.obsidian/snippets/` folder
   - Copy the `_templates/` folder into your vault root

2. **Enable the CSS snippet**
   - Open Settings → Appearance → CSS Snippets
   - Click the reload button to detect the new snippet
   - Toggle on `page-template-system`

3. **Configure Templater**
   - Open Settings → Templater
   - Set "Template folder location" to `_templates`

4. **Configure Dataview** (optional)
   - Open Settings → Dataview
   - Enable "Enable JavaScript Queries" if you want inline stats
   - Enable "Enable Inline Queries" for the `= expression` syntax in banners

5. **Create a new note**
   - Use Templater (Ctrl/Cmd+T or your hotkey) to create a note from template
   - Select the appropriate template for your note type
   - The Templater script will prompt you to choose type and domain

## Template Variants

| Template | Use For | Context Nav |
|----------|---------|-------------|
| `page_template` | General notes | Global nav only |
| `page_template_dataview` | Notes with live stats | Global nav + file/link counts |
| `page_template_dt_unit` | Doctor Thesis units | FACTS, Kill Conditions, Lowe Battery |
| `page_template_axiom` | Axiom notes | Technical (188), Public (22), Map |
| `page_template_paper` | Paper notes | Drafts, Published, Data |

## Three-Zone Layout

Every note gets three zones:

- **Zone 1 (Banner)** — Category nav, quick links, status badge at the top
- **Zone 2 (Content)** — Your actual note content in the middle
- **Zone 3 (Footer)** — Sticky navigation bar at the bottom: Home, Back, Forward, Recent, Vault Home

## Navigation

- **Home** links to the current folder's `00_INDEX.md`
- **Vault Home** links to the canonical index (`00_Canonical/CANONICAL_INDEX`)
- **Back/Forward** auto-populated by Templater based on alphabetical file order in the folder
