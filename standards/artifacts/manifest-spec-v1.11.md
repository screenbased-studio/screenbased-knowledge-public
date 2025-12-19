# Manifest Specification v1.11 (FINAL — Split Schema)

## Basics
- ZIP root folder = root_slug/
- manifest.json liegt immer unter:
  <root_slug>/manifest.json
- Alle Pfade im Manifest sind relativ zu root_slug
- total_* zählen NUR inkludierte Dateien + manifest.json
- Ignorierte Dateien zählen NICHT zu total_*

## Structure
schema_version = 1

Top-Level-Felder:
- root_slug: string
- schema_version: number (1)
- constraints: object (Echo der angewendeten Constraints)
- files: array (nur inkludierte Dateien)
- ignored_entries: array (nur ignorierte Dateien)
- included_count: number (files ohne manifest.json)
- ignored_count: number
- total_files: number (included + manifest.json)
- total_bytes: number

## files[] (Included Only)
Jeder Eintrag enthält:
- path: string (relativ zu root_slug)
- sha256: string (hex)
- size_bytes: number

Pflicht:
- manifest.json MUSS sich selbst als Eintrag enthalten:
  { "path": "manifest.json", "sha256": "...", "size_bytes": ... }

## ignored_entries[]
Jeder Eintrag enthält:
- requested_path: string
- normalized_path: string
- reason: string (ENUM)

## Ignore Reasons (ENUM — Fixed)
- extension_not_allowed
- path_traversal_or_invalid
- nested_archive_disallowed
- size_limit_exceeded
- file_count_exceeded

## Manifest Self-Hash (Mandatory)
- Für den manifest.json-Eintrag wird sha256 über die Manifest-JSON-Bytes berechnet,
  wobei der sha256-Wert dieses Eintrags temporär auf
  "0000000000000000000000000000000000000000000000000000000000000000"
  gesetzt wird.
- Diese Regel MUSS identisch für Chat-Manifest und ZIP-Manifest angewendet werden.
