DOC_ID: SB-TPR-EW-001
DOC_VERSION: v1.0
DOC_STATUS: EVOLVING
SCOPE: DEV,DESIGN
CUSTOMER: technikpr
LAST_UPDATED: 2025-12-16
REGISTRY_MASTER: true

# A) Zweck
- Bündel-Plugin für technikPR-spezifische Elementor Widgets mit modularer Aktivierung pro Widget (Admin UI).
- Ergänzt Standard-Elementor-Widgets durch projekt-/kundenspezifisches Markup, Controls und Styling-Annahmen.

# B) Abhängigkeiten
- WordPress (Plugin-typisch; keine Core-Modifikationen)
- Elementor: Free (mind. 3.5+ empfohlen; Widgets-Registration via `elementor/widgets/register`)
- Theme/CSS-Kontext:
  - Page Grid nutzt Qodef-kompatible Klassen (z.B. `.qodef-m-*`).
  - Optionales Styling ist teils auf `#technikpr` gescoped (wenn Container-ID fehlt, greifen diese Regeln nicht).
- bekannte Inkompatibilitäten
  - Keine offiziell dokumentierten; Parallelbetrieb mit Legacy-Plugin `technikpr-31110-page-grid` ist nicht Teil des Releases (Ops: Plugin B entfernen).

# C) Widgets-Übersicht
- technikPR Headline (`technikpr_headline`)
  - Einfache Headline-Ausgabe mit wählbarem HTML-Tag.
- Page Grid (technikPR) (`tpr31110_page_grid`)
  - Query-basierte Ausgabe von Pages als Grid, optional gefiltert über Taxonomy `technikpr_page_category`, Subline aus Meta `technikpr_subline`.

# D) Wichtige Controls / Parameter
## 1) Headline Widget (`technikpr_headline`)
- `headline` (Text)
- `tag` (h1–h6)

## 2) Page Grid Widget (`tpr31110_page_grid`)
### Query
- `posts_per_page` (1–50)
- `categories` (SELECT2, multiple; Terms der Taxonomy `technikpr_page_category`)
- `show_all_when_empty` (yes/no)
- `orderby` (menu_order/date/title)
- `order` (ASC/DESC)

### Layout
- `columns_responsive` (default / predefined / custom)
  - default: `columns_desktop` → CSS Var `--tpr-grid-cols`
  - predefined: responsive `columns` (Desktop/Tablet/Mobile) → CSS Var `--tpr-grid-cols`
  - custom: feste Ranges (siehe unten) → Inline Vars + `.is-cols-custom`
- Custom ranges (1–6):
  - `cols_1367_1440`, `cols_1025_1366`, `cols_769_1024`, `cols_681_768`, `cols_481_680`, `cols_0_480`
- `space_between_items` → CSS Var `--tpr-grid-gap` (0–40px via Presets)

### Ausgabe-Steuerung
- `display` (aktuell nur `text_only` aktiv; andere Werte fallbacken auf `text_only`)

# E) Zero-Regression-Zonen
- Elementor Category
  - Slug: `technikpr-widgets`
  - Titel: `technikPR`
- Widget IDs (Elementor `get_name()`)
  - Headline: `technikpr_headline`
  - Page Grid: `tpr31110_page_grid`
- Settings/Optionen
  - Option Key: `technikpr_ew_active_widgets`
  - Admin Menü Slug: `technikpr-ew`
- Assets
  - Handles: `technikpr-page-grid` (CSS/JS)
  - Keine globalen Enqueues; Assets nur via `get_style_depends()` / `get_script_depends()`.
- Page Grid Markup (stabil halten)
  - Wrapper: `.tpr-31110-page-grid` (inkl. optional `.is-cols-custom`)
  - Items: `.tpr-31110-page-grid__items`
  - Card: `.qodef-m-content`
  - Link/Inner: `.qodef-m-content-inner.qodef-m-content-link` (ein Link pro Card)
  - Title/Sub: `.qodef-m-title`, `.qodef-m-subtitle`
  - Arrow (dekorativ): `.qodef-m-arrow` als `<span>` (kein nested `<a>`)
  - Data attributes: `data-display`

# F) A11y-Hinweise
- Page Grid: ein Link pro Card (ganze Box klickbar, kein verschachtelter Link).
- Sichtbarer Fokus: `:focus-visible` Styling für `.qodef-m-content-link`.
- Keine `tabindex`-Hacks auf non-interactive Elementen.
- Tab-Reihenfolge: stabil, da pro Card nur ein Fokusziel.

# G) Edge-Cases
- Taxonomy `technikpr_page_category` existiert nicht
  - Control-Options leer; Widget funktioniert weiter (kein Fatal).
- Kategorie leer + `show_all_when_empty` = no
  - Query zeigt explizit keine Ergebnisse (`post__in = [0]`).
- Responsive Verhalten
  - Custom-Ranges wirken nur mit `.is-cols-custom` + Inline Vars.
  - CSS-Regeln, die auf `#technikpr` scopen, greifen nur wenn Container-ID vorhanden.
- Caching / Performance
  - `WP_Query` läuft pro Widget-Instanz; bei vielen Instanzen ggf. Caching/Query-Reduktion prüfen.
- Assets
  - Ohne Widget auf Seite: keine `technikpr-page-grid` Assets.
  - Mit Widget: Assets genau einmal, widget-gebunden.

# H) Change Notes
- v1.0: Initiale Pilot-Dokumentation (Remote Knowledge) für „TECHNIKPR Elementor Widgets“ inkl. Page Grid Integration.
