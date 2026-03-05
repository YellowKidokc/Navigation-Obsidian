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
<div class="dashboard-banner">
<div class="banner-title">THEOPHYSICS · <% folder_name %></div>
<div class="banner-row">
<div class="banner-nav">
[[00_Canonical/CANONICAL_INDEX|📚 Canon]] ·
[[04_THEOPYHISCS/Doctor thesis/00_DR_THESIS_INDEX|🎓 DT Index]] ·
[[MASTER_EQUATION_10_LAWS/INDEX|⚡ Master Eq]] ·
[[00_Canonical/MASTER_EQUATION_10_LAWS/TEN_LAWS_CANONICAL_EQUATIONS|📐 Ten Laws]] ·
[[24_PROPERTIES|🔑 24 Props]]
</div>
<div>
<span class="banner-status draft"><% status %></span>
</div>
</div>
<div class="banner-row" style="margin-top:6px; font-size:0.85em; color:#888;">
<% type %> · <% domain %> · Modified: <% date %>
</div>
</div>

---

[YOUR CONTENT HERE]

---

<div class="nav-footer">
🏠 [[00_INDEX|Home]] ·
◀ [[<% prev_note %>|Back]] ·
▶ [[<% next_note %>|Forward]] ·
📋 [[_RECENT|Recent]] ·
🏗️ [[00_Canonical/CANONICAL_INDEX|Vault Home]]
</div>
