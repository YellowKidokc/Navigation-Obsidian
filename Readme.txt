# Obsidian Page Template System
## Dashboard Banner + Sticky Navigation Footer
## Version 1.0 | March 4, 2026

---

## What This Is

Every note in the vault gets three zones:

```
┌─────────────────────────────────────────────┐
│  ZONE 1: DASHBOARD BANNER (top)             │
│  Category nav · Quick links · Status badge  │
├─────────────────────────────────────────────┤
│                                             │
│  ZONE 2: CONTENT (scrollable)               │
│  The actual note content                    │
│                                             │
│                                             │
│                                             │
├─────────────────────────────────────────────┤
│  ZONE 3: STICKY NAV (always visible bottom) │
│  🏠 Home  ◀ Back  ▶ Forward  📋 Recent     │
└─────────────────────────────────────────────┘
```

Zone 3 stays visible no matter how far you scroll. Zone 1 scrolls with the content but is always at the top when you open the note.

---

## Implementation Options (Pick One)

### Option A: CSS Snippet + Markdown Template (Simplest, No Plugin Needed)

This uses Obsidian's native CSS snippets plus a standard markdown template at the top and bottom of every note.

**Pros:** No plugins, works everywhere, easy to maintain
**Cons:** Footer won't truly "stick" (Obsidian doesn't support position:fixed in reading mode easily). Footer stays at bottom of content but doesn't float.

### Option B: Simple Banner Plugin + CSS + Templater (Your Link)

Use obsidian-simple-banner for Zone 1 visuals, CSS for layout, Templater plugin to auto-insert the zones on new notes.

**Pros:** Nice banner visuals, automated insertion
**Cons:** Plugin dependency, banner is image-focused (not ideal for nav links)

### Option C: Custom CSS Snippet + Dataview Inline (Best for Us)

Use CSS for layout and sticky footer. Use Dataview inline queries for dynamic content in the banner (file counts, status, links). Use a Templater template for new note creation.

**Pros:** Dynamic content, sticky footer works in Live Preview, no image dependencies
**Cons:** Requires Dataview + Templater + custom CSS

**Recommendation: Option C.** Here's how to build it.

---

## Option C: Full Build Spec

### Part 1: The CSS Snippet

Save as: `.obsidian/snippets/page-template-system.css`

```css
/* ═══════════════════════════════════════════
   THEOPHYSICS PAGE TEMPLATE SYSTEM
   Dashboard Banner + Sticky Navigation Footer
   ═══════════════════════════════════════════ */

/* ─── ZONE 1: DASHBOARD BANNER ─── */

.theme-dark .dashboard-banner,
.theme-light .dashboard-banner {
    background: linear-gradient(135deg, #1a1a2e 0%, #2a2a4e 100%);
    border-bottom: 2px solid #c9a84c;
    border-radius: 8px;
    padding: 12px 16px;
    margin-bottom: 16px;
    color: #f8f6f0;
    font-family: var(--font-interface);
    font-size: 0.85em;
}

.dashboard-banner a {
    color: #c9a84c !important;
    text-decoration: none;
    padding: 4px 8px;
    border-radius: 4px;
    transition: background 0.2s;
}

.dashboard-banner a:hover {
    background: rgba(201, 168, 76, 0.15);
}

.dashboard-banner .banner-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 8px;
}

.dashboard-banner .banner-title {
    font-size: 0.75em;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: #c9a84c;
    margin-bottom: 6px;
}

.dashboard-banner .banner-nav {
    display: flex;
    gap: 4px;
    flex-wrap: wrap;
}

.dashboard-banner .banner-status {
    font-size: 0.8em;
    padding: 2px 8px;
    border-radius: 10px;
    font-weight: 600;
}

.banner-status.draft { background: #c9a84c33; color: #c9a84c; }
.banner-status.complete { background: #2e7d3233; color: #2e7d32; }
.banner-status.published { background: #2e7d3266; color: #fff; }

/* ─── ZONE 3: STICKY NAVIGATION FOOTER ─── */

/* In Live Preview / Source Mode - true sticky */
.cm-editor .nav-footer-sticky {
    position: sticky;
    bottom: 0;
    z-index: 100;
}

/* The footer bar itself */
.theme-dark .nav-footer,
.theme-light .nav-footer {
    background: linear-gradient(135deg, #1a1a2e 0%, #2a2a4e 100%);
    border-top: 2px solid #c9a84c;
    border-radius: 8px;
    padding: 8px 16px;
    margin-top: 24px;
    display: flex;
    justify-content: center;
    gap: 16px;
    align-items: center;
    font-family: var(--font-interface);
    font-size: 0.85em;
}

.nav-footer a {
    color: #c9a84c !important;
    text-decoration: none;
    padding: 6px 12px;
    border-radius: 4px;
    transition: background 0.2s;
    display: flex;
    align-items: center;
    gap: 4px;
}

.nav-footer a:hover {
    background: rgba(201, 168, 76, 0.2);
}

.nav-footer .nav-separator {
    color: #4a4a6a;
    font-size: 0.8em;
}

/* ─── RESPONSIVE ─── */

@media (max-width: 600px) {
    .dashboard-banner .banner-row {
        flex-direction: column;
        align-items: flex-start;
    }
    .nav-footer {
        flex-wrap: wrap;
        gap: 8px;
    }
}
```

