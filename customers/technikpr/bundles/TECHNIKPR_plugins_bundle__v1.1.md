────────────────────────────────────────
FILE: customers/technikpr/bundles/TECHNIKPR_plugins_bundle__v1.1.md
────────────────────────────────────────
DOC_ID: SB-TPR-BND-PLUGINS-001
DOC_VERSION: v1.1
DOC_STATUS: ACTIVE
SCOPE: DEV,DESIGN
CUSTOMER: technikpr
LAST_UPDATED: 2025-12-17
BUNDLE_TYPE: LOGICAL_REFERENCE (NO_ZIP)

# A) Zweck
Logischer Container (ohne ZIP), der alle technikPR-relevanten Custom Plugins
als Referenzen bündelt. Kein Kopieren von Specs.

# B) Bundle-Regeln
- Registry-first: Plugin-Specs bleiben Source of Truth für Details.
- Dieses Bundle referenziert nur (keine Kopien).
- Neue Plugins: MINOR bump (v1.0 -> v1.1).
- Entfernen/Umbenennen: MAJOR bump.

# C) Enthaltene Plugins (Referenzen)

## C1) SB-TPR-EW-001 · TECHNIKPR Elementor Widgets
- Spec (RAW):
  https://raw.githubusercontent.com/screenbased-studio/screenbased-knowledge-public/main/customers/technikpr/plugins/SB-TPR-EW-001_technikpr_elementor_widgets__v1.0.md
- Status: ACTIVE

## C2) SB-TPR-PLH-002 · technikPR Placeholder Plugin (Simulation)
- Spec (RAW):
  https://raw.githubusercontent.com/screenbased-studio/screenbased-knowledge-public/main/customers/technikpr/plugins/SB-TPR-PLH-002_technikpr_placeholder_plugin__v0.1.md
- Status: PLACEHOLDER
- Hinweis: Nur Prozess-/Skalierungstest. Nicht produktiv.

# D) Change Notes
- v1.1: Added SB-TPR-PLH-002 placeholder entry (simulation). SB-TPR-EW-001 unchanged.
────────────────────────────────────────
END OF FILE
────────────────────────────────────────
