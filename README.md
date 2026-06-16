# AM Verdienst-Rechner

Interaktives Dashboard, mit dem ein **Account Manager** seinen monatlichen Verdienst aus dem Heartbeat-Incentive-Modell selbst durchrechnet – inkl. Empfehlungen, wie er sein Gehalt steigert, und eines Zielgehalt-Rechners.

**Live:** https://gernotkleindienst.github.io/am-rechner/

---

## Dateien

| Datei | Inhalt |
|-------|--------|
| `index.html` | **Das komplette Dashboard** – eine einzige, eigenständige Datei (HTML + CSS + JavaScript, keine Abhängigkeiten). Doppelklick öffnet sie in jedem Browser. |
| `robots.txt` | Hält Suchmaschinen fern (`Disallow: /`). |
| `README.md` | Diese Datei. |

Es gibt **kein Build-Tool, kein npm, kein Framework**. Alles steckt in `index.html`. Zum Bearbeiten reicht ein Texteditor.

---

## So bearbeitest du den Rechner

Alle Logik und alle Werte stehen im `<script>`-Block unten in `index.html`. Die wichtigsten Stellen:

### 1. Feste Zahlen (Provisionen, Preise) — Objekt `P`
```js
const P = {
  incubator:3500, elevate:2000, masterclass:1000, scaling:3500, legacy:6000,
  betreuung:0.04, verlaengerung:0.14, closing:0.10,
  bewertung:100, ticket:1500, d4yBewerber:1000, d4yFunnel:2000,
  empfehlungNormal:1500, empfehlungRamping:4000, selfCloseBonus:1050,
  testimonialNormal:300, testimonialRamping:700,
  rampingFixum:2000
};
```
Hier änderst du z. B. Produktpreise oder Provisionssätze. Alles andere rechnet automatisch mit.

### 2. Die Hebel (Eingabezeilen) — Array `LEVERS`
Jede Zeile der Eingabetabelle ist ein Eintrag. `unit` = €-Wert pro Stück.
Ein Hebel mit `rampingOnly:true` (Reaktivierung) erscheint nur im Ramping-Modus.

### 3. Verrechnungsmodelle — `mode` / Funktion `unitOf()` / `fixum()`
- **Normal:** Standardmodell.
- **Ramping (Monat 1–3):** Empfehlung 4.000 € statt 1.500 €, Testimonial 700 € statt 300 €, + 2.000 € Fixum, zusätzlicher Hebel „Inaktiver Kunde reaktiviert" (28 % auf 10.500 €).

### 4. Verdienst-Formel — Funktion `verdienst(col)`
> Verdienst = Σ (Anzahl × €-Wert je Hebel) + Fixum (nur Ramping)

### 5. Zielgehalt-Aufstellung — `renderGoal()`
Baut ein realistisches volles Konto von **~45 Kunden** (max **25 Scaling**, Rest Elevate/Accelerator/Masterclass) und gleicht es mit den aktuellen Eingaben ab (`+X aufbauen` / `an Juniors abgeben`). Caps: `CLIENT_CAP_TOTAL = 45`, `SCALING_CAP = 25`.

### 6. Empfehlungen — `renderRecs()`
Die Karten unter „Empfehlungen". Texte/Bedingungen hier anpassen.

---

## Vorschau beim Bearbeiten

Einfach `index.html` im Browser öffnen (Doppelklick). Jede Änderung wird nach „Neu laden" sichtbar. Die eingegebenen Testing-Werte werden pro Browser in `localStorage` gespeichert.

---

## Veröffentlichen / Aktualisieren

Die Live-Seite läuft über **GitHub Pages** und baut sich nach jedem Push automatisch neu (~1 Min).

```bash
git add -A
git commit -m "Beschreibung der Änderung"
git push
```

Voraussetzung: Schreibrechte auf das Repo `gernotkleindienst/am-rechner`. Wer den Ordner übernimmt, braucht entweder Collaborator-Zugriff oder forkt das Repo und hostet selbst (GitHub Pages, Google Sites, oder die Datei einfach direkt verschicken – sie läuft offline).

---

## Hinweise

- **Sichtbarkeit:** Der Live-Link ist technisch öffentlich, aber per `noindex` + `robots.txt` von Suchmaschinen ausgenommen – faktisch nur über den genauen Link erreichbar. Er enthält interne Provisions-/Preisdaten; entsprechend nur intern teilen.
- **Datenquelle:** Das Modell stammt aus „AM Incentive Modell Plan" (Excel). Die Zahlen in `P` und `LEVERS` sind 1:1 daraus übernommen bzw. mit dem Team abgestimmt.
