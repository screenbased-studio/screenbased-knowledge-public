────────────────────────────────────────
FILE: customers/technikpr/bundles/TECHNIKPR_plugins_bundle__v1.0.md
────────────────────────────────────────
DOC_ID: SB-TPR-BND-PLUGINS-001
DOC_VERSION: v1.0
DOC_STATUS: ACTIVE
SCOPE: DEV,DESIGN
CUSTOMER: technikpr
LAST_UPDATED: 2025-12-17
BUNDLE_TYPE: LOGICAL_REFERENCE (NO_ZIP)

# A) Zweck
Dieses Bundle ist ein logischer Container (ohne ZIP), der alle technikPR-relevanten Custom Plugins als Referenzen bündelt.
Es dient als:
- Einstiegspunkt für Dev/Design/GPT
- Bündel-Index für Registry- und Kundenkontext
- Grundlage für spätere „Pakete“ (z. B. Installation Sets), ohne Wissen/Specs zu duplizieren

# B) Bundle-Regeln
## B1) Source of Truth
- Plugin-Spezifikationen liegen ausschließlich als einzelne, versionierte MD-Dateien im Public Knowledge Repo.
- Dieses Bundle referenziert nur (keine Kopien).

## B2) Aufnahme-Kriterium
Ein Plugin darf aufgenommen werden, wenn:
- eine Plugin-Spec als Public MD existiert (versioniert)
- es keine Secrets enthält
- es einem klaren Kunden-/Projektkontext zugeordnet ist (hier: technikpr)

## B3) Update-Policy
- Neue Plugins: Bundle-Version MINOR erhöhen (v1.0 → v1.1)
- Entfernen/Umbenennen von Einträgen: Bundle-Version MAJOR erhöhen (v1.x → v2.0)
- Plugin-Spec Updates verändern das Bundle nicht automatisch (nur wenn Einträge/URLs/IDs sich ändern)

# C) Enthaltene Plugins (Referenzen)
## C1) SB-TPR-EW-001 · TECHNIKPR Elementor Widgets
- ID: SB-TPR-EW-001
- Spec (RAW): https://raw.githubusercontent.com/screenbased-studio/screenbased-knowledge-public/main/customers/technikpr/plugins/SB-TPR-EW-001_technikpr_elementor_widgets__v1.0.md
- Status: ACTIVE
- Scope: DEV,DESIGN
- Kurzinhalt:
  - Widgets: technikpr_headline, tpr31110_page_grid
  - Zero-Regression: Category (technikpr-widgets/technikPR), Option Key (technikpr_ew_active_widgets), Admin Slug (technikpr-ew), Asset Handle (technikpr-page-grid), Markup-Stabilität
  - A11y: 1 Link/Card, :focus-visible, keine tabindex-Hacks

# D) Installations-/Ops-Hinweis (nicht technisch, nur Governance)
- Dieses Bundle ist kein Installationspaket.
- Installation/Deployment-Listen sind separate Ops-Artefakte (optional, später).
- Legacy/Parallelbetrieb ist NICHT Bestandteil dieses Bundles, außer explizit aufgenommen.

# E) Tests & Verifikation
## E1) Live Knowledge Read Test
- Muss nach jeder Bundle-MINOR/MAJOR Änderung einmal durchgeführt werden:
  - Dev liest alle RAW-Links im Bundle
  - Validiert: IDs/Keys/Handles/Zero-Regression/A11y
  - Markiert UNKNOWN nur, wenn Quelle unlesbar oder unvollständig ist

## E2) Zero Regression
- Bundle darf keine alten Einträge überschreiben oder „still“ entfernen.
- Jede Änderung muss in Change Notes dokumentiert werden.

# F) Change Notes
- v1.0: Initialer Start des technikPR Plugins Bundles mit SB-TPR-EW-001 (Elementor Widgets).
────────────────────────────────────────
END OF FILE
────────────────────────────────────────
