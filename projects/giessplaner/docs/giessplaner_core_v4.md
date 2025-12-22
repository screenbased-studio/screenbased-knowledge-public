# PRIVATE PROJECT — GIESSPLANER v4.2.2 ULTIMATE
Screenbased Knowledge File für das private Projekt „Heckengießplaner“.  
Dieses Dokument beschreibt Architektur, Systeme, Logiken, Zero-Regression-Zonen und Projektregeln.

Version: v4.2.2 ULTIMATE (mit Postpone-Funktion)  
Quelle: watering-v422-postpone.html

────────────────────────────────────────
# 1. PROJEKTÜBERSICHT

## Projektname
Heckengießplaner (PRIVATE PROJECT 5120)

## Ziel
Erstellung eines intelligenten, UI-basierten Gießplaners für Heckenpflanzen.  
Der Planer berücksichtigt:
- Pflanzalter
- Wasserbedarf
- Trockenperioden
- Regenfenster
- Verzögerungen („Postpone“)
- Jahreszeiten (Winter-Modus)
- Hanglage
- Historie
- Undo-Mechanismus

## Nutzerziel
Optimale, adaptive Gießempfehlung für alle Pflanzgruppen über die Saison hinweg.

────────────────────────────────────────
# 2. HAUPTSYSTEME DES GIESSPLANERS

## 2.1 AGE SHIFT ENGINE
Das Alter jeder Pflanze beeinflusst den Wasserbedarf:
- Jungpflanzen = hohe Frequenz
- Etablierte Pflanzen = geringere Frequenz
- Altersverschiebung automatisch pro Jahrgang

## 2.2 RAIN WINDOW ENGINE
Regenwerte werden als „Fenster“ interpretiert:
- Reduziert automatisierte Trockenperioden
- Wasserbedarf wird verschoben oder reduziert
- Regen kann Gießtage "neutralisieren"

## 2.3 POSTPONE SYSTEM (NEU in v4.2.2)
Benutzer kann einzelne Gießtage verschieben:
- 1–7 Tage Aufschub
- System recalculated den Rest automatisch
- Postpone beeinflusst:
  - nächsten Gießtermin
  - Trockenperioden
  - Gesamtoffset

## 2.4 DROUGHT STREAK FIX
Wenn Pflanzen über mehrere Tage trockenstehen:
- das System erkennt Trockenserien
- empfiehlt frühzeitig Korrekturen
- verhindert schleichende Unterversorgung

## 2.5 WINTER GATE
Wenn Temperaturen niedrig oder Winterzeit →  
**Gießlogik wird deaktiviert**, außer manuell.

## 2.6 SCHEDULE ENGINE (CORE)
Berechnet Hauptausgabe:
- Nächster Gießtermin
- Anzahl trockener Tage davor/danach
- Verschiebungen durch Regen oder Postpone
- Gruppenlogik (mehrere Zonen)

## 2.7 SLOPE OFFSET
Hanglage beeinflusst Wasserverteilung:
- + Offset (Oberer Bereich trockener)
- - Offset (Unterer Bereich feuchter)

Offset wird direkt in die Berechnung eingewoben.

## 2.8 GROUP ENGINE
Pflanzen sind gruppiert:
- Name
- Pflanzjahr
- Hanglage
- Gießprofil
Jede Gruppe hat eigene Berechnungspfade.

## 2.9 HISTORY ENGINE
System dokumentiert:
- vergangene Gießaktionen
- postpones
- Regenwerte
- Fehlerkorrekturen
- Undo Snapshots

## 2.10 UNDO SNAPSHOT SYSTEM
Vor jeder Veränderung:
- aktueller Zustand gesichert
- Undo stellt vorherigen Zustand her

## 2.11 UI ENGINE
Besteht aus:
- Date Input
- Dropdowns für Age/Hanglage
- Berechnungsfeldern
- Ergebnisblöcken
- Postpone Buttons
- Reset-Funktionen

UI stärkt Verständnis der Logik.

────────────────────────────────────────
# 3. DATENSTRUKTUR

Pseudostruktur aus HTML/JS:

groups = [
  {
    id: "g1",
    name: "Hecke Bereich A",
    plantYear: 2022,
    slope: 0,
    history: [],
    postpone: 0
  },
  { … }
]

Regenwerte:
rainWindows = [
  { date: "2025-05-21", mm: 4 },
  { … }
]

Historie:
actionHistory = [
  { date, action, value }
]

Global:
- winterMode: boolean
- today: Date
- globalOffset
- droughtFixEnabled

────────────────────────────────────────
# 4. ARCHITEKTUR

## Layer 1 – UI
HTML-Struktur mit:
- Eingabefeldern
- Buttonflächen (Postpone + Undo)
- Ergebnisblöcken
- dynamischen Anzeigen

## Layer 2 – Core Logic
Funktionen:
- calculateNextWatering()
- applyRainWindows()
- applyPostpone()
- adjustForAgeShift()
- adjustForSlope()
- droughtStreakFix()
- winterGate()

## Layer 3 – State Management
- globale Variablen
- gruppenspezifische States
- Snapshot/Undo

## Layer 4 – Output
- UI-Rendering
- Gießempfehlung
- Trockenperioden-Info
- Verzögerung/Offset-Info

────────────────────────────────────────
# 5. ZERO-REGRESSION ZONEN
Diese Bereiche **dürfen vom Director/Dev NIE verändert werden**, außer ausdrücklich angefordert:

### 5.1 Age Shift Formel  
Logik der Altersberechnung darf nicht verändert werden.

### 5.2 Rain Window Interpretation  
Regen neutralisiert/verschiebt Trockenperioden → Funktion nicht verändern.

### 5.3 Postpone System  
Aufschubmechanismus ist kritisch → keine Veränderung der Kernlogik.

### 5.4 Drought Streak Fix  
System zur Erkennung trockener Ketten darf nicht entfernt werden.

### 5.5 Winter Gate  
Winterabschaltung bleibt fix.

### 5.6 Slope Offset  
Hanglage-Offset Berechnung nicht verändern.

### 5.7 Schedule Engine  
Hauptberechnungslogik niemals überschreiben.

### 5.8 Undo Snapshot System  
Undo darf niemals deaktiviert oder verändert werden.

────────────────────────────────────────
# 6. DESIGN- & UX-GRUNDSÄTZE (NEUTRAL)
- klare, helle UI
- direkte Ergebnislesbarkeit
- Input links → Ergebnis rechts
- keine CI nötig (private App)
- Fokus auf Transparenz der Berechnung

────────────────────────────────────────
# 7. OFFENE FEATURES / ROADMAP (OPTIONAL)
Hier Empfehlungen für zukünftige Versionen:

- Regen-API-Einbindung
- Forecast-Integration
- Hydrologisches Modell
- Pflanzenkatalog
- Mobile-App-Version (PWA)
- Light/Dark Mode
- Multi-Garten-Unterstützung
- Export von Pflegeprotokollen

────────────────────────────────────────
# 8. DIRECTOR-VERHALTEN

Wenn Nutzer sagt:
„Director, privates Projekt Heckengießplaner …“

Dann:
- lade **diese Datei**
- kein Kunde
- keine CI
- keine Kundensysteme
- vollständige Projektlogik aktiv
- Briefings wie bei Kunden generieren (DEV, DESIGN, ARCH etc.)
- Zero Regression beachten

────────────────────────────────────────
# ENDE DER DATEI
