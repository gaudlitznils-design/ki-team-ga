# Microsite „Unser KI-Team“

Statische Seite für GitHub Pages, ohne Build-Schritt. Inhalt und Design sind getrennt:
`index.html` enthält nur Darstellung und Logik, alle Inhalte stehen in `data/`, die Bilder in `img/`.

## Regel

**data/ und img/ pflegt der Teamchef bzw. Media-Mike, Design nur in index.html.**

Neue Teammitglieder, Texte, Farben oder Gerichte also nie in `index.html` eintragen, sondern in den JSON-Dateien.
Umgekehrt gehören Layout, Farben der Seite selbst und Verhalten nicht in `data/`.

## Dateistruktur

```
index.html          Seite: HTML, CSS und JavaScript (lädt data/*.json per fetch)
data/team.json      Teammitglieder (Organigramm, Detailansicht, Köpfe oben)
data/gerichte.json  Gerichte-Liste und Verlauf (Essensplan der letzten Woche)
img/<skill>.jpg     Porträts, eins je Teammitglied
.nojekyll           GitHub Pages ohne Jekyll ausliefern
README.md           Kurzbeschreibung
CLAUDE.md           diese Datei
```

## data/team.json

```json
{
  "stand": "YYYY-MM-DD",
  "team": [
    {
      "name": "Kunibert",
      "skill": "kunibert",
      "rolle": "Unser Koch",
      "farbe": "#D9861C",
      "bild": "img/kunibert.jpg",
      "kurz": "Ein Satz für die Karte.",
      "stimme": "Typischer Spruch, erscheint als Sprechblase in der Detailansicht.",
      "faehigkeiten": [
        { "name": "Woche planen", "text": "Ein Satz in Ich-Form." }
      ],
      "beispiel": "Beispielsatz, wie man ihn anspricht."
    }
  ]
}
```

- `stand`: Datum der letzten Änderung, erscheint unten auf der Seite.
- `skill`: Kennung in Kleinbuchstaben mit Bindestrich, z. B. `media-mike`. Daraus werden der Bildname und der Seitenanker `#p-<skill>`.
- Alle Einträge außer dem Teamchef stehen in der Reihenfolge der Datei oben im Organigramm (links nach rechts, auf dem Handy von oben nach unten). Dahinter hängt die Seite automatisch die Platzhalter-Karte „Noch frei“ an.
- Der Eintrag mit `"skill": "teamchef"` steht, egal wo in der Datei, mit Abstand darunter; alle Linien laufen bei ihm zusammen.
- `farbe`: Teamfarbe als `#RRGGBB`. Die Seite leitet daraus Schrift-, Flächen- und Dunkelmodus-Farbe ab.
- `bild`: Pfad zum Porträt oder `null`. Ohne Bild erscheint ein neutraler Kreis mit dem Anfangsbuchstaben in der Teamfarbe.
- Vergebene Farben: teamchef `#6D4FA0`, kunibert `#D9861C`, gustav `#23847E`, weihnachtself `#C0392B`, media-mike `#2F6FB5`.

## data/gerichte.json

```json
{
  "stand": "YYYY-MM-DD",
  "gerichte": [
    {
      "n": "Pasta Verdura",
      "text": "Kurzbeschreibung",
      "status": "bewährt",
      "beliebt": "Favorit",
      "koch": "Nils",
      "dauer": 45,
      "rezept": "<Google-Doc-ID oder leer>",
      "zuletzt": "YYYY-MM-DD"
    }
  ],
  "verlauf": [
    { "d": "YYYY-MM-DD", "g": "Gericht", "v": "Variante oder leer", "koch": "Miriam" }
  ]
}
```

- Die Seite nutzt bei Gerichten: `n` (Name, Pflicht), `text`, `status` (`"offen"` = neu zum Ausprobieren, sonst bekannt), `beliebt` (`"Favorit"` = Liebling), `koch` (`"beide"` wird zu „kochen beide“), `dauer` (Minuten), `rezept` (ID eines Google Docs, ergibt den Link „Rezept“), `zuletzt` (Sortierung).
- Weitere Felder aus der Gerichte-DB (`aufwand`, `solo`, `saison`, `basis`, `eiweiss`, `varianten`) dürfen drinstehen, werden aber nicht angezeigt.
- `verlauf`: gekochte Abende. Die Seite zeigt die Woche Freitag bis Donnerstag, in die der letzte Eintrag fällt.

## Bilder

- Format: **512 × 512 Pixel, JPG**, quadratisch, Gesicht mittig (wird rund zugeschnitten).
- Dateiname: `img/<skill>.jpg`, in `data/team.json` unter `bild` eintragen.
- Möglichst unter 60 KB halten (JPG-Qualität um 80).

## Lokal testen

Die Seite lädt ihre Daten per `fetch`, direkt als Datei geöffnet (`file://`) funktioniert das nicht. Im Repo-Ordner starten:

```bash
python3 -m http.server
```

Dann http://localhost:8000 im Browser öffnen. Prüfen: Organigramm, Detailansicht (Klick auf eine Karte), Handy-Breite, Dunkelmodus.
