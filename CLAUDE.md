# AM Verdienst-Rechner — Projektkontext für Claude Code

Interaktives Dashboard, mit dem ein Heartbeat **Account Manager** seinen monatlichen Verdienst aus dem Incentive-Modell selbst durchrechnet — inkl. Empfehlungen zur Gehaltssteigerung und einem Zielgehalt-Rechner.

**Live:** https://gernotkleindienst.github.io/am-rechner/

## Architektur — wichtig

- **Eine einzige Datei: `index.html`.** HTML + CSS + JavaScript inline, **keine Abhängigkeiten, kein Build, kein npm, kein Framework.**
- Bearbeiten = `index.html` mit Edit anpassen. Vorschau = Datei im Browser öffnen (Doppelklick) oder lokalen Static-Server starten.
- Eingaben des Nutzers werden im Browser unter `localStorage`-Key `hb_am_calc_v2` gespeichert.

## Wo was steht (alles im `<script>`-Block in `index.html`)

| Stelle | Inhalt |
|--------|--------|
| `const P = {...}` | Feste Zahlen: Produktpreise, Provisionssätze, Boni. Einzige Quelle der Wahrheit für alle Beträge. |
| `const LEVERS = [...]` | Die Eingabezeilen der Tabelle. `unit` = €-Wert pro Stück. `rampingOnly:true` → nur im Ramping-Modus sichtbar/aktiv. |
| `unitOf(l)` / `fixum()` | Mode-abhängige Werte (Normal vs. Ramping). |
| `verdienst(col)` | **Verdienst = Σ (Anzahl × unitOf(Hebel)) + fixum()** |
| `TESTING_DEFAULT` | Startwerte der „Testing"-Spalte (= „Zurücksetzen"). Aktuell: 30 Accelerator, 5 Elevate, 5 Scaling, 1 Bewertung. |
| `SCEN` (c/d/e) | Beispiel-Szenarien V1/V2/V3 zum Vergleich. |
| `renderGoal()` | Zielgehalt: baut ein realistisch volles Konto (`CLIENT_CAP_TOTAL = 45`, `SCALING_CAP = 25`), Delta zu aktuellen Eingaben („+X aufbauen" / „an Juniors abgeben"), plus Booster pro Stück. |
| `renderRecs()` | Die Empfehlungskarten. |
| `renderTable()` / `renderFix()` | Eingabetabelle bzw. die fixe Datengrundlage unten. |

## Das Vergütungsmodell

Zwei umschaltbare Verrechnungsmodelle (Toggle oben):

- **Normal (Standard):** Empfehlungskunde 1.500 € (+1.050 € fürs Selbst-Closen = 10 % von 10.500 €), Testimonial 300 € (inkl. Bewertung), Verlängerung 14 %, Betreuung 4 %, Cross-Sell/Closing 10 %.
- **Ramping (nur Monat 1–3):** Empfehlung 4.000 € statt 1.500 €, Testimonial 700 € statt 300 €, + 2.000 € Fixum/Monat, zusätzlicher Hebel „Inaktiver Kunde reaktiviert" (28 % auf Starterpaket 10.500 €).

Die Beträge stammen aus „AM Incentive Modell Plan" (Excel) und sind mit dem Team abgestimmt (Testimonial- und Empfehlungs-Logik wurden bewusst gegenüber der Roh-Excel korrigiert).

## Verifizieren

Kein Test-Framework. Nach Änderungen:
1. **JS-Syntax prüfen** (Node ist auf diesem Mac evtl. nicht installiert — dann via osascript):
   Script aus `index.html` extrahieren und `new Function(code)` parsen lassen.
2. **Funktional:** `index.html` im Browser öffnen, Werte eintragen, Verdienst/Empfehlungen/Zielgehalt + Moduswechsel prüfen.

## Deployen

Live-Seite läuft über **GitHub Pages**, baut nach jedem Push auf `main` automatisch neu (~1 Min):

```bash
git add -A && git commit -m "…" && git push
```

Voraussetzung: Schreibrechte auf `gernotkleindienst/am-rechner` (Collaborator) — sonst forken und eigenes Pages aktivieren.

## Konventionen / Hinweise

- Sprache der UI und Commits: **Deutsch**.
- `robots.txt` + `noindex`-Meta halten Suchmaschinen fern. Der Link ist technisch öffentlich und enthält **interne Provisions-/Preisdaten** → nur intern teilen.
- Beim Ändern von Beträgen immer nur `P` (und ggf. `LEVERS`) anfassen — der Rest rechnet automatisch mit.