### Part 2: The Markdown Template

This goes at the top and bottom of every note. Use Templater to auto-insert on new notes.

**Save as Templater template:** `_templates/page_template.md`

```markdown
<div class="dashboard-banner">
<div class="banner-title">THEOPHYSICS · {{folder_name}}</div>
<div class="banner-row">
<div class="banner-nav">
[[00_Canonical/CANONICAL_INDEX|📚 Canon]] · 
[[04_THEOPYHISCS/Doctor thesis/00_DR_THESIS_INDEX|🎓 DT Index]] · 
[[MASTER_EQUATION_10_LAWS/INDEX|⚡ Master Eq]] · 
[[00_Canonical/MASTER_EQUATION_10_LAWS/TEN_LAWS_CANONICAL_EQUATIONS|📐 Ten Laws]] · 
[[24_PROPERTIES|🔑 24 Props]]
</div>
<div>
<span class="banner-status draft">{{status}}</span>
</div>
</div>
<div class="banner-row" style="margin-top:6px; font-size:0.85em; color:#888;">
{{type}} · {{domain}} · Modified: {{date}}
</div>
</div>

---

[YOUR CONTENT HERE]

---

<div class="nav-footer">
🏠 [[00_INDEX|Home]] · 
◀ [[{{prev_note}}|Back]] · 
▶ [[{{next_note}}|Forward]] · 
📋 [[_RECENT|Recent]] · 
🏗️ [[00_Canonical/CANONICAL_INDEX|Vault Home]]
</div>
```

### Part 3: The "Home" System

Every folder has its own `00_INDEX.md` — that's the folder's "Home." The vault has a master home at the canonical index. The nav footer always has both:

- **🏠 Home** → This folder's `00_INDEX.md` (local home)
- **🏗️ Vault Home** → The canonical index (global home)

So no matter where you are, you can get back to the folder you're in OR back to the vault root.

### Part 4: Dynamic Banner Content (Dataview)

For power users, the banner can pull live data using Dataview inline queries:

```markdown
<div class="dashboard-banner">
<div class="banner-title">THEOPHYSICS · Doctor Thesis</div>
<div class="banner-row">
<div class="banner-nav">
[[00_Canonical/CANONICAL_INDEX|📚 Canon]] · 
[[00_DR_THESIS_INDEX|🎓 DT Index]] · 
[[MASTER_EQUATION_10_LAWS/INDEX|⚡ Master Eq]]
</div>
<div>
`= "📄 " + length(filter(this.file.folder.files, (f) => f.extension = "md")) + " notes"`
`= "🔗 " + length(this.file.outlinks) + " links out"`
</div>
</div>
</div>
```

