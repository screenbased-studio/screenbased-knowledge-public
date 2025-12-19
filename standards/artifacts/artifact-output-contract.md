# Artifact Output Contract v1.11 (FINAL)

## Gate (Fail-Closed)
- DEV darf Dateien oder ZIPs NUR ausgeben, wenn das Briefing enthält:
  - scope
  - goal
  - deliverables
- Fehlt eines davon, antworte ausschließlich:
  "BLOCKED: missing briefing fields: …"

## Output Rules
- Kein Erzeugungs-, Hilfs- oder Debug-Code anzeigen
- Nur finaler Output:
  - Metadaten
  - Codeblock (Dateiinhalt)
  - Datei- oder ZIP-Attachment
- Erlaubte Dateitypen (Text-only):
  php, css, js, md, json, yml, yaml, txt
- Keine Secrets oder Tokens
- Keine Pfade mit ".." oder führendem "/"

## Single File Output
Wenn deliverables = single:
- Metadaten:
  - path
  - filename
  - sha256
  - size_bytes
- Codeblock mit vollständigem Dateiinhalt (mit Language-Tag)
- Datei-Attachment, byte-identisch zum Codeblock

## ZIP Output
Wenn deliverables = zip:
- Metadaten:
  - root_slug
  - total_files
  - total_bytes
- Struktur-Tree
- Manifest-JSON im Chat
- ZIP-Attachment mit exakt:
  - Struktur-Tree
  - manifest.json
- Chat-Manifest MUSS byte-identisch zur Datei
  <root_slug>/manifest.json im ZIP sein

## Optional Consistency (Non-Blocking)
- root_slug SHOULD konsistent normalisiert werden
- Empfehlung: ohne trailing slash (z. B. "final-bundle")
- Stilregel, darf Output NICHT blockieren

## Optional Text Normalization (Non-Blocking)
- Beim Erweitern von Textdateien:
  - exakt ein "\n" zwischen bestehendem Inhalt und neuem Text
  - Datei endet mit genau einem abschließenden "\n"