This shows live file counts and link counts right in the banner.

### Part 5: Templater Auto-Insert Script

When Templater creates a new note, it auto-fills the template variables:

**Templater script (for `_templates/page_template.md`):**

```javascript
<%*
// Auto-fill template variables
const folder_name = tp.file.folder(true).split("/").pop();
const status = "Draft";
const type = await tp.system.suggester(
    ["Axiom","Theorem","Claim","Paper","DT-Unit","Article","Note","Index"],
    ["Axiom","Theorem","Claim","Paper","DT-Unit","Article","Note","Index"]
);
const domain = await tp.system.suggester(
    ["Physics","Theology","Consciousness","Information","Mathematics","Biology","Multiple"],
    ["Physics","Theology","Consciousness","Information","Mathematics","Biology","Multiple"]
);
const date = tp.date.now("YYYY-MM-DD");

// Find prev/next notes in folder
const files = app.vault.getFiles()
    .filter(f => f.parent.path === tp.file.folder(true))
    .filter(f => f.extension === "md")
    .sort((a,b) => a.name.localeCompare(b.name));
const currentIdx = files.findIndex(f => f.path === tp.file.path(true));
const prev_note = currentIdx > 0 ? files[currentIdx-1].basename : "00_INDEX";
const next_note = currentIdx < files.length-1 ? files[currentIdx+1].basename : "00_INDEX";
-%>
```

---

## The Banner Navigation Categories

The top banner nav links should be customizable per folder, but here's the default set:

### Global Nav (always present)
- 📚 Canon → Canonical Index
- 🎓 DT Index → Doctor Thesis Index  
- ⚡ Master Eq → Master Equation
- 📐 Ten Laws → Ten Laws Canonical Equations
- 🔑 24 Props → 24 Properties

### Context Nav (changes per folder type)

**For DT Units:**
- 📋 FACTS → FACTS Format reference
- 🎯 Kill Conditions → Falsification ledger
- 🔬 Lowe Battery → Battery reference

**For Axiom folders:**
- 🧮 Technical (188) → PostgreSQL axiom table
- 📢 Public (22) → Public axiom set
- 🗺️ Map → Public-to-technical map

**For Paper folders:**
- 📝 Drafts → Papers in draft
- ✅ Published → Published papers
- 📊 Data → Associated datasets

---

## Installation Checklist

1. [ ] Save CSS to `.obsidian/snippets/page-template-system.css`
2. [ ] Enable the snippet in Settings → Appearance → CSS Snippets
3. [ ] Install Templater plugin (if not already)
4. [ ] Save template to `_templates/page_template.md`
5. [ ] Set Templater's template folder to `_templates/`
6. [ ] Install Dataview plugin (if not already, for dynamic banner content)
7. [ ] Test by creating a new note with Templater
8. [ ] Verify banner displays at top with navy/gold styling
9. [ ] Verify footer displays at bottom with nav links
10. [ ] Verify Home links work (local and global)

---

## About the Simple Banner Plugin

The plugin you found (obsidian-simple-banner) adds image banners to notes. It's nice for visual headers but it's image-focused — you'd need to create banner images for each category. 

**My recommendation:** Use it AS WELL if you want visual flair (a subtle navy/gold gradient image as the banner background), but put the functional navigation in the CSS/markdown template system described above. The two work together — Simple Banner for the visual, our template for the functional nav.

If you want to use Simple Banner, add this to your note's YAML:
```yaml
banner: "assets/banners/theophysics_banner.png"
banner_height: 120
```

And create a set of banner images (navy/gold gradient with category text) in `assets/banners/`.

---

*"Navigation is not decoration. It's how knowledge moves through the system."*
